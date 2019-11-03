---
title: Source Plugins
typora-copy-images-to: ./
disableTableOfContents: true
---

> Ten poradnik jest częścią serii na temat warstwy danych w Gatsby. Upewnij się, że zapoznałeś się z [częścią czwartą](/tutorial/part-four/) znaim będziesz kontynuował.

## Czego nauczysz się w tym poradniku?

W tym poradniku dowiesz się, jak pobierać dane do strony Gatsby przy użyciu GraphQL i wtyczek źródłowych. Zanim jednak dowiesz się o tych wtyczkach, najpierw musisz wiedzieć, jak korzystać z czegoś co nazywa się GraphiQL - narzędzia, które pomaga ułożyć strukturę Twoich zapytań.

## Przedstawiamy GraphiQL

GraphiQL jest zintegrowanym środowiskiem deweloperskim (IDE) w GraphQL. Jest to potężne (i pod wieloma względami niesamowite) narzędzie, którego będziesz często używać podczas tworzenia stron internetowych w Gatsby.

Masz do niego dostęp, gdy Serwer Deweloperski Twojej strony działa zwyczajnie pod adresem
<http://localhost:8000/___graphql>.

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="/graphiql-explore.mp4"></source>
  <p>Twoja przeglądarka nie obsługuje tego elementu wideo.</p>
</video>

Zapoznaj się z "typem" `Site` i sprawdź jakie pola są dostępne -- sprawdź też obiekt `siteMetadata` do którego zapytanie wykonałeś wcześniej. Spróbuj otworzyć GraphiQL i pobawić się trochę ze swoimi danymi! Wciśnij <kbd>Ctrl + Spacja</kbd> (lub <kbd>Shift + Spacja</kbd> jako alternatywny skrót), żeby wyświetlić okno autouzupełniania i <kbd>Ctrl + Enter</kbd>, aby wykonać zapytanie GraphQL. Będziesz używał GraphiQL znacznie częściej w dalszej części poradnika.

## Używanie Eksploratora GraphiQL

Eksplorator GraphiQL pozwoli Ci interaktywnie budować kompletne zapytania poprzez kliknięcia w dostępne pola oraz wejścia/inputy, oszczędzając Ci żmudnego procesu wpisywania tych zapytań ręcznie na klawiaturze.

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-build-a-graphql-query-using-gatsby-s-graphiql-explorer"
  lessonTitle="Build a GraphQL Query using Gatsby’s GraphiQL Explorer"
/>

## Wtyczki Sources

Dane w stronach Gatsby mogą pochodzić z różnych źródeł: API, bazy danych, CMS, pliki lokalne, itp.

Wtyczki Sources pobierają dane ze swoejgo źródła. Np. wtyczka `filesystem source` wie jak pobierać dane z systemu plików. Wtyczka WordPress wie jak pobierać dane z WordPress API.

Dodaj wtyczkę [`gatsby-source-filesystem`](/packages/gatsby-source-filesystem/) i sprawdź jak działa.

Po pierwsze, zainstaluj wtyczkę do katalogu głównego projektu:

```shell
npm install --save gatsby-source-filesystem
```

