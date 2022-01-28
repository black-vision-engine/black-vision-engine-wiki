# Gizmos/HUDs - project

## Mechanizm renderowania

Trzeba oddzielić renderowanie HUDów od renderowania nodów. Jednocześnie potrzebujemy informacji o z-tce, żeby
poprawnie wyglądało przysłanianie się obiektów. Kolejnym problemem jest to, że efekt może zrobić w zasadzie z nodami
co chce, więc mogą zostać one przesunięte w dowolne miejsce, zupełnie zignorowane itp...

Założenia są więc następujące:

- Trzeba zrzucić część odpowiedzialności za renderowanie HUDów na twórców efektów, bo tylko oni wiedzą, co się powinno stać
- HUDy muszą być renderowane do oddzielnego render targetu
- Silnik będzie miał tryb Edycji i Produkcji, żeby HUDy nie spowalniały gdy nie są potrzebne

Oprócz main render targetu każda scena (czy to powinno być per channel ???) będzie posiadała HUD render target (dalej rtHUD).
rtHUD będzie schodził wgłąb drzewa wraz z całym kontekstem renderera i będzie dostępny dla efektów.
Renderowanie nodów będzie się odbywało dwukrotnie - zostanie dodany przebieg odpowiedzialny za zapisanie z-tki do rtHUD.
Ponieważ rtHUD jest jeden, to na koniec będziemy mięli z-tkę z całego drzewa.

Większość naszych obecnych efektów będzie po prostu renderowała drzewo tak jak to robi normalnie + będzie renderowała jedynie z-tkę
do rtHUD. Można to zrealizować np. poprzez dodanie nowego passa renderowania. W przypadku bardziej skomplikowanych efektów
twórca będzie się martwił, co ma w zasadzie wyrenderować.

Mając z-tkę możemy na koniec wyrenderować wszystkie HUDy. (ewentualnie tu trzeba pomyśleć czy HUDy nie mogą wymagać blendingu
i w którym momencie je najlepiej renderować). Na koniec rtHUD można zblittować do mainRT.

Ponieważ ta metoda dodaje dość znaczny koszt, to trzeba będzie w edytorze zrobić dwa tryby - Edycji i Produkcji.
W trybie produkcji HUDy będzie można całkowicie wyłączyć.

## Interakcja z modelem

Aby zminimalizować nakład pracy, HUDy powinny jak najbardziej wykorzystywać istniejące mechanizmy silnikowe i modelowe. Dlatego najlepiej będzie, jeżeli po stronie modelu HUDy będą budowane z nodów i pluginów. Dzięki temu do budowania HUDów będzie można użyć BVProjectEditora.

### Założenia:

- HUDy są poddrzewami zbudowanymi z normalnych nodów.
- Do każdego noda można podpiąć wiele HUDów jako dzieci. HUDy nie powinny być widoczne z poziomu zwykłych nodów (czyli np. logik tych nodów), w związku z czym powinny one zawierać flagę isHUD i być ukrywane przez funkcje do pobierania dzieci.
- Na szczycie poddrzewa stoi logika, która steruje całym HUDem. Ta logika powinna dziedziczyć po zwykłym interejsie logiki ale mieć dodatkowe funkcjonalności.
    - Powinna dostawać wskaźnik na noda i plugin/effect/logikę/inny element, który obsługuje.
    - Trzeba dodać jakieś dodatkowe funkcje hooki, które będą wywoływane w odpowiednich miejscach update'u obsługiwanego noda
    - Powinna sam tworzyć całe poddrzewo z HUDem. Oczywiście może to robić w kodzie, albo może sobie wczytać preset, w każdym razie stworzenie HUDu powinno wymagać jedynie stworzenia node'a, logiki i wywołania inicjalizacji tej logiki.
