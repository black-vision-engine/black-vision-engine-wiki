Opis efektu
===========

Globalny efekt image mask pełnoekranowy.

Parametry
=========

blend (Float32) (default 1.0)
:   Parametr przez, który przemnażana jest alpha maski

position (vec2) (default (0.0, 0.0))
:   Przesunięcia maski na ekranie

scale (vec2) (default (1.0, 1.0))
:   Skalowanie maski na ekranie

invert(Int32) (default 0)
:   Odwrócenie wartości alpha maski.

fit (Int32) (default 0)
:   dopasowanie do ekranu gdy 0 albo do obiektu jeśli 1. (Jeszcze nie
    działa)

wrap
:   Określa sposób wrapowania przy próbkowaniu tekstury maski. (Jeszcze
    nie działa)

alphaOnly (Int32) (default 0)
:   Pokaż tylko kanał alpha.

softMask (Int32) (default 0) (Jeszcze nie działa)
:   ?????

JSON API control
================

Examples:

    {
        "Event" : "ParamKeyEvent",
        "EventID" : "0",
        "Target" : "GlobalEffectParam",
        "SceneName" : "sceneFromEnv@ IMAGE_MASK_EFFECT",
        "NodeName" : "root",
        "ParamName" : "blend",
        "ParamValue" : "0.5",
        "Time" : "0.0",
        "Command" : "AddKey"
    }
    ,
    {
        "Event" : "ParamKeyEvent",
        "EventID" : "0",
        "Target" : "GlobalEffectParam",
        "SceneName" : "sceneFromEnv@ IMAGE_MASK_EFFECT",
        "NodeName" : "root",
        "ParamName" : "position",
        "ParamValue" : "0.1 , 0.1",
        "Time" : "0.0",
        "Command" : "AddKey"
    }
    ,
    {
        "Event" : "GlobalEffectEvent",
        "SceneName" : "sceneFromEnv@ IMAGE_MASK_EFFECT",
        "NodeName" : "Root",
        "GlobalEffectName" : "image mask",
        "Command" : "LoadGlobalEffectAsset",
            "Request" : 
        {
            "assetIdx" : "0"
        },
            "AssetData" : 
        {
            "asset" :
            {
                "filter" : "bilinear",
                "loading_type" : "ORIGINAL_ONLY",
                "path" : "textures/butterfly1.png",
                "type" : "TEXTURE_ASSET_DESC"
            }
        }
    }