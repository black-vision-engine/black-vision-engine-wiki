# Plugin Expert

## Opis

Plugin umożliwia ustawianie parametrów kontektu OpenGLa takich jak blendowanie, culling itp.
Plugin Expert jest wstawiany zawsze na koniec.

Plugin po wpięciu ustawia domyślne wartości contextu.
Użytkownik może wymusić pobranie wartości z poprzednich pluginów poprzez ustawienie odpowiedniego parametru.

# Parametry

Parameter name         	| Type      	| Default Value    	| Description
----------------------- | -------------	| -----------------	| -----------
color blend				| BlendMode		| BM_Alpha			| Ustawia równanie blendowania coloru. Udostępnia tylko niektóre tryby blendowania. Zobacz [BlendingModes](BlendingModes)
alpha blend				| BlendMode		| BM_None			| Ustawia równanie blendowania alphy. Udostępnia tylko niektóre tryby blendowania. Zobacz [BlendingModes](BlendingModes)
blend enable			| bool			| false				| Włącza blendowanie w OpenGLu.
culling enable			| bool			| true				| Włącza culling dla trojkątów. Parametr **cc order** decyduje, która strona trójkąta będzie obcinana.
cc order				| bool			| true				| Obcinanie trójkątów o orientacji odwrotnej do wskazówek zegara lub zgodnej.
enable depth test		| bool			| true				| Włącza testowanie z-tki przy renderowaniu.
enable depth write		| bool			| true				| Włącza zapis z-tki do z-bufora.
fill mode				| FillContext::Mode | M_POLYGONS	| Tryb wypełniania wielokątów.
reset settings			| bool			| false				| Resetuje ustawienia pluginu. Wartości są pobierane z pluginu stojącego przed Expertem. Ten parametr powinien miec tylko jeden klucz w zerze. Po zresetowaniu parametrów, wartość jest automatycznie ustawiana na false przez plugin Expert