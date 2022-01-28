Opis
====

Światła oddziałują tylko na node, które mają przypięty plugin
[Material](Material "wikilink").\
Można dodać maksymalnie 8 świateł każdego typu. Maksymalna liczba
świateł może być dowolna, ale z góry zdefiniowana na sztywno.

Eventy
======

Parametry można ustawiać standardowo przy pomocy
[ParamKeyEvent](ParamKeyEvent "wikilink") (Target: LightParam).\
Dodawanie/usuwanie świateł przy pomocy
[LightEvent](LightEvent "wikilink").

Parametry
=========

Directional
-----------

color (vec3) (default 1.0, 1.0, 1.0)
:   Kolor światła.

direction (vec3) (default 0.0, 0.0, 0.0)
:   Kierunek światła - kąty w stopniach. Kierunek początkowy, który jest
    obracany - (0.0, 0.0, -1.0).

Point
-----

color (vec3) (default 1.0, 1.0, 1.0)
:   Kolor światła.

position (vec3) (default 0.0, 0.0, 0.0)
:   Pozycja światła (we współrzędnych kamery).

attenuation (vec3) (default 1.0, 0.0, 0.02)
:   Współczynnik tłumienia światła (attenuation.x + attenuation.y \*
    dist + attenuation.z \* dist \* dist).

Spot
----

color (vec3) (default 1.0, 1.0, 1.0)
:   Kolor światła.

direction (vec3) (default 0.0, 0.0, 0.0)
:   Kierunek światła - kąty w stopniach. Kierunek początkowy, który jest
    obracany - (0.0, 0.0, -1.0).

position(vec3) (default 0.0, 0.0, 0.0)
:   Pozycja światła (we współrzędnych kamery).

attenuation (vec3) (default 1.0, 0.0, 0.02)
:   Współczynnik tłumienia światła (attenuation.x + attenuation.y \*
    dist + attenuation.z \* dist \* dist).

cutOff (float) (default 100.0)
:   Kąt odcięcia światła w stopniach.

exponent (int) (default 10)
:   Wykładnik tłumienia kątowego światła.