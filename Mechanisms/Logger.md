# Poziomy szczegółowości

Logger wspiera następujące poziomy:

* **debug** - Informacje ułatwiające debugowanie dla developerów.
* **info** - Przydatne informacje, które nadają kontekst błędom i są rzadkie i charakterystyczne. To będzie domyślny tryb jaki będzie włączony u użytkownika.
* **warning** - Błąd, który można naprawić automatycznie
* **error** - Błąd, który uniemożliwia ukończenie pojedynczej operacji
* **critical** - Błąd, który uniemożliwia dalsze działanie BVa i zmusza do jego zamknięcia

# Moduły

Każda wiadomość opatrzona jest nazwą modułu, z którego pochodzi komunikat.
Aktualnie istnieją następujące moduły:

* LibBlackVision
* LibCore
* LibImage
* Prototyper
* BlackVisionApp
* LibProjectManager
* TCPServer
* LibVideoCards
* XMLScenParser

Aby logować wiadomości trzeba zaincludować nagłówek dla odpowiedniego modułu, który definiuje odpowiednią stałą.

# Argumenty linii komend

Logger może być konfigurowalny przy pomocy argumentów wywołania BVa.
Konfigurować można zarówno miejsce, w które będą trafiać logi jak i poziom szczegółowości.

W trybie debug domyślnym poziomem szczegółowości jest **[debug]**.
W trybie release domyślnym poziomem jest **[warning]**.

## Domyślny log

Domyślnie włączany jest logger wysyłający komunikaty do pliku: *BlackVision/Logs/DefaultLog.txt*.
Log może zostać wyłączony poprzez podanie argumentu:
**-DisableDefaultLog**

```
#!bat
BlackVision -DisableDefaultLog
```

Aktualnie nie ma możliwości zmiany poziomów logowania domyślnego loga.

## Dodatkowe logi

###Logowanie na konsolę

Aby włączyć logowanie na konsolę trzeba podać argument: **-ConsoleLog**

Następnie opcjonalnie można podać poziom szczegółowości. W innym razie zostanie wybrany poziom domyślny.

**Przykładowe wywołania:**

Włączenie logowania na konsolę:
```
#!bat
BlackVision.exe -ConsoleLog
```

Logowanie na konsolę jedynie komunikatów o ważności warning i większej (czyli *warning, error, critical*)
```
#!bat
BlackVision.exe -ConsoleLog warning
```

###Logowanie do pliku

Można również dodać dowolną liczbę plików, do których będą logowane komunikaty podając argument:
**-FileLog**

Bezpośrednio za -FileLog musi znajdować się nazwa pliku, a dalej opcjonalnie poziom szczegółowości.
Ścieżkę do pliku podaje się względem katalogu roboczego BVa.

**Przykładowe wywołania:**

Stworzenie pliku ErrorLog.txt zawierającego komunikaty o poziomie error i critical:
```
#!bat
BlackVision.exe -FileLog Logi/ErrorLog.txt error
```

Stworzenie dwóch plików zawierających komunikaty o różnych poziomach szczegółowości:
```
#!bat
BlackVision.exe -FileLog Logi/ErrorLog.txt error -FileLog Logi/DebugLog.txt debug
```