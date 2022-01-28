# VideoInputPlugin

## Opis

Plugin can load video and audio input from video card.

**Name**: video input

**Name abbreviation**: vidin

**UID**: VIDEO_INPUT_PLUGIN

## Parameters

Parameter name         	| Type      	| Default Value     | Description
----------------------- | -------------	| ----------------- | -----------
alpha                   | float         | 1.0f              | Alpha to attenuate loaded texture.
txMat                   | transform     |                   | Asset transformation applied only to texture loaded by this plugin.
gain                    | float         | 1.0f              | Audio from video input gain.
enableKey               | bool          | false             | Enable/Disable video input key texture.

## Assets

- **VideoInput0**

    VideoInputAsset to display.

#### Asset Loading

Set **VideoFillIdx** and **VideoKeyIdx** field to input index configured as **linkedVideoInput** in config file.

```
#!c++

{
    "Event" : "LoadAssetEvent",
    "EventID" : "1",
    "NodeName" : "root/videoInput",
    "SceneName" : "witek/Mechanisms/VideoInput.scn",
    "PluginName" : "video input",
    "AssetData" :
	{
		"asset" :
		{
			"type" : "VIDEO_INPUT_ASSET_DESC",
            "VideoFillIdx" : "0",
            "VideoKeyIdx" : "0"
		}
	}
}
```