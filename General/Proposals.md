#Lista propozycji i różnych przemyśleń odnośnie featurów w BVle.


1. **Uwagi odnośnie obecnej wersji**
    * Trzeba pomyśleć o wygodzie użytkowania. Przykładem jest logika cloner, którą Paweł sobie ostatnio zażyczył. Zmiana parametrów musi powodować natychmiastową zmianę ułożenia klonowanych nodów. Nie może być tak, że najpierw się ustawia parametry, a potem się je akceptuje i dopiero wtedy widać efekt (tak jak jest teraz w Replikatorze). Musimy przejrzeć wszystkie funkcjonalności i zidentyfikować miejsca, które mogą być niewygodne dla użytkownika. Oczywiście osobną sprawą jest wygoda użytkowania edytora, ale to już nie nasz działka.
    * Płynne zmiana geometrii przy edycji. Pluginy geometryczne zawsze generują geometrię od nowa, co jest długim procesem i czasem zdarzają się przycięcia silnika. Są jednak takie parametry, które nie wymagają całkowitej przebudowy (np. jakieś width, height itp). Inne programy graficzne potrafią sobie z nimi radzić płynniej. Może nie jest to kluczowa sprawa, ale może odrobinę zmniejszyć wygodę użytkową.
    * Zwalczenie lagów. Np. użytkownik wciska w edytorze zakładkę z assetami i po widzialnym czasie updatuje mu się widok. To jest troszkę wkurzające, jak bym był użytkownikiem to bym się obraził. Podejrzewam, że nie jest to kwestia edytora, ale np. połączenia TCP. Warto by troszkę powalczyć o większą responsywność albo przynajmniej jakoś to bardziej estetycznie zamaskować edytorem.


2. **Kierunki rozwoju BVa** - wysokopoziomowe przemyślenia w jakim kierunku powinny iść funkcjonalności w BVle

    * Bardzo skomplikowane przetwarzanie geoemtrii nie ma za dużego sensu. I tak nie będziemy w tym lepsi niż programy do modelowania, a robienie takich rzeczy w BVle nie będzie wygodne. Lepiej się skupiać na niewielkich pluginach do geometrii, które produkują dość proste kształty, ale z dużą siłą wyrazu. Do tego warto mieć kilka efektów jak np. Extrude, które potrafią to przetworzyć w coś ciekawego. Myślę, że można by założyć, że taki pipeline do przetwarzania powinien się składać max z kilku elementów - czyli prosty kształt i do tego 1-2
    * Statyczną geometrią konkurować nie możemy, ale gdyby dało się wypracować taki model, który pozwalałby na dynamiczne przetwarzanie dużych ilości geometrii w czasie rzeczywistym, to może to by miało jakiś sens.
    * Skoro nie możemy konkurować z programami do modelowania, to warto zapewnić, żebyśmy mogli importować gotowe zewnętrzne sceny. I nie chodzi tu tylko o wczytanie mesha, ale całej sceny razem z animacjami, kluczami, światłami, kamerami i wszystkim co się tam da znaleźć. Po zaimportowaniu takiej sceny użytkownik nie powinien być zmuszony do robienia poprawek czy coś, bo to jest strata jego czasu, który już włożył w pracę w zewnętrznym programie. Myślę że ta zasada się tyczy wszystkich assetów, że musimy zapewnić jak najlepszą konwersje do naszej struktury drzewa bez utraty informacji. Poza tym trzeba zidentyfikować formaty, które potencjalny użytkownik mógłby chcieć wczytać.
    * Przemyśleć narzędzia offline do przetwarzania/analizy danych wczytywanych z zewnętrznych źródeł (tutaj sceny)
    * Zwiększenie wygody użytkowania i szybkości pracy. Chodzi nie tylko o szybkość pracy z GUI, ale również o odpowiednią konstrukcję funkcjonalności. Powinniśmy projektować takie pluginy, żeby oczekiwane efekty osiągnąć jak najmniejszym kosztem. Taką przybliżoną wskazówką może być np. liczba nodów jakie trzeba wstawić, żeby coś osiągnąć. Powinniśmy dążyć, żeby było ich jak najmniej. Najlepiej by było, gdybyśmy mogli poobserwować jak pracują graficy, a potem spróbować na tej podstawie wymyślać nowe funkcjonalności, które mogłyby ten proces skrócić.
