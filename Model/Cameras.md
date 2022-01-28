Opis
====

W silniku może być nieograniczona liczba kamer dla każdej sceny. Każda
scena ma w ustawioną w danym momencie jedną kamerę, która służy do
renderowania. Można zmienić tę kamerę przy pomocy eventu
[CameraEvent](CameraEvent "wikilink") (Command: SetCurrentCamera).

Każda scena jest tworzona z domyślną kamerą z parametrami ustawionymi na
wartości podane w dziale Parametry. Ostatnia kamera w scenie nie może
zostać usunięta.

Eventy
======

Modyfikowanie parametrów przy pomocy eventu
[ParamKeyEvent](ParamKeyEvent "wikilink") (Target: CameraParam).\
Pobieranie informacji o istniejących kamerach
[InfoEvent](InfoEvent "wikilink") (Command: CamerasInfo).\
Ustawianie kamer [CameraEvent](CameraEvent "wikilink")

Parametry
=========

IsPerspective (bool) (default true)
:   Ustawia kamerę perspektywiczną lub ortogonalną.

ViewportSize(float) (default 2.0)
:   Rozmiar pola widzenia kamery ortogonalnej. Proporcje widoku wyliczane są z rozmiarów
    RenderTargetów. Następnie są one wymnażane przez tę wartość.

Position(vec3) (default 0.0f, 0.0f, 5.0f)
:   Pozycja kamery

Direction(vec3) (default 0.0f, 0.0f, -1.0f)
:   Kierunek w jakim skierowana jest kamera.

UpVector(vec3) (default 0.0f, 1.0f, 0.0f)
:   Wektor wskazujący, gdzie jest kierunek “do góry”. Jeżeli wektor nie
    będzie prostopadły do kierunku patrzenia (Direction), to zostanie on
    zortogonalizowany. Jeżeli wektor jest równoległy do kierunku
    patrzenia, zachowanie jest niezdefiniowane.

FOV(float) (default 45)
:   Kąt widzenia wyrażony w stopniach.

NearClippingPlane(float) (default 0.1f)
:   Bliższa płaszczyzna przycinania pola widzenia.

FarClippingPlane(float) (default 100.0f)
:   Dalsza płaszczyzna przycinania pola widzenia.