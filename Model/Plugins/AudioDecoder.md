Plugin AudioDecoder
===================

**Name**: audio\_decoder

**UID**: DEFAULT\_AUDIO\_DECODER

Plugin obsługujący strumień audio przy pomocy biblioteki ffmpeg.

Eventy: [VideoDecoderEvent](VideoDecoderEvent "wikilink")

Parametry
=========

state (Int32) (default 0)
:   Parametr służy do sterowania pluginem - odtwarzania/zatrzymywania
    video.
:   0 - STOP
:   1 - PLAY
:   2 - PAUSE

offset (Vec2) (default \[0.f, 0.f\])
:   vec2.x - parametr służy do przewijania video do podanego czasu
    wyrażonego w sekundach.
:   vec2.y - sztuczny parametr, który zawsze powinien być przekazywany
    jako 0.f

loopEnabled (bool) (default false)
:   Zapętlenie video.

loopCount (Int32) (default 0)
:   Liczba pętli video, 0 oznacza nieskończoność.

Wczytywanie
===========

     {
         "Event" : "LoadAssetEvent",
         "EventID" : "1",
         "NodeName" : "video_node",
         "SceneName" : "sceneFromEnv: VIDEO_STREAM_TEST_SCENE",
         "PluginName" : "audio_decoder",
         "AssetData" : 
        {
            "asset" :
            {
                "path" : "ActivisionLogo.bik",
                "type" : "AV_ASSET_DESC"
            }
        }
     }

JSON API control
================

Play:

    {
        "Event" : "ParamKeyEvent",
        "EventID" : "0",
        "Target" : "PluginParam",
        "SceneName" : "sceneFromEnv: VIDEO_STREAM_TEST_SCENE",
        "NodeName" : "video_node",
            "PluginName" : "audio_decoder",
        "ParamName" : "state",
        "ParamValue" : "1",
        "Time" : "0.0",
        "Command" : "AddKey"
    }

Offset:

    {
        "Event" : "ParamKeyEvent",
        "EventID" : "0",
        "Target" : "PluginParam",
        "SceneName" : "sceneFromEnv: VIDEO_STREAM_TEST_SCENE",
        "NodeName" : "video_node",
            "PluginName" : "audio_decoder",
        "ParamName" : "offset",
        "ParamValue" : "0.1,0.0",
        "Time" : "0.0",
        "Command" : "AddKey"
    }