Następnie dodaj wtyczkę do pliku `gatsby-config.js`:

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
  },
  plugins: [
    // highlight-start
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `src`,
        path: `${__dirname}/src/`,
      },
    },
    // highlight-end
    `gatsby-plugin-emotion`,
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    },
  ],
}
```

Zapisz plik i uruchom serwer deweloperski Gatsby. Następnie otwórz ponownie GraphiQL.

W panelu Explorer, zobaczysz `allFile` oraz `file` jako dostępny wybór:

![graphiql-filesystem](graphiql-filesystem.png)

Kliknij w menu `allFile`. Ustaw kursor zaraz za `allFile` w obszarze zapytania, a następnie wciśnij <kbd>Ctrl + Enter</kbd>. Uruchomi to autouzupełnienie zapytania o `id` każdego z plików. Wciśnij "Play" aby wykonać zapytanie:

![filesystem-query](filesystem-query.png)

W panelu Explore, pole `id` zostało automatycznie zaznaczone. Wybierz więcej kategorii pól klikając w odpowiadające im pola wyboru. Wciśnij "Play" aby wykonać zapytanie, ponownie, tym razem z nowymi polami:

![filesystem-explorer-options](filesystem-explorer-options.png)

Alternatywnie, możesz dodawać pola używając skrótu autouzupełnienia (<kbd>Ctrl + Space</kbd>). To pokaże wszystkie dostępne pola dla zapytań na Węzłach (nodes) `File`.

![filesystem-autocomplete](filesystem-autocomplete.png)

Spróbuj teraz dodać większą liczbę pól do twojego zapytania, wciskając po każdym razie <kbd>Ctrl + Enter</kbd>
by wykonywać kolejne zapytania. Zobaczysz aktualizujące się wyniki zapytań:

![allfile-query](allfile-query.png)

Wynikiem jest tablica "węzłów" `File` (węzeł to takie ekstrawaganckie słówko na obiekt w "graph").
Każdy obiekt-węzeł `File` zawiera pola dla których wykonałeś zapytanie.

## Zbuduj stronę używając zapytań GraphQL

Budowanie nowych stron z Gatsby często rozpoczyna się w GraphiQL. Zaczynasz najpierw
tworzyć szkic zapytań o dane metodą prób i błędów w GraphiQL,
a potem kopiujesz działające już zapytania do komponentu strony React by rozpocząć budowę UI.

Spróbujmy tego.

Stwórz nowy plik w `src/pages/my-files.js` wraz z zapytaniem GraphQL `allFile`, 
które przed chwilą tworzyłeś:

```jsx:title=src/pages/my-files.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data) // highlight-line
  return (
    <Layout>
      <div>Hello world</div>
    </Layout>
  )
}

export const query = graphql`
  query {
    allFile {
      edges {
        node {
          relativePath
          prettySize
          extension
          birthTime(fromNow: true)
        }
      }
    }
  }
`
```

Wyróżniliśmy powyżej linię `console.log(data)`. Gdy tworzymy nowy komponent,
często pomocnym okazuje się, by wyświetlić w konsoli dane które otrzymujemy z zapytań do GraphQL,
aby można było sprawdzać sobie te dane w konsoli przeglądarki, podczas budowania UI.

Jeśli odwiedzisz nową stronę pod adresem `/my-files/` i otworzysz konsolę przeglądarki której używasz,
zobaczysz coś co wygląda mniej więcej tak:

![data-in-console](data-in-console.png)

Format otrzymanych danych odpowiada formatowi zapytania GraphQL.

Dodaj teraz kod do twojego komponentu by wyświetlić dane Pliku.

```jsx:title=src/pages/my-files.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data)
  return (
    <Layout>
      {/* highlight-start */}
      <div>
        <h1>My Site's Files</h1>
        <table>
          <thead>
            <tr>
              <th>relativePath</th>
              <th>prettySize</th>
              <th>extension</th>
              <th>birthTime</th>
            </tr>
          </thead>
          <tbody>
            {data.allFile.edges.map(({ node }, index) => (
              <tr key={index}>
                <td>{node.relativePath}</td>
                <td>{node.prettySize}</td>
                <td>{node.extension}</td>
                <td>{node.birthTime}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
      {/* highlight-end */}
    </Layout>
  )
}

export const query = graphql`
  query {
    allFile {
      edges {
        node {
          relativePath
          prettySize
          extension
          birthTime(fromNow: true)
        }
      }
    }
  }
`
```

A teraz wejdź pod adres [http://localhost:8000/my-files](http://localhost:8000/my-files)… 😲

![my-files-page](my-files-page.png)

## Czego nauczysz się w nastęnej części?

Teraz wiesz już jak wtyczki Source wprowadzają dane _do wnętrza_ sytemu danych Gatsby. W kolejnym poradniku nauczymy cię jak wtyczki Transformer _transformują_ surowe dane wprowadzone przez wtyczki Source. Kombinacja wtyczek Source i Transformer potrafi kontrolować wszystkie źródła oraz transformacje danych, których możesz potrzebować budując strony Gatsby. Naucz się wtyczek Transformer w [części szóstej poradnika](/tutorial/part-six/).
