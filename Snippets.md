#BV Snippets

## Dodawanie snippetów

Snippety pozwalają wstawić całe fragmenty kodu.

Aby dodać snippet należy wkleić plik z jego definicją do katalogu Visual Studio:

**C:\Program Files (x86)\Microsoft Visual Studio 11.0\VC\Snippets\1033\Visual C++**

Dla Visual Studio 2017 katalog ten znajduje się w:

**C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\Common7\IDE\VC\Snippets\1033\Visual C++**


## Używanie snippetów

Są dwa typy snippetów: **Expansion** i **SurroundsWith**.

1. Typ **Expansion** wstawia snippet w miejscu, w którym znajduje się kursor. Skrót klawiaturowy: ctrl + k + x

2. Typ **SurroundsWith** otacza zaznaczony fragment  kodu.Skrót klawiaturowy: ctrl + k + s


Po wstawieniu snippetu mozna uzupełnić zdefiniowane w nim pola (np. nazwę pluginu/logiki), które zostaną potem powielone, do wszystkich miejsc, w które należy je wstawić.


## Tworzenie snippetów

Snippety są po prostu plikami XML. Po dokładniejsze informacje jak tworzyć snippety, polecam zajrzeć do MSDNu oraz przejrzeć istniejące snippety. Przy tworzeniu snippetów warto brać pod uwagę kilka rzeczy:

1. Trzeba zawsze sprawdzić co się wygenerowuje ze snippetów. Snippet powinien zawsze generować dokładnie to, co chce się otrzymać inaczej jest bardzo denerwujące, jak po wstawieniu trzeba jeszcze robić jakieś zmiany.
2. W snippetach można definiowac literały, który zostaną potem wypełnione przez użytkownika. Dzięki temu użytkownik raz wpisze jedną nazwę, a zostanie ona wpisana we wszystkie miejsca, gdzie twórca snippeta ustali. Przykład znajduje się w NodeLogicHeader.snippet.
3. Można ustawić końcowa pozycję kursora przy pomocy literału $end$.

## Snippety BVowe

### Dekoratory

1. **decorate.snippet**

Wstawia komentarz nad funkcją:

```
#!c++

// ***********************
//
void    DefaultExtrudePlugin::AddSymetricalPlane      ( IndexedGeometry& mesh, glm::vec3 translate )
{}

```

2. **section.snippet**

Wstawia większy komentarz do rozdzielania wizualnego sekcji kodu:


```
#!c++

// ========================================================================= //
// SectionName
// ========================================================================= //

```

3. **bvnamespace.snippet**

Otacza zaznaczony kawałek kodu namespacem bv.

### Tworzenie logik

1. **NodeLogicHeader.snippet**
2. **NodeLogicCpp.snippet**

Wstawiają szablon logiki dla pliku nagłówkowego i pliku cpp. Po użyciu snippetu należy wpisać nazwę klasy logiki, która zostanie wklejona we wszystkie pola, w których jest to potrzebne.

### Tworzenie pluginów

1. **GeometryPluginHeader.snippet**
2. **GeometryPluginCpp.snippet**

Wstawiają szablon pliku nagłówkowego i .cpp dla pluginu geometrycznego generującego geometrię.

3. **ShaderPluginHeader.snippet**
4. **ShaderPluginCpp.snippet**

Wstawiają szablon pliku nagłówkowego i .cpp dla pluginu shaderowego (np. dla tekstur, gradientów).

### Tworzenie testów pisanych pod framework testowy

1. **FrameworkTest.snippet**

Tworzy klasę testową i formatki podstawowych funkcji.

### Tworzenie Logik Gizm

1. **GizmoLogicHeader.snippet**
2. **GizmoLogicCpp.snippet**

Tworzą szablon logiki siedzącej w korzeniu poddrzewa z gizmem