# Systemy kontroli wersji
* Dwa modelowe rozwiązania
    * scentralizowane - Centralized Version Control System (CVCS)
      * wspólny serwer przechowuje całe repozytorium
      * Przykładowe implementacje: CVS (Concurrent Versions System), SVN (Subversion), TFS (teraz serwer Azure DevOps Server 2019)
      * ZALETA:
         * może być wydajniejsze przy projektach z dużymi plikami (np. multimedialnymi)
      * WADY:
         * centralizacja powoduje, że tylko jedna maszyna przechowuje całe repozytorium, zatem nie ma kopii zapasowych na każdej maszynie korzystającej z systemu
         * przeważnie niska wydajność
    * rozproszone (zdecentralizowane) - Distributed Version Control System
      * każda maszyna przechowuje całe repozytorium
      * Przykładowe implementacje: Git, Mercurial (HG), Bazaar
      * możliwość wykonania lokalnego commitu odróżnia DVCS od CVCS
      * ZALETY (Git):
         * możliwość pracy offline, ponieważ każda maszyna posiada kopię repozytorium
         * wysoka wydajność ze względu na offline
         * możliwość stosowania wielu sposobów pracy (workflows)
         * możliwość uporządkowania i zmiany commitów przed wysłaniem na serwer
       * WADY (Git):
         * wielkie pliki binarne są przechowywane na każdej maszynie (da się obejść)
       * CECHY ;) (Git):
         * pobranie jednynie części repozytorium jest trudne
         * do identyfikacji commitów używany jest SHA1 a nie kolejne numery
 * cecha wspólna systemów kontorli wersji to komenda `commit`
 * przeważnie najlepszym wyborem jest system DVCS 
# Podstawowe informacje o GIT
* zrewolucjonizował świat kontorli wersji
* zastępuje ZIP, RAR itp., ale wielu programistów używa go jakby był szybszą wersją tych rozwiązań
* umożliwia:
   * (zaawansowane) śledzenie historii zmian projektu 
   * współpracę wielu osób jednocześnie
* powstał 7 kwietnia 2005 roku
* autor: Linus Torvalds
  * tutaj opowiada o Git na Google Tech Talk w maju 2007 roku: https://www.youtube.com/watch?v=4XpnKHJAok8
    * tutaj jest transkrypcja: https://git.wiki.kernel.org/index.php/LinusTalk200705Transcript
    * *"I decided I can write something better than anything out there in two weeks. And I was right."*
    * **Zadanie 00**: wyszukaj w transkrypcji słów: "moron", "mental", "stupid". Zapoznaj się z kontekstem (przeczytaj otaczające akapity). **Porada**: jeśli nie jesteś Linusem Torvaldsem bądź ostrożna/ostrożny w wygłaszaniu radykalnych poglądów.
* Repozytorium
 * potocznie: repo
 * katalog zawierający wszystkie informacje o aktualnym stanie projektu i jego historii
   * może zawierać odwołania do innych repozytoriów
* Git to nie tylko komendy, to przede wszystkim zapis WORKFLOW
## Konsola czy UI?
* będziemy używać konsoli, bo w ten sposób lepiej zrozumiemy zasadę działania Git i unikniemy używania go jak ZIP czy RAR...
* użyjemy UI w następujących celach:
   * przeglądanie historii projektu
   * porównywanie zmian (DIFF)
   * rozwiązywanie konfliktów w plikach (merge)
* Jakie UI?
   * **Gitk** - Git repository browser (instalowany razem z Gitem)
   * **Git GUI** - Git Graphical Interface (instalowany razem z Gitem)
   * **zintegrowane z IDE** (np. Visual Studio, IntelliJ, Eclipse, Webstorm, VS Code)
   * **inne** (np. GitHub Desktop, Git Extensions, SmartGit, gmaster)
