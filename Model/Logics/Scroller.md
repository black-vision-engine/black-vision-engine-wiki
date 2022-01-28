Opis logiki
===========

Radek opiszesz go tak jak jest zrobiony teraz?

Z jakimi pluginami się łączy
============================

[transform](transform "wikilink")

Obsługa za pomocą jsona
-----------------------

### Dodawanie crawlera

Do stworzenia crawlera należy wysłać następujący event:

     {
        "Event" : "NodeLogicEvent",
        "Command" : "AddNodeLogic",
        "EventID" : "1",
        "NodeName" : "Dummy0",
        "SceneName" : "sceneFromEnv: STARWARS_TEST_SCENE",
        "Action" :
        {
            "logic" : 
            {
                "interspace" : "0.400000",
                "speed" : "0.100000",
                "type" : "crawler",
                "view" : 
                {
                    "empty" : "false",
                    "xmax" : "1.000000",
                    "xmin" : "-1.000000",
                    "ymax" : "1.000000",
                    "ymin" : "-1.000000"
                }
            }
        }"
     }

Crawler przyjmuje w JSONie listę node'ów, które ma obsługiwać. Node'y te
muszą być bezpośrednimi dziećmi node'a, do którego został przyczepiony
crawler. Przy przyczepianiu node'ów crawler bierze pod uwage jedynie
indeksy, nazwy są dodane jedynie informacyjnie.

Dodawanie node'ów może się odbyć również na późniejszym etapie. Lista
node'ów może być pusta.

\[opisać parametry view, speed, interspace\]

### Obsługa crawlera

\[do napisania\]

-   Reset
-   Clear
-   Stop

#### Dodawanie node'ów

Crawler może sam stworzyć node'a na podstawie presetu wskazanego w
evencie. Węzeł zostanie podpięty jako dziecko node'a, do którego
przyczepiony jest crawler.

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "SceneName" : "sceneFromEnv: STARWARS_TEST_SCENE",
        "NodeName" : "Dummy0",
        "Action" :
        {
            "Action" : "AddPresetNode",
            "NewNodeName" : "PresetNode1",
            "TimelinePath" : "sceneFromEnv: STARWARS_TEST_SCENE",
            "PresetProjectName" : "",
            "PresetPath" : "witek/crawlerPreset.bvpreset"
        }
     }

Można też dodać dowolny node znajdujący się w poddrzewie node'a, do
którego przyczepiony jest crawler wysyłając event:

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "SceneName" : "sceneFromEnv: STARWARS_TEST_SCENE",
        "NodeName" : "Dummy0",
        "Action" :
        {
            "Action" : "AddNode",
            "NodeName" : "Text"
        }
     }

#### Uruchamianie Crawlera

Następnie trzeba wysłać event Start. Event ten powoduje ustawienie
node'ów w docelowej pozycji i uruchomienie przewijania

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "SceneName" : "sceneFromEnv: STARWARS_TEST_SCENE",
        "NodeName" : "Dummy0",
        "Action" :
        {
            "Action" : "Start"
        }
     }

#### Dodawanie tekstu

Dodawanie tekstu do crawlera odbywa się przez event:

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "SceneName" : "sceneFromEnv: STARWARS_TEST_SCENE",
        "NodeName" : "Dummy0",
        "Action" :
        {
            "Action" : "AddText",
            "Message" : "Nie wierzę, to działa"
        }
     }

#### Ustawianie prędkości przewijania

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "SceneName" : "sceneFromEnv: STARWARS_TEST_SCENE",
        "NodeName" : "green rect",
        "Action" :
        {
            "Action" : "SetSpeed",
            "Speed" : "1.0"
        }
     }

#### Usuwanie node'ów ze scrollera

**RemoveNodes** pozwala usunąć listę node'ów ze scrollera. Dodanie flagi
DeleteFromTree oznacza usunięcie danego node'a również z poddrzewa
crawlera. Po usunięciu node'a trzeba ponownie wystartować scroller, żeby
node'y zostały od nowa ustawione.

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "SceneName" : "sceneFromEnv: CRAWLER_TEST_SCENE",
        "NodeName" : "green rect",
        "Action" :
        {
            "Action" : "RemoveNodes",
            "DeleteFromTree" : "true",
            "NodesArray" :
            [
                {
                    "NodePath" : "green rect/#0"
                }
            ]
        }
     }

