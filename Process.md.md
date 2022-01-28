BV proces developerski:

- Cotygodniowe spotkania (w poniedziałki), na których omawiamy ostatnią iterację (sprint?) i ustalamy priorytety na kolejny tydzień, oceniamy taski
- Standupy 5 minutowe codziennie


Polityka pivotalowa:

- Wszystkie taski trafiają do pivotala
- Pawełek akceptuje zamówione feautury i zgłoszone przez siebie bugi
- testy oraz bugi znalezione przez nas akcpetujemy sami (ta druga osoba najlepiej robiąc małe code review)
- Trzymamy porządek w pivotalu (np. żeby taski były posegregowane zgodnie z priorytetami)
- Jezeli task przekracza dwa dni to jego wykonawca rozbija go na mniejsze
- Każda dodatkowa czynność, która się pojawia w trakcie robienia trafia do pivotala jako subtask lub osobny task.


Polityka testów:

- Jeżeli znajdujemy buga to tworzymy dwa commity - w pierwszym jest test, który replikuje buga, a w drugim dopiero fix
- Do mastera tylko mergujemy branche z testami, fixami itp.
- W przypadku odkrycia buga w trakcie pisania testów:
	- Jeżeli bug jest zwizany z robionym taskiem to:
		- Tworzymy taska w pivotalu dla tego buga
		- Tworzymy test replikujcy ten bug w tym samym branchu
		- Tagujemy commit idkiem taska z bugiem (czyli np Bug[#id])
		- Naprawiamy buga w osobnym commicie
	- Jeżeli bug nie jest zwizany z aktualnym testem
		- Tworzymy taska w pivotalu
		- Jeżeli da się to łatwo zrobić, dodajemy nowy branch i replikujemy w nim buga. Jeżeli replikacja jest za trudna, to opisujemy
		dokładnie buga w pivotalu.
		- Zostawiamy bugu do oceny jego priorytetu na później

		
Polityka branchów:

- Branch per task
- Po zakończeniu taska:
	- Wykonawca robi pull requesta
	- Druga osoba robi code review i merguje, jeżeli akceptuje
	- Zamykamy branche po zakończeniu

~~Polityka zmiennej `bv::Version::ReleaseType`~~

- pozwala przede wszystkim zidentyfikować prototypy
- "normalnie" powinna być pusta, bo:
    - zakładamy, że wszystko co wychodzi na zewnątrz to release
    - jeśli coś pójdzie nie tak, możemy je odróżnić po numerze wersji
- jeśli chcemy coś sprototypować, ustawiamy jej wartość na "Prototype of ..."
- po zaakceptowaniu funkcjonalności prototypu ustawiamy z powrotem na "" i (ewentualnie) merdżujemy

Polityka wersjonowania:

- Zmiana wersji serializacji następuje, gdy:
    - poprawiamy bugi w serializacji i mają one wpływ na plik xml
    - zmieniają się serializowane obiekty np. scena lub jej składniki
    - Zmienia się reprezentacja obiektu w xmlu



- Numer wersji inkrementujemy ręcznie i zaszywamy w kodzie.

- Starsza wersja deserializacji BVa może wczytać nowszą wersję, ale informuje użytkownika warningiem o niezręcznej sytuacji.


- Można rozważyc wprowadzenie kompatybilności wstecznej przy deserializacji na podstawie numeru wersji.
BV mógłby trzymać kod deserializacji z poprzednich wersji.

- Plugin/Logika/Efekt traktujemy jako zewnętrzne w stosunku do sceny, w zwiazku z tym ich wersja jest niezależna
od wersji serializacji sceny. Pomijam fakt że może się zmienić np. kod serializacji pluginu.

- Pluginy/Logiki/Efekty mają własną osobną wersję zapisywaną w pliku sceny.


- W pliku sceny zapisujemy wersję BV w postaci:
    - numeru wersji dużego i małego
    - numeru wersji serializacji
    - numeru patcha
    - numeru builda