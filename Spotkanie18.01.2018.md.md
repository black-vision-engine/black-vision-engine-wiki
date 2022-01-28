https://docs.google.com/document/d/10MyquTlTGbYx3DpPrOvyuY-s4QJl7zP134scMQo5gSk/edit


Logger domyślnie pisze wszystko a potem config może to zmienić. (p: 2)
Jeżeli nie ma karty z configa, to wypisujemy, że jej nie ma i kończymy pracę. (p: 3)
Podobnie jeśli jest za mało outputów lub inputów. (p: 3)
Zapisywanie configa. (p: 3)
Tworzenie domyślnego configa, jeśli go nie ma. (p: 3)
Wypisywanie, że jakaś wartość w configu jest błędna. (p: 2)
Miniaturki wideo: 
sprawdzić czy są generowane za każdym razem (p: 4)
wydajność wysyłania do edytora (p: 4)
problemy ze zwieszaniem się (p: 4)
różne rozmiary miniaturek (p: 0)
Określić formaty wideo, które obsługujemy. (Pawełek dostarczy pliki)
Żeby BV się nie wywalał jak dostanie zły format i o tym opowie. (p: 5)
Przy wczytywaniu sekwencji loader wysyła progres przez eventy. (p: 4)
Przekompilować ffmpeg. (w szczególności żeby było speedhq + magicYUV) (p: 5)
Optymalizacja odtwarzania wideo (powinno się dać odtwarzać 8 strumieni HD) (p: 3)
Audio decoder - kopiuj wklej z video decoder.
Check-box osi w logice Follow (p: 7)
Wyrzucamy w wersji produkcyjnej:
HeightMap
PieChart (?)
GeoSphere (?)
NodeReplicator
Przywracamy Prisma (p: 1)
VideoInput (p: 10)
Kolory w SVG przez logikę (p: 1)
UIDy node’ów (Pawełek pisze specyfikację)
Colorize (Pawełek daje specyfikację, p: 6)
GridLines w 3d - na początek badanie.
Przetestować konfigurowanie z configa różnych kombinacji wejść/wyjsć: (p: 5)
Interlace
Progressive
SD
HD
FPS 50, 60
Przygotować przykładowe configi do punkty powyżej. (p: 5)
Ustawianie fazy H+V
Jak przetestować zapisywanie video do pliku? Sprawdzić w BVle.
Jak przetestować pamięć współdzieloną (p:5).
Sprawdzić czy działa czyszczenie cacha (p:6)
Wczytać plik .jpg
Zmienić plik
Usunąć cacha
zobaczyć co się wczytało
Ustawić leading pluginu text na 1.5
Rozszerzenia dla assetów upper case są ignorowane (p:8)
MouseHoover - edytor wysyła event, żeby zaznaczyć noda po przejechaniu nad nim - przemyśleć
Nowe API (p:4)