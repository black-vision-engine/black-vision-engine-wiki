# Problemy

## Kopiowanie DefaultParamValModel:

- zaimplementowanie funkcji virtualnej Copy dla wszystkich podklas:
    - parametry
    - valuesy
    - evaluatory
    - staty
- w zasadzie to prosty przypadek, bo użytkownik nic nie implementuje sam
- problem: timeliny
    - dopóki jesteśmy w tej samej scenie to po prostu przepinamy wskaźnik
    - jak kopiujemy do innej sceny to zaczynają się problemy
        - jeżeli timeline o tej samej nazwie istnieje w drugiej scenie, to podpinamy pod niego
        - jeżeli timeline o tej nazwie nie istnieje, to powinniśmy go stworzyć
        - skąd w ogóle DefaultParamValModel ma wiedzieć, że przepinamy między scenami? Wygląda na to, że trzeba zrobić jakiś postprocessing który przepnie te timeliny.

## Kopiowanie logik

Tutaj niestety twórca logiki będzie musiał sobie poradzić sam najczęściej. Być może da się dostarczyć domyślną implementację, która będzie potrafiła po prostu skopiować DefaultParamValModel i wszystkie zmienne lokalne, ale to nie zawsze będzie dobre, np:

- Co jeżeli logika zapisuje wskaźniki na nody w poddrzewie ?
- W zasadzie logika może zawierać dowolne pola i nie zawsze będzie wiadomo, czy da się po prostu je skopiować.

## Kopiowanie efektów

To chyba jest bajecznie proste, bo efekty mają tylko model.

## Kopiowanie Pluginów

### Propozycja 1

BasePlugin ma metodę wirtualną Clone. Każdy plugin musi zaimplementować coś takiego:

```
#!c++


BasePluginPtr DerivedPlugin::Clone()
{
    return DerivedPlugin( *this );
}
```

To jest minimum kodu, który musiałby zaimplementować twórca pluginu. Ale do tego trzeba by spełnić następujące warunki:

- Zaimplementować konstruktor kopiujący dla kanałów shaderowych i geometrii
- Do klasy bazowej trzeba przenieść takie rzeczy jak:
    - DefaultVertexShaderChannelPtr, VertexAttributesChannelPtr, DefaultPixelShaderChannelPtr - wystarczyłoby zaimplementować kopiowanie w konstruktorze kopiującym BasePluginu
- Zaimplementować konstruktor kopiujący dla ValueParamState. Tylko nie wiem jak ta klasa miałaby pobrać wskaźniki na valuesy i paramy. Być może niestety się nie da bez dodatkowego kodu w pluginie.
- Nie używać bezpośrednio Valuesów i Paramów w pluginach. Ten sam problem, co z ValueParamState.

#### **Wady**:

- Dużo roboty dla twórców pluginów, znacznie przedłuży czas pisania. Prawdopodobnie nie da się zrobić tego tak, żeby wystarczyło napisać tyle kodu ile powyżej.
- Dużo zmian w kodzie. Praktycznie każdy plugin trzeba zmienić.
- Jeszcze nie wymyśliłem, co z kopiowaniem między scenami i problemem z timelinami


#### **Zalety**

- Nie trzeba od nowa generować geometrii, zostanie ona po prostu skopiowana

#### Propozycja 1.2 (iteracja 2)

Kopiowanie nodów jest wywoływane od ostatniego pluginu. Każdy plugin wywołuje funkcję kopiującą dla swojego poprzednika (ten kod powinien się znaleźć w funkcji PluginBase). Klonowanie trzeba zaimplementować w Finalize pluginie (nie można użyć funkcji AttachPlugin, bo ona wywołuje SetPrevPlugin, co może spowodować przegenerowanie całej geometrii).

Konstruktor kopiujący dla pluginu może wyglądać mniej więcej tak:

```
#!c++


TypedPlugin::TypedPlugin( const TypedPlugin & pluginToCopy )
    : BasePlugin( pluginToCopy )
{
    // Przepisanie valuesów i zmiennych lokalnych, przykładowo:
    auto modelVAC = GetPluginParamValModel()->GetVertexAttributesChannelModel();

    m_fontSize              = QueryTypedValue< ValueFloatPtr >( modelVAC->GetValue( PARAMS::FONT_SIZE ) );
    m_spacingValue          = QueryTypedValue< ValueFloatPtr >( modelVAC->GetValue( PARAMS::SPACING ) );
}

```

Ponieważ ten sam kod trzeba wywołać w konstruktorze (nie kopiującym), to będzie można to zapakować w jedną funkcję i pisanie konstruktora kopiującego nie będzie kosztowało zbyt dużo pracy.

