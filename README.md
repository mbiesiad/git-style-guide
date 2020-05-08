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

* **Do not rewrite published history.** The repository's history is valuable in
  its own right and it is very important to be able to tell *what actually
  happened*. Altering published history is a common source of problems for
  anyone working on the project.

* However, there are cases where rewriting history is legitimate. These are
  when:

  * You are the only one working on the branch and it is not being reviewed.

  * You want to tidy up your branch (eg. squash commits) and/or rebase it onto
    the "master" in order to merge it later.

  That said, *never rewrite the history of the "master" branch* or any other
  special branches (ie. used by production or CI servers).

* Keep the history *clean* and *simple*. *Just before you merge* your branch:

    1. Make sure it conforms to the style guide and perform any needed actions
       if it doesn't (squash/reorder commits, reword messages etc.)

    2. Rebase it onto the branch it's going to be merged to:

       ```shell
       [my-branch] $ git fetch
       [my-branch] $ git rebase origin/master
       # then merge
       ```

       This results in a branch that can be applied directly to the end of the
       "master" branch and results in a very simple history.

       *(Note: This strategy is better suited for projects with short-running
       branches. Otherwise it might be better to occassionally merge the
       "master" branch instead of rebasing onto it.)*

* If your branch includes more than one commit, do not merge with a
  fast-forward:

  ```shell
  # good - ensures that a merge commit is created
  $ git merge --no-ff my-branch

  # bad
  $ git merge my-branch
  ```

## Misc.

* There are various workflows and each one has its strengths and weaknesses.
  Whether a workflow fits your case, depends on the team, the project and your
  development procedures.

  That said, it is important to actually *choose* a workflow and stick with it.

* *Be consistent.* This is related to the workflow but also expands to things
  like commit messages, branch names and tags. Having a consistent style
  throughout the repository makes it easy to understand what is going on by
  looking at the log, a commit message etc.

* *Test before you push.* Do not push half-done work.

* Use [annotated tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging#_annotated_tags)
  for marking releases or other important points in the history. Prefer
  [lightweight tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging#_lightweight_tags)
  for personal use, such as to bookmark commits for future reference.

* Keep your repositories at a good shape by performing maintenance tasks
  occasionally:

  * [`git-gc(1)`](http://git-scm.com/docs/git-gc)
  * [`git-prune(1)`](http://git-scm.com/docs/git-prune)
  * [`git-fsck(1)`](http://git-scm.com/docs/git-fsck)

# License

![cc license](http://i.creativecommons.org/l/by/4.0/88x31.png)

This work is licensed under a [Creative Commons Attribution 4.0
International license](https://creativecommons.org/licenses/by/4.0/).

# Credits

Agis Anastasopoulos / [@agisanast](https://twitter.com/agisanast) / http://agis.io
... and [contributors](https://github.com/agis-/git-style-guide/graphs/contributors)!
