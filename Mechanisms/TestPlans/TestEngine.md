# Test Engine

# Jak testować renderer i jego logikę ??

- Zaimplementować TestBVGL, żeby przechwytywał stan GLa przed każdym draw callem i wstawienie do kolejki stanów
- Umożliwienie odpytania GLa o aktualny stan w dowolnym momencie
- Zapisywanie stanu updatów buforów, uniformów (może i tekstur)
	- Zapisywanie zawartości (to może się przydać, ale to w sumie drugorzędna rzecz)
	- Zapisywanie co jest aktualnie zbindowane do pipelinu (to w zasadzie też jest stan)

Przydałoby się odwrotne mapowanie z Pdrów do resourców modelowych.
To powinien robić mechanizm testowy.
	
## Plan testów	

- Engine
	- Updaters
		- NodeUpdater
			- IsVisible
			- Przepisywanie transforma
			- Update geometry
				- Poprawnośc VAO
					- Deskryptory
					- zawartość
					- Liczba connected componentów
					- Flaga NeedRecreation
				- Update bounding box
			- Udpate Node effect
				- ustawianie dobrego node effectu
				- usuwanie efektu
				- IsEnabled
				- Przepisywanie valuesów
			- Update parametrów shadera
			- Update textur
				- Update zawartości
				- update parametrów samplera
				- Test dla wielu tekstur
				- test dla animacji
			- Update render state'u
		- SceneUpdater
			- Update camera
			- Update lights
			- SetOuputChannel
		- SceneEditor
			- Zwalanianie dodawanie nodów
			- SceneNode
				- Dodawanie/usuwanie dzieci
	- Rendering
		- SceneNode
			- Renderowanie pojednyczego noda (tworzymy scene noda w kodzie z żądanymi parametrami)
				- Testy bindowania poprawnych shaderów
				- Testy ustawiania uniformów/buforów (i updatowania zawartości)
				- Testy ustawiania tesktur
				- Testy updatowania tekstur
				- Poprawne ustawianie stanu GLa (culling, blending bla bla)
				- Porównanie stanu GLa ( cullign, blending itp) ze stanem w zapisanym w rendererze
				- Test Renderer::DrawRenderable ( dla rożnych topologii Trnagles, TriangleStrips, Lines)
					- Sprawdzenie czy wyrenderowana jest dobra liczba connected componentów
					- Poprawna liczba wierzchołków
			- Renderowanie pojedynczego noda z różnymi efektami
		- Logika renderowania
			- Poprawność IsVisible
			- NodeEffectEnabled
			- NodeRenderLogic
			- DepthRenderLogic
			- QueueRendering
	- Resourcy silnikowe (PDR)
		- VAO
			- Tworzenie PDRa
				- Sprawdzanie czy deskryptor dobrze przepisuje się na atrybuty (opcjonalne, bo móże być trudne do sprawdzenia po stworzeniu)
			- Memory upload
			- Recreation
			- Bindowanie (może być testowane w testach SceneNoda)
		- VertexBuffer
		- Texture (tylko 2D)
			- Przepisywanie formatu i innych paramrtów
			- Tworzenie PBOMemTransfer
			- Data Update
		- Render Target
		- Uniform Buffer
		- Zwalnianie zasobów
	- RenderChannele
	- Outputs
	
	
Stworzenie helperów do ustawiania parametrów shaderów i innych niewygodnych rzeczy.