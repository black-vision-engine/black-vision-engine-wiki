- Karta wideo u creeda
- Komputer do benchmarkowania
- Kolejność pluginów, po której stronie? Czy możemy ją usztywnić? **Przemyśleć**
- Testy komunikacji silnik<>edytor, licznik otrzymanych/poprawnych eventów
- Video inputy, interlace trzeba robić na wejściu
    - Alpha przychodzi na innym kanale niż RGB i musi być możliwość złożenia tego w całość
    - W takim wypadku trzeba jakoś zrobić synchronizację ramek z obu kanałów, żeby składać ze sobą to, co trzeba
- Plugin Video Input powinien zapewniać funkcjonalność odfiltrowania green/blue-boxa
    - Przykładowy use case: Solid Rect -> Video Input. Wtedy odfiltrowany input powinien być na tle wyniku recta.
- Tylko niektóre kanały audio sa istotne (pewnie 2, lewy i prawy). Zawsze bierzemy pierwsze dwa a resztę ignorujemy
- Spojrzeć jak jest video output i zrobić odwrotnie
- Plugin Video Input potrafi wziąć dwa inputy i jeden potraktować jako RGB a drugi jako Alphę. Boolem możemy wyłączyć -
Alphę. Jest też parametr alpha, przez którą mnożymy cały obrazek.
    - Trzeba zbadać czy możemy mieć wpiętą tylko 1 teksturę kiedy shader chce 2 i czy możemy po wczytaniu wyrzucić asset.
Inputy z karty powinny być numerowane w configu.
- Żeby zminimalizować efekty frame dropów, możemy wprowadzić Ring Buffer, ale w przypadku użycia Video Inputów one również muszą to implementować. Wartość jest konfigurowana z configa.