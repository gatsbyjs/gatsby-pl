---
title: Szybki Start
---

Ten szybki start jest przeznaczony dla średnio-zaawansowanych i zaawansowanych deweloperów. Po łagodniejsze wprowadzenie do Gatsby zapraszamy do [naszego poradnika](/tutorial/)!

## Użyj Gatsby CLI

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-quick-start-with-gatsby-create-develop-and-build-gatsby-sites-from-the-command-line"
  lessonTitle="Quick Start with Gatsby: Create, Develop, and Build Gatsby Sites From the Command Line"
/>

**Uwaga**: Ten filmik korzysta z `npx`, które jest narzędziem uruchamiającym paczki npm bez potrzeby ich instalacji. Uruchomienie komendy `npx gatsby new` jest tym samym co `gatsby new`, jeśli masz zainstalowane gatsby-cli na swoim komputerze.

<<<<<<< HEAD
### Zainstaluj Gatsby CLI.
=======
### Install the Gatsby CLI
>>>>>>> 79b09bc29f133961f3d7de0f36a25ff727e6c22a

```shell
npm install -g gatsby-cli
```

<<<<<<< HEAD
### Stwórz nową stronę.
=======
### Create a new site
>>>>>>> 79b09bc29f133961f3d7de0f36a25ff727e6c22a

```shell
gatsby new gatsby-site
```

<<<<<<< HEAD
### Zmień katalog na ten z Twoją stroną.
=======
### Change directories into site folder
>>>>>>> 79b09bc29f133961f3d7de0f36a25ff727e6c22a

```shell
cd gatsby-site
```

<<<<<<< HEAD
### Włącz serwer deweloperski.
=======
### Start development server
>>>>>>> 79b09bc29f133961f3d7de0f36a25ff727e6c22a

```shell
gatsby develop
```

<<<<<<< HEAD
Gatsby uruchomi środowisko deweloperskie z natychmiastowym odświeżaniem, dostępne domyślnie na `localhost:8000`.
=======
Gatsby will start a hot-reloading development environment accessible by default at `http://localhost:8000`.
>>>>>>> 79b09bc29f133961f3d7de0f36a25ff727e6c22a

Spróbuj dokonać edycji stron JavaScript w `src/pages`. Zapisane zmiany odświeżą się natychmiastowo w przeglądarce.

<<<<<<< HEAD
### Zbuduj wersję produkcyjną.
=======
### Create a production build
>>>>>>> 79b09bc29f133961f3d7de0f36a25ff727e6c22a

```shell
gatsby build
```

Gatsby przeprowadzi zoptymalizowaną kompilację produkcyjną dla Twojej strony, generując przy tym statyczne pliki HTML i pakiety JavaScript dla każdej ściezki.

<<<<<<< HEAD
### Zaserwuj wersję produkcyjną lokalnie.
=======
### Serve the production build locally
>>>>>>> 79b09bc29f133961f3d7de0f36a25ff727e6c22a

```shell
gatsby serve
```

Gatsby rozpocznie lokalny serwer HTML aby przetestować Twoją zbudowaną stronę. Pamiętaj, żeby zbudować stronę używając `gatsby build` przed użyciem tej komendy.

### Sprawdź dokumentację dla komend CLI

Aby zobaczyć szczegółową dokumentację dla komend CLI, uruchom `gatsby --help` w terminalu.

Dla konkretnych komend, uruchom `gatsby NAZWA_KOMENDY --help` np. `gatsby new --help`.

Po więcej informacji o Gatsby CLI, odwiedź [sekcję o CLI](/docs/gatsby-cli/) w dokumentacji.
