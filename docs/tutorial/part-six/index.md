---
title: Wtyczki przekształcające
typora-copy-images-to: ./
disableTableOfContents: true
---

> Ten samouczek jest częscią serii o Warstwie Danych Gatsby. Upewnij się, że masz już za sobą [część czwartą](/tutorial/part-four/) i [część piątą](/tutorial/part-five) zanim kontynuujesz.

## Co czeka Cię w tym poradniku?

Poprzednia część pokazała Ci, jak wtyczki źródłowe pobierają dane _do_ systemu Gatsby. W tym samouczku, dowiesz się jak wtyczki przekształcające potrafią _przekształcić_ surową zawartość, która została pobrana przez wtyczki źródłowe. Kombinacja tych dwóch rodzajów wtyczek poradzi sobie z całym problemem pobierania i przekształcania danych, który napotkasz podczas tworzenia strony Gatsby.

## Wtyczki przekształcające (transformer plugins)

Często, format danych którzy otrzymujesz z wtyczek źródłowych nie jest taki, jaki potrzebujesz do stworzenia witryny. Wtyczka źródłowa systemu plików pozwoli Ci na pobranie danych _o_ plikach, ale co jeśli potrzebujesz tych znajdujących się _w środku_ plików?

Aby to było możliwe, Gatsby wspiera wtyczki przekształcające, które biorą surową zawartość z wtyczek źródłowych i _przekształcają_ je w coś bardziej użytecznego.

Na przykład, pliki markdown. W Markdownie świetnie się pisze, ale co kiedy chcesz stworzyć stronę z jego zawartością? Potrzebujesz wtedy przekształcić markdown do HTML.

Dodaj plik markdown to swojej strony:
`src/pages/sweet-pandas-eating-sweets.md` (To będzie Twój pierwszy post na blogu w formacie markdown) i naucz się jak _przekształcić_ go do HTML używając wtyczek przekształcających oraz GraphQL.

```markdown:title=src/pages/sweet-pandas-eating-sweets.md
---
title: "Sweet Pandas Eating Sweets"
date: "2017-08-10"
---

Pandas are really sweet.

Here's a video of a panda eating sweets.

<iframe width="560" height="315" src="https://www.youtube.com/embed/4n0xNbfJLR8" frameborder="0" allowfullscreen></iframe>
```

Po zapisaniu pliku, spójrz na katalog `/my-files/`—nowy plik markdown znajduje się w tabeli. Jest to potężna zaleta Gatsby. Jak we wcześniejszym przykładzie z `siteMetadata`, wtyczki źródłowe potrafią odświeżać dane na żywo. `gatsby-source-filesystem` ciągle skanuje Twój projekt w poszukiwaniu nowych plików i kiedy je znajdzie, odświeża twoje zapytania.

Dodaj teraz wtyczke przekształcającą, która potrafi transformować pliki markdown:

```shell
npm install --save gatsby-transformer-remark
```

Teraz, dodaj ją do `gatsby-config.js` tak jak zawsze:

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
  },
  plugins: [
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `src`,
        path: `${__dirname}/src/`,
      },
    },
    `gatsby-transformer-remark`, // highlight-line
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

Uruchom ponownie serwer deweloperski i odśwież (albo otwórz na nowo) GraphiQL i spójrz na automatyczne uzupełnianie:

![markdown-autocomplete](markdown-autocomplete.png)

Wybierz `allMarkdownRemark` ponownie i uruchom je tak, jak zrobiłeś/aś to wcześniej dla `allFile`. Zobaczysz wtedy nowy plik markdown, który został ostatnio dodany. Odkryj nowe pola które zostały udostępnione na węźle (node) `MarkdownRemark`.

![markdown-query](markdown-query.png)

Ok! Mamy nadzieję, że zaczyna Ci się już wszystko powoli układać. Wtyczki źródłowe pobierają dane _do_ systemu danych Gatsby, a wtyczki _przekształcające_ przekształcają surową zawartość pobraną poprzednio przez wtyczki źródłowe. Ten schemat obsłuży całe pobieranie i przekształcanie danych, których mógłbyś/mogłabyś potrzebować do tworzenia strony przy użyciu Gatsby.

## Stwórz listę plików markdown twojej witryny w `src/pages/index.js`

Teraz będziesz tworzyć listę plików markdown na stronie głównej. Jak na wielu blogach, Twoim końcowym celem jest skończyć z listą odnośników na stronie głównej, kierujących do każdego z postów na blogu. Używając GraphQL możesz tworzyć _zapytania_ o obecną listę postów na blogu, tak aby nie trzeba było zarządzać nią manualnie.

