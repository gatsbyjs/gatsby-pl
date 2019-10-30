---
title: Skonfiguruj środowisko programistyczne
typora-copy-images-to: ./
disableTableOfContents: true
---

Before you start building your first Gatsby site, you’ll need to familiarize yourself with some core web technologies and make sure that you have installed all required software tools.

Zanim zaczniesz budować swoją pierwszą strone Gatsby, musisz zapoznać się z niektórymi podstawowymi internetowymi technologiami i upewnić się, że zainstalowałeś wszystkie wymagane narzędzia oraz programy.

## Zapoznaj się z wierzem poleceń

Wiersz poleceń to interfejs tekstowy służący do uruchamiania poleceń na komputerze. Często występuje pod nazwą terminal. W tym samouczku będziemy używać obu określeń zamiennie. Uźywanie terminala przypomina używanie Findera na komputerach Mac lub Eksploratora w systemie Windows. Finder i Explorer to przykłady graficznych interfejsów użytkownika (GUI). Wiersz poleceń to potężny, tekstowy sposób interakcji z komputerem.

Poświęć chwilę, aby zlokalizować i otworzyć interfejs wiersza poleceń (CLI) dla swojego komputera. W zależności od używanego systemu operacyjnego zobacz [**instrukcje dla komputerów Mac**](https://www.imymac.com/pl/mac-cleaner/how-to-open-terminal-on-mac.html), [**instrukcje dla systemu Windows**](https://www.download.net.pl/10-sposobow-na-uruchomienie-wiersza-polecenia-w-windows-10/n/7949/) or [**instrukcje dla systemu Linux**](https://pl.wikibooks.org/wiki/Ubuntu/Podstawowe_polecenia).

## Zainstaluj Homebrew dla Node.js

To install Gatsby and Node.js, it is recommended to use [Homebrew](https://brew.sh/). Trochę konfiguracji na początku może uchronić Cię przed niektórymi bolączkami w późniejszych krokach!

Jak zainstalować lub zweryfikować Homebrew na komputerze:

1.  Otwórz Terminal.
2.  Sprawdź czy Homebrew jest zainstalowane uruchamiając komendę `brew -v`. Powinieneś zobaczyć "Homebrew" oraz numer wersji.
3.  Jeśli nie, pobierz i zainstaluj [Homebrew wraz z instrukcją](https://docs.brew.sh/Installation) dla swojego systemu operacyjnego (Mac, Linux lub Windows).
4.  Po zainstalowaniu Homebrew powtórz krok 2, aby zweryfikować.

### Użytkownicy komputerów Mac: zainstaluj Xcode Command Line Tools

1.  Otwórz Terminal.
2.  Na Macu, zainstaluj Xcode Command Line Tools uruchamiając komendę `xcode-select --install`.
3.  Jeśli ten sposób zawiedzie, pobierz [bezpośrednio ze strony Apple](https://developer.apple.com/download/more/), po zalogowaniu się za pomocą konta programisty Apple.
4.  Po wyświetleniu monitu o rozpoczęcie instalacji ponownie pojawi się monit o zaakceptowanie licencji na oprogramowanie do pobrania narzędzi.

## ⌚ Zainstaluj Node.js oraz npm

Node.js to środowisko, które może uruchamiać kod JavaScript poza przeglądarką internetową. Gatsby jest napisane w Node.js. Aby rozpocząć pracę z Gatsby, musisz mieć najnowszą wersję zainstalowaną na komputerze.

_Note: Minimalna wersja Node wspierana przez Gatsby to wersja 8, ale możesz teź użyć nowszej wersji._

1.  Otwórz Terminal.
2.  Uruchom komendę `brew update` aby upewnić się, że masz najnowszą wersję Homebrew.
3.  Uruchom tę komendę, aby zainstalować Node i npm naraz: `brew install node`

Po wykonaniu wszystkich kroków upewnij się, że wszystko zostało poprawnie zainstalowane:

### Sprawdź czy Node.js jest zainstalowane poprawnie

1.  Otwórz Terminal.
2.  Uruchom komendę `node --version`. (Jeśli dopiero zaczynasz korzystać z terminala, “uruchom `komendę`” oznacza “wpisz `node -version` w wierszu polecenia i naciśnij klawisz Enter”. Od tego momentu właśnie to rozumiemy jako "Uruchom `komendę`”)
3.  Uruchom komendę `npm --version`.

The output of each of those commands should be a version number. Your versions may not be the same as those shown below! If entering those commands doesn’t show you a version number, go back and make sure you have installed Node.js.

Po uruchomieniu kaźdego z tych poleceń powinienes zobaczyć numer wersji. Twoje wersje mogą się różnić od pokazanych poniżej! Jeśli w terminalu nie widzisz numeru wersji, wróć i upewnij się, że poprawnie zainstalowałeś Node.js.

![Sprawdź wersje node i npm w terminalu](01-node-npm-versions.png)

## Zainstaluj Git

Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency. When you install a Gatsby "starter" site, Gatsby uses Git behind the scenes to download and install the required files for your starter. You will need to have Git installed to set up your first Gatsby site.

Git to darmowy i open-sourcowy system kontroli wersji. Przeznaczony jest on do szybkiej i wydajnej obsługi wszelkiego rodzaju projektów - od małych po bardzo duże. Podczas instalowania startera Gatsby, Gatsby wykorzystuje Git, aby pobrać i zainstalować wymagane pliki projektu startowego. Aby skonfigurować pierwszą stronę Gatsby, musisz zainstalować Git.

The steps to download and install Git depend on your operating system. Follow the guide for your system:
Procesy pobierania i instalacji Git zależą od Twojego systemu operacyjnego. Postępuj zgodnie z instrukcjami dla swojego systemu:

- [Zainstalul Git na macOS](https://www.atlassian.com/git/tutorials/install-git#mac-os-x)
- [Zainstalul Git na Windows](https://www.atlassian.com/git/tutorials/install-git#windows)
- [Zainstalul Git na Linux](https://www.atlassian.com/git/tutorials/install-git#linux)

## Interfejs Gatsby CLI

Interfejs Gatsby CLI pozwala szybko tworzyć nowe strony oparte na Gatsby i uruchamiać komendy potrzebne do tworzenia stron w Gatsby. Gatsby CLI jest paczką npm.

Interfejs Gatsby CLI jest dostępny za pośrednictwem npm i powinien zostać zainstalowany globalnie, uruchamiając komendę `npm install -g gatsby-cli`.

_**Uwaga**: po zainstalowaniu Gatsby i uruchomieniu go po raz pierwszy zobaczysz krótki komunikat informujący o gromadzeniu anonimowych danych dotyaczących użytkowania komend Gatsby CLI, możesz przeczytać więcej o tym, jak te dane są pobierane i wykorzystywane w [dokumencie o telemetrii](/docs/telemetry)._

Aby zobaczyć dostępne komnendy, uruchoem w terminalu `gatsby --help`.

![Sprawdź dostępne komendy w terminalu](05-gatsby-help.png)

> 💡 Jeśli nie możesz pomyślnie uruchomić interfejsu Gatsby CLI przez problemy z uprawnieniami, możesz sprawdzić [dokoumentację npm na temat rozwiązywania problemów z uprawnieniami](https://docs.npmjs.com/getting-started/fixing-npm-permissions), lub [ten poradnik](https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md).

## Utwórz witrynę Gatsby

Now you are ready to use the Gatsby CLI tool to create your first Gatsby site. Using the tool, you can download “starters” (partially built sites with some default configuration) to help you get moving faster on creating a certain type of site. The “Hello World” starter you’ll be using here is a starter with the bare essentials needed for a Gatsby site.

Teraz możesz zacząć korzystać z Gatsby CLI, aby utworzyć swoją pierwszą stroen Gatsby. Przy pomocy narzędzia możesz pobrać „startery” (częściowo zbudowane strony z domyślną konfiguracją), aby szybciej zacząć tworzyć określony typ strony. Starter „Hello World”, którego będziesz tutaj używać, to starter z elementami niezbędnymi do stworzenia witryny Gatsby.

1.  Otwórz terminal.
2.  Uruchom komendę `gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world`. (_Note: Uwaga: w zależności od prędkości pobierania, ilość czasu może się różnić. Dla zwięzłości, poniższy gif został wstrzymany podczas części instalacyjnej_).
3.  Uruchom komendę `cd hello-world`.
4.  Uruchom komendę `gatsby develop`.

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="./03-create-site.mp4" />
  <p>Sorry! You browser doesn't support this video.</p>
</video>

Co się właściwie wydarzyło?

```shell
gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world
```

- `new` to komenda, która tworzy nowy projekt Gatsby.
- W tym wypadku, `hello-world` to dowolny tytuł - możesz wybrać dowolną nazwę. Narzędzie CLI umieści kod strony w nowym folderze o nazwie „hello-world”.
- Wreszcie, podany adres URL wskazuje repozytorium kodu na GitHubie, w którym znajduje się kod startowy, którego chcesz użyć.

```shell
cd hello-world
```

- Oznacza to 'Chcę zmienić folder (`cd`) na subfolder “hello-world” subfolder'. Ilekroć chcesz uruchomić jakiąś komendę dla swojej witryny, musisz znajdować się w jej kontekście (innymi słowy, terminal musi być skierowany na folder, w którym znajduje się kod strony).

```shell
gatsby develop
```

- This command starts a development server. You will be able to see and interact with your new site in a development environment — local (on your computer, not published to the internet).
To polecenie uruchamia serwer developerski. Dzięki temu będziesz mógł zobaczyć i przetestować nową witryną w lokalnym środowisku programistycznym - (na twoim komputerze, niepublikowanym w Internecie).

### Wyświetl swoją witrynę lokalniey

Otwórz nową kartę w przeglądarce i przejdź do [**http://localhost:8000**](http://localhost:8000/).

![Sprawdź stronę główną](04-home-page.png)

Gratulacje! To początek Twojej pierwszej strony zbudowanej z Gatsby! 🎉

You’ll be able to visit the site locally at [**_http://localhost:8000_**](http://localhost:8000/) for as long as your development server is running. That’s the process you started by running the `gatsby develop` command. To stop running that process (or to “stop running the development server”), go back to your terminal window, hold down the “control” key, and then hit “c” (ctrl-c). To start it again, run `gatsby develop` again!

**Note:** If you are using VM setup like `vagrant` and/or would like to listen on your local IP address, run `gatsby develop -- --host=0.0.0.0`. Now, the development server listens on both 'localhost' and your local IP.

## Set up a code editor

A code editor is a program designed specifically for editing computer code. There are many great ones out there.

### Download VS Code

Gatsby documentation sometimes includes screenshots that were taken in VS Code, so if you don't have a preferred code editor yet, using VS Code will make sure that your screen looks just like the screenshots in the tutorial and docs. If you choose to use VS Code, visit the [VS Code site](https://code.visualstudio.com/#alt-downloads) and download the version appropriate for your platform.

### Install the Prettier plugin

We also recommend using [Prettier](https://github.com/prettier/prettier), a tool that helps format your code to avoid errors.

You can use Prettier directly in your editor using the [Prettier VS Code plugin](https://github.com/prettier/prettier-vscode):

1.  Open the extensions view on VS Code (View => Extensions).
2.  Search for "Prettier - Code formatter".
3.  Click "Install". (After installation you'll be prompted to restart VS Code to enable the extension. Newer versions of VS Code will automatically enable the extension after download.)

> 💡 If you're not using VS Code, check out the Prettier docs for [install instructions](https://prettier.io/docs/en/install.html) or [other editor integrations](https://prettier.io/docs/en/editors.html).

## ➡️ What’s Next?

To summarize, in this section you:

- Learned about the command line and how to use it
- Installed and learned about Node.js and the npm CLI tool, the version control system Git, and the Gatsby CLI tool
- Generated a new Gatsby site using the Gatsby CLI tool
- Ran the Gatsby development server and visited your site locally
- Downloaded a code editor
- Installed a code formatter called Prettier

Now, move on to [**getting to know Gatsby building blocks**](/tutorial/part-one/).

## References

### Overview of core technologies

It’s not necessary to be an expert with these already — if you’re not, don’t worry! You’ll pick up a lot through the course of this tutorial series. These are some of the main web technologies you’ll use when building a Gatsby site:

- **HTML**: A markup language that every web browser is able to understand. It stands for HyperText Markup Language. HTML gives your web content a universal informational structure, defining things like headings, paragraphs, and more.
- **CSS**: A presentational language used to style the appearance of your web content (fonts, colors, layout, etc). It stands for Cascading Style Sheets.
- **JavaScript**: A programming language that helps us make the web dynamic and interactive.
- **React**: A code library (built with JavaScript) for building user interfaces. It’s the framework that Gatsby uses to build pages and structure content.
- **GraphQL**: A query language that allows you to pull data into your website. It’s the interface that Gatsby uses for managing site data.

### What is a website?

For a comprehensive introduction to what a website is--including an intro to HTML and CSS--check out “[**Building your first web page**](https://learn.shayhowe.com/html-css/building-your-first-web-page/)”. It’s a great place to start learning about the web. For a more hands-on introduction to [**HTML**](https://www.codecademy.com/learn/learn-html), [**CSS**](https://www.codecademy.com/learn/learn-css), and [**JavaScript**](https://www.codecademy.com/learn/introduction-to-javascript), check out the tutorials from Codecademy. [**React**](https://reactjs.org/tutorial/tutorial.html) and [**GraphQL**](http://graphql.org/graphql-js/) also have their own introductory tutorials.

### Learn more about the command line

For a great introduction to using the command line, check out [**Codecademy’s Command Line tutorial**](https://www.codecademy.com/courses/learn-the-command-line/lessons/navigation/exercises/your-first-command) for Mac and Linux users, and [**this tutorial**](https://www.computerhope.com/issues/chusedos.htm) for Windows users. Even if you are a Windows user, the first page of the Codecademy tutorial is a valuable read. It explains what the command line is, not just how to interface with it.

### Learn more about npm

npm is a JavaScript package manager. A package is a module of code that you can choose to include in your projects. If you just downloaded and installed Node.js, npm was installed with it!

npm has three distinct components: the npm website, the npm registry, and the npm command line interface (CLI).

- On the npm website, you can browse what JavaScript packages are available in the npm registry.
- The npm registry is a large database of information about JavaScript packages available on npm.
- Once you’ve identified a package you want, you can use the npm CLI to install it in your project or globally (like other CLI tools). The npm CLI is what talks to the registry — you generally only interact with the npm website or the npm CLI.

> 💡 Check out npm’s introduction, “[**What is npm?**](https://docs.npmjs.com/getting-started/what-is-npm)”.

### Learn more about Git

You will not need to know Git to complete this tutorial, but it is a very useful tool. If you are interested in learning more about version control, Git, and GitHub, check out GitHub's [Git Handbook](https://guides.github.com/introduction/git-handbook/).
