#Blending Modes

W silniku istnieją dwa mechanizmy blendowania warstw:

1. Po pierwsze silnik blenduje wyrenderowane obiekty z zawartością rendertargetu. Tryb blendowania można w takim przypadku ustawiać w większości istniejących pluginów shaderowych jak np. tekstura, color, gradient. Ten tryb jest realizowany poprzez ustawienie odpowiedniego stanu OpenGLa, w związku z czym można wybierać jedynie z ograniczonego zbioru równań blendowania.
2. Silnik może również blendować ze sobą dwie różne tekstury używając równań znanych z programów graficznych jak Photoshop. Wtedy wyliczenie koloru odbywa się w pixel shaderze. Aby blendować ze sobą w ten sposób tekstury należy wpiąć __TexturePlugin__, a za nim [BlendTexturePlugin](BlendTexture).

## Premultiplied alpha

Założenie jest takie, że użytkownik podaje teksturę, która ma wcześniej wymnożone kanały koloru przez wartość alfy.
Wyjątkiem jest tryb blendowania **Alpha**, który został wprowadzony dla zachowania zgodności z teksturami, które nie zostały wymnożone.

Tesktury powinny być wymnożone, żeby poprawnie działały mipmapy w miejscach o alfie innej niż 1. Więcej na ten temat można przeczytać tutaj:
https://developer.nvidia.com/content/alpha-blending-pre-or-not-pre


## Tryby blendowania

Większość równań blendowania jest napisana na podstawie: https://mouaif.wordpress.com/2009/01/05/photoshop-math-with-glsl-shaders/

Tryby blendowania:

- **None** oznacza brak blendowania. Nowy piksel zastępuje stary
- **Alpha** oznacza standardowy tryb blendowania z wymnożeniem koloru przez alfę. Powinien być stosowany, jeżeli podana tekstura ma nie wymnożoną alfę.


Blend mode name | BlendTexture support | Blend with Background support   | Effects support (??)
--------------- | -------------------- | ------------------------------- | -----------------
Normal          | yes                    | yes                           | 
Lighten         | yes                    | no                            | 
Darken          | yes                    | no                            | 
Multiply        | yes                    | yes                           | 
Average         | yes                    | yes                           | 
Add             | yes                    | yes                           | 
Substract       | yes                    | no                            | 
Difference      | yes                    | no                            | 
Negation        | yes                    | no                            | 
Exclusion       | yes                    | no                            | 
Screen          | yes                    | no                            | 
Overlay         | yes                    | no                            | 
SoftLight       | yes                    | no                            | 
HardLight       | yes                    | no                            | 
ColorDodge      | yes                    | no                            | 
ColorBurn       | yes                    | no                            | 
LinearDodge     | yes                    | no                            | 
LinearBurn      | yes                    | no                            | 
LinearLight     | yes                    | no                            | 
VividLight      | yes                    | no                            | 
PinLight        | yes                    | no                            | 
HardMix         | yes                    | no                            | 
Reflect         | yes                    | no                            | 
Glow            | yes                    | no                            | 
Phoenix         | yes                    | no                            | 
Hue             | yes                    | no                            | 
Saturation      | yes                    | no                            | 
Color           | yes                    | no                            | 
Luminosity      | yes                    | no                            |
None            | yes                    | yes                           | 
Alpha           | yes                    | yes                           |