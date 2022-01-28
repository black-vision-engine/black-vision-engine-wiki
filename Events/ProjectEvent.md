#ProjectEvent

Command:

- SavePreset

Save preset from "SceneName" and "NodePath" to "ProjectName" and "Path".


```
#!json


{                                           
	"Event" : "ProjectEvent",
	"Command" : "LoadPreset",
	"Request" : 
	{
	    "ProjectName" : "",
	    "Path" : "pp\\test_preset.bvpreset",
            "SceneName" : "sceneFromEnv: SHADOW_EFFECT",
            "NodePath" : "root/Text"
	}
}

```



- LoadPreset

Load preset from "ProjectName" and "Path" into "SceneName" and "NodePath".


```
#!json


{                                           
	"Event" : "ProjectEvent",
	"Command" : "LoadPreset",
	"Request" : 
	{
	    "ProjectName" : "",
	    "Path" : "pp\\test_preset.bvpreset",
            "SceneName" : "sceneFromEnv: SHADOW_EFFECT",
            "NodePath" : "root/Text"
	}
}
```

- SaveScene

Event saves scene. **saveTo** contains new path and name of scene.
If path already exists, scene want be save and event will return false. If you want to overwrite scene
set **forceSave** to true.

Event will rename existing scene. If you don't want this behavior set **renameScene** flag to false.


```
#!json

{
	"Event" : "ProjectEvent",
	"EventID" : "2",
	"Request" :
	{
		"sceneName" : "MyNewAddedScene",
		"saveTo" : "NewScene.scn",
		"forceSave" : "true",
                "renameScene" : "false"
	},
	"Command" : "SaveScene"
}
```