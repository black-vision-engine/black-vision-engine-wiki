Opis efektu
===========

Globalny efekt blur pełnoekranowy.

Parametry
=========

blurSize (Float32) (default 0.0)
:   Wielkość blura w pixelach. Implemetancja wspiera blur subpixelowy.

normalize (Int32) 0 lub 1 (default 1)
:   Decyduje, czy normalizujemy po zblurowaniu.

blurKernelType (Int32) (default 0)
:   0 - box
:   1 - triangle
:   2 - gaussian
:   3 - TODO

JSON API control
================

Examples:

    {
        "Event" : "ParamKeyEvent",
        "EventID" : "0",
        "Target" : "GlobalEffectParam",
        "SceneName" : "sceneFromEnv: BLUR_EFFECT",
        "NodeName" : "root",
        "ParamName" : "blurSize",
        "ParamValue" : "12",
        "Time" : "0.0",
        "Command" : "AddKey"
    }
    ,
    {
        "Event" : "ParamKeyEvent",
        "EventID" : "0",
        "Target" : "GlobalEffectParam",
        "SceneName" : "sceneFromEnv: BLUR_EFFECT",
        "NodeName" : "root",
        "ParamName" : "blurKernelType",
        "ParamValue" : "2",
        "Time" : "0.0",
        "Command" : "AddKey"
    }