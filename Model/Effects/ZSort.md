Opis efektu
===========

Efekt z-sort pozwala na wyłączenie domyślnego trybu renderowania dla
poddrzewa i zamiast tego zastosowanie kolejności wynikającej z jego
struktury tzn.: - najpierw renderowany jest node rodzic - później
renderowane są node'y dzieci w kolejności w jakiej znajdują się w
drzewie

Domyślny tryb renderowania
==========================

Domyślnie renderer ustawia kolejność renderowania node'ów tak, żeby
poprawnie wyświetlać przezroczystość. Node'y z ustawioną
przezroczystością są renderowane w kolejności od najdalszych do
najbliższych. Node'y bez przezroczystości są renderowane w kolejności
odwrotnej. Na początku renderuje się wszystkie node'y bez
przezroczystości, w drugiej fazie z przezroczystością.

Sortowane są wszystkie node'y w poddrzewie, tzn. bierze się pod uwagę
nie tylko bezpośrednie dzieci, ale również wnuczki i tak dalej.

Kolejność renderowania jest ustalana na podstawie głębokości na jakiej
znajduje się dany node względem płaszczyzny ekranu. Do obliczania
głębokości używany jest środek ciężkości bounding boxa danego node'a.

Renderowanie efektów w poddrzewie
---------------------------------

Jeżeli w poddrzewie obsługiwanym przez z-sorta znajduje się jakiś efekt,
z-sort przy sortowaniu nie schodzi poniżej tego node'a. Node z efektem
zostanie wyrenderowany jako całość wraz ze swoim poddrzewem.

Dzieje się tak ze względu na to, że efekty mają własną logikę
renderowania, której nie można nadpisać logiką z-sorta.

Efekty mogą być odpytywane o z-tkę, z jaką powinny zostać wyrenderowane
i na tej podstawie są dodawane do kolejki renderowania. Z-tka może być
obliczana przez efekt lub w niektórych przypadkach jawnie nadpisywana
przez edytor.

Problemy z domyślnym trybem renderowania
----------------------------------------

**Nachodzenie obiektów po obrocie**

Problem z kolejnością renderowania, który jest opisany:
<https://documentation.vizrt.com/viz-artist-guide/3.6/z_sort.html#t_Transparency__Rendering_Order_Logic_and_Priority>

Jeżeli dwa node'y są ustawione jeden za drugim (po z-tce), ale zostaną
obrócone, że zaczną na siebie nachodzić, to środek ciężkości pozostanie
w tym samym miejscu i z się nie zmieni. Jednak kolejność renderowania
powinna się zmienić, ponieważ node'y zaczęły na siebie nachodzić i jeden
wystaje spod drugiego. Rozwiązaniem jest użycie z-sorta, który wyłącza
sortowanie po osi z i włącza renderowanie zgodne z kolejnością w
drzewie.

Takie rozwiązanie jest dobre jedynie wtedy, gdy scena jest statyczna.
Jeżeli chcemy obracać obiektami, to w ogóle nie mamy możliwości
poprawnego ustawienia kolejności, jeżeli sortowanie wynikające z
bounding boxów nie odpowiada temu czego oczekujemy.