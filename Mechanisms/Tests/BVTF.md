[TOC]

# Black Vision Test Framework

Framework ten zaprojektowany jest w celu umożliwienia tworzenia testów automatycznych wymagających dostępu do stanu silnika, modelu oraz renderera.

Założenia projektowe znajdują się w dokumencie: [Black Vision Test Framework - Project](BVTF_Project)

## Tworzenie testów

### Struktura projektów

Aby dodać test, najlepiej stworzyć nowy projekt na podstawie szablonu FrameworkBasedTest. Projekt ten linkuje TestFramework, który przedefiniowuje logikę aplikacji. Do projektu należy dodać plik main.cpp o następującej zawartości:

```
#!c++


#include "Framework/bvAppTest.h"


// Registers initializers for BlackVisionApp instance and other usufull things like loggers etc.
bool bv::BlackVisionAppFramework::m_configOverrideInitialized = bv::BlackVisionAppFramework::OverrideConfig( "TestConfigs/TestFrameworkConfig.xml" );
bool bv::BlackVisionApp::m_sWindowedApplicationInitialized = bv::BlackVisionAppFramework::RegisterFrameworkInitializer();

#include "Framework/TestMain.h"
```

Funkcja OverrideConfig pozwala podać ścieżkę do configa, który zostanie użyty przy wykonywaniu testu. Jeżeli tej linijki nie będzie, zostanie użyty defaultowy config Test/Configs/DefaultConfig.xml.


### Dodawanie assetów

Przy kompilacji testów kopiowane są wszystkie assety z katalogu Test/Assets/. Normalnie assety lepiej trzymać w folderze bv_media. Tutaj można wrzucić np. screeny do porównywania z wyrenderowanymi rzeczami itp.


### Pisanie testów

Framework testowy używa gtesta jako wewnętrznego frameworku testowego. Aby dodać przypadek testowy trzeba zaimplementować klasę dziedziczącą po FrameworkTest.

Domyślnie FrameworkTest wykonuje jedną pętlę renderowania i pozwala ustawiać stan silnika i sprawdzać jego poprawność pomiędzy kolejnymi fazami tej pętli. Do zaimplementowania są metody, które są wywoływane w odpowiednich momentach pętli renderowania (domyśla implementacja tych metod jest pusta):

- **PreEvents**
- **PreModelUpdate**
- **PreRender**
- **PostRender**

Przykładowa implementacja może wyglądać tak:
```
#!c++


// ***********************
//
class TestTest : public bv::FrameworkTest
{
    DECALRE_GTEST_INFO_WITH_CONSTRUCTOR( TestTest )
private:
public:

    virtual void        PreEvents           () override;
};

REGISTER_FRAMEWORK_GTEST_INFO( TestTest, TestCaseName, TestTest )


// ***********************
//
void        TestTest::PreEvents     ()
{
    /// Test code
}

```

#### Prosty test

W większości przypadków nie ma znaczenia, w którym miejscu pętli głównej zostanie wywołany test, a ważne
jest jedynie, żeby mieć dostępny cały kontekst BVa. Wtedy można skorzystać z makra, które tworzy domyślny test:


```
#!c++

SIMPLE_FRAMEWORK_TEST_IN_SUITE( Model_Timelines, AddGotoKeyframe )
{
    // Test code here
}

```

W takim przypadku kod zostanie umieszczony w funkcji **PreEvents**. Mimo że używana jest tylko jedna funkcja, BV wykonuje całą pętlę główną.


#### Wiele obiegów pętli w jednym teście

Framework testowy może też uruchomić pętlę renderowania wielokrotnie. Przykładowa implementacja:


```
#!c++

// ***********************
//
void        TestTest::PreEvents     ()
{
    if( GetFrameNumber() == 0 )
    {
        PathVec paths;
        paths.push_back( Path( "witek/NodeMaskPreview.scn" ) );

        GetAppLogic()->LoadScenes( paths );

        EndTestAfterThisFrame( false );
    }

    if( GetFrameNumber() == 1 )
    {
        auto scenes = GetAppLogic()->GetBVProject()->GetScenes();

        ASSERT_TRUE( scenes.size() == 1 );
        EXPECT_TRUE( scenes.size() == 3 );
        ASSERT_TRUE( scenes.size() == 2 );
    }

    if( GetFrameNumber() >= 100 )
    {
        EndTestAfterThisFrame( true );
        GetAppLogic()->UnloadScenes();
    }
}

```

Aby test nie skończył się po pierwszej klatce, trzeba jawnie wywołać funkcję EndTestAfterThisFrame( false ). W momencie, gdy chcemy zakończyć test, musimy wywołać tę samą funkcję z parametrem false. Jeżeli tego nie zrobimy test będzie działał w nieskończoność.

FrameworkTest automatycznie zlicza liczbę klatek, którą można pobrać przy pomocy funkcji GetFrameNumber().

#### Konstruktor dla FrameworkTestu

Makro DECALRE_GTEST_INFO_WITH_CONSTRUCTOR definiuje od razu pusty defaultowy konstruktor. Jeżeli potrzebna jest inicjalizacja jakichś pól w klasie, można zdefiniować konstruktor samodzielnie. Do tego trzeba użyć makra DECALRE_GTEST_INFO zamiast DECALRE_GTEST_INFO_WITH_CONSTRUCTOR.

Nie ma możliwości definiowania konstruktora z argumentami, ponieważ gtest tworzy obiekt testowy konstruktorem domyślnym.

```
#!c++


// ***********************
//
class TestTest : public bv::FrameworkTest
{
    DECALRE_GTEST_INFO( TestTest )
private:

    int    Field;
public:

    TestTest()
        : Field( 1 )
    {}


    virtual void        PreEvents           () override;
};

REGISTER_FRAMEWORK_GTEST_INFO( TestTest, TestCaseName, TestTest )
```

#### Nadpisywanie czasu

Twórca testu może podawać własny czas wyliczany w dowolny sposób. Do tego trzeba wywołać funkcję UseOverridenTime( true ) i zaimplementować funkcję ComputeFrameTime.

#### Implementacja wielu przebiegów renderowania w jednej funkcji

Twórca testu może ręcznie wywoływać poszczególne fazy renderowania bez pomocy frameworku. Wystarczy zaimplementować funkcję RunImplNotConst.

## Uruchamianie testów

### Automatyczne uruchomianie wszystkich testów

Do uruchomienia wszystkich testów lokalnie na konsoli, można użyć pliku RunAllTests.bat. Uruchamiane są wszystkie testy w podkatalogach _Builds\x64-v110-Debug\Tests\. Przykładowe wywołanie:


```
#!c++

RunAllTests x64 Debug v140
```

Testy mogą zamiast logować wszystko na konsolę, wyprodukować wynik do wskazanego katalogu:



```
#!c++

RunAllTests x64 Debug v140 Reports/Tests/
```


### Uruchamianie pojedynczego testu

Pierwszą rzeczą jest ustawienie w opcjach Visual Studio Working directory na **$(OutDir)**. Do tego katalogu są kopiowane wszystkie potrzebne zasoby (tzn. shadery, assety oraz configi).