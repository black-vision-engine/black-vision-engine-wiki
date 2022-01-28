Opis logiki
===========

Logika pozwala na powielanie jednego node'a z jednoczesną zmianą
parametrów.

Tworzenie
---------

    {
        "Event" : "NodeLogicEvent",
        "Command" : "AddNodeLogic",
        "EventID" : "1",
        "NodeName" : "root",
        "SceneName" : "ReplicatorTest.scn",
        "Action" :
        {
            "logic" : 
            {
                "type" : "replicator",
                "numRepetitions" : "10",
                "replicatorModifier" :
                {
                    "type" : "shiftReplicationModifier",
                    "paramShifts" :
                    [
                        {
                            "pluginName" : "transform",
                            "paramName" : "translation",
                            "paramDelta" : 
                            {
                                "startTime" : "0",
                                "deltaTime" : "0.23",
                                "delta" :
                                {
                                    "type" : "vec3",
                                    "value" : "-0.9,0.0,-0.5"
                                }
                            }
                        },
                        {
                            "pluginName" : "transform",
                            "paramName" : "translation",
                            "paramDelta" : 
                            {
                                "startTime" : "1",
                                "deltaTime" : "0.23",
                                "delta" :
                                {
                                    "type" : "vec3",
                                    "value" : "-0.9,0.0,-0.5"
                                }
                            }
                        },
                        {
                            "pluginName" : "transform",
                            "paramName" : "translation",
                            "paramDelta" : 
                            {
                                "startTime" : "2",
                                "deltaTime" : "0.23",
                                "delta" :
                                {
                                    "type" : "vec3",
                                    "value" : "-0.9,0.0,-0.5"
                                }
                            }
                        },
                        {
                            "pluginName" : "transform",
                            "paramName" : "translation",
                            "paramDelta" : 
                            {
                                "startTime" : "3",
                                "deltaTime" : "0.23",
                                "delta" :
                                {
                                    "type" : "vec3",
                                    "value" : "-0.9,0.0,-0.5"
                                }
                            }
                        },
                        {
                            "pluginName" : "transform",
                            "paramName" : "translation",
                            "paramDelta" : 
                            {
                                "startTime" : "4",
                                "deltaTime" : "0.23",
                                "delta" :
                                {
                                    "type" : "vec3",
                                    "value" : "-0.9,0.0,-0.5"
                                }
                            }
                        },
                        {
                            "pluginName" : "circle",
                            "paramName" : "outer radius",
                            "paramDelta" : 
                            {
                                "startTime" : "1",
                                "deltaTime" : "0.3",
                                "delta" :
                                {
                                    "type" : "float",
                                    "value" : "0.0"
                                }
                            }
                        },
                        {
                            "pluginName" : "circle",
                            "paramName" : "outer radius",
                            "paramDelta" : 
                            {
                                "startTime" : "1.1",
                                "deltaTime" : "0.3",
                                "delta" :
                                {
                                    "type" : "float",
                                    "value" : "0.0"
                                }
                            }
                        },
                        {
                            "pluginName" : "circle",
                            "paramName" : "outer radius",
                            "paramDelta" : 
                            {
                                "startTime" : "1.2",
                                "deltaTime" : "0.3",
                                "delta" :
                                {
                                    "type" : "float",
                                    "value" : "0.0"
                                }
                            }
                        },
                        {
                            "pluginName" : "circle",
                            "paramName" : "outer radius",
                            "paramDelta" : 
                            {
                                "startTime" : "2.7",
                                "deltaTime" : "0.3",
                                "delta" :
                                {
                                    "type" : "float",
                                    "value" : "0.0"
                                }
                            }
                        },
                        {
                            "pluginName" : "circle",
                            "paramName" : "outer radius",
                            "paramDelta" : 
                            {
                                "startTime" : "2.8",
                                "deltaTime" : "0.3",
                                "delta" :
                                {
                                    "type" : "float",
                                    "value" : "0.0"
                                }
                            }
                        },
                        {
                            "pluginName" : "circle",
                            "paramName" : "outer radius",
                            "paramDelta" : 
                            {
                                "startTime" : "2.9",
                                "deltaTime" : "0.3",
                                "delta" :
                                {
                                    "type" : "float",
                                    "value" : "0.0"
                                }
                            }
                        },
                        {
                            "pluginName" : "circle",
                            "paramName" : "outer radius",
                            "paramDelta" : 
                            {
                                "startTime" : "4.3",
                                "deltaTime" : "0.3",
                                "delta" :
                                {
                                    "type" : "float",
                                    "value" : "0.0"
                                }
                            }
                        },
                        {
                            "pluginName" : "circle",
                            "paramName" : "outer radius",
                            "paramDelta" : 
                            {
                                "startTime" : "4.4",
                                "deltaTime" : "0.3",
                                "delta" :
                                {
                                    "type" : "float",
                                    "value" : "0.0"
                                }
                            }
                        },
                        {
                            "pluginName" : "circle",
                            "paramName" : "outer radius",
                            "paramDelta" : 
                            {
                                "startTime" : "4.5",
                                "deltaTime" : "0.3",
                                "delta" :
                                {
                                    "type" : "float",
                                    "value" : "0.0"
                                }
                            }
                        }
                    ]
                }
            }
        }
    }

