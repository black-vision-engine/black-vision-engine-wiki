[TOC]

# Bad things

## Bad design

### Zwalnianie assetów

Powiązanie między assetami po stronie modelu i silnika jest dość luźne. Żeby dało się zwolnić np tekstury z cacha silnikowego, powstała klasa AssetsTracker, która przechowuje mapowanie assetów. AssetTracker jest powiadamiany poprzez eventy.

Problem polega na tym, że AssetTracker nie nadaje się do wielowątkowego wykonania. Np. funkcja GetUnusedAssetUIDs zwraca listę nieużywanych assetów, ale w momencie zwalniania assetów, stan może być już zupełnie inny jeżeli inne wątki coś modyfikują.

Kolejny problem powstaje, gdy jest asset (np. Texture2D), który nie ma swojej reprezentacji w modelu.
Takie obiekty nie są automatycznie zwalniane.

Warto przemyśleć od nowa jak mają wyglądać assety i jak łączyc model z silnikiem.

### Wielowątkowość

Główny problem to to, że wszystko ma dostęp do wszystkiego, mamy dużo klas statycznych, mało rzeczy lokalnych.
Żeby zrobic coś wielowątkowo, trzeba przepisać kawał kodu. Przykładowymi problemami może być wielowątkowe wczytywanie scen lub rozdzielenie na wątki funkcji Update.

### Play/Stop audio

W tej chwili odbywa się to z pośrednictwem AssetTrackera poprzez wysłanie eventu. Jest tak zdaje się dlatego, że
trzeba wywołać Play na AudioEntity i potrafi to AudioRenderer. Nie wiem czy AudioRenderer jest na prawdę do tego konieczny. Przydałoby się to rozwiazać tak, żeby wszystko działo się w NodeUpdaterze.

## Bad reality

### Wykrywanie nadmiarowych danych w XMLach

W tej chwili serializator nie wykrywa sytuacji, w której w pliku XML znajduje się więcej danych (node'ów lub atrybutów) niż zostało z niego przeczytane.
Zdecydowaliśmy na razie tego nie ruszać, bo zrobienie tego wymagałoby albo

a) dopisania walidacji w każdym obiekcie

albo

b) śledzenia przez deserializator odwiedzanych elementów

Tak czy siak nakład pracy zdaje się przekraczać potencjalny zysk.