* TIP: w regularnej pracy warto ustawić sobie skrót do szybkiego uruchamiania konsoli oraz rozejrzeć się za zewnętrznym narzędziem (niezależnym od Git) typu text expanding np. AutoHotKey dla Windows. 
#### Zadanie 01 - przygotowanie środowiska
1. pobierz Gita ze strony projektu: www.git-scm.com (SCM od Source Control Management)
1. W trakcie instalacji dobrze jest zwrócić uwagę czy:
   * Windows Explorer Integration --> **Git Bash** here
   * Default Text Editor: **VIM**
   * Use Git from the **Windows Command Prompt**
   * Terminal Emulator: **MinTTY**
1. Sprawdź czy instalacja przebiegła pomyślnie:
   1. Uruchom konsolę WIN+R --> cmd --> Enter
   1. Wpisz komendę `git version` - w ten sposób możesz sprawdzić numer wersji Git, która została zainstalowana na Twoim komputerze.
#### Zadanie 02 - wstępna konfiguracja
Warto udostępnić uczestnikom projektu (zwykle pracujemy w zespołach) informację kto jest autorem zmian i w jaki sposób można się z tą osobą skontaktować. Te informacje będą dostępne w historii projektu. Użyjemy w tym celu opcji `--global`, bo chcemy aby te informacje były dostępne dla wszystkich Twoich repozytoriów.
1. Użyj komendy do ustawienia nazwy użytkownika (_user name_) - jeśli występuje spacja to musisz użyć cudzysłowu.
   * `git config --global user.name "Imię Nazwisko"` 
1. Użyj komendy do ustawienia adresu poczty elektronicznej
   * `git config --global user.email twojadrespoczty@email.com` 
1. Zweryfikuj czy te wartości zostały zapamiętane przez Git.
   * `git config user.name`
   * `git config user.email`
