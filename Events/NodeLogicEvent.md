NodeLogicEvent
==============

     {
        "Event" : "NodeLogicEvent",
        "Command" : "SetLogicParam",
        "SceneName" : "sceneFromEnv: STARWARS_TEST_SCENE",
        "NodeName" : "Dummy0",
        "Action" : ""
     }

**Command**

-   AddNodeLogic
-   DeleteNodeLogic
-   LogicAction

AddNodeLogic
------------

**Parametr Action**

Zawiera zagnieżdżonego JSONa z następującymi polami:

-   *timelinePath* - ścieżka do timeline'u
-   *logic* - zserializowana logika node'a

### Przykład Crawlera

     {
        "Event" : "NodeLogicEvent",
        "Command" : "AddNodeLogic",
        "EventID" : "1",
        "NodeName" : "Dummy0",
        "SceneName" : "sceneFromEnv: STARWARS_TEST_SCENE",
        "Action" :
        {
            "timelinePath" : "sceneFromEnv: STARWARS_TEST_SCENE",
            "logic" : 
            {
                "crawlerNodes" : 
                [
                    {
                        "name" : "Text",
                        "nodeIdx" : "0"
                    },
                    {
                        "name" : "Text",
                        "nodeIdx" : "1"
                    },
                    {
                        "name" : "Text",
                        "nodeIdx" : "3"
                    },
                    {
                        "name" : "Text",
                        "nodeIdx" : "2"
                    }
                ],
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
        }
     }

### Przykład Replikatora

     {
        "Event" : "NodeLogicEvent",
        "Command" : "AddNodeLogic",
        "EventID" : "1",
        "NodeName" : "DEFAULT_CONE",
        "SceneName" : "sceneFromEnv: REMOTE_EVENTS_TEST_SCENE",
        "Action" : "
        {
            "timelinePath" : "sceneFromEnv: REMOTE_EVENTS_TEST_SCENE",
            "logic" : 
            {
                "numRepetitions" : "2",
                "replicatorModifier" : 
                {
                    "paramShifts" : 
                    [
                        {
                            "paramDelta" : 
                            {
                                "delta" : 
                                {
                                    "type" : "vec3",
                                    "value" : "0.400000, 0.400000, 0.000000"
                                },
                                "deltaTime" : "0.000000",
                                "startTime" : "0.000000"
                            },
                            "paramName" : "translation",
                            "pluginName" : "transform"
                        }
                    ],
                    "type" : "shiftReplicationModifier"
                },
                "type" : "replicate"
            }
        }
     }
     

### Przykład Countera

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

DeleteNodeLogic
---------------

Usuwa logikę z podanego w parametrach node'a.

     {
        "Event" : "NodeLogicEvent",
        "Command" : "DeleteNodeLogic",
        "SceneName" : "sceneFromEnv: STARWARS_TEST_SCENE",
        "NodeName" : "Dummy0",
     }

LogicAction
-----------

Generyczny mechanizm obsługi logiki. Pole *Action* zawiera
zagnieżdżonego JSONa z wartościami potrzebnymi do obsługi tej logiki.

Zobacz:

-   [Logiki nodów](../Model/Logics/LogicsList)