# Budowanie BVa

## Kompilowanie boosta

Ściągnij i rozpakuj boosta. Wejdź do jego katalogu i odpal konsolę.

```
#!c++

bootstrap.bat --with-libraries=thread,filesystem,serialization,log
```


### Wersja 32-bitowa

#### Debug
```
#!c++

b2 variant=debug --stagedir=stage/debug32 -j4 link=static --layout=system --with-thread --with-filesystem --with-serialization --with-log toolset=msvc-14.0
```

#### Release

```
#!c++

b2 variant=release --stagedir=stage/release32 -j4 link=static --layout=system --with-thread --with-filesystem --with-serialization --with-log toolset=msvc-14.0
```


### Wersja 64-bitowa

#### Debug
```
#!c++

b2 variant=debug --stagedir=stage/debug64 -j4 link=static --layout=system --with-thread --with-filesystem --with-serialization --with-log address-model=64 toolset=msvc-14.0
```

#### Release

```
#!c++

b2 variant=release --stagedir=stage/release64 -j4 link=static --layout=system --with-thread --with-filesystem --with-serialization --with-log address-model=64 toolset=msvc-14.0
```

## Konfigurowanie boosta

Ścieżki do boosta muszą zostać ustawione w zmiennych środowiskowych:

- BOOST_INC_1_61 - powinna wskazywać na katalog **boost_1_61_0\**
- BOOST_LIB_1_61 - powinna wskazywać na folder zawierający biblioteki

W podfolderach powinny znaleźć się katalogi z libkami skopiowanymi z: **stage/***

- msvc-14-x64-debug
- msvc-14-x64-release
- msvc-14-Win32-debug
- msvc-14-Win32-debug

## Uruchamianie BVa

Working directory projektów kompilujących się do execów powinien być ustawiony na $(OutDir). (Uwaga Visual Studio domyślnie wpisuje tam $(ProjectDir). Trzeba to zmienić dla wszystkich konfiguracji projektu).

Do katalogu z .exe trzeba przekopiować config.xml.