Opis ogólny
===========

Mechanizm zapewniający przyciąganie node-ów do poziomych bądź pionowych
linii 2D w obszarze roboczym renderera

Założenia:

-   mechanizm działa poprawnie jeśli operujemy statyczną kamerą
-   przyciągane są środki ciężkości obiektów
-   linie te można nazywać
-   w trakcie zmiany położenia linii “przypięte” node-y powinny się
    poprawnie przesunąć razem z nią
-   załóżmy że istnieją maksymalnie 64 linie poziome oraz 64 linie
    pionowe TabStopów per output renderera
-   linie powinny być wizualizowane na rendererze (flaga bool)

<!-- -->

-   na tym etapie nie jestem pewien czy wymusić sortowanie indexów linii
    względem położenia czy też nie, albo czy wogóle zablokować
    przesuwanie TabStopa o indeksie N na pozycję N+1 jeśli jego
    położenie jest mniejsze od położenia TabStopa N+1. Psotaram się to
    do jutra wyklarować

pseudoJSON :) obrazujący wykorzystanie

     {
        "Event" : "TabStopEvent",
        "Command" : "AddNodeToTabStop",
        "Type" : "vertical", // bądź horizontal
        "EventID" : "1",
        "NodeName" : "#0/#0",
        "SceneName" : "scenka.scn",
        "TabStopName" : "nazwaTabStopa", // bądź #1 (index tabStopa 0-based)
     }

Funkcjonalności eventów:

-   ustaw położenie tabStopa
-   podepnij Node pod tabstop
-   odepnij Node od tabstopa
-   odepnij wszystkie node-y od tabstopa
-   zmień nazwę tabstopa

Do dokładniejszego zdefiniowania
--------------------------------

Pewnie jeszcze są potrzebne mechanizmy odpytania o tab stopy ?

-   si

Czy nie sa potrzebne linie w 3-cim wymiarze ? Jeżeli kamera ma ustawioną
perspektywę, to obiekty, które nie są w tej samej płaszczyźnie co tab
stopy, po przyciągnięciu nie będą wyrównane do tych linii. Będzie to
bardzo mylące dla użytkownika.

-   nie są potrzebne w 3cim wymiarze, założenie korzystania z tab stopów
    jest takie że korzystamy z nich jak w photoshopie ( na jednej
    plaszczyznie, zbliżone wartości z, 2d, brak rotacji względem oY )

Czy linie są zapisywane w scenie?

-   tak

Dlaczego tab stopy sa per output renderera ? Bo to trochę sprzeczne z
zapisywaniem

-   masz rację

w scenie by było, bo output renderera jest dla wszystkich wczytanych
scen w silniku.

-   ok

Co to znaczy, że tab stopy działają ze statyczną kamerą ?

Jeżeli kamera będzie dynamiczna to rozumiem, że tab stopy mają być
wyrównane nadal względem współrzędnych świata ?

-   co masz na mysli mówiąc o wspólrzędnych świata (punkt poczatku
    układu wspolrzednych niezależny względem kamery? )

<!-- -->

-   -   nooo to właśnie miałem na myśli

Jak jest hierarchia nodów i dzieci dziedzicza transformację rodzica, to
znalezienie poprawnej transformacji robi się skomplikowane. Sprawa
komplikuje się jeszcze bardziej, gdy trzeba aktualizować całą hierachię
w momencie, gdy rodzice są przypięci do tab stopów. Wtedy trzeba
zachować kolejność nakładania transformacji. (no ale to zasadniczo nasz
problem :P)

-   ok, przemyślę ten problem, zasadniczo najlepiej modyfikować
    bezpośrednio transformację node-a którego przypisujemy do tabstopa,
    fakt problem jest kiedy rodzic będzie w jednym tabstopie a dzieci w
    drugim -&gt; mysle ze trzeba bedzie nalozyc jakies constrainty na to
    zeby nie tworzyc wycudowanych mechanizmow do prostej w zasadzie
    rzeczy

Jeżeli przy przesuwaniu tab stopów mam przesuwać obiekty, to muszę mieć
zapamiętane gdzie co jest przypięte. A co ma się dziać w momencie, gdy
mamy wiele kluczy transformacji. Czy mam zapamietywać przypięcie dla
każdego klucza ? W ogóle sprawa z kluczami bardzo komplikuje wszystkie
rzeczy. To trzeba jakoś bardzo kompleksowo przemyśleć.

-   Tab stop z załozenia powinien overrideowac transformację względem
    konkretnej osi (wtedy klucze np dla osi X są nieważne)

Przyciągane mają być środki cieżkości obiektów związane z boundign boxem
czy z parametrem weight center pluginu transform? Jeżeli to drugie, to
znów pytanie jak to ma współpracować z kluczami?

-   a czy te środki nie oznaczają tego samego punktu? czy środek
    ciężkości liczysz na zasadzie (max-min)/2 ?

<!-- -->

