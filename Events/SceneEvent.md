# SceneEvent

```
#!json

{
    "Event" : "SceneEvent",
    "EventID" : "0",
    "Command" : "AddScene",
    "SceneName" : "",
    "NewSceneName" : "",
    "AttachIndex" : "0"
}
```


# Komendy

* AddScene
* RemoveScene
* RemoveAllScenes
* SetSceneVisible
* SetSceneInvisible
* RenameScene
* AttachScene
* DetachScene
* MoveScene
* CopyScene
* SetOutputChannel

## SetOutputChannel

Ustawia renderowanie sceny do wybranego kanału. Numer kanału należy wpisać pole **AttachIndex**.

```
#!json

{
    "Event" : "SceneEvent",
    "EventID" : "0",
    "Command" : "SetOutputChannel",
    "SceneName" : "NazwaSceny",
    "AttachIndex" : "0"
}
```