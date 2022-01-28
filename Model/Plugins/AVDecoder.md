#Plugin AVDecoder

**Name**: video_decoder

**UID**: DEFAULT_VIDEO_DECODER


Plugin obsługujący strumienie audio i video przy pomocy biblioteki ffmpeg.

Eventy: [VideoDecoderEvent](VideoDecoderEvent)

#Parametry

; alpha (Float32)  (default 1.0)
: Alpha tekstury.
; txMat (Transform)
: Transformacja tekstury.
; state (Int32) (default 0)
: Parametr służy do sterowania pluginem - odtwarzania/zatrzymywania video.
: 0 - STOP
: 1 - PLAY
: 2 - PAUSE
; offset (Vec2) (default [0.f, 0.f])
: vec2.x - parametr służy do przewijania video do podanego czasu wyrażonego w sekundach.
: vec2.y - sztuczny parametr, który zawsze powinien być przekazywany jako 0.f
; loopEnabled (bool) (default false)
: Zapętlenie video.
; loopCount (Int32) (default 0)
: Liczba pętli video, 0 oznacza nieskończoność.

#Assety

; Tex0


# Wczytywanie


```
#!json

 {
     "Event" : "LoadAssetEvent",
     "EventID" : "1",
     "NodeName" : "video_node",
     "SceneName" : "sceneFromEnv: VIDEO_STREAM_TEST_SCENE",
     "PluginName" : "video_decoder",
     "AssetData" : 
 	{
 		"asset" :
 		{
 			"path" : "ActivisionLogo.bik",
 			"format" : "3",
 			"type" : "AV_ASSET_DESC"
 		}
 	}
 }
```


#JSON API control

Play:


```
#!json

{
	"Event" : "ParamKeyEvent",
	"EventID" : "0",
	"Target" : "PluginParam",
	"SceneName" : "sceneFromEnv: VIDEO_STREAM_TEST_SCENE",
	"NodeName" : "video_node",
	"PluginName" : "video_decoder",
	"ParamName" : "state",
	"ParamValue" : "1",
	"Time" : "0.0",
	"Command" : "AddKey"
}
```


Offset:

```
#!json
{
	"Event" : "ParamKeyEvent",
	"EventID" : "0",
	"Target" : "PluginParam",
	"SceneName" : "sceneFromEnv: VIDEO_STREAM_TEST_SCENE",
	"NodeName" : "video_node",
	"PluginName" : "video_decoder",
	"ParamName" : "offset",
	"ParamValue" : "0.1,0.0",
	"Time" : "0.0",
	"Command" : "AddKey"
}
```