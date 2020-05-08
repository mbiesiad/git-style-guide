# Przewodnik po stylu Git - Git Style Guide

Jest to przewodnik po stylu Git zainspirowany przez [*How to Get Your Change Into the Linux
Kernel*](https://kernel.org/doc/html/latest/process/submitting-patches.html),
[git man pages](http://git-scm.com/doc) i różne popularne praktyki
wśród społeczności.

Tłumaczenia są dostępne w następujących językach:

* [Chinese (Simplified)](https://github.com/aseaday/git-style-guide)
* [Chinese (Traditional)](https://github.com/JuanitoFatas/git-style-guide)
* [French](https://github.com/pierreroth64/git-style-guide)
* [German](https://github.com/runjak/git-style-guide)
* [Greek](https://github.com/grigoria/git-style-guide)
* [Japanese](https://github.com/objectx/git-style-guide)
* [Korean](https://github.com/ikaruce/git-style-guide)
* [Polish](https://github.com/mbiesiad/git-style-guide/edit/pl_PL)
* [Portuguese](https://github.com/guylhermetabosa/git-style-guide)
* [Russian](https://github.com/alik0211/git-style-guide)
* [Spanish](https://github.com/jeko2000/git-style-guide)
* [Thai](https://github.com/zondezatera/git-style-guide)
* [Turkish](https://github.com/CnytSntrk/git-style-guide)
* [Ukrainian](https://github.com/denysdovhan/git-style-guide)

Jeśli chcesz wnieść swój wkład, zrób to! Zrób fork projektu i otwórz pull request.

# Spis treści

1. [Gałęzie](#gałęzie)
2. [Commity](#commity)
  1. [Wiadomości](#wiadomości)
3. [Mergowanie](#mergowanie)
4. [Różnorodne](#różnorodne)

## Gałęzie

* Wybierz *krótkie* i *opisowe* nazwy:

  ```shell
  # dobre
  $ git checkout -b oauth-migration

  # złe - zbyt pobieżne
  $ git checkout -b login_fix
  ```

* Identyfikatory z odpowiednich ticketów w usłudze zewnętrznej (np. GitHub
  issue) są również dobrymi kandydatami do stosowania w nazwach branchy. Na przykład:

  ```shell
  # GitHub issue #15
  $ git checkout -b issue-15
  ```

* W nazwach branchy używaj małych liter. Zewnętrzne identyfikatory ticketów z wielkimi literami
  są ważnym wyjątkiem. Użyj *myślników* aby rozdzielać słowa.

  ```shell
  $ git checkout -b new-feature      # dobre
  $ git checkout -b T321-new-feature # dobre (Phabricator task id)
  $ git checkout -b New_Feature      # złe
  ```

* Gdy kilka osób pracuje nad *tym samym* featurem, może być wygodne
   mieć *osobiste* gałęzie feature'a i podobnie dla *całego zespołu*.
   Użyj następującej konwencji nazewnictwa:

  ```shell
  $ git checkout -b feature-a/master # branch w całym zespole
  $ git checkout -b feature-a/maria  # gałąź osobista dla Marii
  $ git checkout -b feature-a/nick   # gałąź osobista dla Nick'a
  ```

  Zmerguj do woli gałęzie osobiste z gałęzią dla całego zespołu (zobacz ["Mergowanie"](#mergowanie)).
  W końcu gałąź całego zespołu zostanie zmergowany do "master".

* Usuń swój branch z repozytorium nadrzędnego po zmergowaniu, chyba że
  istnieje konkretny powód, aby tego nie robić.

  Wskazówka: Użyj poniższego polecenia będąc w "master", aby wyświetlić listę zmergowanych
  gałęzi:

  ```shell
  $ git branch --merged | grep -v "\*"
  ```

## Commity

* Każdy commit powinien być pojedynczą *zmianą logiczną*. Nie rób kilku
  *zmian logicznych* w jednym commit'cie. Na przykład, jeśli łatka (patch) naprawia błąd i
  optymalizuje wydajność feature'a, podziel ją na dwa osobne commity.

  *Wskazówka: Użyj `git add -p` aby interaktywnie stage'ować określone części
  zmodyfikowanych plików.*

* Nie dziel pojedynczej *zmiany logicznej* na kilka commitów. Na przykład,
  implementacja feature'a i odpowiednie testy powinny być w
  tym samym commit'cie.

* Commituj *wcześnie* i *często*. Małe, niezależne commity są łatwiejsze
  do zrozumienia i cofnięcia, gdy coś pójdzie nie tak.

* Commity powinny być uporządkowane *logicznie*. Na przykład, jeśli *commit X* zależy
  od zmian zakończonych w *commit Y*, wtedy *commit Y* powinien nastąpić przed *commit X*.

Uwaga: Podczas pracy na gałęzi lokalnej, która *nie został jeszcze push'nięta*, jest
w porzadku użyć commitów jako tymczasowych snapshotów twojej pracy. Jednakże, wciąż
jest prawdą, że powinieneś zastosować wszystkie powyższe *przed* wypchnięciem.

### Wiadomości

* Użyj edytora, nie terminala, podczas pisania wiadomości, opisu commita:

  ```shell
  # dobre
  $ git commit

  # złe
  $ git commit -m "Quick fix"
  ```

  Committowanie z terminala zachęca do myślenia o konieczności dopasowania wszystkiego
  w jednym wierszu, co zwykle skutkuje nieinformowaniem, niejednoznaczną wiadomością, opisem
  commita.

* Wiersz streszczenia (tj. pierwszy wiersz wiadomości) powinien być
   *opisowy* i *zwięzły*. Idealnie byłoby nie dłużej niż
   *50 znaków*. Powinien być pisany wielkimi literami i pisany w trybie rozkazującym
   czasu teraźniejszego. Nie powinien kończyć się kropką, ponieważ jest to faktycznie
   *tytuł* commita:

  ```shell
  # dobre - czas teraźniejszy z trybem rozkazującym, z uwzględnieniem wielkiej litery, mniej niż 50 znaków
  Mark huge records as obsolete when clearing hinting faults

  # złe
  fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails.
  ```

* Następnie powinna pojawić się pusta linia, a następnie dokładniejszy
  opis. Powinien zmieścić się w *72 znaki* i wyjaśnić *dlaczego*
  zmiana jest potrzebna, *jak* rozwiązuje problem i jakie *skutki uboczne*
  może mieć.

  Powinno również zapewnić wszelkie wskazówki dotyczące powiązanych zasobów (np. link do
  odpowiedniego issue w bug trackerze):

  ```text
  Krótkie (50 znaków lub mniej) podsumowanie zmian

  Bardziej szczegółowy tekst objaśniający, jeśli to konieczne. Zmieść w
  72 znakach. W niektórych kontekstach pierwsza
  linia jest traktowana, jako temat wiadomości e-mail i reszta
  tekstu, jako ciało. Pusty wiersz oddzielający
  podsumowanie z ciała jest krytyczny (chyba że pominiesz ciało
  całkowicie); narzędzia takie jak rebase mogą się pomylić, jeśli uruchomisz
  dwa razem.

  Kolejne akapity pojawiają się po pustych wierszach.

  - Punkty wypunktowania są też w porządku

  - Użyj wypunktowania z myślnikiem lub gwiazdką,
    po którym następuje pojedyncza spacja, z pustymi wierszami pomiędzy 
    

  Wskaźniki do powiązanych zasobów mogą służyć jako stopka
  dla twojego opisu commita. Oto przykład, który się odwołuje do
  issues w bug trackerze:

  Rozwiązuje: #56, #78
  Zobacz też: #12, #34

  Źródło http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
  ```

  Ostatecznie, pisząc komunikat commita, zastanów się, czego będziesz potrzebował
  wiedzieć, gdy natkniesz się na tego commita za rok.

* Jeśli *commit A* zależy od *commit B*, zależność powinna być
  określona w wiadomości *commit A*. Użyj SHA1 odnosząc się do
  commitów.

  Podobnie, jeśli *commit A* rozwiązuje błąd wprowadzony przez *commit B*, powinno
  to być również określone w wiadomości z *commit A*.

* Jeśli commit ma być squash'owany do innego commita, użyj flag `--squash` i
  `--fixup` odpowiednio, aby wyjaśnić zamiar:

  ```shell
  $ git commit --squash f387cab2
  ```

  *(Wskazówka: Użyj flagi `--autosquash` podczas rebasing. Oznaczone commity będą
  'squashed' automatycznie.)*

## Mergowanie

* **Nie przepisuj opublikowanej historii.** Historia repozytorium jest cenna w
   swój sposób i bardzo ważne jest, aby móc *powiedzieć, co właściwie
   się stało*. Zmiana opublikowanej historii jest częstym źródłem problemów
   dla każdego, kto pracuje nad projektem.

* Są jednak przypadki, w których przepisywanie historii jest uzasadnione. To są
  przypadki kiedy:

  * Tylko ty pracujesz na branchu i nie jest on sprawdzany.

  * Chcesz uporządkować swój branch (np. squash commits) i/lub rebase na
    "master" w celu późniejszego scalenia.

  To powiedziawszy, *nigdy nie przepisuj historii gałęzi "master"*, ani żadnych innych
   specjalnych gałęzi (tzn. używanych przez serwery produkcyjne lub CI).

* Trzymaj historię *czystą* i *prostą*. *Tuż przed scaleniem* twojej gałęzi:

    1. Upewnij się, że jest ona zgodna z przewodnikiem po stylach i wykonaj wszelkie niezbędne działania,
       jeśli nie jest (squash/reorder commits, przeredagowanie wiadomości itp.)

    2. Rebase na branch, który będzie mergowany:

       ```shell
       [my-branch] $ git fetch
       [my-branch] $ git rebase origin/master
       # wtedy merge
       ```

       W rezultacie powstaje gałąź, którą można zastosować bezpośrednio na końcu
       "master" brancha i skutkuje bardzo prostą historią.

       *(Uwaga: Ta strategia lepiej nadaje się do projektów o krótkim okresie funkcjonowania
        gałęzi. W przeciwnym razie lepiej od czasu do czasu scalić
        gałąź "master" zamiast opierać się na niej.)*

* Jeśli twój branch zawiera więcej niż jeden commit, nie merguj z
  przewijaniem do przodu:

  ```shell
  # dobre - zapewnia utworzenie zatwierdzenia scalania
  $ git merge --no-ff my-branch

  # złe
  $ git merge my-branch
  ```

## Różnorodne

* Istnieją różne przepływy pracy i każdy ma swoje mocne i słabe strony.
  To, czy przepływ pracy pasuje do twojego przupadku, zależy od zespołu, projektu i twoich
  procedur developmentu.

  To powiedziawszy, ważne jest *wybranie* przepływu pracy i trzymanie się go.

* *Zachowaj spójność.* Jest to związane z przepływem pracy, ale obejmuje także różne rzeczy
   takie jak komunikaty commitów, nazwy gałęzi i tagi. Mając spójny styl
   w całym repozytorium ułatwia to zrozumienie, co się dzieje
   przeglądając dziennik, wiadomość commita itp.

* *Przetestuj przed wypchnięciem.* Nie wypychaj nieskończonej pracy.

* Użyj [tagi z adnotacjami](https://git-scm.com/book/en/v2/Git-Basics-Tagging#_annotated_tags)
  do oznaczania wydań lub innych ważnych punktów w historii. Preferuj
  [lightweight tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging#_lightweight_tags)
  do użytku osobistego, na przykład do zakładki commitów na przyszłość.

* Utrzymuj swoje repozytoria w dobrym stanie, wykonując sporadycznie zadania konserwacyjne:

  * [`git-gc(1)`](http://git-scm.com/docs/git-gc)
  * [`git-prune(1)`](http://git-scm.com/docs/git-prune)
  * [`git-fsck(1)`](http://git-scm.com/docs/git-fsck)

# Licencja

![cc license](http://i.creativecommons.org/l/by/4.0/88x31.png)

Ta praca jest na licencji [Creative Commons Attribution 4.0
International license](https://creativecommons.org/licenses/by/4.0/).

# Stworzone przez

Agis Anastasopoulos / [@agisanast](https://twitter.com/agisanast) / http://agis.io
... i [współtwórców](https://github.com/agis-/git-style-guide/graphs/contributors)!

_________________________________________________

Wersja polska od @[mbiesiad](github.com/mbiesiad)
