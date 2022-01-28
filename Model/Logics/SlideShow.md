Opis logiki
===========

Pokaz slajdów.

Parametry
---------

Tworzenie
---------

    {
        "Event" : "NodeLogicEvent",
        "Command" : "AddNodeLogic",
        "EventID" : "1",
        "NodeName" : "root",
        "SceneName" : "witek/SlideShowTest.scn",
        "Action" : 
        {
            "logic" : 
            {
                "type" : "SlideShow",
                "timelinePath" : "witek/SlideShowTest%default"
            }
        }
    }