Może warto też pomyśleć o jakiejś bazie presetów, które byśmy dostarczali od razu z silnikiem.
    * BV powinien iść w stronę rozbudowania shaderów i efektów na nich. Np. jeżeli się spojrzy na scenkę brain/zegarek/zegarek_kolko.scn, to wiele gradientów, odblasków i podobnych rzeczy zostało zrobionych na teksturach. Powinniśmy tworzyć takie shadery, żeby zminimalizować konieczność używania tekstur, przez co zmniejszymy pracochłonność tworzenia scen.
    * Prezentacja danych. Obecnie mamy jedynie PieChart, nieskończony LineChart. Poza tym wszelkie słupki do wyswietlania wartości Paweł robi używając logiki Autofollow i składając pracowicie nody. Powinno być tak, że BVowi się dostarcza się danych i wszystko już jest gotowe. Ewentualnie użytkownik mógłby sobie coś tam customizować. No i trzeba oczywiście zwiększyć rożnorodność możliwości prezentacji danych.


3. **Przemyślenia przyszłościowe na temat obecnie istniejących rzeczy:**

    * Światła i kamery powinny być w drzewie. Chodzi głównie o dziedziczenie transformacji.
    * Ustawianie aktualnie wybranej kamery powinno się dać robić na kluczach. (czyli użytkownik może ustawić sobie taki timeline, przy pomocy którego będzie zmieniał, która kamera w danym momencie jest używana do wyświetlania).
    * Dodanie bufora indeksów. Większość pluginów geometrycznych używa list trójkątów i przez to jest ok. 3 razy za dużo zużycie pamięci.
    * Wczytywanie z FBXów kluczy animacji (animacja nodów, pewnie głównie ich transformacji, ale może da się również jakieś inne parametry powyciągać, np. klucze do parametrów materiałów), można również wczytywać światła oraz kamery	
    * Dodanie do mechanizmu gizm możliwości obsługi myszki. Mechanizm mógłby wyglądać tak, że w danym momencie tylko jedno gizmo dostaje eventy od myszki. Edytor mógłby wybierać, które gizmo to będzie. Trzeba by dodatkowo przepisać logikę klikania nodów, żeby też była jakimś globalnym gizmem i domyślnie dostawał input, jeżeli nikt inny nie dostaje.
    * Renderowanie przez kolejkę renderowania. Punktem wyjścia mogłaby być ta implementacja:
		https://blog.molecular-matters.com/2014/11/06/stateless-layered-multi-threaded-rendering-part-1/
		Logika renderowania jedynie wstawia komendy do kolejki. Opisują one w pełni stan renderera i zawierają wszystkie potrzebne dane.
		Każda komenda ma dołączony do siebie klucz, który służy do sortowania. Komendy są wywoływane po wypełnieniu i posortowaniu kolejki.
		Renderowanie przez kolejkę ma wiele zalet:
        * Sortowanie po kluczu umożliwia minimalizowanie zmian stanów GLa lub posortowanie obiektów po z-tce. Nie jest też sprzeczne z renderowaniem zgodnym z kolejnością w drzewie, bo da się tym sterować budując odpowiednio klucze.
        * Funkcje do generowania klucza można łatwo zmienić. Co więcej można by ją nawet zmieniać w trakcie wykonania albo mieć zestaw różnych funkcji pod różne karty graficzne i optymalizować wszystko pod nie. Generalnie dużo możliwość o wiele mniejszym kosztem.
        * Lepsza separacja renderera od reszty silnika i separacja API graficznego. Ponieważ wywołujemy całe zbiory komend, to można optymalizować ich wykonanie pod konkretnym API graficznym.
        * Wielowątkowość. W kolejce jest zapisany cały stan, więc można jednocześnie wysyłać komendy do GLa i robić kolejny update modelu.
    * Nowe typy parametrów:
        * Czasem pojawia się potrzeba stworzenia parametru, który ma tylko klucz w zerze (np. w logikach). To byłby taki Values, ale powinien mieć api dostępowe jak Parametr, żeby Edytor mógł go ustawiać. Oczywiście w tej chwili łatwo to jest obejść, robiąc parametr, a edytor wie, że modyfikuje tylko klucz zerowy, ale może warto by to jakoś semantycznie rozdzielić.
        * Parametry do odczytu. Chodzi o to, żeby pluginy mogły wpisać jakąś wartość do takiego parametru, którą edytor mógłby odczytać. Ten mechanizm dawałby dużo, jeżeli mięlibyśmy zaimplementowanego ParamSourca. Przykładowym use casem jest https://www.pivotaltracker.com/story/show/135202197. Poza tym może warto by było, gdyby bounding box dał się odczytać w ten sposób i żeby można go było podpiąć.