## Tryby pracy w Git
### Praca lokalna
#### Zadanie 03 - rozpoczęcie pracy 
* `mkdir my_project` - utwórz katalog (który będzie Twoim repozytorium)
* `cd my_project' - przejdź do tego katalogu
* `git init` - zainicjalizuj repozytorim git w tym katalogu
* Powstał ukryty katalog `.git`, w którym będą znajdować się wszystkie informacje dotyczące Twojego repozytorium.
   * sprawdź jakiego rodzaju są tam pliki (przede wszystkim tekstowe)
   * otwórz pliki tekstowe i przeglądnij ich treść
#### Zadanie 04 - ustaw notatnik zamiast VIMa
VIM jest bardzo przydatny, jednak wymaga pewnej wprawy. Te zajęcia dotyczą Git a nie VIM, więc ułatwimy sobie nieco.  
* `git config` - sprawdź konfigurację (jaki edytor jest ustawiony?)
* `git.config core.editor notepad` - ustawia Notatnik jako edytor
* `git config --list` - sprawdź jaki edytor jest teraz ustawiony
#### Zadanie 05 - pierwszy commit
**Commit** 
* inne nazwy: changeset, revision, version 
* stan repozytorium w konkretnym momencie
* każdy stan oznaczony jest identyfikatorem SHA1 
* historia składa się z kolejnych stanów projektu zarejestrowanych za pomocą komendy `git commit` 
* zawiera:
   * informacje o autorze zmian (patrz Zadanie 02)
   * czasie utworzenia 
   * opis zmian _commit message_. 
* **TIP**: pierwszy _commit_ należy wkonać jak najwcześniej, można go wykonać z pustym plikiem np. readme.md

**Wykonaj polecenia:**
1. notepad readme.md / edit readme.md
1. wpisz treść: _This is probably my first Git versioned project!_
1. `git add readme.md` - zapisuje nowy plik w repozytorium
1. `git commit`  - dodatkowo otworzy się plik tekstowy, w którym opisujemy dokonane przed chwilą zmiany (_commit message_)
1. opisz zmiany np. _Initial commit with project decription file_. i zapisz plik.
1. `git commit --help` - otwórz manual dla komendy `commit` - przeglądnij
   * alternatywnie: `git help commit`

#### Zadanie 06 - Working Copy
Working copy (inaczej: working directory), czyli katalog z plikami, na których aktualnie pracujemy. W tym katalogu umiesczana jest zawartość wybranej wersji repozytorium. _Commit_ rejestruje aktulaną wersję _Working Copy_.
Zmodyfikujmy stan Working Copy:
1. `touch new_file.txt` - utwórz nowy plik
1. notepad readme.md / edit readme.md
1. `git status` - sprawdź status tzw. Working Copy. Możesz zobaczyć, które pliki zostały dodane, które usunięte, a które zmodyfikowane itd.
#### Zadanie 07 - dodajemy gotowy kod do repo
1. pobierz Bootstrap 3.3.7 - https://github.com/twbs/bootstrap/archive/v3.3.7.zip
1. Rozpakuj, skopiuj ZAWARTOŚĆ katalogu "Docs" do swojego repozytorium (`my_project`).
1. `git add .` - dodaje wszystkie nowe pliki do repozytorium
1. `git commit` - "Added initial project template".
1. strona prywatna znajduje się w katalogu bootstrap-3.3.7\docs\examples\jumbotron
#### Zadanie 08 - generowanie commitów
1. zmień TITLE w pliku index.html (w katalogu bootstrap-3.3.7\docs\examples\jumbotron)
1. `git commit` i odpowiednio opisz zmiany w _commit message_
1. zmień "Hello world!" w pliku index.html
1. `git commit` i odpowiednio opisz zmiany w _commit message_
1. ukryj box logowania ("Sign in" itd.)
1. `git commit` i odpowiednio opisz zmiany w _commit message_
1. zaktualizuj rok (w stopce jest 2016)
1. `git commit` i odpowiednio opisz zmiany w _commit message_
#### Zadanie 09 - sprawdzanie zmian w plikach
1. `git diff` - pokazuje zmiany
1. pobierz i zainstaluj narzędzie wizualne do obserwacji zmian (np. WinMerge)
1. `gif config diff.tool winmerge` - ustalenie narzędzia do obserwacji zmian
1. `git difftool` - pokazuje zmiany w plikach w skonfigurowanym wcześniej narzędziu wizualnym
1. `git config --global difftool.prompt false` - wyłącza ....
#### Zadanie 10 - sprawdzanie historii projektu
1. `git log`
1. `git log -n4` - pokazuje ostatnie 4 commity
1. `git log --oneline' - pokazuje commity w wersji skróconej
1. `git log --since='{15 minutes ago}'` - pokazuje commity z ostatnich 15 minut
1. `gitk`  - umożliwia obejrzenie historii projektu w wersji graficznej
1. wypróbuj inne opcje dla komendy `git log` - możesz je znaleźć w dokumentacji dla `git log`: https://git-scm.com/docs/git-log

#### Tagi:
* wskazują na konkretny commit
* stanowią jego etykietę (nazwę)
* zawierają dodatkowe informacje w zależności od rodzaju taga:
   * tagi typu lightweight
      * to tylko dodatkowe słowo wyświetlane obok ID commita
      * używane są do prywatnego oznaczenia konkretnego commita (np. przed wykonaniem jakichś skomplikowanych i wysoce ryzykownych zmian w kodzie)
   * tagi typu annotated 
      * dodatkowe słowo wyświetlane obok ID commita
      * autor taga
      * "tag message"
      * data utworzenia*

#### Zadanie 11 - otaguj ostatni commit
* `git tag` - wypisuje wszystkie tagi
* `git tag <nazwa>` - nadaje tag aktualnemu commitowi
* `git tag <nazwa> <commit>` - nadaje tag wskazanemu commitowi
* `git tag <nazwa> <commit>` -a -m "msg" - tworzy _annotated_ tag
* `git tag -d <nazwa>` -kasuje tag (skrót od `--delete`)

