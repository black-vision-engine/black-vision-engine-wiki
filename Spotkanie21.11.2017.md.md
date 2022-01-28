#Spotkanie 21.11.2017r.

## Priorytety - wersja production:

- Audio
    - sprawdzić czy działa na kartach video 
    - naprawić
- Text
    - zdobyć scenki z niepoprawnym tekstem i namierzyć problem
    - być może będzie to wymagało zmiany algorytmu renderowania tekstu
- Scenki z lutego powinny się wczytywać na obecnej wersji silnika (tam jest podobno spora baza scenek, która się wywala i blokuje to
    przejście na nową wersję silnika)
- Poprawić wydajność
    - profilowanie scenek basketball (+ trzeba wydobyć od Blackburstów resztę scenek)
    - trzeba sprawdzić jakie są najbardziej problematyczne elementy, żeby opracowac plan testów
    - napisać testy regeresji konieczne przed podejściem do optymalizacji
    - zoptymalizować
  
## Po zakończeniu wersji produkcyjnej:

- BlackBurst ustali swoje podejście do Undo/Redo i jak to się będzie łączyło z silnikowym Undo/Redo
- Uporządkowanie API
    - zaprojektować docelową strukturę eventów
    - przepisać API po stronie silnika oraz edytora - to powinno się stać zanim nowy edytor zacznie używać
    wersji production. Trzeba zsynchronizowac te dwie rzeczy.
  
    
## Pozostałe zdania do wymagań funkcjonalnych z mniejszym priorytetem:

- Audio - pokazywanie głośności (albo wysyła dane do edytora, albo pokazuje je na gizmo)
    - ustalić dokładniej wymagania
        - co ma być wysyłane - maksymalna próbka dźwięku, średnia maksymalna może obie te wartości
        - jak często ma być wysyłana - wszystkie próbki, uśrednione wartości z jednej ramki czy inne
        - czy to ma być prezentowane poprzez gizmo czy do edytora
- Efect Colorize (efekt, który zaczął keidy pisać Viggith, ale nie skończył)
- Karty BlackMagic - sprawdzić czy działa audio i video (podobno działa, ale trzeba się upewnić)
- Video Input - to zostało zaczęte, ale nie jest aż takim priorytetem, więc trzeba skończyć później


## Wymagania pozafunkcjonalne:

- Niestabilny framerate - niektóre ramki są niesamowicie długie (tu nie chodzi o spadek FPSów)
    - trzeba zbadać jakie są przyczyny
- Poprawić kwantowanie czasu (nie działa i nie ruszamy póki nie sprawi problemów)
    - Zdarza się, że odpytanie timelinu daje czas, jaki się nie powinien pojawić
    - Jeżeli ustawi się sekwencje, niekóre klatki są gubione
    - Zdobyc odpowiednią sekwencję od Pawełka
- Kwantowanie czasu połaczone z synchronizacją z kartą video
    - podobno niektóre ramki są liczone dla czasu między klatkami
    - zdobyć od Pawełka odpowiednie scenki
- Ramki zamiast czasu
    - przyczyny
        - użytkownik chce się raczej posługiwać ramkami
        - parametry powinny się ewaluować w skwantowanym czasie
        - podobno występują jakieś problemy numeryczne (np. niektóre wartości czasu nie są wyrażalne 0.2999999 itp)
    - zastanowić się czy są konieczne zmiany
        - jedną opcją jest implementowanie tego wszystkiego po stronie edytora, żeby w widokach było wszystko wyrażone w ramkach
        - inną opcją jest robienie tego po stronie silnika. Z dyskusji wynikały następujące rzeczy:
            - wymagany fps (czas ramki) jest odczytywany z konfiga
            - w plikach sceny czas jest zapisywany w ramkach
            - jeżeli scena została zapisana przy innym FPSie, to po prostu będzie działać szybciej lub wolniej - nie uważamy tego za błąd
    
    
    
## Ustalenia natury projektowej:

- Buildy będą ściagane z bitbucketa (ale dopiero po przygotowaniu wersji produkcyjnej)
    - trzeba dorobić w Jenkinsie automat, który będzie te buildy tam umieszczał
- Zostanie nam udostępnione repo z Edytorem
- BlackBurstci będą prowadzić Pivotala (albo znajdą jakieś inne narzędzie), które nam udostępnią