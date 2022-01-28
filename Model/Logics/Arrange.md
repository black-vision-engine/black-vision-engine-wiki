Opis Logiki
===========

Ustawia dzieci node'a, do którego jest przypięta logika w wybranym
porządku.

Dostępne komendy (**Action**):

-   **LineArrange**
-   **CircleArrange**
-   **SphereArrange**
-   **Grid2DArrange**
-   **Grid3DArrange**

Obsługa logiki
==============

Tworzenie
---------

    {
        "Event" : "NodeLogicEvent",
        "Command" : "AddNodeLogic",
        "EventID" : "1",
        "NodeName" : "root/ArrangeCircle",
        "SceneName" : "witek/ArrangeTest.scn",
        "Action" : 
        {
            "logic" : 
            {
                "type" : "Arrange",
                "timelinePath" : "witek/ArrangeTest%default"
            }
        }
    }

Parametry
---------

Każda metoda układania posiada swój własny zestaw parametrów.

-   **Line\_StartPoint**
-   **Line\_EndPoint**

<!-- -->

-   **Circle\_Radius**
-   **Circle\_Rotation**
-   **Circle\_Center**

<!-- -->

-   **Sphere\_Radius**
-   **Sphere\_Rotation**
-   **Sphere\_Center**
-   **Sphere\_Rows**
-   **Sphere\_Columns**

<!-- -->

-   **Grid2D\_Rotation**
-   **Grid2D\_Center**
-   **Grid2D\_Rows**
-   **Grid2D\_Columns**
-   **Grid2D\_Interspaces**

<!-- -->

-   **Grid3D\_Rotation**
-   **Grid3D\_Center**
-   **Grid3D\_Rows**
-   **Grid3D\_Columns**
-   **Grid3D\_Layers**
-   **Grid3D\_Interspaces**

Po wczytaniu sceny z xmla logika automatycznie ustawia swoje dzieci.
Można to wyłączyć zmieniając parametr:

-   **ArrangeAfterLoad**

Ustawianie w okrąg
------------------

    {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root/ArrangeCircle",
        "SceneName" : "witek/ArrangeTest.scn",
        "Action" : 
        {
            "Action" : "CircleArrange"
        }
    }

Ustawianie na jednej linii
--------------------------

    {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root/LineArrange",
        "SceneName" : "witek/ArrangeTest.scn",
        "Action" : 
        {
            "Action" : "LineArrange"
        }
    }

Układanie siatki 2D
-------------------

    {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root/Grid2DArrange",
        "SceneName" : "witek/ArrangeTest.scn",
        "Action" : 
        {
            "Action" : "Grid2DArrange"
        }
    }

Układanie siatki 3D
-------------------

    {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root/Grid3DArrange",
        "SceneName" : "witek/ArrangeTest.scn",
        "Action" : 
        {
            "Action" : "Grid3DArrange"
        }
    }

Ustawianie na sferze
--------------------

    {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root/SphereArrange",
        "SceneName" : "witek/ArrangeTest.scn",
        "Action" : 
        {
            "Action" : "SphereArrange"
        }
    }

Ustawianie nodów po ząładowaniu sceny
-------------------------------------

Domyślnie po wczytaniu logiki wszystkie nody dzieci są ustawiane według
wartości zapisanych w XMLu. To domyślne zachowanie można wyłączyć
ustawiając logice parametr ArrangeAfterLoad na false.