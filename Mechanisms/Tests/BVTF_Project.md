[TOC]

# Black Vision Test Framework - faza projektowania

Framework ten zaprojektowany jest w celu umożliwienia tworzenia testów automatycznych wymagających dostępu do stanu silnika, modelu oraz renderera.

## Wstępny opis funkcjonalny do dalszej obróbki.

_W szczególności po przeanalizowaniu i skomentowaniu tego dokumentu warto będzie się zastanowić nad strukturą projektu - mała uwaga, projekt podzielony na podprojekty łatwo zbić w jeden, duży, natomiast w drugą stornę to nie jest takie oczywiste_

**Spotkanie 2.3.2017**

BVTF powinien umożliwiać dostęp do w zasadzie całości stanu silnika. Później wraz z SDK można wydzielić cześć, która może być dostępna tylko dla programistów pluginów. 

Na pierwszym spotkaniu ustaliliśmy, że BVTF powinien umożliwiać przeciążenie następujących metod, które wołane będą per test. Programista piszący test powinien uzupełnić ciało tych funkcji zgodnie z tym jak zaprojektował swój scenariusz testowy.

> ## Draft proposal
> 
> _Poniższe API jest pierwszym podejściem do ogólnego frameworku testowego. Bardzo prosimy o uwagi i komentarze. Plan działania jest taki, że próbujemy sobie przygotować kilka scenariuszy testowych z możliwie różnych części aplikacji / silnika / modelu i na bazie zebranych doświadczeń modyfikujemy propozycję tego API oraz przygotowujemy specyfikację kontrolera, który odpowiada za odpalanie testów (testy pewnie powinny komunikować się z kontrolerem). Dodatkowo framework testowy powinien pozwalać na przeprowadzenie testów zarówno w programie z pętlą główną, jak i w normalnym, liniowym mainie._
>
> * **PreTest**  np. ustawienie czasu silnikowego dla jakiego wykona się test. 
> + **Test**  zmiana struktury sceny po stronie modelu, silnika lub gdziekolwiek 
> + **PostTest**  dodatkowa hook pozwalający ja wykonanie kodu zaraz po teście. _Nie wiemy jeszcze co pomiędzy tymi funkcjami może się wydarzyć_
> + **Verify**  wywołana po updacie drzewa i renderowaniu. _Sprawdzamy aktualny stan. Weryfikujemy poprawność wykonanych modyfikacji._
> + **Finalize**  np. resetowanie stany silnika, wypisanie danych na konsolę albo do pliku. 
> + **IsIdle** Zwraca booloską flagę czy test powinien zostać wykonany. 
> - **IsFinished** Zwraca boolowską flagą czy test został już wykonany. BVTF powinien wywoływać tyle razy funkcję. Test dopóki IsFinished zwraca false. _Powodować to może zapętlenie. Należy tutaj ustawić jakieś sztywne ograniczenie_


> ##Draft proposal 2
> Z tych przypadków testowych, co do tej pory wypisałem, wynika, że potrzebne są funkcje w następujących miejscach
>
> * Przed updatem EventManagera (wynika z: **Test API**)
> * Po updacie EventManagera (ale przed zinkrementowaniem UpdateCountera) (wynika z: **Test updatowania assetów**)
> * Przed updatem modelu (ale po zinkrementowaniu UpdateCountera) (wynika z: **Test updatowania assetów**)
> * Przed wywołaniem renderowania, po updacie modelu (trzeba się zastanowić czy nie trzeba rozdzielić NodeUpdaterów od updatu modelu poprzez wstawienie między nie jakiejś funkcji) (wynika z: **Test parametrów w pluginach**)
> * Po wyrenderowaniu sceny (wynika z: **Test wizualne renderowania**)
>
> Słowo dopowiedzenia: Potrzeba rozdzielenia wywołań UpdateCountera wynika głównie z **Testu updatowania assetów**. Bug który jest tam testowany pokazuje, że bardzo istotne jest, między którymi wywołaniami zostaną wstawione funkcje testujące.
>
> Żeby nie umknęło: Trzeba się zastanowić czy potrzebne są testy porównawcze pośrednich wyników renderowania przez global effecty. Oznaczało by to konieczność odczytu ze stosu render targetów!!

### Po co testy?
1. Wykrywanie regresji.
2. Utrzymanie stabilności kodu implementacji ( pokrycie przypadków brzegowych )
3. Określają wymagania i częściowo dokumentują kod.

### Co musimy testować?
1. "Małe" algorytmy dobrze odseparowane od reszty (MMBuilder, serializacja, TCPServer) (UT)
2. Testy modelu (Po wydzieleniu silnika i modele powstanie możliwość implementacji czegoś w rodzaju MockEngine do testowania modelu lub do innych zastosowań) (UT + BVTF)
    1. Parametrów + wygenerowana reprezentacja w OGL.
    2. Logika parametrów, pluginów, efektów ...
