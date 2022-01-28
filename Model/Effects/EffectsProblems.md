Globalne i lokalne efekty
-------------------------

Przykład globalnego i lokalnego efektu.
:   Przykład blur. (globalny)

    :   Renderujemy do FS node, do którego jest przypięty efekt blura.
        (renderowanie od oddzielnego RT) (Logika Pre blur efekt z 1
        outputem)
    :   Output z Pre przekazywany jest jako input do FSE typu blur
        horyzontalny. (renderowanie do oddzielnego RT) (1 output)
    :   Output z horizontal blur przekazywany jest jako input do FSE
        typu blur wertykalny. (renderowanie do oddzielnego RT) (1
        output)
    :   Output z vertical blur blitowany jest do głównego RT. Blitowanie
        jest wykonywane przez funkcję Render w
        CompositeFullscreenEffect.

<!-- -->

:   Przykład blur. (lokalny. To znaczy. Blurujemy teksturę, która jest
    inputem dla pluginu. Na przykład pluginu Texture)

    :   Logika Pre blur efekt wyciąga teksturę z SceneNode i ustawia ją
        jako teksturę w dla output RenderTarget logiki. Zobacz funkcją
        RenderImpl w BlurPreFullscreenEffectLogic
    :   Implementacja CopositeFullScreenEffect pozostaje bez zmian poza
        funkcją Render. W przypadku lokalnego efektu nie blitujemy na
        główny render target, a tylko renderujemy graf efektów do
        oddzielnego render targetu.
    :   Niestety dla lokalnego efektu funkcja Render w klasie
        NodeEffectLogic musi wyglądać inaczej. W tym miejscu musimy
        podmienić teksturę wejściową w do shadera w SceneNode. Powodem
        jest brak dostępu to zburowanej tekstury z poziomu Logik Post
        blur efect.
    :   Logika Post dla blur efekt renderuje SceneNode wraz z dziećmi na
        główny render target.

<!-- -->

Wnioski (efekt lokalny - dla noda)
:   Zakładamy, że działają one tylko na nodzie, do którego ten efekt
    jest podpięty. Nie na jego dzieciach.
:   Chyba dla każdego rodzaju zestawu pluginów w nodzie sprowadza się to
    tylko i wyłącznie do preprocessingu tekstury, która używana jest
    nodzie.

    :   Dla texture plugin jest efekt stosowany na teksturze wejsciowej
    :   Dla Text plugin na atlasie. (Nie bardzo ma to sens dla blura na
        przykład. Text ma już parametr blur w sobie.)
    :   Dla Video Stream plugin działa tak samo jak na texture plugin,
        ale zblurowana tekstura nie może być cachowana.
    :   Dla pozostałych pluginów które nie mają wejsciowej tekstury taki
        efekt nie ma sensu. W szczególności dla solidcolor i wszystkich
        geometrii.

<!-- -->

Pytania (efekt lokalny - dla noda)
:   Czy założenie, że lokalny efekt ma działać tak samo jak globalny ale
    na teksturze wejściowej do pluginu teksturującego (zamiast na
    wyrenderowanym poddrzewie do FS.) zawsze ma sens?
:   Jak przerobić istniejącą logikę renderowania efektów, żeby mogła
    lepiej wspierać lokalne efekty? Teraz działający kod z implementacją
    lokalnego blur efektu jest w w gałęzi 'new\_fse\_blur". Tam nie będą
    popranie działały globalne efekty.
:   Może warto dla lokalnego blura przenieść całość procesu blurowania
    tekstury wejściowej do BlurPreFullscreenEffectLogic? Wtedy jednak
    musimy implementować logikę V i H blur ponownie mimo, że jest ona
    już zaimplementowana w logice FSE dla FET\_BLUR.
:   Co w przypadku efektu Image Mask? Ze względów wydajnościowych
    powinniśmy oddać dodatkową tekturę do shadera i próbkować ją tak jak
    próbkujemy nakładaną teksturę. Dodatkowo można dorzucić jakieś
    przekształcenie, dla UV maski.