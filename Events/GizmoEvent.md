#GizmoEvent

Event do zarządzania gizmami.

Aby gizma były widoczne konieczne jest przełączenie silnika w tryb edycji (Zobacz [EngineStateEvent](EngineStateEvent) ).
Dokładniejszy opis mechanizmu gizm znajduje się [tutaj](../Mechanisms/Gizmos/Gizmo).

W przypadku gizm typu **Scene** pole *NodeName* należy zostawić puste.
Pole *ElementName* powinno zawierać nazwę typu pluginu/logiki/efektu, dla którego tworzone jest gizmo.

## Dodawanie gizma

```
#!c++


{	
	"Event" : "GizmoEvent",
	"EventID" : "1",
	"SceneName" : "witek/Environment/EnvCombinations.scn",
	"NodeName" : "",
	"ElementName" : "",
	"FunctionalityName" : "FPS",
	"GizmoType" : "Scene",
	"Command" : "CreateGizmo"
}
```

```
#!c++


{
	"Event" : "GizmoEvent",
	"EventID" : "2",
	"SceneName" : "witek/Environment/EnvCombinations.scn",
	"NodeName" : "rootNode/sphereEnv",
	"ElementName" : "",
	"FunctionalityName" : "BoundingBox",
	"GizmoType" : "Node",
	"Command" : "CreateGizmo"
}

```


# Usuwanie gizma

```
#!c++


{
	"Event" : "GizmoEvent",
	"EventID" : "2",
	"SceneName" : "witek/Environment/EnvCombinations.scn",
	"NodeName" : "rootNode/sphereEnv",
	"FunctionalityName" : "BoundingBox",
	"Command" : "RemoveGizmo"
}
```