Wpis podstawowych pojęć używanych w projekcie.

* **Silnik** - Moduły odpowiadające za logikę renderowania oraz komunikację z OpenGlem.
    * **SceneNode** - Reprezentacja noda po stronie silnika. Przechowuje dane transformacji i dane potrzebne do renderowania (uniformy, shadery, tekstury, samplery)
* **Model** - Moduły odpowiadające ewaluowanie parametrów, generowanie geometrii, wybór shaderów, ustawienie uniformów i tekstur)
    * **Scena** - Niezależny zbiór nodów, świateł, kamer, logik, efektów. Modelowa reprezentacja sceny to **SceneModel**.
    * **Node** - Pojedynczy węzeł w drzewie sceny, która ma swoją transformację i może przechowywać pluginy. Mówiąc o nodzie mamy na myśli najczęściej modelowy BasicNode, ale istnieje też silnikowy SceneNode.
    * **Efekt** (Node Effect, Global Effect) - Logika renderowania, która może zostać zastosowana do noda i jego dzieci. Efekty mają część modelową (Parametry) i silnikową (Kod renderowania).
    * **Logika** - Obiekt pozwalający na implementowanie funkcjonalności wymagających manipulowania nodami. Logika jest wpinana w node i może wykonywać operację na nim i jego dzieciach.
    * **Timeline** - Obiekt, z którego można pobrać czas.
    * **Parametr** - Zbiór kluczy animacji.
    * **Values**- Struktura przechowująca wartość parametru evaluowaną w danym czasie.
    * **Evaluator** - Pobiera czas z timelinu, wyznacza wartość parametru w tym czasie i wpisuje ją do Valuesa.
    * **State** - Struktura przechowująca informację czy od ostatniej klatki zmienił się stan parametru.
    * **ParamValModel** - Zbiór parametrów, evaluatorów, valusów oraz state'ów, w pluginach oraz logikach. Istnieje jeszcze coś takiego jak PluginParamValModel - jest to struktura, która zawiera kilka ParamValModeli, które pełnią semantycznie inną funkcję w pluginach.
* **Updater** - Są warstwą pośredniczącą między silnikiem a modelem. Pobierają dane z modelu i ustawiają odpowiedni stan silnika.
* **Aplikacja** [BlackVision] - Moduł kompilujący się do pliku wykonywalnego, łączy model, silnik, updatery, i wysokopoziomową logikę aplikacji.
    * **Handlery** - Funkcje obsługujące eventy z Edytora.