4. **Nowy model:**

    * Składanie shaderów !! - całkowita podstawa, bez tego nie napiszemy więcej pluginów shaderowych bo liczba kombinacji już jest duża.
    * Nowy model dla geometrii tworzyć z myślą o łatwiejszym dynamicznym przetwarzaniu. W tej chwili jest tak, że w zasadzie zmienianie parametrów modyfikujących geometrię nie nadaje się do czasu rzeczywistego. Podobnie jest przy wstawieniu pluginu, który zajmuje się przetwarzaniem geometrii. To bardzo ograniczy nas w przyszłości, bo z jednej strony chcielibyśmy tworzyć pluginy o jak najmniejszej funkcjonalności, takiej minimalnej, żeby potem z nich składać wieksze rzeczy, ale z drugiej, jeżeli chcemy, żeby coś działało szybko, to będziemy musieli robić uber-pluginy łączące wiele funkcjonalności. Troszkę widać już problemy na przykładzie tekstu 3D. Najlepszą formą rozdzielenia jest to co teraz, czyli:
        * Plugin Text3D produkujący kontury
        * Plugin Triangulate robiący trójkąty (czyli samą powierzchnię tekstu)
        * Plugin Extrude, który bierze powierzchnię i robi z niej obiekt 3D
			To rozdzielenie jest bardzo eleganckie, ale tekst działa wolno i pewnie dużo by się dało zrobić, gdyby plugin text3D od razu produkował geometrię 3D (albo dawał wybór co dać na wyjściu)
    * W nowym modelu trzeba jakoś zręcznie oddzielić zabawę z deksryptorami kanałów od samego przetwarzania.
			To jest element, który sprawia, że kode do przetwarzania jest brzydki, bo mieszamy generowanie, z przekształcaniem
			deskryptorów. Najgorszy przypadek jest wtedy, gdy plugin potrafi przyjąć wejścia rożnego typu i od tego zależy, jak wygląda
			proces generacji.
			Być może przyjemnie by było, gdyby użytkownik definiował mapowanie deskryptorów wejściowych na wyjściowe i jakiś mechanizm
			wywoływał by wskazaną przez niego funkcję. Tutaj jest wiele do przemyślenia.
* Pozbycie się generatorów geometri (GeometryPluginBase), bardzo niewygodne przekazywanie parametrów z pluginu
			do tych generatorów - mocno przedłuża tworzenie pluginu. Zamiast tego trzeba zrobić jakąś nową klasę nazową dla pluginów
			geometrycznych, która będzie o wiele bardziej upraszczała tworzenie geoemtrii.
    * Dodatkowy sposób określania semantyki kanałów geometrycznych. W planach mamy semantykę oznaczającą pozycje, UVki, normalne lub zdefiniowane przez użytkownika.
	        Warto by wprowadzić zbiór constraintów jakie spełnia zawartość kanału geometrycznego. Przykładowo plugin triangulate wymaga, żeby dostać na wejściu
			zbiór konturów mających topologię LINES i będących w płaszczyźnie XY. Dodatkowo przydatną informacją jest to, czy kontury mają prawidłową orientację
			(tzn. np. kontur zagnieżdżony, który jest dziurą w zewnętrznym konturze, powinien być zorientowany przeciwnie, niż ten zewnętrzny kontur).
			Taki zbiór warunków możnaby opisać jakimś zestawem stałych np. CONTOURS, PLANE_XY, ORIENTED. Każdy plugin mógłby deklarować zbiór constraintów,
			które spełnia jego wyjście.
			Plugin Triangulate mógłby deklarować jakich constraintów wymaga na wejściu, a jakie są mile widziane. Na tej podstawie można odrzucić niektóre
			połączenia pluginów. Ponadto w przypadku spełniania constraintów opcjonalnych, Trianagulate mógłby np. pominąć jakiś krok przetwarzania
			wstępnego (czyli w tym przypadku sprawdzenie czy orientacja konturów jest poprawna i jej naprawienie), jeżeli wcześniejszy plugin deklaruje
			spełnianie danego warunku.
        * Constrainty powinny dać się rejestrować przez przyszłych twórców pluginów.
        * Przy rejestracji constraintu trzebaby podać funkcję testującą czy constraint jest spełniony. Dzięki temu moglibyśmy sprawdzać offlinowo
			w trakcie testów czy warunki są spełnione i dzięki temu wymusilibyśmy, żeby constrainty nie były jedynie pustą deklaracją pluginów.


