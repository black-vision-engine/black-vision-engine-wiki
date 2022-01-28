Opis efektu
===========

Globalny efekt shadow/glow/outline (innner and outer) pełnoekranowy.

Parametry
=========

blurSize (Float32) (default 0.0)
:   Wielkość blura w pixelach. Implemetancja wspiera blur subpixelowy.

normalize (Int32) 0 lub 1 (default 1)
:   Decyduje, czy normalizujemy po zblurowaniu. Jeśli nie znormalizujemy
    dostaniemy efekt outline

blurKernelType (Int32) (default 0) TODO: oddać do implementacji. Na razie nie wspierany. Zawsze blurowanie boxem
:   0 - box
:   1 - triangle
:   2 - gaussian
:   TODO:

color (vec4) (default (0.0, 0.0, 0.0, 0.0))
:   określa kolor efektu

inner (Int32) (default 0)
:   0 - outer
:   1 - inner
:   2 - inner and outer

shift (vec2) ( default (0.0, 0.0) )
:   określa stopień przesunięcia efektu względem tekstury wejściowej we
    współrzędnych ekranowych.

JSON API control
================

Examples:

    {
        "Event" : "ParamKeyEvent",
        "EventID" : "0",
        "Target" : "GlobalEffectParam",
        "SceneName" : "sceneFromEnv: SHADOW_EFFECT",
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
        "SceneName" : "sceneFromEnv: SHADOW_EFFECT",
        "NodeName" : "root",
        "ParamName" : "shift",
        "ParamValue" : "0.1 , 0.1",
        "Time" : "0.0",
        "Command" : "AddKey"
    }