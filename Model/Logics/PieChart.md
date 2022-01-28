Opis logiki
===========

Logika tworzy piechart przy pomocy pluginów cylinder i text.

Tworzenie
---------

     {
        "Event" : "NodeLogicEvent",
        "Command" : "AddNodeLogic",
        "EventID" : "1",
        "NodeName" : "root",
        "SceneName" : "new_scene.scn",
        "Action" : 
        {
            "logic" : 
            {
                    "type" : "pie_chart",
                    "timelinePath" : "new_scene.scn%default",
                    "pieChartType" : "material",
                    "textEnabled" : "true"
            }
        }
     }

pieChartType : color / material

Dodawanie nowej wartości
------------------------

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root",
        "SceneName" : "new_scene.scn",
        "Action" : 
        {
            "Action" : "AddPieSlice",
            "pieSlice":
            {
                    "percent" : "10",
                    "offset" : "0.1"
            }
        }
     }

Usuwanie wartości
-----------------

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root",
        "SceneName" : "new_scene.scn",
        "Action" : 
        {
            "Action" : "RemovePieSlice",
            "pieSliceIdx": "1"
        }
     }

Aktualizacja wartości
---------------------

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root",
        "SceneName" : "new_scene.scn",
        "Action" : 
        {
            "Action" : "UpdatePieSlice",
            "pieSliceIdx": "1",
            "pieSlice":
            {
                    "percent" : "20",
                    "offset" : "0.1"
            }
        }
     }

Aktualizacja całego wykresu
---------------------------

     {
        "Event" : "NodeLogicEvent",
        "Command" : "LogicAction",
        "EventID" : "1",
        "NodeName" : "root",
        "SceneName" : "new_scene.scn",
        "Action" : 
        {
            "Action" : "UpdatePieChart"
        }
     }