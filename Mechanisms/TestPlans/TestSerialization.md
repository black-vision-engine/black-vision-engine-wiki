- Serialization (Testy serializacji)
	- Serializers
		- ~~XML~~
		- ~~JSON~~
	- Scene
		- Node
			- ParamValModel
				- Serializacja/Deserializacja parametru
					- Timeliny (tutaj jest pewnie dużo rzeczy, bo tam się dużo dzieje)
					- Deserializacja parametru, który nie istnieje w modelu
					- Deserializacja parametru, który ma inny typ niż ten w modelu
					- Param types serialization/deserialization (ma sprawdzić czy tworzy się poprawny typ)
						- bool
						- int
						- string
						- wstring
						- float
						- transform
						- vec2
						- vec3
						- vec4
						- enum
						- mat2 (??)
						- przetestować co się dzieje w przypadku niepoprawnego typu
						- zastanowić się czy nie można juz wyrzucić kodu, który dla kompatybilności wstecznej
						używa innych nazw typów (np. numerów lub transform_vec)
				- Keys serialization/deserialization (testy dla każdego z typów)
					- Klucze niepoprawnych typów
					- Klucze, które się nie parsują
					- Mieszanie kluczy poprawnych i niepoprawnych
					- Klucze ustawione w czasie, który się nie parsuje
					- Dwa klucze w tym samym czasie
					- Klucze w złej kolejności (czasowo)
					- Klucze w ujemnym czasie
					- Brak kluczy
				- Interpolators
					- Serializacja/deserializacja różnych typów interpolatorów
					- Liczba intepolatorów różna od liczby kluczy
					- Klucze ustawione w złej kolejności (co robia interpolatory wtedy)
					- Niepoprawne parametry interpolatora
					- Brak interpolatorów
                                        - Nadmiar interpolatorów
			- Plugin
				- Flaga detailedInfo serialization contextu
				- Serializacja timelinu
				- Serializacja assetów
			- Logic (w zasadzie tylko poprawność serializacji typu. Reszta to już serializacja konkretnych implementacji logik)
			- Effect
				- Type efektu
				- Serializacja assetu
				- flaga detailedInfo
			- Prawidłowa struktura dzieci
			- Istniennie efektu, jeżeli był w nodzie (poprawność deserializacji efektu w teście dla efektów)
			- Istnienie logiki, jezeli była zserializowana
			- Flaga recursive (serialization context)
		- Camera
			- Poprawność ustawienia indexu aktualnie używanej kamery
		- Light
			- Serializacja i tworzenie poprawnego typu
			- Większa liczba świateł niż maksymalna
			- To w zasadzie jest już tylko serializacja parametrów. Można zrobić jakiś mały test czy parametry w ogóle są
			bo tutaj jest inny kod niż np. w pluginach.
		- ParamDescriptor
		- GridLines
		- SceneVariables
		- Timeliny
		- RenderChannelIdx
	- Serializacja deskryptorów assetów
		- Mesh
		- Font
		- SVG
		- Texture
		- Sequence
		- DataArray
		- AVStream
		
		
Refaktoring serializacji:

- Podzielić na więcej funkcj, żeby nie było jednego create, który zajmuje milion linijek. Wtedy będzei łatwiej
testować poszczególne funkcjonalności bez szerszego kontekstu.
- Przemyśleć dodanie w niektórych miejscach funckji deserialize
- Przenieść wszystkie statyczne klasy, które są używane przez serializację do kontekstu serializacji.
To będą na pewno PluginsManager i TimelineManager, może coś jeszcze ?? Generalnie trzeba się wycofywać ze statycznych klas.
- W serializacji enumów używać makr DECLARE_ENUM_SERIALIZATION i IMPLEMENT_ENUM_SERIALIZATION. Poprzenosić tablicę
z nazwami dla poszczególnych wartości w takei miejsca, żeby można było je łatwiej znaleźć.