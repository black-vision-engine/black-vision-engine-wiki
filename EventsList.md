TODO: dokończyć, przepisać eventy z poprzedniego wiki

#O eventach

Jeżeli gdziekolwiek istnieją wątpliwości, jakie stringi atrybutów można
wpisać w danym evencie, Można zajrzeć do pliku Events.cpp, który na
górze w namespace SerializationHelper zawiera definicje tych stałych.

#Lista eventów


[AssetEvent](Events/AssetEvent.md)
-----------------------------------

[CameraEvent](Events/CameraEvent.md)
-------------------------------------

[CheckTimelineTime](Events/CheckTimelineTime.md)
-------------------------------------------------

[ConfigEvent](Events/ConfigEvent.md)
-------------------------------------

[EngineStateEvent](Events/EngineStateEvent.md)
-----------------------------------------------

[GenericEvent](Events/GenericEvent.md)
-----------------------------------------------

[GizmoEvent](Events/GizmoEvent.md)
-------------------------------------------------

[GlobalEffectEvent](Events/GlobalEffectEvent.md)
-------------------------------------------------

[HightmapEvent](Events/HightmapEvent.md)
-----------------------------------------

[InfoEvent](Events/InfoEvent.md)
---------------------------------

[LightEvent](Events/LightEvent.md)
-----------------------------------

[LoadAssetEvent](Events/LoadAssetEvent.md)
-------------------------------------------

[MouseEvent](Events/MouseEvent.md)
-----------------------------------

[NodeLogicEvent](Events/NodeLogicEvent.md)
-------------------------------------------

[NodeStructureEvent](Events/NodeStructureEvent.md)
---------------------------------------------------

[ParamDescriptorEvent](Events/ParamDescriptorEvent.md)
-------------------------------------------------------

[ParamKeyEvent](Events/ParamKeyEvent.md)
-----------------------------------------

[PluginStructureEvent](Events/PluginStructureEvent.md)
-------------------------------------------------------

[ProjectEvent](Events/ProjectEvent.md)
---------------------------------------

[SceneEvent](Events/SceneEvent.md)
-----------------------------------

[SceneVariableEvent](Events/SceneVariableEvent.md)
---------------------------------------------------

[TimerEvent](Events/TimerEvent.md)
-----------------------------------

[TimeLineEvent](Events/TimeLineEvent.md)
-----------------------------------------

[TimelineKeyframeEvent](Events/TimelineKeyframeEvent.md)
---------------------------------------------------------

[UndoRedoEvent](Events/UndoRedoEvent.md)
---------------------------------------------------------

[VideoCardEvent](Events/VideoCardEvent.md)
-------------------------------------------

[VideoDecoderEvent](Events/VideoDecoderEvent.md)
-------------------------------------------------



Odpowiedzi od eventów
=====================

Każda komenda wysyła wiadomość czy została poprawnie wykonana. Format
tych wiadomości opiera się na następującym szablonie:

       
    {
       "EventID" : "0",
       "Success" : "true",
       "cmd" : "MinimalSceneInfo",
       "ErrorInfo" : “”
    }

Pole cmd zawiera string oznaczający komendę, która została wywołana.
EventID jest identyfikatorem komendy, który został wysłany przez edytor.

Jeżeli komenda została wykonana poprawnie, pole Success zawiera wartość
true. W przeciwnym razie zawiera wartość false, a dla niektórych eventów
dodatkowo może zostać dodane pole ErrorInfo, zawierające string z
bardziej szczegółową informacją o błędzie.

Oprócz tych pól, poprawnie wykonana komenda zawiera również pola, które
są zdefiniowane w dokumentacji poszczególnych eventów.

Uwaga!! Jeżeli wyśle się event o niepoprawnej nazwie (pole “Event”), to
wtedy nie dojdzie w ogóle do przetwarzania eventu w związku z czym nie
będzie też odpowiedzi. Zostanie za to wysłana wiadomość z logera o
niepoprawnym evencie.