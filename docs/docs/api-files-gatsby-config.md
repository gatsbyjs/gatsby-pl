---
title: Plik API gatsby-config.js
---

W pliku `gatsby-config.js` zdefiniowane zostać mogą metadane twojej strony, wtyczki, oraz reszta ogólnej konfiguracji. Plik ten powinien znajdować się w głównym katalogu twojej strony Gatsby.

## Przygotowanie pliku konfiguracyjnego

## Przygotowanie pliku konfiguracyjnego

Plik konfiguracyjny powinien eksportować obiekt JavaScript.  Wewnątrz tego obiektu możesz zdefiniować kilka różnych opcji konfiguracyjnych.
```

```javascript:title=gatsby-config.js
module.exports = {
  //obiekt konfiguracyjny
}
```

Przykład poprawnego wyglądu pliku `gatsby-config.js`:

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Gatsby`,
  },
  plugins: [
    `gatsby-transform-plugin`,
    {
      resolve: `gatsby-plugin-name`,
      options: {
        optionA: true,
        optionB: `Another option`,
      },
    },
  ],
}
```

## Opcje konfiguracyjne

Jest [wiele dostępnych opcji konfiguracyjnych](/docs/gatsby-config), ale najczęstszym zestawieniem jest ustawienie metadanych oraz włączonych wtyczek.

### Metadane strony

Obiekt `siteMetadata` może zawierać dowolne dane które chcesz wykorzystać później na swojej stronie. Właściwym użyciem metadanych jest np. tytuł strony. Jeśli przechowujesz tytuł w `siteMetadata` - możesz zmienić go w jednym miejscu, a ten zaktualizuje się wszędzie gdzie był użyty. Aby dodać metadane umieść obiekt `siteMetadata` w swoim pliku konfiguracyjnym:

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Gatsby`,
  },
}
```

Teraz [dostęp do tytułu strony możesz wykorzystać poprzez GraphQL](/tutorial/part-four/#your-first-graphql-query) w dowolnym miejscu na stronie.

### Wtyczki

Wtyczki dodają nowe funkcjonalności na twojej stronie Gatsby. Na przykład, niektóre wtyczki pobierają dane z usług hostowanych, przekształcają formaty danych lub zmieniają rozmiar obrazów. [Biblioteka wtyczek Gatsby](/plugins) pomoże ci znaleźć odpowiednią wtyczkę dostosowaną do twoich potrzeb.

Instalacja wtyczki poprzez poprzez użycie menedżera paczek jak np. `npm` **nie spowoduje** włączenia jej na twojej stronie Gatsby. Aby poprawnie ukończyć proces dodawania wtyczki, upewnij się że twój plik `gatsby-config.js` zawiera tablicę `plugins`. Jest to miejsce na wszystkie potrzebne wtyczki do zbudowania strony:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    //umieść wtyczki tutaj
  ],
}
```

Podczas dodawania kolejnych wtyczek w tablicy `plugins`, powinny być one rozdzielane przecinkiem, tak aby spełniały poprawną składnie JavaScript.

#### Wtyczki bez opcji konfiguracyjnych

Jeśli wtyczka nie potrzebuje żadnych opcji konfiguracyjnych, możesz dodać jej nazwę w string'u do tablicy `plugins`:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-name`],
}
```

#### Wtyczki z opcjami konfiguracyjnymi

Wiele wtyczek posiada opcjonalne lub wymagane warianty konfiguracji. Zamiast dodawać nazwę w string'u do tablicy `plugins`, dodaj obiekt z nazwą i opcjami konfiguracyjnymi. Większość wtyczek ma pokazane przykłady konfiguracji w swoim pliku `README` lub na ich stronie w [bibliotece wtyczek Gatsby](/plugins).

To jest przykład który pokazuje jak poprawnie napisać obiekt z kluczami `resolve` - do wyciągania nazwy wtyczki oraz obiektem `options` z dowolnymi odpowiednimi ustawieniami:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-name`,
      options: {
        optionA: true,
        optionB: `Another option`,
      },
    },
  ],
}
```

#### Wtyczki mieszane

Możesz dodać wtyczki z lub bez opcji konfiguracyjnych w tej samej tablicy. Twój plik konfiguracyjny strony mógłby wyglądać tak:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    `gatsby-transform-plugin`,
    {
      resolve: `gatsby-plugin-name`,
      options: {
        optionA: true,
        optionB: `Another option`,
      },
    },
  ],
}
```

## Dodatkowe opcje konfiguracyjne

Jest wiele innych opcji konfiguracyjnych dostępnych dla `gatsby-config.js`. Możesz znaleźć listę dla każdej opcji na stronie [API Konfiguracji Gatsby](/docs/gatsby-config/).