3. BVModelEditor ( styk wszystkich elementów ) (BVTF? lub UT po wydzieleniu mockowego modelu.)
4. Testowanie Event API ( w połączniu z BVProjectEditor ) (UT jeśli wydzielony model lub BVTF jeśli nie)
5. Unit Testy NodeUpdater (UT + BVTF)
6. Testy Renderera. (przydatny MockModel do testowania silnika BVTF)
7. Porównywanie wyrenderowanych danych referencyjnymi. (BVTF)
8. Testy zgodności pomiędzy wersjami (scen, parametrów, itp.) (BVTF + UT)
9. Testy wydajnościowe (mechanizm do zbierania statystyk.)
10. Testy sekwencji połączeń pluginów, efektów, logik. (BVTF + UT)


## Przykładowe testy

### Test BVProjectEditora

Ten test jest już już zaimplementowany w TestBVProjectEditor plik TestMoveNode. Nie działa, bo BVProject wymaga stworzenia renderera, a on wymaga stworzenia okna aplikacji. W innym wypadku mógłby to być zwykły gtest. W pierwszej kolejności powinien pozwalać na przetestowanie wszyskich operacji, które daje się wykonać na drzewie sceny modelowej, w oderwaniu od renderera.


**Scenariusz:**

* Inicjalizacja dwóch różnych scen, stworzenie prostej struktury nodów. Najlepiej by było mieć jakiś zestaw funkcji, umożliwiających wybór spośród kilku domyślnych struktur, żeby nie trzeba było się napracować. Najlepiej, żeby te funkcje wyryfikowały od razu poprawność stworzonych scenek żeby potem użytkownik nie musiał nic robić.
* Przepinanie nodów.
* Sprawdzenie struktury nodów (ewentualnie dodatkowo struktury nodów silnikowych)

**Wymagany dostęp:** BVProjectEditor, sceny modelowe i silnikowe


### Test updatowania assetów

**Przykładowy bug:** https://www.pivotaltracker.com/story/show/135479613

W tym bugu wszystkie eventy z edytora dochodziły i były wykonywane w tej samej klatce silnika, co powodowało problemy. Błąd nie występował, jeżeli eventy były rozdzielone na wiele klatek.
Ten bug można replikować na poziomie API silnika, ale wtedy ma się mniejszą kontrolę nad tym czy zostaną one wykonane w jednej klatce, dlatego robię opis z poziomu _BVProjectEditora_.

**Scenariusz**

* Inicjalizacja sceny jedynie z rootem.
* Wywołanie zestawu operacji poniżej. **Te operacje powinny zostać wywołane mniej więcej w tym miejscu, w którym jest robiony update EventManagera.** Szczególnie ważne jest, żeby mechanizm update countera z _ApplicationContext_ zadziałał tak samo, jak w realnym wykonaniu silnika.
    * Dodanie noda
    * Dodanie pluginu rectangle
    * Dodanie pluginu texture
    * Dodanie assetu
* Update modelu
* Update NodeUpaterów
* Sprawdzenie stanu silnika. Trzeba sprawdzić czy node silnikowy zawiera domyślną teksturę czy wczytaną.

**Wymagany dostęp:** _BVProjectEditor_, sceny silnikowe (dokładniej dane tekstur w strukturze shaderów)


### Test parametrów w pluginach

Ten test miałby na celu sprawdzenie czy plugin się nie wywala, jeżeli poda mu się nieakceptowalne parametry.

**Scenariusz**

* Inicjalizacja sceny z rootem.
* Wstawienie testowanego pluginu i jakiegoś kontekstu dla niego (innych pluginów)
* Testowanie parametrów. Wykonujemy wielokrotnie dla każdego zestawu parametrów:
    * Ustawienie parametrów
    * Update modelu
    * Tutaj można ewentualnie sprawdzić np. jakie są ConnectedComponenty itp.
* Zmiana kontekstu pluginu i ponowne przetestowanie wszystkich zestawów parametrów


**Wymagany dostęp:** BVProjectEditor, bezpośredni dostęp do pluginu