Kod w BasePlugin musi skopiować poprzedni plugin, a następnie skopiować parametry, kanały (geometria, pixel shader - w tym się zawierają assety, vertex shader), Renderer Context.


```
#!c++

BasePlugin::BasePlugin( const BasePlugin & pluginToCopy )
{
    m_prevPlugin = pluginToCopy.GetPrevPlugin()->Clone();

    m_psc = pluginToCopy.GetPSC()->Clone();
    m_vac = pluginToCopy.GetVAC()->Clone();
    m_vsc = pluginToCopy.GetVSC()->Clone();
    m_rendererContext = pluginToCopy.GetRendererContext()->Clone();
}
```

#### **Wady**:

- Dużo przeróbek względem obecnej wersji (każdy plugin).

#### **Zalety**

- Geometria nie musi być generowana od nowa, będzie kopiowany cały kanał.
- Twórca pluginów nie będzie miał do napisania wcale aż tak dużo jak myślałem. Wystarczy dobrze zorganizować kod, dostarczyć, dobrze napisane przykłady + snippety, które same wygenerują większość kodu.
- Twórca pluginu nie będzie musiał rozumieć więcej niż konieczne (w poprzedniej iteracji miałem obawę, że twórca będzie musiał świadomie pisać kod tak, żeby geometria się nie przegenerowała. W tej wersji trudniej będzie coś zepsuć)

### Propozycja 2

```
#!c++

BasePluginPtr PluginBase::Clone()
{
    auto pluginType = this->GetPluginType();    // Na pewno UID pluginu da się wyciągnąć jakoś.
    IPluginPtr plugin = PluginsManager::DefaultInstanceRef().CreatePlugin( pluginType, pluginName, te );

    CopyParameters( this, plugin );
    CopyAssets( this, plugin );
}
```

Pomysł jest taki:

- Tworzymy plugin przez PluginsManager, UID pobieramy sobie z pluginu, który kopiujemy, jakąś metodą wirtualną.
- Plugin się stworzył z domyślnymi wartościami parametrów, więc ustawiamy mu parametry z ''this''.
- Plugin ma pewni jakiś domyślny asset, więc też musimy go mu ręcznie ustawić.
- W kolejnym updacie stan wewnętrzny pluginu ustawi się na dokładnie taki jak trzeba. To w zasadzie jest analogiczne do tego co robi serializacja obecnie.

#### **Zalety**

- Twórca pluginu nie musi w ogóle pisać funkcji do kopiowania
- Mamy do zrobienia stosunkowo mało zmian w kodzie. Nie tykamy implementacji pluginów.
- Twórca pluginu nadal może zaimplementować kopiowanie po swojemu, żeby np. je jeszcze bardziej przyspieszyć.

#### **Wady**:

- Może być troszkę wolniejsze niż wersja z konstruktorem kopiującym.
- W tej wersji geometria musi się zawsze wygenerować od nowa w kolejnym updacie. Nie wiem czy dałoby się
zaimplementować jakąś funkcję w GeometryPluginBase, która by wszystko kopiowała i nie wymagała implementacji 
w każdym pluginie z osobna.
- Jeszcze nie wymyśliłem, co z kopiowaniem między scenami i problemem z timelinami


### Propozycja 3

Napisać klasę, która będzie implementowała interfejs serializatora i deserializatora.
Będzie ona sobie normalnie tworzyła całe drzewo wewnątrz, ale nie będzie robiła żadnego zapisywania do pliku ani nic. Po prostu po serializacji sceny będzie się ją traktowało jako deserializator i podawało na wejście pluginów.

W ten sposób wszystko będzie działało tak samo jak teraz, a jakieś przyspieszenie da się w tym osiągnąć. Być może nawet dało by się wykorzystać jedną z istniejących implementacji serializatora. Np. wziąć sobie JsonCPP i go odrobinę zmodyfikować.

#### **Zalety**

- To chyba jest minimum pracy jakie się da włożyć, bo wszystko w pluginach i wszystkim zostaje tak samo
- Na pewno nic się nie popsuje
- Nie mamy problemu z kopiowaniem timelinów między scenami, bo wszystko już jest zaimplementowane w serializacji

#### **Wady**:

- Być może zysk z takiej implementacji nie będzie satysfakcjonujący.

# Rozwiązania

Dla timelinów trzeba zrobić jakiś postprocessing. Jeżeli kopiowanie odbywa się między scenami, to najpierw się skopiuje wszystko jak leci, a potem trzeba stworzyć timeliny, których brakuje i poprzepinać do nich parametry.
Generalnie problemy z timelinami nie ingerują w proces kopiowania.