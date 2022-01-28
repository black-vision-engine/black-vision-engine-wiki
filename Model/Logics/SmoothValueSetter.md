Założenia projektowe
====================

Logika ta wpina się w konkretny parametr pluginu.

Kiedy logika jest wpięta w ten parametr - overrideuje klasyczną jego
interpolację

zadaniem logiki jest płynna interpolacja między konkretnymi wartościami
wpiętego parametru w zadanym czasie, np.:

1.  Podpinamy tę logikę do node-a (nic się jeszcze nie dzieje)
2.  Podpinamy pod nią parametr alpha pluginu texture
3.  Alpha pluginu texture ustawia się na domyślną wartość pluginu
    SmoothValueSetter (0)
4.  Ustawiamy w logice interpolację cosinusową i czas interpolowania na
    1.5 sekundy
5.  Zmieniamy w logice SmoothValueSetter wartość na 1.0
6.  Logika wykrywa że został zmieniony jej parametr kluczowy i za pomocą
    coisnusowego interpolatora przez najbliższe 1.5 sek zmienia wartość
    parametru alpha textury z 0.0 na 1.0

<!-- -->

1.  Jeśli ktoś w międzyczasie usunie plugin texture logika
    odpowiedzialna za interpolację parametru alpha powinna się
    zdezaktywować

<!-- -->

1.  W ramach logiki powinniśmy móc dodać dowolną liczbę parametrów (z
    których każdy ma niezależny interpolator, czas trwania oraz wartość)

Opis logiki
===========

Logika pozwala na płynne zmienianie wartości parametrów.

Parametry
---------

**SmoothTime**

Każdy parametr źródłowy ma bliźniaczy parametr o nazwie
\[SourceName\]\_SmoothTime, który pozawala ustawić czas po jakim
zostanie osiągnięta docelowa wartość parametru. Parametr jest ewaluowany
przy każdej zmianie parametru, co oznacza, że może on mieć wartości
zmienne w czasie.

Obsługa logiki
==============

Tworzenie
---------

     {
        "Event" : "NodeLogicEvent",
        "Command" : "AddNodeLogic",
        "EventID" : "1",
        "NodeName" : "root",
        "SceneName" : "SmoothValueSetterTest.scn",
        "Action" : 
        {
            "logic" : 
            {
                "type" : "SmoothValueSetter",
                "timelinePath" : "SmoothValueSetterTest%default"
            }
        }
     }

Dodawanie powiązań wartości
---------------------------

Logika SmoothValueSetter ma zestaw parametrów (**SourceName**), które są
źródłami wartości dla parametrów w scenie. Do każdego parametru logiki
można podpiąć dowolną ilość parametrów scenicznych. Dzięki temu zmiana
tej jednej wartości parametru źródłowego skutkuje modyfikacją wszystkich
podpiętych do niego parametrów.

Podpinanie parametrów realizuje się wysyłając event:

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root",
        "SceneName" : "SmoothValueSetterTest.scn",
        "Action" : 
        {
            "Action" : "AddParamBinding",
            "NodeName" : "wynik1",
            "PluginName" : "cube",
            "ParamName" : "dimensions",
            "SourceName" : "wynik1",
            "SourceParamType" : "vec3"
        }
     }

Parametr SourceName jest tworzony, jeżeli jeszcze nie istnieje. W
przeciwnym razie do istniejącego już parametru źródłowego podpinany jest
nowy parametr sceniczny. Nowo utworzony parametr źródłowy pobiera swoją
wartość początkową z parametru scenicznego, do którego został podpięty.

Parametry źródłowe są widoczne z poziomu eventu ParamKeyEvent. W ten
sposób można modyfikować krzywą interpolacji. Nie należy modyfikować
kluczy tych parametrów ręcznie. Parametry źródłowe są ustawiane przez
logikę po wysłaniu eventu **SetParameter**. Wszystkie klucze, które
stoją za aktualnie ustawianym, zostają usunięte.

### Wiązania dla parametrów złożonych

Dla parametrów takich jak transformacja, trzeba podać dodatkowe
informacje takie jak typ transformacji (**Kind**):

-   center
-   rotation
-   scale
-   translation

oraz komponent wektora, który ma być modyfikowany (zmienna
**Component**).

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root",
        "SceneName" : "SmoothValueSetterTest.scn",
        "Action" : 
        {
            "Action" : "AddParamBinding",
            "NodePath" : "wynik4",
            "PluginName" : "transform",
            "ParamName" : "simple_transform",
            "SourceName" : "wynik3",
            "SourceParamType" : "float",
            "Kind" : "translation",
            "Component" : "X"
        }
     }

Ustawianie wartości parametrów źródłowych
-----------------------------------------

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root",
        "SceneName" : "SmoothValueSetterTest.scn",
        "Action" : 
        {
            "Action" : "SetParameter",
            "SourceName" : "wynik1",
            "Value" : "0.2, 1.0, 0.2"
        }
     }

Kasowanie powiązań parametrów
-----------------------------

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root",
        "SceneName" : "SmoothValueSetterTest.scn",
        "Action" : 
        {
            "Action" : "RemoveParamBinding",
            "NodePath" : "wynik2",
            "PluginName" : "cube",
            "ParamName" : "dimensions",
            "Component" : "Y"
        }
     }
     

Listowanie powiązań parametrów
------------------------------

Wysłanie eventu wyświetla listę wszystkich parametrów źródłowych i ich
powiązań.

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root",
        "SceneName" : "SmoothValueSetterTest.scn",
        "Action" : 
        {
            "Action" : "ListBindings"
        }
     }

Listę parametrów źródłowych można też pobrać odpytując logikę o
zarejestrowane parametry.