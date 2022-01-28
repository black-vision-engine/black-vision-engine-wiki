# Continuous Integration

## Opis

Na serwerze sa stworzone dwa projekty jenkinsowe:

- BlackVision-master
- BlackVisionAll

Pierwszy zawiera jedynie branch master i jest oddzielony, żeby mógł przechowywać historię buildów. Drugi obsługuje wszystkie pozostałe branche i jest skonfigurowany tak, żeby wyrzucać buildy i nie zaśmiecać dysku.

## Instalacja i konfigurowanie Jenkinsa

Ten akapit powstał, żeby nikt więcej nie musiał stracić jednego ze swoich siedmiu żyć na Jenkinsa.

Instrukcja:

- Trzeba ściągnąć Jenkinsa i zainstalować go koniecznie w katalogu, który nie ma **spacji** i polskich znaków.
    - Przy instalacji wybrać domyślne wtyczki
    - Po zainstalowaniu doinstalować następujące pluginy, jeżeli ich nie ma:
        - AnsiColor
        - XUnit
        - Mercurial
        - JUnit
        - junit-realtime-test-reporter
        - MSBuild
        - Slack plugin
        - Doxygen plugin
        - HTML Publisher
        - Copy Artifact Plugin
        - plot plugin
        - pipeline utility steps
        - compress artifacts plugin
- Zainstalować mercuriala tak, żeby się dodał do systemowego PATHa. Można np. zainstalować całego TortoiseHg. Plugin mercurial z Jenkinsa nie instaluje własnego mercuriala.
    - Czasami trzeba podać ścieżkę do mercuriala w ustawieniach globalnych jenkinsa. Np. multibranch plugin wymaga ustawienia jawnie ścieżki do mercuriala.
- Skompilować Boosta według [instrukcji](Building). Trzeba pamiętać o ustawieniu zmiennych środowiskowych.
- Stworzyć nowy projekt Jenkinsowy i wkleić do katalogu Jenkins/jobs/BlackVision plik konfiguracyjny przygotowany na lokalnym komputerze.
- Skonfigurować Jenkinsa
    - Ustawić ścieżkę do MSBuild.exe w pluginie jenkinsowym (Globalne narzędzia do konfiguracji / MSBuild instalacji). MSBuild znajduje się najprawdopodobniej w katalogu C:\Program Files (x86)\MSBuild\14.0\Bin\MSBuild.exe.
    - Dodać do credentials dane do autoryzacji użytkownika w konfiguracji projektu (Konfiguruj/Pipeline). Jeżeli logujemy się do bitbucketa, to trzeba podać nazwę użytkownika, a nie maila z tego widzę (logowanie na stronie jest na podstawie maila)
    - Wyłączyć usługę Jenkins (Menadżer Zadań / Usługi -> Trzeba znaleźć usługę Jenkinsa, zatrzymać i wyłączyć automatyczne uruchamianie przy starcie)
    - Jeżeli występuja błędy typu "nie można znaleźć pliku" w repozytorium, może to oznaczać, że ścieżki sa za długie. Wtedy w pliku Jenkins/jenkins.xml należy ustawić w polu arguments coś takiego: -Djenkins.branch.WorkspaceLocatorImpl.PATH_MAX=30
    - Jeżeli testy z BVowym frameworkiem testowym wieszaj się w nieskończoność, jest to najpewniej spowodowane tym, że nie ma możliwości uruchomienia UI przez testy. Można próbować opcji Zezwalaj usłudze na dostęp do pulpitu. Jeżeli to nie działa być może trzeba uruchomić jenkinsa jako aplikację w command linie i trzymać cały czas otwarta. Z jakiegoś powodu czasem miałem takie problemy, a czasem nie.
    - Jeżeli jenkins nie może znaleźć użytkowników do wysłania emaila należy wpisać w Jenkins/jenkins.xml -Dhudson.tasks.MailSender.SEND_TO_UNKNOWN_USERS=true
    - Aby dokumentacja generowana przez doxygena wyświetlała się poprawnie należy w pliku Jenkins/jenkins.xml ustawić zmienną -Dhudson.model.DirectoryBrowserSupport.CSP="". Ustawienie to obniża politykę bezpieczeństwa jenkinsa.