5. **Problemy, które może warto rozwiązać a chyba się nie da ani w obecnym ani w planowanym modelu:**

    * MeshPlugin powiela geometrię. Jest ona trzymana po pierwsze w assecie, potem jest przekształcona przez plugin i tworzony jest z niej vertex attributes channel. Nawet jeżeli kilka pluginów wczyta ten sam mesh, to i tak każdy będzie miał osobny kanałów geometryczny z osobnymi danymi. Można by sprawić, żeby MeshAsset zachowywał się troszkę analogicznie do TextureAssetu, gdzie pluginy przechowują wskaźnik na chunk z danymi i jest do tego specjalny kanał.
    * Globalna geometria (??). Jeżeli użytkownik stworzy wiele pluginów tworzących sferę z tymi samymi parametrami, to będziemy mięli dużo nadmiarowej geoemtrii. Z dużym prawdopodobieństwem parametry sfery nie będą modyfikowane, więc równie dobrze można by mieć tylko jeden bufor wierzchołków współdzielony przez wiele pluginów. To można jakoś powiązać z rozwiązaniem problemu powielania geometrii przez MeshPlugin. Wtedy użykownik mógłby zdecydować, żeby wygenerować sferę do MeshAssetu i po prostu ją wczytać zamiast wstawiać SpherePluginy.
    * Globalne tekstury. Przydałoby, żeby pluginy mogły wpinać tekstury wyrenderowane przez silnik. To może się przydać do robienia environment reflection, renderowania masek (do wielokrotnego użytku w wielu miejscach), może shadow mappingu i pewnie wielu innych rzeczy. Obecnie niektóre z tych rzeczy dałyby się zrobić global effectami, ale pytanie czy wszystko da sie tak zrobić i czy będzie to eleganckie rozwiązanie. Paweł zgłaszał propozycję częściowo pokrywającą się z moją ideą w tasku https://www.pivotaltracker.com/story/show/122125603.



