#Plugin Extrude


**Name**: extrude

**UID**: DEFAULT\_EXTRUDE\_PLUGIN

Plugin wyszukuje krawędzie w geometrii pobranej na wyjściu poprzedniego
pluginu i wyciąga je w kierunku podanym w parametrze. Plugin powinien
być łączony z geometrią 2D. Dla geometrii 3D efekt działania może nie
być zgodny z oczekiwaniami.

Wyekstrudowana płaszczyzna może mieć krzywiznę określoną wybraną
funkcją. Funkcja ta określa odchylenie wierzchołków płaszczyzny wzdłuż
jej normalnej.

##Parametry


Parameter name         	| Type      	| Default Value    	| Description
----------------------- | -------------	| -----------------	| -----------
extrude vector         	| vec3	   		| 0.0, 0.0, -0.9	| Wektor o jaki zostaną przesunięte wyekstrudowane wierzchołki.
smooth threshold angle 	| float			| 160				| Kąt decydujący o tym czy dana krawędź ma zostać wygładzona czy nie. Wszystkie kąty o wartości mniejszej niż podana będą ostre. Parametr wyrażony w stopniach.
tesselation				| int			| 40				| Tesselacja bocznej ścianki (wyekstrudowanej). Parametr używany jedynie wtedy, gdy dla bocznej ściany została ustawiona jakaś funkcja krzywizny.
extrude curve			| enum ExtrudeCurveType | None		| Enumeracja służąca do wybierania funkcji krzywizny ściany bocznej.
curve scale				| float			| 0.2				| Parametr służy do skalowania wybranej funkcji wzdłuż normalnej.
cosinus curve period	| int			| 1					| Liczba okresów funkcji cosinus
bevel depth	front		| float			| 0.0				| Jak daleko wzdłuż **extrude vector** sięga bevel. Parametr dla przedniego bevela.
bevel depth	back		| float			| 0.0				| Jak daleko wzdłuż **extrude vector** sięga bevel. Parametr dla tylnego bevela.
bevel height			| float			| 0.0				| Jak wysoko sięga bevel wzdłuż wektora normalnego powierzchni bocznej.
bevel tesselation		| int			| 0					| Tesselacja bevela. Jest jedna tesselacja, która jest stosowana do obydwu beveli. W wewnętrznej implementacji będzie to tak napisane, jakby były dwie niezależne, żeby można było w przyszłości wystawić dodatkowy parametr.
bevel curve front		| enum BevelCurveType | None		| Enumeracja oznaczająca krzywą używaną do tworzenia bevela. Można ustawić niezależnie krzywą dla przedniego i tylnego końca.
symetrical bevel		| bool			| false				| Po włączeniu tego boola, do przedniego i tylnego bevela są stosowane te same parametry **bevel depth** i **bevel curve**.


**enum ExtrudeCurveType**

1. **None (0)** - brak krzywziny
2. **Parabola (1)** - funkcja kwadratowa
3. **Cosinus (2)** - funkcja cosinus. Liczba okresów funkcji jakie mają
    zostać wygenerowane wybierana jest parametrem **cosinus curve
    period**.
4. **Gauss (3)** - krzywa Gaussa
5. **Circle (4)** - okrąg

## Planowane zmiany

### Mapowanie tekstury

1. Jeżeli w pluginie stojącym przed Extrudem są UVki, to zostaną one wykorzystane, jeżeli ich nie ma, to zostaną wygenerowane domyślne UVki. Obliczany jest bounding box geometrii na wejściu i współrzędne są mapowane planarnie na wyliczony fragment płaszczyzny.
2. Na bocznych wyekstrudowanych powierzchniach kopiujemy UVkę brzegową. Oznacza to, że na bokach stworzą się takie pasy, bo tekstura będzie próbkowana tak jakby tylko w jednym wymiarze. Jeżeli nawet w jakimś przypadku będzie to wyglądać obrzydliwie to się z tym godzimy
3. UVki są przepisywane na tylną wyekstrudowaną powierzchnię. Zostanie dodany parametr boolowski do wyłączania tego przepisywania. Wtedy no właśnie **Paweł** co będzie się działo ? Bo ty być chciał mieć wtedy jednolitą powierzchnię tam z tyłu ?? (Bo zauważ, że przepisanie UVek z brzegów jest dokładnie tym samym co skopiowaniem mapowania na tylną powierzchnię!!)


### Nowe krzywe extrudowania

Obecne krzywe, które mogą się znajdować na krawędzi extrudowanej powierzchni mogą być jedynie gładkie. Trzeba dodać możliwość ustawiania krzywych z punktami niegładkimi.

Czyli obecnie wyglądałoby to tak:

- Pierwszy fragment krzywej (to co w vizie się nazywa bevelem)
- Środkowy fragment, który jest dokładnie tym samym co mamy w tej chwili
- Symetryczny bevel po drugiej stronie

####Nowe parametry

- **bevel depth**
    Jak daleko wzdłuż **extrude vector** sięga bevel. Będą dwa niezależne parametry dla przedniego i tylnego bevela.
- **bevel height**
    Jak wysoko sięga bevel wzdłuż wektora normalnego powierzchni bocznej. Wprowadzenie tego parametru modyfikuje znaczenie parametru **curve scale**. Punktem zerowy środkowego fragmentu będzie wyznaczany przez **bevel height**. Skalowanie będzie się odbywało względem tego punktu.
- **bevel curve**
    Enumeracja oznaczająca krzywą używaną do tworzenia bevela. Można ustawić niezależnie krzywą dla przedniego i tylnego końca. Tutaj będą stosowane inne funkcje niż w środkowym fragmencie i trzeba zdefiniować czego chcesz (Paweł).
- **bevel tesseletion**
    Tesselacja bevela. Jest jedna tesselacja, która jest stosowana do obydwu beveli. W wewnętrznej implementacji będzie to tak napisane, jakby były dwie niezależne, żeby można było w przyszłości wystawić dodatkowy parametr.
- **symetrical bevel**
    Po włączeniu tego boola, do przedniego i tylnego bevela są stosowane te same parametry **bevel depth** i **bevel curve**.


####Krzywe

- **Prosta**
    Linia prost łącząca krawędź ekstrudowanego obiektu 2D ze środkowym segmentem
- **Wypukła sfera**
    W zasadzi jak się uwzględni bevel depth i height to wyjdzie pewnie z tego jakaś elipsa
- **Wklęsła sfera**

Paweł wypowiedz się!!