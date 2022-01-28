[TOC]

# Zmiany w eventach

## Zmiany w strukturze

### Eventy do assetów

- Scalić LoadAssetEvent i AssetEvent.
Docelowy event ma się nazywać AssetEvent.

Przemyśleć czy GlobalEffectEvent::LoadGlobalEffectAsset nie powinien trafić do tego scalonego eventu.

### Rozbicie InfoEvent

- ListSceneAssets -> AssetEvent
- EffectInfo -> GlobalEffectEvent
- PluginInfo -> PluginStructureEvent
- NodeInfo -> NodeStructureEvent
- LogicInfo -> NodeLogicEvent
- ParamInfo -> ParamKeyEvent
- GetParamDescriptor -> ParamDescriptorEvent
- ListParamDescriptors -> ParamDescriptorEvent
- CamerasInfo -> CameraEvent
- LightsInfo -> LightEvent
- MinimalSceneInfo -> SceneEvent
- TreeStructure -> SceneEvent
- MinimalTreeStructure -> SceneEvent
- ListProjectNames -> ProjectEvent
- ListProjects -> ProjectEvent
- ListAllFolders -> ProjectEvent
- Performance -> GenericEvent
- Timelines -> TimeLineEvent. **Trzeba zmienić nazwę Timelines -> ListTimelines**
- ListTimelineKeyframes -> TimelineKeyframeEvent
- ListTimelinesParams -> TimeLineEvent
- CheckTimelineTime -> TimeLineEvent
- Videocards -> VideoCardEvent

#### Stworzyć ProjectSceneEvent (?? Wymyśleć nazwę)

- ListScenes
- ListPresets
+ ProjectEvent::SaveScene
+ ProjectEvent::LoadScene
+ ProjectEvent::RemoveScene
+ ProjectEvent::CopyScene
+ ProjectEvent::MoveScene
+ ProjectEvent::SavePreset
+ ProjectEvent::LoadPreset
+ ProjectEvent::EditPreset


#### Stworzyć ProjectContentEvent (??Nazwa??)

- GetAssetDescriptor
- GetAssetThumbnail
- GetSceneThumbnail
- GetPresetThumbnail
- GenerateMeshThumbnail
- ListAssetsPaths
- ListCategoriesNames
- GetPMItemStats
+ ProjectEvent::CopyAsset
+ ProjectEvent::MoveAsset
+ ProjectEvent::RemoveAsset
+ ProjectEvent::ImportAsset


## Zmiany stylu nazewnictwa

- InfoEvent::Command::Performance (**fps_avg**). Poza tym warto się zastanowić czy dobrze, że sa nazwy GPU render time, CPU render time, CPU queueing time ze spacjami
- cmd w odpowiedziach -> Command

## Inne znaki zapytania

- ProjectEvent::Command::GenerateMeshThumbnail ma komentarz "//temp testing event". Trzeba zbadać gdzie jest ostatecznie ta funkcjonalność.
- Zmiana na CamelCase ?? To jest głęboka zmiana, bo dotyka również serializacji sceny.

## Zmiany nazwy

- Zmienić "pingPong" na "mirror" w WrapMethod (interpolatory) w celu ujednolicenia z timeline'ami