Powielanie node'ów
------------------

Aby powielić node'y należy wysłać komendę Replicate, w wyniku czego
zostanie powielony pierwszy node jaki zostanie znaleziony na liście
poddzieci.

`{`\
`   `“`Event`”` : `“`NodeLogicEvent`”`,`\
`   `“`Command`”` : `“`LogicAction`”`,`\
`   `“`SceneName`”` : `“`ReplicatorTest.scn`”`,`\
`   `“`NodeName`”` : `“`root`”`,`\
`   `“`Action`”` :`\
`   {`\
`       `“`Action`”` : `“`Replicate`”\
`   }`\
`}`

Interakcja z replicatorem
-------------------------

Replicator obsługuje następujące komedy:

### GetModifier

Zwraca aktualny modyfikator, który zostanie użyty do powielania node'ów.

**Komenda:**

         {
            "Event" : "NodeLogicEvent",
            "Command" : "LogicAction",
            "SceneName" : "ReplicatorTest.scn",
            "NodeName" : "root",
            "Action" :
            {
                "Action" : "GetModifier"
            }
         }

**Zwracany JSON:**

         {
            "EventID" : "7456",
            "Success" : "true",
            "cmd" : "LogicAction",
            "replicatorModifier" :
            {
               "type" : "shiftReplicationModifier",
               "paramShifts" :
               [
                  {
                     "paramName" : "translation",
                     "pluginName" : "transform",
                     "paramDelta" :
                        {
                        "delta" :
                        {
                           "type" : "vec3",
                           "value" : "-0.900000, 0.000000, -0.500000"
                        },
                        "deltaTime" : "0.000000",
                        "startTime" : "0.000000"
                     }
                  }
               ]
            }
         }

### GetNumRepetitions

**Komenda:**

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "SceneName" : "ReplicatorTest.scn",
        "NodeName" : "root",
        "Action" :
        {
            "Action" : "GetNumRepetitions" 
        }
     }
     

**Zwracany JSON:**

    {
        "EventID" : "7456",
        "Success" : "true",
        "cmd" : "LogicAction",
        "numRepetitions" : "10"
    }

### SetNumRepetitions

**Komenda:**

    {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "SceneName" : "ReplicatorTest.scn",
        "NodeName" : "root",
        "Action" :
        {
            "Action" : "SetNumRepetitions",
            "numRepetitions" : "3"
        }
    }

### AddParamShift

    {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root",
        "SceneName" : "ReplicatorTest.scn",
        "Action" :
        {
            "Action" : "AddParamShift",
            "paramShifts" :
            [
                {
                    "pluginName" : "transform",
                    "paramName" : "translation",
                    "paramDelta" : 
                    {
                        "startTime" : "0",
                        "deltaTime" : "0",
                        "delta" :
                        {
                            "type" : "vec3",
                            "value" : "-0.9,0.0,-0.5"
                        }
                    }
                }
            ]
        }
    }

### ClearShifts

    {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root",
        "SceneName" : "ReplicatorTest.scn",
        "Action" :
        {
            "Action" : "ClearShifts"
        }
    }

### RemoveParamShift

    {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root",
        "SceneName" : "ReplicatorTest.scn",
        "Action" :
        {
            "Action" : "RemoveParamShift",
            "pluginName" : "transform",
            "paramName" : "translation",
            "startTime" : "0.0"
        }
    }