#### Dodawanie marginesów do elementów scrollera

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "SceneName" : "sceneFromEnv: CRAWLER_TEST_SCENE",
        "NodeName" : "green rect",
        "Action" :
        {
            "Action" : "SetNodeMargin",
            "NodePath" : "#0",
            "MarginLeft" : "2.0",
            "MarginRight" : "0.5",
            "MarginTop" : "0.0",
            "MarginBottom" : "0.0"
        }
     }

Crawl - poszerzony opis
=======================

Dostępne funkcje
----------------

### Start

Uruchamia przewijanie Crawla

### Reset

Ponownie ustawia node-y w swoich zerowych pozycjach

### Stop

Zatrzymuje przewijanie Crawla i ustawia wszystkie elementy poza ekranem,
tak by były ponownie gotowe do wyświetlenia

### Pause

Pauzuje przewijanie Crawla (Start wznawia jego przewijanie)

### Clear

Usuwa node'y spod kontroli scrollera.

### SmoothStart

Płynnie startuje przewijanie Crawla (speed rośnie od 0 do zadanej
wartości Speed crawla)

### SmoothPause

Płynnie pauzuje przewijanie Crawla

### AddPreset(String PresetPath, String name)

Wczytuje preset do NodePtr node i następnie Crawl.AddNode(node)

### AddPresetAndFillWithData(String PresetPath, String name, String\[\] txt, String \[\] img)

Wczytuje preset do NodePtr node

Następnie uzupełnia node-a wartościami przekazanymi jako tablice jsonowe
w następujący sposób:

     int val=1
     foreach (string s in txt)
     {
       NodePtr text_node = node.find("text_"+val);
       text_node.SetText(s)
       val++
     }
     val=1
     foreach (string s in img)
     {
       NodePtr text_node = node.find("image_"+val);
       ImageAsset asset = ImageAssetFromPath(s)
       text_node.SetImageAsset(asset)
       val++
     }

W dużym skrócie wyszukuje nodey prefiksowane “text\_” oraz “image\_” i
odpowiednio ustawia wartości

Następnie Crawl.AddNode(node)

### AddNode(NodePtr node, String Name)

Dodaje nodea do crawla, powinno działać zarówno wtedy kiedy crawl stoi w
miejscu oraz kiedy jest uruchomiony

