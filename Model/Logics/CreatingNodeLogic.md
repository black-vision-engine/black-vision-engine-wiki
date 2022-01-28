#Tworzenie logik

## Założenia projektowe

Logiki nodów pozwalają na sterowanie nodami i wykonują dodatkowe funkcjonalności, których nie można osiągnąć poprzez pluginy. Logika jest przyczepiana do node'a i ma prawo wykonywać operacje na tym nodzie oraz wszystkich jego dzieciach. Logiki nie mogą operować na braciach czy rodzicach noda, do którego zostały przyczepione.

Logiki mogą zmieniać strukturę poddrzewa, dodawać, usuwać, przenosić nody, modyfikować przypięte do nodów pluginy oraz parametry, zmieniać assety i wiele innych rzeczy.

## Klasa bazowa INodeLogic

Każda logika dziedziczy po interfejsie INodeLogic, która zawiera zbiór funkcji, które powinny zaimplementować logiki.

### Inicjalizacja i deinicjalizacja

Logika jest tworzona przez obiekt NodeLogicFactory. (Tworzenie logik jest na sztywno wywoływane w funkcji CreateLogic. Warto by było w przyszłości zrobić to tak, żeby można było dodawać logiki dynamicznie.) W parametrach podawany jest obiekt deserializatora oraz wskaźnik na node, do którego logika będzie przypięta.

Po stworzeniu logika jest przypinana do node'a. Po przypięciu wywoływana jest funkcja INodeLogic::Initialize, która pozwala na wstępną inicjalizację logiki. Przy odpinaniu logiki wywoływana jest funkcja INodeLogic::Deinitialize.

Logika w zasadzie nie może być przepięta do innego noda, ale Initialize i Deinitialize może być wywoływane wielokrotnie, jeżeli ktoś będzie robił undo/redo.

### Komunikacja ze światem zewnętrznym

Logika może się komunikować ze światem przy pomocy mechanizmu parametrów tak samo, jak robią to pluginy i efekty. Do tego musi zaimplementować funkcje INodeLogic::GetParameter i INodeLogic::GetParameters. Aby nie implementować tych funkcji można odziedziczyć po klasie NodeLogiBase, która ma wbudowany model i implementuje podstawowe funkcjonalności.

Logika może też obsługiwać eventy o dowolnej zawartości wysyłane z edytora. Do tego trzeba zaimplementować funkcję INodeLogic::HandleEvent, która dostaje w parametrach deserializator, który zawiera potrzebne dane. Logika może też odesłać dowolną odpowiedź w polu response (też serializator).

### Klasa NodeLogicBase

Klasa implementuje model parametrów. Dodatkowo dostarcza implementacje funkcji Update, która ten model updatuje oraz defaultową serializację i deserializację parametrów. Poza tym większość implementacji pozostawia puste.

### Updatowanie drzewa

Są trzy fazy updatowania logik:

- INodeLogic::PreNodeUpdate
- INodelOgic::Update
- INodeLogic::PostChildrenUpdate

INodeLogic::PreNodeUpdate jest wywoływane przed updatem node do którego przyczepiona jest logika oraz jego efektów i pluginów.
INodelOgic::Update jest wywoływany po updacie efektów i listy pluginów, ale przed updatem dzieci
INodeLogic::PostChildrenUpdate jest wywoływany po updacie dzieci.

### Śledzenie node'ów

Często logika z jakichś powodów chce sobie zapisać wskaźnik na node będący w obsługiwanym przez siebie poddrzewie. Trzeba pamiętać, że struktura drzewa może się zmieniać i np. edytor może go usunąć z poddrzewa albo go przenieść. Dlatego logika powinna zawsze o tym pamiętać i podpinać się pod eventy informujące o zmianach w drzewie jak NodeRemovedEvent czy NodeMovedEvent.

### Helpery

Do dodawania parametrów najlepiej korzystać z klasy ModelHelper (tej samej co w pluginach).
Do generowania szablonu logiki można skorzystać z odpowiedniego [Snippetu](../../Snippets).