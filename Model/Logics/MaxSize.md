Opis logiki
===========

Skaluje noda, jeżeli rozmiar bounding boxa przekracza ustawione
wartości.

Parametry
---------

-   MaxWidth
-   MaxHeight
-   MaxDepth
-   IsProportional

Tworzenie
---------

    {
        "Event" : "NodeLogicEvent",
        "Command" : "AddNodeLogic",
        "EventID" : "1",
        "NodeName" : "root/TextNode",
        "SceneName" : "witek/Logic/MaxSizeTest.scn",
        "Action" : 
        {
            "logic" : 
            {
                "type" : "MaxSize",
                "timelinePath" : "witek/Logic/MaxSizeTest%default"
            }
        }
    }