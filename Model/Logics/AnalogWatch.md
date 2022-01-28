Opis Logiki
===========

Logika ustawia swoje dzieci zgodnie z aktualnym czasem systemowym.
Pierwsze dziecko jest wskazówką godzinową, drugie minutową, a trzecie
sekundową.

Obsługa logiki
==============

Tworzenie
---------

     {
        "Event" : "NodeLogicEvent",
        "Command" : "AddNodeLogic",
        "EventID" : "1",
        "NodeName" : "root",
        "SceneName" : "witek/AnalogWatchTest.scn",
        "Action" : 
        {
            "logic" : 
            {
                "type" : "AnalogWatch",
                "timelinePath" : "witek/AnalogWatchTest%default"
            }
        }
     }

Uruchamianie
------------

Wysłanie eventu powoduje ustawienie pierwszych trzech nodów w pozycje
początkowe i rozpoczęcie animacji.

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root/AnalogWatch",
        "SceneName" : "witek/AnalogWatchTest.scn",
        "Action" : 
        {
            "Action" : "StartWatch"
        }
     }

Kasowanie ustawień
------------------

Zegar się zatrzymuje, a wskazówki wybrane jako dzieci zostają usunięte z
pamięci. Po wywołaniu tej komendy można poprzestawiać dzieci z
wystartować zegar ponownie.

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root/AnalogWatch",
        "SceneName" : "witek/AnalogWatchTest.scn",
        "Action" : 
        {
            "Action" : "ClearWatch"
        }
     }