Tak jak na stronie `src/pages/my-files.js`, zmień `src/pages/index.js` na następujący kod aby dodać zapytanie GraphQL z wstępnym HTML i stylami.

```jsx:title=src/pages/index.js
import React from "react"
import { graphql } from "gatsby"
import { css } from "@emotion/core"
import { rhythm } from "../utils/typography"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data)
  return (
    <Layout>
      <div>
        <h1
          css={css`
            display: inline-block;
            border-bottom: 1px solid;
          `}
        >
          Amazing Pandas Eating Things
        </h1>
        <h4>{data.allMarkdownRemark.totalCount} Posts</h4>
        {data.allMarkdownRemark.edges.map(({ node }) => (
          <div key={node.id}>
            <h3
              css={css`
                margin-bottom: ${rhythm(1 / 4)};
              `}
            >
              {node.frontmatter.title}{" "}
              <span
                css={css`
                  color: #bbb;
                `}
              >
                — {node.frontmatter.date}
              </span>
            </h3>
            <p>{node.excerpt}</p>
          </div>
        ))}
      </div>
    </Layout>
  )
}

export const query = graphql`
  query {
    allMarkdownRemark {
      totalCount
      edges {
        node {
          id
          frontmatter {
            title
            date(formatString: "DD MMMM, YYYY")
          }
          excerpt
        }
      }
    }
  }
`
```

Twoja strona tytułowa powinna teraz wyglądać tak:

![strona tytułowa](frontpage.png)

Ale Twój jeden post wygląda trochę samotnie. Dodajmy jescze jeden:
`src/pages/pandas-and-bananas.md`

```markdown:title=src/pages/pandas-and-bananas.md
---
title: "Pandas and Bananas"
date: "2017-08-21"
---

Do Pandas eat bananas? Check out this short video that shows that yes! pandas do
seem to really enjoy bananas!

<iframe width="560" height="315" src="https://www.youtube.com/embed/4SZl1r2O_bY" frameborder="0" allowfullscreen></iframe>
```

![dwa posty](two-posts.png)

Wygląda świetnie! Tylko… kolejność postów jest nieprawidłowa.

Naprawmy to. Kiedy tworzysz zapytania o połączenia jakiegoś typu, możesz przekazać różne argumenty do zapytania GraphQL. Możesz wybrać, czy chcesz sortować (`sort`) albo filtrować (`filter`) któreś z węzłów (nodes), ustawić ile węzłów ominąć (`skip`), a także wybrać ile węzłów chcesz otrzymać, korzystając z argumentu `limit`. Z tym potężnym zestawem operatorów, jesteś w stanie wybrać jakie i ile danych chcesz dokładnie otrzymać.

Na twojej stronie głównej (index page), zmień `allMarkdownRemark` na `allMarkdownRemark(sort: { fields: [frontmatter___date], order: DESC })`. _Uwaga: Pomiędzy `frontmatter` i `date` są trzy podkreślniki._ Zapisz, a kolejność postów powinna być prawidłowa.

Otwórz teraz GraphiQL i popróbuj inne rodzaje sortowania. Możesz sortować połączenie (connection) `allFile` razem z innymi połączeniami.

Aby dowiedzieć się więcej o operatorach zapytań, sprawdź nasz [podręcznik referencyjny GraphQL](/docs/graphql-reference/)

## Wyzwanie

Spróbuj stworzyć nową stronę zawierającą post blogowy i zobacz co się stanie z listą postów na stronie głównej!

## Co dalej?

Idzie Ci świetnie! Właśnie stworzyłeś/aś ładną stronę główną, z której jesteś w stanie wysyłać zapytania o pliki markdown i produkować listę tytułów postów i krótkich fragmentów. Ale prawdopodobnie nie chcesz widzieć tylko fragmentów, a prawdziwe strony dla plików markdown.

Możesz kontynuować tworzyć strony poprzez zamieszczanie komponentów React w `src/pages`. Aczkolwiek, w następnym poradniku nauczysz się jak _programatycznie_ tworzyć strony z otrzymywantch _danych_. Gatsby _nie_ jest ograniczony do tworzenia stron tylko z plików jak inne statyczne generatory stron. Gatsby pozwala ci używać GraphQL aby tworzyć zapytania o _dane_ i _mapować_ otrzymane dane do _stron_—wszystko w czasie budowania. To jest jedna z największych zalet Gatsby. Odkryjesz implikacje tego rozwiązania i różne sposoby jak je wykorzystać w następnym poradniku, gdzie nauczysz się jak [programatycznie tworzyć strony z danych](/tutorial/part-seven/).
