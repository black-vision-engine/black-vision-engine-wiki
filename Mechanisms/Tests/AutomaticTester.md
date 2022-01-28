[TOC]

#Opis

Aplikacja AutomaticTester służy do odtwarzania sekwencji eventów wysyłanych do BVa. Pozwala to na wykonywanie
wysokopoziomowych scenariuszy testowych oraz łatwiejszą replikacje błędów w przypadku skomplikowanych scen.
Mechanizm działania opiera się na wysyłaniu eventów zapisanych w pliku .bvtest i porównaniu odpowiedzi z oczekiwanymi odpowiedziami z pliku.


AutomaticTester jest aplikacją uruchamianą z linii poleceń, głównie do przeprowadzania automatycznych testów na serwerze. Do celów debuggowania scen i replikacji błędów lepiej użyć aplikacji ProfilerEditor, która umożliwia
wstawianie breakpointów, przeglądanie eventów i ich odpowiedzi i daje większa interaktywność.

# Nagrywanie sekwencji zdarzeń #

Silnik może sam nagrywać wszystkie eventy dochodzące do silnika. Wystarczy dodać odpowiedni wpis w configu.
Pole Debug/CommandsDebugLayer/UseDebugLayer należy ustawić na true. FilePath ustawia ścieżkę, w której zostanie zapisany plik względem working directory. Nazwa pliku wygląda następująco: test_[aktualny_timestamp].bvtest.

	<property name="Debug">
		<property name="LoadSceneFromEnv" value="false" />    <!-- czy ma wczytywać deserializator pawła czy mechanizm z defaulta -->
		<property name="UseVideoInputFeeding" value="false" />  <!-- czy ma wczytywać deserializator pawła czy mechanizm z defaulta -->
                <property name="SceneFromEnvName" value="SCENE_STRUCTURE_INNER_EVENTS" />
		
		<property name="CommandsDebugLayer">
			<property name="UseDebugLayer" value="true" />
			<property name="FilePath" value="../../../../Logs/" />
		</property>
	</property>

# Testowanie z linii komend #

Do testowania służy aplikacja AutomaticTester.

## Parametry ##

* input (-i) - Ścieżka pliku do przetestowania
* output (-o) - Ścieżka do katalogu z wynikami.
* exec (-e) - Ścieżka pliku wykonywalnego BVa
* port (-p) - Port do połączenia z BVem
* verbose (-v) - Program będzie wypisywał na konsolę informacje w trakcie wykonywania


```
#!bat
AutomaticTester.exe -i "C:\Users\Witek\Dysk Google\bv_data\Regression\\" -e ..\..\..\x64-v110-Release\Applications\BlackVision\BlackVision.exe -v

```

```
#!bat

AutomaticTester.exe --input="C:\Users\Witek\Dysk Google\bv_data\Regression\\" --exec=C:\Users\Witek\BV\BlackVision\_Builds\x64-v110-Release\Applications\BlackVision\BlackVision.exe

```

Poniższa komenda umieści wyniki w katalogu Output/ względem katalogu, z którego wywołano tester.

```
#!bat

AutomaticTester.exe -i "C:\Users\Witek\Dysk Google\bv_data\Regression\\" -e ..\..\..\x64-v110-Release\Applications\BlackVision\BlackVision.exe -v --output=Output/

```

# Testowanie przy użyciu aplikacji z GUI #

TODO: