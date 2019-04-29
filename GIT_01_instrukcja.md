# Systemy kontroli wersji
* Dwa rodzaje
    * Centralized Version Control System
      * Przykładowe: CVS (Concurrent Versions System), SVN (Subversion), TFS (teraz serwer Azure DevOps Server 2019)
      * WADY
         * 
    * Distributed Version Control System
      * Git
      * Mercurial (HG)
      
# Podstawowe informacje o GIT
* zrewolucjonizował świat kontorli wersji
* rozproszony system kontroli wersji (Distributed Version Control System)
* powstał 7 kwietnia 2005 roku
* autor: Linus Torvalds
  * tutaj opowiada o Git na Google Tech Talk w maju 2007 roku: https://www.youtube.com/watch?v=4XpnKHJAok8
    * tutaj jest transkrypcja: https://git.wiki.kernel.org/index.php/LinusTalk200705Transcript
    * *"I decided I can write something better than anything out there in two weeks. And I was right."*
    * **Zadanie 00**: wyszukaj w transkrypcji słów: "moron", "mental", "stupid". Zapoznaj się z kontekstem (przeczytaj otaczające akapity). **Porada**: jeśli nie jesteś Linusem Torvaldsem bądź ostrożna/ostrożny w wygłaszaniu radykalnych poglądów.
    
 ## Repozytorium
 * potocznie: repo
 * Katalog zawierający wszystkie informacje o aktualnym stanie projektu i jego historii.
  * może zawierać odwołania do innych repozytoriów

## Zadanie 01 - przygotowanie środowiska
1. pobierz Gita ze strony projektu: www.git-scm.com (SCM od Source Control Management)
1. W trakcie instalacji dobrze jest zwrócić uwagę czy:
   * Windows Explorer Integration --> **Git Bash** here
   * Default Text Editor: **VIM**
   * Use Git from the **Windows Command Prompt**
   * Terminal Emulator: **MinTTY**
1. Sprawdź czy instalacja przebiegła pomyślnie:
   1. Uruchom konsolę WIN+R --> cmd --> Enter
   1. Wpisz komendę 'git version' - w ten sposób możesz sprawdzić numer wersji Git, która została zainstalowana na Twoim komputerze.
   
   