#### Zadanie 12 - otaguj hello
Kolejnym commitom nadaj kolejno tagi "H"; "E"; "L"; "L"; "O". Da się?
Zatem zamiast "L" ustaw tag "LL" a kolejnemu commitowi nadaj tag "O".

#### Nawigacja po historii
* Każdy commit posiada ID wyrażony za pomocą hasha SHA1. 
* Funkcja haszująca wylicza ciąg znaków (0-9, a-f) o stałej długości (40) na podstawie dowolnych danych.
* Commit ID = zawartość + rodzic (parent) + data utworzenia + autor + commit message
   * poczytaj akapit "A SHORT NOTE ABOUT SHA-1" na tej stronie: https://git-scm.com/book/en/v2/Git-Tools-Revision-Selection
   * w przyszłości SHA256
* **Parent** to poprzedni commit.
   * commit może mieć wielu rodziców, jeśli jest efektem operacji merge
   * pierwszy commit w historii (**root commit**) nie ma rodzica
   
#### Zadanie 13 - zaobserwuj zawartość commita w historii
1. `git show <commit>` - pokazuje zawartość commita i jego tagów
1. `git show master^^^` - trzy commity wcześniej (liczba symboli ^ oznacza ile commitów wcześniej chcemy się cofnąć)
1. `git show L~2` - dwa commity wczesniej (liczba następująca po symbolu ~ określa ile commitów wcześniej chcemy się cofnąć
   * zatem można zauważyć, że ^^^^^ = ~5
1. `git log tag1..tag2` - wszystkie commity od tag1 do tag2 (np. od E do O - sprawdź)


### Gałęzie (Branches)
* **Gałąź** to kopia "koncepcyjna" repozytorium umożliwiająca pracę niezależną od pozostałych kopii. 
   * Git fizycznie nie kopiuje, nie duplikuje plików pomiędzy gałęziami.
   * gałąź składa się commitów (to lista commitów).
   * **Master** to domyślna nazwa pierwszej gałęzi repozytorium Gita (w innych systemach kontroli wersji: trunk, Main, default).
   * **Tip** to najświeższy commit w gałęzi.
   * Tag - nieruchomy wskaźnik na commit.
   * Gałąź - ruchomy wskaźnik na commit (zmienia się z każdym commitem).
   * Cechy gałęzi w Git
      * najbardziej elastyczny model
      * nie są zasobożerne - można tworzyć do woli
      * brak informacji w commicie na której gałęzi został utworzony
      * commity można przenosić między gałęziami
      * commit może zawierać się w wielu gałęziach lub w żadnej
      * gałęzie nie mają statusu "aktywne/zamknięte" - można je tworzyć i usuwać
   * Do czego służą gałęzie?
      * swoboda - tworząc na swojej gałęzi nie przeszkadzamy innym developerom
      * umożliwiają niezależną pracę wielu osób
      * umożliwiają niezależną pracę nad wieloma funkcjami jednocześnie
      * umożliwiają niezależną pracę nad wieloma wersjami oprogramowania jednocześnie
      * umożliwiają tworzenie śladu po pracy nad konkretnymi funkcjami co łatwo potem można przeglądnąć w historii
      * umożliwiają eksperymenty
#### Zadanie 14 - prównaj gałęzie 
* jak oznaczona jest master a jak oznaczone są tagi? Użyj Gitk.
#### Zadanie 15 - użyj podstawowych komend dot. gałęzi
* git branch my_new_branch - tworzy nową gałąź
* git branch - lista gałęzi (asterisk, czyli * wskazuje na której gałęzi jesteśmy)
* git branch -d <branch> - kasuje gałąź
Użyj powyższych komend. 



###### Źródła:
* materiały Macieja Aniserowicza dot. Git dostępne w Internecie (w szczególności kurs gita)
* manual Git
* https://git-scm.com/book/en/v2

   
