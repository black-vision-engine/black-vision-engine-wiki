[TOC]

# Gizmo/HUD - opis

## Tryb edycji i tryb produkcji

Silnik może pracować w dwóch trybach - edycji i produkcji. Gizma są renderowane jedynie w trybie edycji. W trybie produkcji są one również tworzone, jeżeli użytkownik wyśle odpowiedni event, ale będą widoczne dopiero jak tryb zostanie zmieniony.

Rozdzielenie tych dwóch trybów ma na celu ulepszenie renderowania gizm. W trybie edycji z-bufor jest renderowany jeszcze raz przed wyrenderowaniem gizm, dzięki czemu efekty fullscreenowe nie niszczą z-tki. Ponieważ wymaga to dodatkowego przebiegu renderowania, tryb edycji jest znacznie wolniejszy.

Tryby można przełączać przy pomocy [eventu](../../Events/EngineStateEvent).

## Opis działania

Gizma są dodatkowymi nodami, które są podpięte w drzewie sceny. Nie są one widoczne dla edytora, logik ani kodu renderującego, ale dziedziczą transformację po nodach, w które są wczepione. Każdy node może posiadać wpiętych wiele gizm (każdy plugin/efekt/logika oraz sam node, może być obsługiwany przez wiele gizm na raz). Gizm są tworzone i usuwane na żądanie edytora, domyślnie nie tworzy się niczego.

Pojedyncze gizmo składa się z node'a **(gizmoRoota)** oraz wpiętej w niego logiki gizma. Logika gizma w momencie tworzenia, może dodać do gizmoRoota całe poddrzewo nodów, jakie jest jej potrzebne. Drzewo gizma nie różni się zasadniczo od pozostałej części drzewa, dlatego można tam wpinać dowolne elementy takie jak pluginy, logiki i efekty.

### Logiki gizm

Logika gizma przy tworzeniu, oprócz wskaźnika na node'a, w którego jest wpięta, dostaje wskaźnik na obsługiwanego node'a **(gizmoOwner)**. Zadaniem logiki gizma jest po pierwsze stworzenie swojego poddrzewa, które będzie wyświetlane. Po drugie logika ta updatuje swoje poddrzewo oraz może updatować parametry obsługiwanego elementu.

### Typy gizm

Gizma mogą obsługiwać różne elementy w drzewie sceny. Na tej podstawie jest podział na typy:

- **Scene**
- **Node**
- **Logic**
- **Plugin**
- **Effect**

Gizma sceniczne, ponieważ nie obsługują żadnego elementu w nodzie, są wpinane bezpośrednio w roota sceny. Zakłada się, że root sceny będzie miał zawsze transformacje identycznościową, tak żeby gizma te były wyświetlane zawsze w przestrzeni świata, a nie lokalnej noda, w którego są wpięte.

## Obsługa gizm z poziomu edytora

### Mapowanie funkcjonalności

Każdy element w scenie, do którego można stworzyć gizmo, posiada mapę obsługiwanych funkcjonalności. Funkcjonalności są mapowane na nazwy logik, które trzeba stworzyć. Edytor tworząc gizmo musi podać nazwę funkcjonalności.

### Dodawanie/Usuwanie gizm

Dodawanie usuwanie gizm jest obsługiwane przez [eventu](../../Events/GizmoEvent).

## Mechanizmy silnika przepisane jako gizma

### Bounding boxy

Bounding boxy są obsługiwane poprzez mechanizmy silnika i nie wymagają osobnego tworzenia przez edytor. Klikanie działa tak jak wczesniej, z tym że trzeba być w trybie edycji.

### Grid lines

Ponieważ gridliny wymagają serializacji w pliku sceny, to częściowo są nadal obsługiwane starym mechanizmem. po wczytaniu gridliny są wczytane razem ze sceną, ale żeby je zobaczyć, trzeba stworzyć gizmo o funkcjonalności **GridLines**. W zasadzie wszystko działa tak jak wcześniej z tą różnicą, że wysłanie eventu Show/HideGridLines będzie działało jedynie w trybie edycji.