- W poddrzewie z HUDem mogą stać dowolne elementy (pluginy, logiki, effecty, jakie mogą być umieszczane w normalnej części poddrzewa. Ponieważ logiki używają BVProjectEditora do operowania na drzewie, to tylko root drzewa HUDów powinien mieć flagę isHUD, reszta drzewa już nie. Dzięki temu HUD nie jest widoczny z zewnątrz dla logik, ale wewnątrz wszystko działa bez żadnych modyfikacji w kodzie logik.
- HUDy są dodawane na żądanie Edytora i powinny być po zakończonej edycji usuwane. Wyobrażam sobie to tak, że edytor wskazuje plugin/effect/logikę, którą chce edytować, ona tworzy swój HUD (być może jeden z wielu), człowiek sobie edytuje co chce, a potem HUD jest usuwany. Na pewno HUDy nie powinny być automatycznie tworzone dla wszystkiego co jest w drzewie.
- Plugin/Efekt/Logika, która posiada swoje HUDy powinna móc decydować czy HUD dziedziczy tę samą transformację. Od tego zależeć będzie czy zostanie on wpięty w roota sceny i będzie zachowywał się niezależnie (np. będzie HUDem w przestrzeni ekranowej) czy zostanie wpięty w tego samego noda co Plugin/Efekt/Logika i odziedziczy transformację.
- Można pomyśleć o zrobieniu dodatkowych ułatwień w mapowaniu parametrów z modelu na HUDy. Np. mógłby być dodatkowy ParamValModel do trzymania parametrów, które zostaną użyte przez HUDy itp. To są raczej takie dodatkowe udogodnienia, o których można pomyśleć na późniejszym etapie.
- Nody HUDowe nie powinny się serializować w większości. Są jednak wyjątki. Np. GridLiny powinny być gdzieś zapisywane. Prawdopodobnie potrzebna będzie do tego jakaś flaga boolowska.
- Edytor zasadniczo nie powinien widzieć nodów z HUDami, ale czasami mogłoby to być przydatne w celach debugowych.

### Pytania do modelowej strony

- Jak obsługiwać HUDy do innych elementów niż Pluginy/Efekty/Logiki ? Np. GridLiny, światła, kamery nie są wpięte w żadnego noda, bo są w SceneModel. Oczywiście najchętniej bym przeniósł światła i kamery do drzewa sceny, bo uważam, że to jest lepsze rozwiązanie z wielu innych względów, ale z GridLinami nadal nie wiadomo, a pewnie w przyszłości może się pojawić więcej takich rzeczy.
- Do tej wersji chyba nie planowaliśmy jeszcze robić żadnego chwytania i przeciągania HUDów, ale trzeba przynajmniej się zastanowić czy to jest niesprzeczne i czy nas w przyszłości nie ograniczy.


## Implementcja silnikowa

### Założenia:

- Nody modelowe z HUDem są tłumaczone na nody silnikowe. To pozwala znowu wykorzystać jak najwięcej istniejących mechanizmów oraz renderować je przy użyciu tych samych funkcji.
- Nody HUDowe muszą być całkowicie niewidoczne dla efektów. Dlatego najlepiej będzie trzymać je jako doczepione do nodów HUDContainery. HUDContainer może po prostu zawierać vector wszystkich nodów z HUDami.
- HUDContainery powinny dostawać transformację z noda, do którego są przyczepione.
- BVProjectEditor przy tworzeniu noda potrafi odróżnić czy to jest zwykły node czy HUD i dzięki temu będzie mógł albo wpiąć go albo w drzewo, albo stworzyć HUDContainer. Tutaj nakład pracy jest chyba minimalny, bo modyfikacje będą w kilku funkcjach.
- Renderowanie odbywa się w dwóch przebiegach:
    - Wywoływana jest funkcja RenderDepth na efektach, która wpisuje jedynie z-tkę do bufora.
    - Całe drzewo jest renderowane jeszcze raz, ale tym razem są wyciągane jedynie HUDy.
- Trzeba stworzyć nowe funkcje pomocnicze do renderowania typu RenderDepth( nodes ), żeby wyrenderować z-tkę z całego poddrzewa itp.


### Pytania do silnikowej części

- Czy Efekt może chcieć przesunąć HUD? Np. jeżeli Efekt przesuwa swoje dzieci, to być może HUDy są nie w tym miejscu co trzeba. Ale oczywiście nie wiadomo z góry czy HUD musi być przesunięty czy nie. Do tego pewnie trzeba jakiś mechanizm zrobić ...
- Czy Efekt może chcieć nie renderować HUDu ??
- Czy przy renderowaniu z-tki nie będzie trzeba zrobić wielu kombinacji pixel shadera w zależności od wyjść z vertex shadera ?? Oczywiści w bieda wersji można renderować tymi shaderami, co zawsze, ale bez podpiętego bufora koloru. To jest o wiele wolniejsze rozwiązanie, ale na początek wystarczy. Kolejnym problemem by było, gdyby pixel shader discardował jakieś pixele. Wtedy podmiana shadera zmienia działanie...

## Use casy do przemyśleń na temat przyszłego mechanizmu

- HUDy używające wyników rednderowania efektów, np.:
    - Lupa
    - Node Maska preview (w tej chwili będzie robione jako parametr node maski, ale w przyszłości można by to potraktować jako HUD)