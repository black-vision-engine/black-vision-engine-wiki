LineChart
=========

Name: line chart

UID: DEFAULT\_LINE\_CHART

Plugin rysuje wykres na podstawie wczytanego assetu z danymi.

Parametry
=========

Wczytywanie assetu
==================

Plugin wykorzystuje DataArrayAsset do rysowania wykresów. Wszystkie dane
dla tego assetu wysyła się w jego deskryptorze. Pole data zawiera
wektory vec2, których składowe są rozdzielane przecinkami, a wektory
średnikami.

Pole name dla pluginu LineChart powinno przyjmować wartość **Plot**.

    {
        "Event" : "LoadAssetEvent",
        "EventID" : "1",
        "NodeName" : "rootNode",
        "SceneName" : "sceneFromEnv@ LINE_CHART_TEST_SCENE",
        "PluginName" : "line chart",
        "AssetData" :
        {
            "asset" :
            {
                "type" : "DATA_ARRAY_ASSET_DESC",
                "dataRows" :
                [
                    {
                        "name" : "Plot",
                        "type" : "vec2",
                        "data" : "-3.0, 0.3; -0.4, 1.0; 0.0, 0.5; 0.3, -0.1; 0.6, -0.4; 0.7, 0.4;"
                    }
                ]
            }
        }
    }