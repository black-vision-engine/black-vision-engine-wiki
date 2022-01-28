# Tworzenie gizm

Ta strona opisuje jak twórca pluginu/efektu/logiki może stworzyć do niego gizmo. Na początek najlepiej zapoznać się z ogólnym opisem [tutaj](Gizmo).

## Logika gizma

Kluczowym elementem gizma jest jego logika wpięta w *gizmoRoota*, która zarządza całym poddrzewem. Logika ta dziedziczy po specjalnym interfejsie IGizmoLogic, który dodaje kilka funkcji do interfejsu INodeLogic.

### Inicjalizacja i tworzenie poddrzewa gizma

Tak jak w przypadku każdej logiki przy dodawaniu logiki jest wywoływana funkcja Initialize, a przy usuwaniu Deinitialize. Pisząc kod w funkcji Initialize trzeba pamiętać, że jest ona wywoływana, zanim gizmoRoot node zostanie podpięty przez BVProjectEditor. Z tego względu, jeżeli chce się korzystać z niektórych funkcjonalności BVProjectEditora, należy ten kod umieścić w specjalnie przygotowanej do tego funkcji wirtualnej CreateGizmoSubtree.

### Interface logiki gizma

Oprócz funkcji wirtualnej do tworzenia poddrzewa, zostały dodane również dwie funkcje **PreOwnerUpdate** i **PostOwnerUpdate**, które są wywoływane przed i po updacie *gizmoOwnera*. Uwaga! Wszystkie funkcje wirtualne z interfejsu INodeLogic są wywoływane w funkcji Update *gizmoRoota* i więc dotyczą jedynie poddrzewa gizma. Z tego względu trzeba było dodać nowe hooki, żeby można było wywołać jakiś kod przed i po zaupdatowaniu *gizmoOwnera*.

### GizmoLogicBase

Klasa bazowa dla gizm, która dostarcza domyślne implementacje niektórych funkcji. Klasa tworzy model parametrów, zapisuje *gizmoRoota* oraz *gizmoOwnera*. Większość funkcji jest zaimplementowana jako pusta. Funkcja update wywołuje Update parametrów. Przy pisaniu własnej funkcji update należy pamiętać, żeby wywołać Update z klasy bazowej.

Ponieważ gizma z założenia nie są serializowane i deserializowane, również implementacja tych funkcji jest pusta.

### Rejestrowanie logiki

Ponieważ logika gizma jest inicjowana dodatkowym parametrem zawierającym wskaźnik na gizmoOnwera, to trzeba ją rejestrować funkcją **NodeLogicFactory::RegisterGizmo**, a nie **NodeLogicFactory::RegisterLogic** jak w przypadku zwykłej logiki.

## Rejestrowanie funkcjonalności gizm

Aby edytor mógł stworzyć gizmo, trzeba dodać mapowanie funkcjonalności i logiki, która ma zostać stworzona, w odpowiednim miejscu. Dla gizm typu:

- **Scene** - W klasie GizmoManager
- **Node** - W klasie GizmoManager
- **Plugin** - W deskryptorze pluginu przy pomocy funkcji RegisterGizmo
- **Logic** - W klasie NodeLogicFactory przy rejestrowaniu logiki zwracany jest deskryptor, na którym można wywołać funkcję RegisterFunctionality.
- **Effect** - W klasie ModelNodeEffectFactory należy dodać mapowanie w funkcji QueryGizmoName

### Kolejność wywołań funkcji podczas Updatu

Update pojedynczego node'a zawiera następujące wywołania:

- **PreOwnerUpdate** na wszystkich gizmach w danym nodzie
- **PreNodeUpdate** na logice przyczepionej do node'a
- Update efektu
- Update listy pluginów
- Update bounding boxa
- Funkcja **Update** logiki node'a
- Funkcja **Update** gizm (tutaj jest wywoływana funkcja Update dla wszystkich gizmoRootów w danym nodzie)
    - **PreNodeUpdate** na logice gizma
    - Update efecktu gizmoRoota
    - Update pluginów gizmoRoota
    - Update BB gizmoRoota
    - **Update** logiki gizma
    - **Update** dzieci gizmoRoota
    - Update BB, żeby zawierał dzieci
    - **PostChildrenUpdate** - logiki gizma
- **Update** dzieci node'a
- Update BB, żeby zawierał dzieci
- **PostChildrenUpdate** - logiki node'a
- **PostOwnerUpdate** - na logice gizma

GizmoRooty są updatowane w kolejności w jakiej zostały dodane. Zasadniczo gizma trzeba pisać tak, żeby kolejność nie miała znaczenia.