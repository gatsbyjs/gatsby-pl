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

### Zainstaluj Gatsby CLI.

```shell
npm install -g gatsby-cli
```

### Stwórz nową stronę.

```shell
gatsby new gatsby-site
```

### Zmień katalog na ten z Twoją stroną.

```shell
cd gatsby-site
```

### Włącz serwer deweloperski.

```shell
gatsby develop
```

Gatsby uruchomi środowisko deweloperskie z natychmiastowym odświeżaniem, dostępne domyślnie na `localhost:8000`.

Spróbuj dokonać edycji stron JavaScript w `src/pages`. Zapisane zmiany odświeżą się natychmiastowo w przeglądarce.

### Zbuduj wersję produkcyjną.

```shell
gatsby build
```

Gatsby przeprowadzi zoptymalizowaną kompilację produkcyjną dla Twojej strony, generując przy tym statyczne pliki HTML i pakiety JavaScript dla każdej ściezki.

### Zaserwuj wersję produkcyjną lokalnie.

```shell
gatsby serve
```

Gatsby rozpocznie lokalny serwer HTML aby przetestować Twoją zbudowaną stronę. Pamiętaj, żeby zbudować stronę używając `gatsby build` przed użyciem tej komendy.

### Sprawdź dokumentację dla komend CLI

Aby zobaczyć szczegółową dokumentację dla komend CLI, uruchom `gatsby --help` w terminalu.

Dla konkretnych komend, uruchom `gatsby NAZWA_KOMENDY --help` np. `gatsby new --help`.

Po więcej informacji o Gatsby CLI, odwiedź [sekcję o CLI](/docs/gatsby-cli/) w dokumentacji.