6. **Całkowicie nowe funkcjonalności:**

    * Physically Based Rendering. W Vizie jak widziałem jest to raczej rozbite po rożnych pluginach (MetalReflection, Microstructure) i nie zostało jawnie tak nazwane. Nie wygląda też, żeby były tam stosowane realnie fizyczne parametry, ale efekty są podobne do PBR.
    * Deferred shading - troszkę jest to powiązane z PBR, ponieważ tam są kosztowne funkcje i trzeba gdzieś zyskać na czasie. Tu jest problem jak to w ogóle można wpisać do modelu, bo takie renderowanie bardzo usztywnia pipeline.
    * High dynamic range. HDR sprawia, że wszystko wygląda bardziej realistycznie. Dodatkowo jest troszkę powiązane z PBR, bo tam mogą wychodzić wartości z zakresów poza 0:255, jeżeli się operuje na jednostkach fizycznych. Do tego niestety potrzeba chyba, żeby cały pipeline akceptował operowanie na teksturach floatowych o większym zakresie, a dopiero ostatni etap robi mapowanie tonów. Ale może dało by się to jakoś inaczej załatwić w naszym modelu z efektami, żeby np. tylko fragment poddrzewa był renderowany w HDR.
    * Cienie. Trzeba przemyśleć jak do tego podejść w ogóle w naszym modelu.
    * Animacja szkieletowa - z jakiś względów Paweł wspominał tylko o animowanych .obj-tach, które mają zapisane tak jakby klatki geometrii i się między nimi interpoluje. To jest jakiś straszny brute force. Trzeba wybadać, jak się to robi obecnie.
    * Multisampling. Już nawet dodanie kilku próbek na piksel więcej, znacznie zwiększa jakość (szczególnie przy ruchomych rzeczach). Niestety oznacza to, że wszystkie pośrednie render targety też muszą wspierać multisampling.
    * Można pomyśleć nad Ctrl-Z, Ctrl-V z innych aplikacji. To mogłoby przyspieszyć użytkowanie, np. ktoś robi teksturę w photoshopie i chce podejrzeć jak wygląda w kontekście, więc przekleja ją do BVa. To byłoby szybsze, niż zapisanie na dysku, przeładowanie przez edytor, ale trzeba rozważyć czy zyski są warte zachodu. Tak samo możnaby z meshami itp.
    * Operacje boolowskie. W kontekście przetwarzania geometrii warto się zastanowić czy by nie pozwalały na robienie ciekawych rzeczy ze stosunkowo prostych kształtów. Wiele silników używa tak tworzonej geometrii (np. do prototypowania poziomów itp.) Trzeba się zastanowić czy nie byłoby z tego pożytku w BVle.




0. ** Uwagi z punktu widzenia usera dot. wersji production **

    * Przydałoby się asynchroniczne wczytywanie assetów

    * brakuje możliwości włączenia / wyłączenia działania efektu

    * Wysłanie wielu (mam na myśli bardzo wielu komend) skutkuje zapchaniem konsoli na czas koło 3.00 min (gdzie silnik radzi sobie z komendami w czasie +/- 0.03 sekund

    * **Bugi - te rzeczy w zasadzie powinny trafić do pivotala oczywiście z bogatym opisem replikacji :)**

        * Pojawiają się problemy z działaniem pluginu soft mask ( czasami żeby wymusić renderowanie tego so siedzi w środku soft maski trzeba do poprzednika nodea z tym efektem dodać efekt alpha mask i ustawić w nim alphę najpierw na 0, później na 1, a na końcu na 0.99 - zachowanie niedeterministyczne)

        * Jeżeli wczytanych jest wiele scen, zapisanie jednej z nich skutkuje błędem w renderowaniu - trzeba zrobić unload all i wczytać pozostałe sceny jeszcze raz

        * Node mask - przy większej liczbie nodeów i załadowanych scen coś się gubi mechanizm mask i pojawiają się maski które powinny być niewidoczne, mam repro w scenie do MMA ale jest bardzo skomplikowane (12 załadowanych scen) i przy próbie minimalizacji drzewka przestaje występować

        * wysłanie jeden po drugim eventów (1. wczytanie obrazka i 2. play na dowolnym timelinie) skutkuje czasami nie wywołaniem się drugiego timeline-a (hackowałem to w sposób Send(event1);Sleep(500);Send(event2);)

        * bardzo często na konsole wypisują się komunikaty że nie istnieje shader o nazwie .default

        * parametry efektów tworzą się domyślnie ze złymi nazwami timelineów

        * czasami psuje się plugin transform **Edit (nieznanysprawiciel): być może jest to naprawione w aktualnym masterze ...**

        * czasami dodanie logiki MAX_SIZE powoduje crash BV-a

        * należałoby domerdżować działajace blend_modes **Edit (nieznanysprawiciel): (blend mody czekają na twoją akceptację)**

    * **(nieznanysprawiciel)** Jaką mamy procedurę mergowania mastera do production ??

0. ** Uwagi użytkowe, na przyszłość**
    * Warto zrobić jakiś system do wyszukiwania pluginów po nazwie, jak już będziemy mieli ich dużo. Jak się włączy viza i próbuje znaleźć plugin, to  jest to trudne.