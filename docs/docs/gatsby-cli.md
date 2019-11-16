---
title: Komendy (Gatsby CLI)
tableOfContentsDepth: 2
---

Interfejs wiersza poleceń Gatsby (CLI) to główny punkt wejściowy do rozpoczęcia pracy nad aplikacją Gatsby oraz do używania funkcjonalności, takich jak uruchamianie serwera deweloperskiego czy budowanie wersji produkcyjnej twojej aplikacji Gatsby.

_Zapewniamy podobną dokumentację dostępną razem z [README](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-cli/README.md) gatsby-cli, a nasz [cheat sheet](/docs/cheat-sheet/) zawiera wszystkie najważniejsze komendy CLI gotowe do wydrukowania_

## Jak używać gatsby-cli

Gatsby CLI (`gatsby-cli`) is packaged as an executable that can be used globally. Gatsby CLI jest dostępne poprzez [npm](https://www.npmjs.com/) i powinno być zainstalowane globalnie poprzez uruchomienie komendy `npm install -g gatsby-cli` żeby móc używać go lokalnie.

Uruchom komendę `gatsby --help` w celu uzyskania pełnej pomocy.

Możesz także użyć opisanych tu komend jako skryptów w `package.json`, które zwykle są wystawione _dla ciebie_ w większości [starterów](/docs/starters/). Na przykład, jeśli chcesz, aby komenda [`gatsby develop`](#develop) była dostępna w twojej aplikacji, otwórz plik `package.json` i dodaj skrypt jak poniżej:

```json:title=package.json
{
  "scripts": {
    "develop": "gatsby develop"
  }
}
```

## API komend

### `new`

```
gatsby new [<site-name> [<starter-url>]]
```

#### Argumenty

| Argument    | Opis                                                                                                                                                                                                                                   |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| site-name   | Nazwa twojej strony Gatsby, która zostanie również użyta, żeby stworzyć katalog projektu.                                                                                                                                              |
| starter-url | URL startera Gatsby lub ścieżka lokalnego pliku. Domyślnie [gatsby-starter-default](https://github.com/gatsbyjs/gatsby-starter-default); zobacz dokumentację [starterów Gatsby](/docs/gatsby-starters/) aby uzyskać więcej informacji. |

> Uwaga: `site-name` powinna zawierać tylko litery i cyfry. Jeśli w nazwie zawrzesz `.`, `./` lub `<spację>`, `gatsby new` wyrzuci błąd.

#### Przykłady

- Stwórz stronę Gatsby o nazwie `my-awesome-site` używając domyślnego startera:

```shell
gatsby new my-awesome-site
```

- Stwórz stronę Gatsby o nazwie `my-awesome-blog-site` używając [gatsby-starter-blog](https://www.gatsbyjs.org/starters/gatsbyjs/gatsby-starter-blog/):

```shell
gatsby new my-awesome-blog-site https://github.com/gatsbyjs/gatsby-starter-blog
```

- Jeśli pozostawisz oba argumenty puste, CLI uruchomi interaktywną powłokę prosząc o wprowadzenie tych danych:

```shell
gatsby new
? What is your project called? › my-gatsby-project
? What starter would you like to use? › - Use arrow-keys. Return to submit.
❯  gatsby-starter-default
   gatsby-starter-hello-world
   gatsby-starter-blog
   (Use a different starter)
```

Zobacz dokumentację [starterów Gatsby](/docs/gatsby-starters/) aby uzyskać więcej informacji.

### `develop`

Po zainstalowaniu strony Gatsby, przejdź do katalogu głównego twojego projektu i uruchom serwer deweloperski:

`gatsby develop`

#### Opcje

|      Opcja      | Opis                                              |
| :-------------: | ------------------------------------------------- |
| `-H`, `--host`  | Ustawienie hosta. Domyślnie localhost             |
| `-p`, `--port`  | Ustawienie portu. Domyślnie 8000                  |
| `-o`, `--open`  | Otwarcie strony w twojej (domyślnej) przeglądarce |
| `-S`, `--https` | Użycie HTTPS                                      |

Follow the [Local HTTPS guide](/docs/local-https/)
to find out how you can set up an HTTPS development server using Gatsby.

#### Podgląd zmian na innych urządzeniach

Możesz użyć komendy `gatsby develop` z opcją hosta, żeby uzyskać dostęp do środowiska programistycznego na innych urządzeniach. Uruchom:

```shell
gatsby develop -H 0.0.0.0
```

Następnie terminal wyświetli standardowe informacje, ale dodatkowo dołączy URL, na który możesz przejść z innego urządzenia będącego w tej samej sieci aby zobaczyć jak strona jest wyświetlana.

```
You can now view gatsbyjs.org in the browser.
⠀
  Local:            http://0.0.0.0:8000/
  On Your Network:  http://192.168.0.212:8000/ // highlight-line
```

**Uwaga**: Nie możesz odwiedzić adresu 0.0.0.0:8000 w systemie Windows (jednak wszystko zadziała poprawnie przy użyciu localhost:8000 lub pokazanego w terminalu adresu URL "On Your Network" na Windows)

### `build`

Uruchom komendę w katalogu głównym strony Gatsby, aby zbudować wersję produkcyjną aplikacji i przygotować ją do wdrożenia:

`gatsby build`

#### Opcje

|            Opcja             | Opis                                                                                                                        |
| :--------------------------: | --------------------------------------------------------------------------------------------------------------------------- |
|       `--prefix-paths`       | Buduje stronę dodając prefiks do ścieżek (ustaw pathPrefix w twojej konfiguracji)                                           |
|        `--no-uglify`         | Buduje stronę bez optymalizacji plików JS (do debuggowania)                                                                 |
| `--open-tracing-config-file` | Plik konfiguracyjny śledzenia (zgodny z OpenTracing). Zobacz dokumentację [Performance Tracing](/docs/performance-tracing/) |
| `--no-color`, `--no-colors`  | Wyłącza wyświetlanie kolorów w terminalu                                                                                    |

In addition to these build options, there are some optional [build environment variables](/docs/environment-variables/#build-variables) for more advanced configurations that can adjust how a build runs. For example, setting `CI=true` as an environment variable will tailor output for [dumb terminals](https://en.wikipedia.org/wiki/Computer_terminal#Dumb_terminals).

### `serve`

Uruchom komendę w katalogu głównym strony Gatsby, aby zaserwować zbudowaną wersję produkcyjną twojej strony w celu jej przetestowania.

`gatsby serve`

#### Opcje

|      Opcja       | Opis                                                                                                                     |
| :--------------: | ------------------------------------------------------------------------------------------------------------------------ |
|  `-H`, `--host`  | Ustawienie hosta. Domyślnie localhost                                                                                    |
|  `-p`, `--port`  | Ustawienie portu. Domyślnie 9000                                                                                         |
|  `-o`, `--open`  | Otwiera stronę w twojej (domyślnej) przeglądarce                                                                         |
| `--prefix-paths` | Serwuje stronę z dodanymi prefiksami do ścieżek (jeśli została zbudowana z użyciem pathPrefix w twoim gatsby-config.js). |

### `info`

Uruchom komendę w katalogu głównym strony Gatsby, aby uzyskać przydatne informacje o środowisku, które będą wymagane przy raportowaniu błędów:

`gatsby info`

#### Opcje

|        Opcja        | Opis                                                     |
| :-----------------: | -------------------------------------------------------- |
| `-C`, `--clipboard` | Automatycznie kopiuje informacje środowiskowe do schowka |

### `clean`

Uruchom komendę w katalogu głównym strony Gatsby, aby wyczyścić cache (folder `.cache`) oraz katalogi publiczne:

`gatsby clean`

Jest to przydatne jako ostatnia deska ratunku, gdy twój lokalny projekt wydaje się mieć problemy lub zawartość się nie odświeża. Częste problemy, które to działanie może rozwiązać to m.in.:

- Nieaktualne dane, np. dany plik/zasób/itp. nie pojawia się
- Błąd GraphQL, np. dany zasób GraphQL powinien być obecny, a nie jest
- Problemy zależności, np. niepoprawna wersja, niejasne błędy w konsoli itp.
- Problemy wtyczek, np. gdy pracujesz nad lokalną wtyczną i wprowadzane zmiany nie przynoszą rezultatów


### `plugin`

Uruchamia komendy typowe dla wtyczek Gatsby.

#### `docs`

`gatsby plugin docs`

Kieruje cię do dokumentacji dotyczącej użycia i tworzenia wtyczek.

### Repl

Get a Node.js REPL (interactive shell) with context of your Gatsby environment:

`gatsby repl`

Gatsby will prompt you to type in commands and explore. When it shows this: `gatsby >`

You can type in a command, such as one of these:

`babelrc`

`components`

`dataPaths`

`getNodes()`

`nodes`

`pages`

`schema`

`siteConfig`

`staticQueries`

When combined with the [GraphQL explorer](/docs/introducing-graphiql/), these REPL commands could be very helpful for understanding your Gatsby site's data.

For more information, check out the [Gatsby REPL documentation](/docs/gatsby-repl/).

### Disabling colored output

In addition to the explicit `--no-color` option, the CLI respects the presence of the `NO_COLOR` environment variable (see [no-color.org](https://no-color.org/)).