0\. Jeśli Name jest niepoprawną nazwą zamienia ją na nazwę zgodną z BV
(np. “item\_”+(miejsce w liscie na którym dodawany jest node).ToString()
1. Dodaje node jako ostatnie dziecko Crawla o nazwie Name 2. Wrzuca go
do wewnętrznej listy niewyświetlonych node-ów 3. Ustawia odpowiednio
jego pozycję (x,y)

Następnie w responsie wysyła ścieżke do crawla w postaci (\#0/\#0) oraz
nazwę nodea który został dodany

Eventy
------

### ItemOnScreen

Wysyła event z ścieżka do node-a z crawlem (“\#0/\#0”) oraz nazwą node-a
który właśnie wskoczył na ekran

### ItemOffScreen

Wysyła event z ścieżka do node-a z crawlem (“\#0/\#0”) oraz nazwą node-a
który całkowicie opuścił ekran

### LowBuffer

W kolejce node-ów do wyświetlenia zostało mniej niż 3 długości pola view
(Suma szerokości nodeów do wyświetlenia). Ewentualnie mniej niż 5
node-ów jeżeli pierwsze założenie jest ciężkie w realizacji

### AllItemsOffScreen

Wszystkie node-y które miały być wyświetlone zostały już wyświetlone

Zmienne Publiczne
-----------------

### OffscreenNodeBehavior

Zmienna mówi o tym co się powinno stać z nodeami które opuściły już
ekran.

**Action:** SetOffscreenNodeBehavior, GetOffscreenNodeBehavior

**OffscreenNodeBehavior** - Looping, DeleteNode, SetNonActive

Jeśli zmienna DeleteOffScreenNodesIfNotLooping ustawiona jest na true
oraz Looping na false wtedy node-y te są usuwane z pamięci renderera.

Jeśli DeleteOffScreenNodesIfNotLooping == false oraz Looping ustawiona
jest na false node-y te przepisywane są do tablicy NonActive

Jeśli Looping == true node po opuszczeniu ekranu wrzucany jest na koniec
kolejki nodeów do ponownego wyświetlenia

### Spacing

Odstęp między kolejnymi elementami crawla.

**Action:** SetSpacing, GetSpacing

**Spacing** - odległość między kolejnymi node'ami

### view

tak jak w przykładzie dodawania logiki do nodea

### Speed

Ustawia prędkość przewijani node-a. Prędkość jest wartością dodatnią.
Jeżeli poda się niepoprawną wartość, zostanie ona ustawiona na 0.0f.

**Action:** SetSpeed, GetSpeed

**Speed** - wartość prędkości

### SmoothTime

Ustawia czas przyspieszania przy startowaniu i pauzowaniu przy pomocy
SmoothStart i SmoothPause.

**Action:** SetSmoothTime, GetSmoothTime

**SmoothTime** - czas po jakim zostanie osiągnięta prędkość Speed.

### Direction

Kierunek przewijania

**Action:** SetScrollDirection, GetScrollDirection

**ScrollDirection:** ScrollUp, ScrollDown, ScrollLeft, ScrollRight

### Status

Mówi o tym czy Crawl jest uruchomiony czy nie (RUNNING, PAUSED, STOPPED,
?)

**Action:** GetStatus

### EnableSendingEvents

Decyduje czy są wysyłane eventy.

**Action:** SetEnableEvents, GetEnableEvents

**EnableEvents:** true, false

Przypadki użycia
----------------

### Dodanie node'a

**Notice:** Silnik powinien móc dodać dowolnego node-a utworzonego w BV.

**Notice:** Edytor może dodać do crawla node-a z puli wcześniej
zapisanych [node\_presetów](node_preset "wikilink").

**Nazwa funkcji:** AddNode(String newNodeName, String presetPath); //
prawdopodobnie trzeba tu dodac jeszcze sceneName, nodeName, pluginName,
argumenty wymagane przez logikę etc

Powoduje to wczytanie przez silnik definicji node-a z podanej ścieżki
presetPath.

1.  Do sceny dodają się [timeline](timeline "wikilink")-y zdefiniowane w
    w/w pliku. Każdy timeline w presecie prefiksowany jest stringiem
    “preset\_”.

    :   **Do rozstrzygnięcia pozostaje czy gwarancja poprawnego
        prefiksowania powinna dziać się na etapie zapisywanie presetu
        czy na etapie jego wczytywania do silnika.**

2.  Moim zdaniem sensownym jest wymuszenie na użytkowniku zapisywanie
    takiego presetu z poprawnie nazwanymi timelinami (żeby się nie
    zdziwił że potem nazywają się nagle innaczej po wczytaniu do innej
    sceny).
3.  Jeżeli w scenie istnieją już timeline'y o tej samej nazwie co
    wczytywany timeline z prefiksu to wczytywany timeline jest
    ignorowany (zostaje poprzedni) Natomiast wypisujemy o tym informację
    na [Logu](Log "wikilink")
4.  Tworzony jest nowy node z transformacją o nazwie nodeName (argument
    funkcji)
5.  Do tego [node](node "wikilink")-a podpinane jest drzewo wczytane z
    presetu.
6.  Nowo dodany node nodeName dopinany jest do poddrzewa node-a w którym
    znajduje się plugin crawl.
7.  W Responsie zwracana jest kolejność nowo dodanego node-a w
    poddrzewie node-a z crawlem. (Znowu mi wychodzi, że każdy node
    powinien mieć UID tak naprawdę - trzeba będzie wrócić w przyszłości
    do tego tematu, kolejność jest zmienna w czasie, to będzie rodziło
    błędy i nieporządane zachowania na 100% )