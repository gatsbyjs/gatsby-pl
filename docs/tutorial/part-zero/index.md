---
title: Skonfiguruj środowisko programistyczne
typora-copy-images-to: ./
disableTableOfContents: true
---

Zanim zaczniesz budować swoją pierwszą strone Gatsby, musisz zapoznać się z niektórymi podstawowymi technologiami internetowymi i upewnić się, że zainstalowałeś wszystkie wymagane narzędzia oraz programy.

## Zapoznaj się z wierszem poleceń

Wiersz poleceń to interfejs tekstowy służący do uruchamiania poleceń na komputerze. Często występuje pod nazwą terminal. W tym samouczku będziemy używać obu określeń zamiennie. Uźywanie terminala przypomina używanie Findera na komputerach Mac lub Eksploratora w systemie Windows. Finder i Explorer to przykłady graficznych interfejsów użytkownika (GUI). Wiersz poleceń to potężne, tekstowe narzędzie do interakcji z komputerem.

<<<<<<< HEAD
Poświęć chwilę, aby zlokalizować i otworzyć interfejs wiersza poleceń (CLI) dla swojego komputera. W zależności od używanego systemu operacyjnego sprawdź [**instrukcje dla komputerów Mac**](https://www.imymac.com/pl/mac-cleaner/how-to-open-terminal-on-mac.html), [**instrukcje dla systemu Windows**](https://www.download.net.pl/10-sposobow-na-uruchomienie-wiersza-polecenia-w-windows-10/n/7949/) lub [**instrukcje dla systemu Linux**](https://pl.wikibooks.org/wiki/Ubuntu/Podstawowe_polecenia).

## Zainstaluj Homebrew dla Node.js

Do instalacji Gatsby oraz Node.js zaleca się użycie [Homebrew](https://brew.sh/). Trochę konfiguracji na początku może uchronić Cię przed niektórymi bolączkami w późniejszych krokach!

Jak zainstalować lub zweryfikować Homebrew na Twoim komputerze:

1.  Otwórz Terminal.
2.  Sprawdź czy Homebrew jest zainstalowane uruchamiając komendę `brew -v`. Powinieneś zobaczyć "Homebrew" oraz numer wersji.
3.  Jeśli nie, pobierz i zainstaluj [Homebrew wraz z instrukcją](https://docs.brew.sh/Installation) dla swojego systemu operacyjnego (Mac, Linux lub Windows).
4.  Po zainstalowaniu Homebrew powtórz krok 2, aby zweryfikować.

### Użytkownicy komputerów Mac: zainstaluj Xcode Command Line Tools

1.  Otwórz Terminal.
2.  Na Macu, zainstaluj Xcode Command Line Tools uruchamiając komendę `xcode-select --install`.
3.  Jeśli ten sposób zawiedzie, zaloguj się za pomocą konta programisty Apple a następnie pobierz narzędzie [bezpośrednio ze strony Apple](https://developer.apple.com/download/more/).
4.  Po wyświetleniu okna zezwolenia na rozpoczęcie instalacji, pojawi się ponownie okno z prośbą o zaakceptowanie licencji pobieranych narzędzi.

## ⌚ Zainstaluj Node.js oraz npm

Node.js to środowisko, które może uruchamiać kod JavaScript poza przeglądarką internetową. Gatsby jest napisane w Node.js. Aby rozpocząć pracę z Gatsby, musisz mieć najnowszą wersję zainstalowaną na komputerze.

_Uwaga: Minimalna wersja Node wspierana przez Gatsby to wersja 8, ale możesz teź użyć nowszej wersji._

1.  Otwórz Terminal.
2.  Uruchom komendę `brew update` aby upewnić się, że masz najnowszą wersję Homebrew.
3.  Uruchom komendę `brew install node`, aby zainstalować jednocześnie Node i npm.

Po wykonaniu wszystkich kroków upewnij się, że wszystko zostało poprawnie zainstalowane:

### Sprawdź czy Node.js jest zainstalowane poprawnie

1.  Otwórz Terminal.
2.  Uruchom komendę `node --version`. (Jeśli dopiero zaczynasz korzystać z terminala, “uruchom `komendę`” oznacza “wpisz `node --version` w wierszu polecenia i naciśnij klawisz Enter”. Od tego momentu właśnie to rozumiemy jako "Uruchom `komendę`”)
3.  Uruchom komendę `npm --version`.

Po uruchomieniu kaźdego z tych poleceń powinieneś zobaczyć numer wersji. Twoje wersje mogą się różnić od pokazanych poniżej! Jeśli w terminalu nie widzisz numeru wersji, wróć i upewnij się, że poprawnie zainstalowałeś Node.js.
=======
Take a moment to locate and open up the command line interface (CLI) for your computer. Depending on which operating system you are using, see [**instructions for Mac**](http://www.macworld.co.uk/feature/mac-software/how-use-terminal-on-mac-3608274/), [**instructions for Windows**](https://www.lifewire.com/how-to-open-command-prompt-2618089) or [**instructions for Linux**](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/).

_Note: If you’re new to the command line, "running" a command, means writing a given set of instructions in your command prompt, and hitting the Enter key”. Commands will be shown in a highlighted box, something like `node --version`, but not every highlighted box is a command! If something is a command it will be mentioned as something you have to run/execute._

## Install Node.js for your appropriate operating system

Node.js is an environment that can run JavaScript code outside of a web browser. Gatsby is built with Node.js. To get up and running with Gatsby, you’ll need to have a recent version installed on your computer. npm comes bundled with Node.js so if you don't have npm, chances are that you don't have Node.js either.

### Mac instructions

To install Gatsby and Node.js on a Mac, it is recommended to use [Homebrew](https://brew.sh/). A little set-up in the beginning can save you from some headaches later on!

#### How to install or verify Homebrew on your computer:

1. Open your Terminal.
1. See if Homebrew is installed by running `brew -v`. You should see "Homebrew" and a version number.
1. If not, download and install [Homebrew with the instructions](https://docs.brew.sh/Installation).
1. Once you've installed Homebrew, repeat step 2 to verify.

#### Install Xcode Command Line Tools:

1. Open your Terminal.
1. Install Xcode Command line tools by running `xcode-select --install`.
   - If that fails, download it [directly from Apple's site](https://developer.apple.com/download/more/), after signing-in with an Apple developer account
1. After being prompted to start the installation, you'll be prompted again to accept a software license for the tools to download.

#### Install Node

1. Open your Terminal
2. Run `brew install node`
   - If you don't want to install it through Homebrew, download the latest Node.js version from [the official Node.js website](https://nodejs.org/en/), double click on the downloaded file and go through the installation process.

### Windows Instructions

- Download and install the latest Node.js version from [the official Node.js website](https://nodejs.org/en/)

### Linux Instructions

Install nvm (Node Version Manager) and needed dependencies. nvm is used to manage Node.js and all its associated versions.

_💡 If when installing a package, it asks for confirmation, type `y` and press enter._

#### Ubuntu, Debian, and other `apt` based distros:

1. Run `sudo apt update` and then `sudo apt -y upgrade` to make sure your Linux distribution is ready to go.
2. Run `sudo apt-get install curl` to install curl which allows you to transfer data and download additional dependencies.
3. After it finishes installing, run `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash` to download the latest nvm version.
4. To confirm this has worked, use the following command. `nvm --version`. The output should be a version number.
5. [Set default Node.js version](#set-default-nodejs-version)

#### Arch, Manjaro and other `pacman` based distros:

1. Run `sudo pacman -Sy` to make sure your distribution is ready to go.
2. These distros come installed with curl, so you can use that to download nvm.
   `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash`
3. Before using nvm, you need to install additional dependencies by running `sudo pacman -S grep awk tar`.
4. To confirm this has worked, use the following command. `nvm --version`. The output should be a version number.
5. [Set default Node.js version](#set-default-nodejs-version)

#### Fedora, RedHat, and other `dnf` based distros:

1. These distros come installed with curl, so you can use that to download nvm.
   `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash`
2. To confirm this has worked, use the following command. `nvm --version`. The output should be a version number.
3. [Set default Node.js version](#set-default-nodejs-version)

If the Linux distribution you are using is not listed here, please find instructions on the web.

#### Set default Node.js version

When nvm is installed, it does not default to a particular node version. You’ll need to install the version you want and give nvm instructions to use it. This example uses the latest release of version 10, but more recent version numbers can be used instead.

```shell
nvm install 10
nvm use 10
```

To confirm that this worked, you can run `npm --version` and `node --version`. The output should look similar to the screenshot below, showing version numbers in response to the commands.
>>>>>>> 79b09bc29f133961f3d7de0f36a25ff727e6c22a

![Sprawdź wersje node i npm w terminalu](01-node-npm-versions.png)

<<<<<<< HEAD
## Zainstaluj Git
=======
Once you have followed the installation steps and you have checked everything is installed properly, you can continue to the next step.

## Install Git
>>>>>>> 79b09bc29f133961f3d7de0f36a25ff727e6c22a

Git to darmowy i open-source'owy system kontroli wersji. Przeznaczony jest on do szybkiej i wydajnej obsługi wszelkiego rodzaju projektów - od małych po bardzo duże. Podczas instalowania startera Gatsby, wykorzystuje on Git, aby pobrać i zainstalować wymagane pliki projektu startowego. Aby skonfigurować pierwszą stronę Gatsby, musisz zainstalować Git.

Procesy pobierania i instalacji Git zależą od Twojego systemu operacyjnego. Postępuj zgodnie z instrukcjami dla swojego systemu:

- [Zainstaluj Git na macOS](https://www.atlassian.com/git/tutorials/install-git#mac-os-x)
- [Zainstaluj Git na Windows](https://www.atlassian.com/git/tutorials/install-git#windows)
- [Zainstaluj Git na Linux](https://www.atlassian.com/git/tutorials/install-git#linux)

## Interfejs Gatsby CLI

Interfejs Gatsby CLI pozwala szybko tworzyć nowe strony oparte na Gatsby i uruchamiać komendy potrzebne do tworzenia stron w Gatsby. Gatsby CLI jest paczką npm.

Interfejs Gatsby CLI jest dostępny za pośrednictwem npm i powinien zostać zainstalowany globalnie, uruchamiając komendę `npm install -g gatsby-cli`.

_**Uwaga**: po zainstalowaniu Gatsby i uruchomieniu go po raz pierwszy zobaczysz krótki komunikat informujący o gromadzeniu anonimowych danych dotyczących użytkowania komend Gatsby CLI, możesz przeczytać więcej o tym, jak te dane są pobierane i wykorzystywane w [dokumencie o telemetrii](/docs/telemetry)._

Aby zobaczyć dostępne komendy, uruchom w terminalu `gatsby --help`.

![Sprawdź dostępne komendy w terminalu](05-gatsby-help.png)

> 💡 Jeśli nie możesz pomyślnie uruchomić interfejsu Gatsby CLI przez problemy z uprawnieniami, możesz sprawdzić [dokoumentację npm na temat rozwiązywania problemów z uprawnieniami](https://docs.npmjs.com/getting-started/fixing-npm-permissions), lub [ten poradnik](https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md).

## Utwórz witrynę Gatsby

Teraz możesz zacząć korzystać z Gatsby CLI i utworzyć swoją pierwszą stronę Gatsby. Przy pomocy interfejsu możesz pobrać „startery” (częściowo zbudowane strony z domyślną konfiguracją), aby szybciej zacząć tworzyć określony typ strony. Starter „Hello World”, którego będziesz tutaj używać, to starter z elementami niezbędnymi do stworzenia witryny Gatsby.

1.  Otwórz terminal.
2.  Uruchom komendę `gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world`. (_Uwaga: w zależności od prędkości pobierania, czas trwania może się różnić. Dla zwięzłości, poniższy gif został wstrzymany podczas części instalacyjnej_).
3.  Uruchom komendę `cd hello-world`.
4.  Uruchom komendę `gatsby develop`.

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="./03-create-site.mp4" />
  <p>Sorry! Your browser doesn't support this video.</p>
</video>

Co się właściwie wydarzyło?

```shell
gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world
```

- `new` to komenda, która tworzy nowy projekt Gatsby.
- W tym wypadku, `hello-world` to tytuł projektu - możesz wybrać dowolną nazwę. Narzędzie CLI umieści kod strony w nowym folderze o nazwie „hello-world”.
- Ostatnia część komendy, czyli adres URL, wskazuje repozytorium kodu na GitHubie, w którym znajduje się kod startowy, którego chcesz użyć.

```shell
cd hello-world
```

- Oznacza to 'Chcę zmienić folder (`cd`) na subfolder “hello-world”'. Ilekroć chcesz uruchomić jakąś komendę dla swojej witryny, musisz znajdować się w jej kontekście (innymi słowy, terminal musi być skierowany na folder, w którym znajduje się kod strony).

```shell
gatsby develop
```

- To polecenie uruchamia serwer developerski. Dzięki temu będziesz mógł zobaczyć i przetestować nową witryną w lokalnym środowisku programistycznym - (na twoim komputerze, niepublikowaną w Internecie).

### Wyświetl swoją witrynę lokalnie

<<<<<<< HEAD
Otwórz nową kartę w przeglądarce i przejdź do [**http://localhost:8000**](http://localhost:8000/).
=======
Open up a new tab in your browser and navigate to `http://localhost:8000/`
>>>>>>> 79b09bc29f133961f3d7de0f36a25ff727e6c22a

![Sprawdź stronę główną](04-home-page.png)

Gratulacje! Właśnie zacząłęś budować swoją pierwszą stronę z Gatsby! 🎉

<<<<<<< HEAD
Możesz zobaczyć stronę lokalnie, pod adresem [**_http://localhost:8000_**](http://localhost:8000/) tak długo jak długo będzie uruchomiony serwer deweloperski. Ten proces rozpoczął się gdy uruchomiłeś komendę `gatsby develop`. Aby go zatrzymać (lub “zatrzymać serwer deweloperski"), wróć do terminala i przyciskając klaiwsz "control" wciśnij klawisz "c" (ctrl+c). By uruchomić serwer ponownie, uruchom ponownie komendę `gatsby develop`!

**Uwaga:** Jeśli używasz wirtualnej maszyny takiej jak `vagrant` i/lub chcesz nasłuchiwać na lokalny adres IP, uruchom komendę `gatsby develop -- --host=0.0.0.0`. Serwer programistyczny będzie teraz nasłuchiwał zarówno na „localhost” jak i na lokalny adres IP.
=======
You’ll be able to visit the site locally at `http://localhost:8000/` for as long as your development server is running. That’s the process you started by running the `gatsby develop` command. To stop running that process (or to “stop running the development server”), go back to your terminal window, hold down the “control” key, and then hit “c” (ctrl-c). To start it again, run `gatsby develop` again!

**Note:** If you are using VM setup like `vagrant` and/or would like to listen on your local IP address, run `gatsby develop --host=0.0.0.0`. Now, the development server listens on both `http://localhost` and your local IP.
>>>>>>> 79b09bc29f133961f3d7de0f36a25ff727e6c22a

## Skonfiguruj edytor kodu

Edytor kodu to program zaprojektowany specjalnie do edycji kodu. Wybór jest ogromny, istnieje wiele świetnych edytorów.

### Pobierz VS Code

Dokumentacja Gatsby czasami zawiera zrzuty ekranu wykonane w VS Code, więc jeśli nie masz jeszcze preferowanego edytora, możesz wybrać właśnie ten. Dzięki temu Twój ekran będzie wyglądał jak zdjęcia z samouczka i dokumentacji. Jeli zdecydujesz się na VS Code, odwiedź [stronę VS Code](https://code.visualstudio.com/#alt-downloads) i pobierz wersję odpowiednią dla Twojego systemu.

### Zainstaluj wtyczkę Prettier

Polecamy również wtyczkę [Prettier](https://github.com/prettier/prettier). Jest to narzędzie, które pomaga sformatować kod, tak aby uniknąć błędów.

Możesz użyć Prettier bezpośrednio w edytorze, używając [wtyczki Prettier VS Code](https://github.com/prettier/prettier-vscode):

<<<<<<< HEAD
1.  Otwórz zakładkę rozszerzeń w VS Code (View => Extensions).
2.  Wyszukaj "Prettier - Code formatter".
3.  Kliknij "Install". (Po instalacji pojawi się komunikat z prośbą o ponowne uruchomienie VS Code, aby włączyć rozszerzenie. Nowsze wersje VS Code automatycznie włączą rozszerzenie po pobraniu.)
=======
1.  Open the extensions view on VS Code (View => Extensions).
2.  Search for "Prettier - Code formatter".
3.  Click "Install". (After installation, you'll be prompted to restart VS Code to enable the extension. Newer versions of VS Code will automatically enable the extension after download.)
>>>>>>> 79b09bc29f133961f3d7de0f36a25ff727e6c22a

> 💡 Jeśli nie używasz VS Code, sprawdź dokumentacje Prettier opisującą [proces instalacji](https://prettier.io/docs/en/install.html) lub [integrację z innymi edytorami](https://prettier.io/docs/en/editors.html).

## ➡️ Co dalej?

Podsumowując, w tej sekcji:

- Nauczyłeś/aś się o wierszu polecenia i o tym jak go używać
- Zainstalowałeś/aś i poznałeś/aś Node.js, npm CLI, system kontroli wersji Git oraz interfejs Gatsby CLI
- Stowrzyłeś/aś nową stronę w Gatsby przy użyciu interfejsu Gatsby CLI
- Uruchomiłeś/aś serwer deweloperski Gatsby oraz odwiedziłeś/aś swoją stronę lokalnie
- Pobrałeś/aś edytor kodu
- Zainstalowałeś/aś narzędzie do formatowania kodu Prettier

Teraz możesz przejść do [**zapoznaj się z blokami konstrukcyjnymi Gatsby**](/tutorial/part-one/).

## Adnotacja

### Przegląd podstawowych technologii

Nie trzeba być ekspertem w tych dziedzinach - jeśli nie jesteś, nie martw się! Podczas tej serii samouczków, wiele się nauczysz. Oto niektóre z głównych technologii internetowych, których będziesz używać podczas tworzenia stron w Gatsby:

- **HTML**: Język znaczników, który jest w stanie zrozumieć każda przeglądarka internetowa. HTML nadaje treści internetowej uniwersalną strukturę, definiując elementy takie jak nagłówki, akapity i inne. Skrót HTML oznacza HyperText Markup Language.
- **CSS**: Język służący do opisu formy prezentacji treści internetowych (czcionek, kolorów, układu itp.). Skrót CSS oznacza Cascading Style Sheets (kaskadowe arkusze stylów).
- **JavaScript**: Język programowania, który pomaga budować dynamiczne i interaktywne strony internetowe.
- **React**: Biblioteka (napisana w JavaScript) używana do budowania interfejsów użytkownika. Jest to framework używany przez Gatsby do tworzenia stron i struktury treści.
- **GraphQL**: Język zapytań, który pozwala pobierać dane do witryny. Jest to interfejs używany przez Gatsby do zarządzania danymi witryny.

### Co to jest strona internetowa?

Jeśli potrzebujesz pełnego wprowadzenia do tego, czym jest strona internetowa - w tym wprowadzenie do HTML i CSS - sprawdź “[**Tworzenie swojej pierwszej strony internetowej**](https://learn.shayhowe.com/html-css/building-your-first-web-page/)”. Jest to świetne miejsce do rozpoczęcią nauki o sieci. Aby uzyskać bardziej praktyczne wprowadzenie do [**HTML**](https://www.codecademy.com/learn/learn-html), [**CSS**](https://www.codecademy.com/learn/learn-css), oraz [**JavaScript**](https://www.codecademy.com/learn/introduction-to-javascript), sprawdź samouczki na Codecademy. [**React**](https://reactjs.org/tutorial/tutorial.html) i [**GraphQL**](http://graphql.org/graphql-js/) również mają własne poradniki wprowadzające.

### Naucz się więcej o wierszu poleceń

Dla świetnego wprowadzenia do korzystania z terminala, sprawdź [**samouczek Codecademy o wierszu poleceń**](https://www.codecademy.com/courses/learn-the-command-line/lessons/navigation/exercises/your-first-command) jeśli używasz Maca lub Linuxa, lub [**ten samouczek**](https://www.computerhope.com/issues/chusedos.htm) jeśli jesteś użytkownikiem Windowsa. Nawet jeśli posiadasz Windowsa, warto zobaczyć pierwszą stronę samouczka od Codecademy--znajdziesz tam wiele interesujących informacji. Dowiesz się m.in. czym jest wiersz poleceń, a nie tylko jak z niego korzystać.

### Naucz się więcej na temat npm

npm to manager paczek dla Javascript. Paczka to kod w formie modułu, którego możesz użyć w swoim projekcie. Jeśli pobrałeś i zainstalowałeś Node.js, npm również został zainstalowany na Twoim komputerze!

npm składa się z trzech odrębnych części: strona npm, rejestr npm, oraz interfejs wiersza polceń npm.

- Na stronie możesz przejrzeć jakie paczki Javascript są dostępne w rejestrze npm.
- Rejestr npm to ogromna baza danych, która zawiera informacje na temat dostępnych paczek.
- Kiedy znajdziesz paczkę, która Cię interesuje, możesz użyć interfejsu npm aby zainstalować ją w swoim projekcie lub globalnie (tak jak inne interfejsy wiersza poleceń). Interfejs npm odpowiada za komunikację z rejestrem - zazwyczaj interakcja odbywa się tylko ze stroną internetową npm lub interfejsem npm.

> 💡 Sprawdź wprowadzenie do npm, “[**Czym jest npm?**](https://docs.npmjs.com/getting-started/what-is-npm)”.

### Naucz się więcej na temat Git

Nie musisz wiedzieć jak działa Git, aby ukończyć ten samouczek, jednakże jest to bardzo użyteczne narzędzie. Jeśli chcesz dowiedzieć się więcej na temat systemów kontroli wersji, Git oraz Githuba, sprawdź ten [Podręcznik Git](https://guides.github.com/introduction/git-handbook/) stworzony przez Githuba.