**Uwagi:** Przyjemnie by było gdyby taki test różnych parametrów odbywał się automatycznie (tzn. żebyśmy napisali jakiś jeden mechanizm co by automatycznie to wykonywał dla wszystkich pluginów, żeby twórca pluginu mógłby się skupić na bardziej konkretnych testach. Problemem mogą być parametry takie jak tesselacja, bo one się długo wykonują.


### Test API

Ponieważ handlery zawierają nie tylko wywołania BVProjectEditora, ale również troszkę kodu, który coś robi,
to czasami będzie trzeba je testować osobno.

**Scenariusz**

* Inicjalizacja jakiejś sceny
* Dodanie eventów do kolejki **(to powinno zostać wywołane przed updatem EventManagera)**
* UpdateModelu
* Porównanie stanu silnika/modelu z oczekiwanym
* Można dodać kolejne przebiegi silnika z innymi eventami


### Test audio.

Testy audio moim zdaniem mogą sprowadzić się tylko i wyłącznie do próby wczytywania różnych plików w różnych formatach. W tym również plików błędnych.

**Przykładowe testy:**

* Dodanie strumienia lub strumieni audio.
* Losowe (*ale deterministyczne*) startowanie stopowanie i pauzowanie wybranych strumieni.
* Odpalenie kilkunastu strumienie w celu sprawdzenia wydajności. 
* Sprawdzenie czy nie ma dropowania ramek. (I tutaj widać, że przydałby się też mechanizm sprawdzający ile ramek wyciąga silnik a ile minimalnie dla testu powonień.)

### Test video.

Tutaj podobnie jak dla audio i dodatkowo:

**Przykładowe testy:**

* Sprawdzenie i porównanie obrazu z referencyją wyciągniętą wcześnie z materiał klatką.
* Sprawdzenie prędkości odtwarzania wielu strumieni video. 
* Test sprawdzający synchronizację pomiędzy odtwarzaną ramką, a ramką w odpowiednim timestamp pobraną  bezpośrednio z materiału video. Na przykład ramką kluczową. 

### Test wizualne renderowania

**Scenariusz**

* Wczytujemy scenę
* **Ustawiamy czas na timelinach** to jest kluczowe, inaczej porównanie wyrenderowanych obrazów będzie zawsze dawało niepoprawne wyniki
* Renderujemy obiekty
* Robimy readback rendertargetu
* Porównujemy z obrazem referencyjnym

**Uwagi**

1. Trzeba się zastanowić czy oprócz sprawdzania ostatecznego wyniku w głównym rendertargecie nie chcemy też sprawdzać wyników pośrednich wygenerowanych przez efekty. To by było całkiem trudne, bo te rendertargety są przechowywane stosowo i mogą zostać nadpisane przez inne efekty. Być może trzeba rozważyć jakiś mechanizm do hookowania w logice efektu. Trzeba przemyśleć bo to trudne zagadnienie, może przesadzam i to się nie przyda.

2. Przydałoby się napisać jakiś mechanizm porównywania, któremu by się podawało obraz wyrenderowany i referencyjny, i żeby to narzędzie dokonywało porównania z jakimiś parametrami (np. można podać prostokąt, który ma zostać porównany, albo jakąś metrykę porównawczą, żeby nie były wymagane idealnie takie same piksele.

3. Warto by stworzyć jakiś mechanizm, który by tym samym kodem generował obrazy referencyjne.


**Wymagany dostęp:** rendertargety, outputy, **potrzebny jest hook wywoływany po renderowaniu**


### Testy efektów

**Uwagi** Pytanie jak testować efekty? Czy efekt wizualny jest wszystkim, co opisuje działanie efektów ? Może można by przechwytywać wywołania renderera (RenderLogica) i jakoś porównywać z oczekiwanymi ??


### Testy renderera

TODO: Jak do tego podejść? Co jest wynikiem i z czym jest porównywane ?


### Testy poprawności plików ze scenami

To pewnie można by po prostu zrobić jako gtest, ale podejrzewam, że bez renderera nie da się wczytać sceny.
Idea jest taka, żeby wygenerować nie do końca poprawne xmle i po prostu jest próbować wczytać (oczywiście błędy w xmlu mają polegać na niepoprawnej zawartości, a nie na tym, że się nie da sparsować. Trzeba wziąć wszystkie symbole jakie używamy w xmlach i powpisywać w niepoprawnych miejscach np. Widziałem taki pomysł, że ktoś zapuszczał jakiś algorytm ewolucyjny i robił mutacje na xmlach, żeby testować jakieś api webowe).

**Scenariusz**

* Startujemy z pustym silnikiem
* Wczytujemy scenę
* Jeżeli nic się nie wywaliło to jest ok
* Czyścimy stan silnika, kasujemy scenę
* Tutaj można zrobić jakiś test sprawdzający czy silnik jest rzeczywiście czysty
* Ładujemy kolejne sceny

**Uwagi**

1. Potrzebny jest mechanizm do testowania czy silnik jest "czysty". Oczywiście musimy zdefiniować co do znaczy (co np. z cachem itp.). Może warto taki mechanizm udostępnić użytkownikowi, żeby mógł sobie wywołać (przemyśleć).