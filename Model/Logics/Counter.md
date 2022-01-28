Opis logiki
===========

Obsługa za pomocą jsona
-----------------------

Do obsługi logik służy [NodeLogicEvent](NodeLogicEvent "wikilink").

### Dodawanie countera

Najprostsza wersja wymaga podania jedynie ścieżki timelinu, do którego
zostanie podłączony parametr sterujący odliczaniem. Do parametru
zostanie domyślnie dodany klucz o wartości 0 w czasie 0.0 sekund.

     {
        "Event" : "NodeLogicEvent",
        "Command" : "AddNodeLogic",
        "EventID" : "1",
        "NodeName" : "Dummy0/Root/Text",
        "SceneName" : "sceneFromEnv: STARWARS_TEST_SCENE",
        "Action" : 
        {
            "logic" : 
            {
                "timelinePath" : "sceneFromEnv: STARWARS_TEST_SCENE%default",
                "type" : "counter"
            }
        }
     }

Można również stworzyć counter od razu z dodanymi parametrami. W takim
przypadku JSON wygląda tak:

     
     {
        "Event" : "NodeLogicEvent",
        "Command" : "AddNodeLogic",
        "EventID" : "1",
        "NodeName" : "Dummy0/Root/Text",
        "SceneName" : "sceneFromEnv: STARWARS_TEST_SCENE",
        "Action" :
        {
            "logic" : 
            {
                "param" : 
                {
                    "interpolator" : 
                    {
                        "curve_type" : "linear",
                        "interpolations" : 
                        [
                            {
                                "type" : "linear"
                            }
                        ],
                        "keys" : 
                        [
                            {
                                "time" : "0.000000",
                                "val" : "0.000000"
                            },
                            {
                                "time" : "8.000000",
                                "val" : "20.000000"
                            }
                        ],
                        "postMethod" : "clamp",
                        "preMethod" : "clamp"
                    },
                    "name" : "alpha",
                    "timeline" : "sceneFromEnv: STARWARS_TEST_SCENE%default",
                    "type" : "float"
                },
                "timelinePath" : "sceneFromEnv: STARWARS_TEST_SCENE%default",
                "type" : "counter"
            }
        }
     }
     

### Obsługa countera

Do obsługi logiki służy komenda (command) LogicAction. Counter pozwala
na dodawanie i usuwanie kluczy.

Dodawanie można zrealizować poprzez wysłanie eventu:

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "Dummy0/Root/Text",
        "SceneName" : "sceneFromEnv: STARWARS_TEST_SCENE",
        "Action" :
        {
            "Action" : "SetParam",
            "Time" : "20.0",
            "Value" : "8.0"
        }
     }
     

Usuwanie można zrealizować poprzez wysłanie eventu:

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "Dummy0/Root/Text",
        "SceneName" : "sceneFromEnv: STARWARS_TEST_SCENE",
        "Action" :
        {
            "Action" : "RemoveParam",
            "Time" : "20.0"
        }
     }