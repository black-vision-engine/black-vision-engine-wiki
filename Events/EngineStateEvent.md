[TOC]

#EngineStateEvent

##Wyłączanie silnika


```
#!c++

{
	"Event" : "EngineStateEvent",
	"Command" : "CloseApplication"
}

```

## Screenshot/nagrywanie sekwencji


```
#!c++

{
	"Event" : "EngineStateEvent",
	"Command" : "ScreenShot",
	"FilePath" : "witek/screenshotTest"
}
```

## Blokowanie wykonania eventów


Aby zablokować wykonywanie eventów należy wysłać następujący event, podając liczbę klatek, przez które eventy nie będą pobierane z kolejki. Eventy nie są usuwane i zostaną wykonane po podanej liczbie klatek

```
#!c++

{
	"Event" : "EngineStateEvent",
	"Command" : "LockEventQueue",
	"NumberFrames" : "360"
}
```

Następnie można np. wysłać event do wczytywania assetu, który zostanie wczytany z żądanym opóźnieniem.

```
#!c++


{
    "Event" : "LoadAssetEvent",
    "EventID" : "1",
    "NodeName" : "DEFAULT_CONE",
    "SceneName" : "sceneFromEnv@ REMOTE_EVENTS_TEST_SCENE",
    "PluginName" : "texture",
    "AssetData" :
	{
		"asset" :
		{
			"filter" : "bilinear",
			"loading_type" : "GENERATE_MIPMAPS",
			"path" : "textures/poison.jpg",
			"type" : "TEXTURE_ASSET_DESC"
		}
	}
}
```

**Uwaga!** Ponieważ blokowanie eventów grozi brakiem możliwości sterowania silnikiem, jest ono domyślnie wyłączone.
Aby je włączyć trzeba ustawić w configu opcję **Application/EnableLockingQueue** na true.

## Przełączanie trybu edycji i produkcji

Event zmienia tryb pracy między edycją a produkcją. (To działa jak switch, nie podaje się jawnie trybu)

```
#!c++

{
	"Event" : "EngineStateEvent",
	"Command" : "SwitchEditMode"
}
```

## Ustawianie RenderChannelów dla scen

```
#!c++


{
	"Event" : "EngineStateEvent",
	"Command" : "OutputCommand",
	"Request" :
	{
		"Action" : "AssignChannel",
		"SceneName" : "witek/RenderChannels/output_2.scn",
		"ChannelIdx" : "0"
	}
}

```

## Listowanie RenderChannelów dla scen

### Wysłany event

```
#!c++


{
	"Event" : "EngineStateEvent",
	"Command" : "OutputCommand",
	"Request" :
	{
		"Action" : "ListChannelsMapping"
	}
}
```


### Przykładowa odpowiedź

```
#!c++


{
   "ChannelsMapping" : [
      {
         "ChannelIdx" : "0",
         "SceneName" : "witek/Environment/EnvCombinations.scn"
      },
      {
         "ChannelIdx" : "1",
         "SceneName" : "witek/RenderChannels/output_2.scn"
      }
   ],
   "EventID" : "2147483647",
   "Success" : "true",
   "cmd" : "OutputCommand"
}
```