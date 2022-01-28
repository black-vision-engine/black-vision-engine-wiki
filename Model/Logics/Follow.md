Opis logiki
===========

Logika pozwala na podążanie za wskazanym nodem.

Parametry
---------

**OffsetX** **OffsetY** **OffsetZ**

Offset względem obserwowanego noda.

**AlignX** { Left = 0, Center = 1, Right = 2 }

**AlignY** { Top = 2, Center = 1, Bottom = 0 }

**AlignZ** { Back = 0, Center = 1, Front = 2 }

Wyrównanie do krawędzi bounding boxa obserwowanego noda.

**FollowerAlignX** { Left = 0, Center = 1, Right = 2 }

**FollowerAlignY** { Top = 2, Center = 1, Bottom = 0 }

**FollowerAlignZ** { Back = 0, Center = 1, Front = 2 }

Wyrównanie do krawędzi BB obserwujacego node'a.

**FollowingMode** { Previous = 0, FirstInSubtree = 1, Path = 2 }

**FollowingNodePath**

Używane w trybie FollowingMode::Path. Logika szuka noda na podstawie
podanej ścieżki.

**FollowX** **FollowY** **FollowZ**

Flaga boolowska decydujaca czy śledzić node'a po danej osi.

**TargetBoxRecursive** **FollowerBoxRecursive**

Flaga boolowska decyduje czy używać BB node'a czy rekurencyjnie liczonego BB łacznie z dziećmi.

Tworzenie
---------

    {
        "Event" : "NodeLogicEvent",
        "Command" : "AddNodeLogic",
        "EventID" : "1",
        "NodeName" : "root/Follower",
        "SceneName" : "witek/FollowTest.scn",
        "Action" : 
        {
            "logic" : 
            {
                "type" : "Follow",
                "timelinePath" : "witek/FollowTest%default"
            }
        }
    }