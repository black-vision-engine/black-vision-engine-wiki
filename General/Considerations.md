1. **Omówienie struktury obecnego projektu**
    * _Co mamy i dlaczego praca z taką struktura projektu robi się coraz bardziej niewygodna_
    * _Jak to wygląda w innych, dużych projektach_
    * _Co możemy zrobić (propozycje, plan, potem podział na zadania i realizacja)_
    * _Jakiego systemu do tworzenia projektów używać - szczególnie w konteście custom build steps i properties_
    * _Jak zotpymalizować proces budowania (np. minimalizacja liczby kopiowań)_ 
        * _Można pozbyć się kopiowania i po prostu dorzucić potrzebne ścieżki do PATH dla lokalnego środowiska w jakim uruchamia się silnik. To wydaje się standardowym rozwiązaniem zarówno dla Windows jak i Linux. Trzeba tylko pamiętać, żeby odpalać BV w tym środowisku. Tak robi to na przykład Viz._
    * _Jakie narzędzia do profilowania i debugowania (CPU/GPU) warto znać i używać_
    * _Dodanie w 'jakiś' sposób do projektu shaderów, które są używane przez core_

    * * *
 
2. **Przejrzeć film od Vitalika i na takiej podstawie omówić sobie proces dodawania zmian oraz analizowania nowych ficzerów**
    * [Prezentacja](https://www.youtube.com/watch?v=qRwS6GtjPkQ)
    * _Warto zobaczyć, jak wygląda wysokopoziomowa specyfikacja kierunków rozwoju projektu bez zbędnego zagłębiania się w szczegóły techniczne_
    * _Diagram opisujący wysokopoziomową strukturę zgłaszania i akceptacji pomysłów_

    * * *
 
3. **Dopisywać sobie plany testów oraz poczyścić dokument opisujący proces testowania**
    * _Zastanowić się nad rolą testerów (bo tego w ogóle nie bierzemy pod uwagę)_
    * _Czy prace nad edytorem wpływają nam na proces testowania. Jeśli tak, to w jaki sposób(?)_

    * * *
 
4. **Zacząć przygotowywać jakąś bardziej techniczną dokumentację z umiarkowanie wysokopoziomową specyfikacją tego, co już mamy w**
    * _Modelu_
    * _Silniku_
    * _Edytorze_
    * _Całej aplikacji (włączając w to przetwarzanie offline)_