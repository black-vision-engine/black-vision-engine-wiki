#Opis

Cloner jest zubożoną logiką [Replicator](Replicator), która służy do klonowania nodów i ich ustawiania. Pozwala na bardziej interaktywne modyfikowanie parametrów klonowania niż [Replicator](Replicator).

##Ząłożenia

- Nody są nazywane zgodnie z kolumną i wierszem, w którym się znajdują node_[row]_[column]


##Parametry

Parameter name         	| Type      	| Default Value     | Description
----------------------- | -------------	| ----------------- | -----------
Clones Rows             | int           | 1                 | Number of rows.
Clones Cols             | int           | 1                 | Number of columns.
Delta X                 | float         | 0.0               | Cloned nodes translation in X axis.
Delta Y                 | float         | 0.0               | Cloned nodes translation in Y axis..
Delta Z                 | float         | 0.0               | Cloned nodes translation in Z axis.

Paweł jakie tu jeszcze parametry mają być??? Opisz wszystko.

##Funkcje

* Regenerate
* Remove Only Excess
* Remove All Clones

#Inne

- wystarczą nam funkcje które przychodzą z opcją MATRIX w  vizie (można z tego zrobić układ kolumnowy, wierszowy jak i macierz)

# Wątpliwości

- Czy dowolna zmiana jednego z parametrów rows lub cols ma powodować przegenerowanie wszystkich nodów? W Vizie nie zawsze się to dzieje. W szczególności Przełączając z opcji MATRIX na SPHERE i z powrotem dostajemy nody poobracane inaczej niż na początku. Również zmniejszanie i zwiększanie ilości kolumn i wierszy nie powoduje przegenorowania lokalnego przekształcenia.

** hmmm ja bym chyba faktycznie przegenerował wtedy wszystkie node-y ( a przynajmniej ich transformacje)**
** W Viz nody są tworzone jeśli jest taka potrzeba. Zwalniane natomiast dopiero jak się kliknie Remove All. **

- Co z pozostałymi opcjami dostępnymi w VIZ? 
    *  Plane: XY, XZ, YZ 
       **zróbmy**
    *  Shape ELIPSE i SPHERE
       **olewamy**
    *  Delta Reprezents: Step, BB
       **olewamy**
    *  Rename SubTree: T/F
       **zróbmy** - wtedy pierwszy node (wzór do klonowanie) jest przezywany zgodnie z zasadą dla pozostałych klonów
    *  Remove Excess Containers: T/F
    *  Co dokładnie robi funkcja: Remove Only Excess ?
       **kasuje nadmiarowe node-y** (liczba większa niż ta wynikająca z parametrów)