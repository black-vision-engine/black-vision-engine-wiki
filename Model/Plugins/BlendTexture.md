#Opis

**Name**: blend texture

**Name abbreviation**: blend

**UID**: DEFAULT_BLEND_TEXTURE



This plugin should follow [TexturePlugin](TexturePlugin). BlendTexture plugin can load additional texture which can be blended using Photoshop blending modes. More about blending modes: [BlendingModes](BlendingModes).

##Parameters

Parameter name         	| Type      	| Default Value     | Description
----------------------- | -------------	| ----------------- | -----------
alpha                   | float         | 1.0f              | Alpha to attenuate loaded texture
blendMode               | enum BlendMode| Normal            | Mode used to blend two textures
txBlendMat              | transform     |                   | Asset transformation applied only to texture loaded by this plugin.

##Assets

- **BlendTex0**

    Texture to blend.