-   -   Środek ciężkości z pluginu transform jest dowolnym punktem, bo
        możesz go zdaje się sam ustawić. Środek bounding boxa jest
        wyliczany na podstawie geometrii rzeczywiście jako (max+min)/2.
        Jest jeszcze trzeci charakterystyczny punkt tzn. (0.0, 0.0),
        który jest środkiem lokalnego układu współrzędnych geometrii.
        Czyli jak masz np. parametr “weight center X” pluginu
        geometrycznego (a nie transformacji), to on przesuwa geometrię
        względem tego środka już na etapie generowania.

<!-- -->

-   wizualizacja BB powinna rowniez wizualizowac weight center pluginu
    transform

Czy na pewno nie będzie tak, że w pewnym momencie bedziesz chciał
przyciągać obiekty również do krawędzi bounding boxów ?

-   przemyślę jeszcze

Co ma się stać, jeżeli obiekt zostanie przypięty do tab stopa, potem
zostanie mu zmieniona transformacja, a potem ktoś ruszy tab stopa, do
którego został on przypięty i wtedy wróci on na poprzednią poprzednią
pozycję, mimo że użytkownik go sobie wcześniej przesunął?

-   tab stop powinien nadpisywać parametr (X lub Y) w zależności od tego
    czy jest horizontal czy vertical (czyli jezeli jest coś przypiętę do
    tab stopa którego linia jest pionowa i próbujemy ruszyć transformem
    X danego nodea to node w efekcie nie jest przesuwany)
-   jezeli tak bedzie wygodniej mozemy kasowac klucze animacji transform
    w momencie podpinania node-a do tabstopa i je niejako zablokowac -
    aczkolwiek to tez jeszcze przemysle

<!-- -->

-   -   Co do tego nadpisywania i w ogóle: Widzę, że chcesz stworzyć
        jakiś mechanizm zupełnie alternatywny do ustawiania kluczy i
        wszystkiego co jest do tej pory zrobione. Nie jestem pewien czy
        to jest dobry pomysł. Kasowanie kluczy node'a podpiętego do tab
        stopa po prostu musi być robione. Wszystkie wartości parametrów
        są ustawiane poprzez ewaluację. Nie ma możliwości wzięcia tej
        wartości z jakiegoś innego miejsca bez jakiegoś dużego gwałtu na
        silniku. Niestety nie mamy też możliwości uniemożliwienia
        dodawania kluczy dla podpiętych nodów. Ewentualnie można by to
        zrobić być może poprzez ParamSource jak będzie zrobione, ale nie
        jestem pewien, bo nie wiem do końca jak to ma być realizowane.
        Zasadnicze pytanie co do tab stopów jest takie czy ma to być
        taki helper przy tworzeniu sceny, że ktoś może sobie ustawić
        linie i do nich poprzyciągać obiekty, czy ma to być używane
        także w trakcie ewaluowania sceny ? Czy ma być możliwość
        animowania położeń tych linii ?

<!-- -->

-   -   TabStop powinien być takim właśnie helperem do projektowania
        scen. Zmiana położenia TabStopa powinna skutkowac zmianą
        polozenia podpietych obiektów. Wydaje mi się, że możemy te
        tabstopy potraktować tak jak sugerujesz (czyli zeby jednorazowo
        (bądź podczas zmiany polozenia tabstopu) ustawiały położenie
        obiektu a my po stronie edytora zapewnimy wtedy w jakiś sposób
        żeby nie mozna bylo dodawac kluczy do ich translacji) - tak
        będzie ok?

<!-- -->

-   -   Hmm. Bo ty chciałbyś sobie przesunąć tab stopa i żeby za nim
        podążyły wszystkie podpięte node'y. Ale taką funkcjonalność
        można łatwo fakeować zrobieniem pustego node'a rodzica dla kilku
        dzieci. Ja to sobie wyobrażałem trochę jako funkcjonalność
        wyrównania do siatki. Klikasz node'a i mówisz mu - wyrównaj się
        do tamtej linii. Czyli takie jednorazowe działanie, ale
        jednocześnie przydałoby się, żeby można było takie coś zrobić w
        każdym kluczu, a nie tylko dla czasu 0. W zasadzie to nawet ta
        wersja z przemieszczaniem tab stopa razem z node'ami powinna
        jakoś obsługiwać różne klucze, a nie tylko zerowy. Tylko trochę
        nie do końca wiadomo jak to zrobić. To kasowanie/blokowanie
        dodawania kluczy, to raczej takie łatanie, boję się, że to mocno
        zuboży przydatność tych tab stopów. Będziesz kiedyś u nas? Myślę
        że powinno się jakoś wyobrazić i prześledzić na żywo sposób
        użycia tego mechanizmu i wtedy ustalić co z tym zrobić.

Czy chcesz włączać i wyłączać wizualizację wszystkich linii na raz czy
chcesz móc sterować każdą z osobna ?

-   wszystkie naraz