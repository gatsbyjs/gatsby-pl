---
title: Dane w Gatsby
typora-copy-images-to: ./
disableTableOfContents: true
---

Witamy w Części Czwartej naszego poradnika! Połowa drogi już za tobą! Mamy nadzieję, że czujesz się coraz bardziej komfortowo.

## Powtórka z pierwszej części poradnika

Do tej pory, uczyłeś/aś się jak korzystać z React.js - jak potężne potrafi być to narzędzie do tworzenia Twoich _własnych_ komponentów, które zachowują się jak bloki do tworzenia witryn.

Odkryłeś/aś również, jak działa stylowanie komponentów przy użyciu Modułów CSS.

## Co Ciebie czeka w tym poradniku?

W ciągu kolejnych czterech części tych samouczków (włączając ten), dowiesz się, jak działa Warstwa Danych Gatsby, która jest niezwykłą właściwością jego ekosystemu. Pozwoli Ci ona z łatwością budować strony, gdzie źródłem Twoich danych może być Markdown, WordPress, „headless CMS” lub inne.

**UWAGA:** Warstwa Danych Gatsby jest napędzana przez GraphQL. Dla bardziej dogłębnego poradnika odnośnie GraphQL polecamy [How to GraphQL](https://www.howtographql.com/).

## Dane w Gatsby

Strona internetowa składa się z czterech części: HTML, CSS, JS oraz dane. Pierwsza połowa naszych poradników skupiona jest na pierwszych trzech z nich. Teraz nauczmy się, jak wykorzystuje się dane w Gatsby.

**Czym są dane?**

Najbardziej "uczelnianą" odpowiedzią byłoby: dane, to rzeczy takie jak: `"łańcuchy znaków"`, liczby całkowite (`42`), obiekty (`{ pizza: true }`) itp.

Podczas pracy w Gatsby, trafniejszym określeniem jest "wszystko, co istnieje poza komponentem Reacta".

Dotychczas, pisałeś/aś tekst i dodawałeś/aś obrazki _bezpośrednio_ w komponentach.
Jest to _świetnym_ rozwiązaniem na zbudowanie wielu stron internetowych. Ale, często chciałbyś/chciałabyś przechowywać dane _na zewnątrz_ komponentów i dodawać je _do_ do komponentu, kiedy to potrzebne.

Jeśli tworzysz stronę w WordPressie (tak, aby inni kontrybutorzy mieli przyjemny w użyciu interfejs do dodawania i zarządzania zawartością) i Gatsby, _dane_ potrzebne Twojej witrynie (strony i posty) znajdują się w WordPressie, skąd Ty możesz, w razie potrzeby, _pobrać_ te dane, wprost do Twoich komponentów.

Dane mogą także występować w plikach typu Markdown, CSV itp. a także w bazach danych oraz wszelakich API.

**Warstwa Danych Gatsby pozwala ci pobierać dane z tych (i jakichkolwiek innych źródeł) bezpośrednio do Twoich komponentów**—w dowolnym kształcie i postaci.

## Używanie nieustrukturyzowanych danych vs GraphQL

### Czy muszę używać GraphQL i wtyczek źródłowych aby pobierać dane do stron Gatsby?

Absolutnie nie! Możesz skorzystać z node `createPages` API aby pobierać nieustrukturyzowane dane bezpośrednio do stron Gatsby, zamiast poprzez Warstwę Danych Gatsby. Jest to świetnym wyborem dla mniejszych stron, ponieważ GraphQL i wtyczki źródłowe zaoszczędzą Ci trochę więcej czasu, gdy potrzebujesz stworzyć bardziej zaawansowane strony.

Zobacz poradnik [Using Gatsby without GraphQL](/docs/using-gatsby-without-graphql/) aby nauczyć się jak pobierać dane do Twoich stron Gatsby, używając node `createPages` API i przejrzeć przykładową stronę!

### Kiedy mam używać nieustrukturyzowane danych, a kiedy GraphQL?

Jeśli tworzysz mniejszą stronę, jedną z najskuteczniejszych opcji jest pobranie nieustrukturyzowanych danych, tak jak to jest pokazane w poradniku, przy użyciu `createPages` API. Natomiast kiedy Twoja strona stanie się bardziej zaawansowana, zaczniesz budować skomplikowane strony lub gdybyś miał/miała potrzebę przekształcenia Twoich danych, podążaj tymi krokami:

1.  Skorzystaj z [Biblioteki Wtyczek](/plugins/) i sprawdź, czy istnieją już wtyczki źródłowe/przekształcające dane, których potrzebujesz.
2.  Jeśli nie istnieją, przeczytaj samouczek [Tworzenie Wtyczek](/docs/creating-plugins/) i zastanów się nad stworzeniem własnej!

### Jak Warstwa Danych Gatsby korzysta z GraphQL, aby pobierać dane do komponentów

Istnieje wiele opcji na ładowanie danych do komponentów React. Jedną z najbardziej popularnych i potężnych technologii jest [GraphQL](http://graphql.org/).

GraphQL został wynaleziony przez Facebook, aby pomóc inżynierom produktów _pobierać_ tylko te dane, których potrzebują, wprost do komponentów.

GraphQL to język zapytań (część _QL_ od angielskiego **q**uery **l**anguage). Jeśli masz doświadczenie z SQL, to GraphQL działa na bardzo podobnej zasadzie. Używasz specjalnej składni, aby opisać dane, które potrzebujesz w swoim komponencie, a one zostają dostarczane do Ciebie.

Gatsby korzysta z GraphQL, aby pozwolić komponentom na deklarowanie danych, których potrzebują.

## Stwórz przykładową stronę.

Utwórz kolejną nową stronę dla tej części poradnika. Zbudujesz blog Markdown nazywający się "Pandas Eating Lots" (Pandy jedzą dużo). Będzie ona dedykowana do pokazywania najlepszych zdjęć i filmików pand obżerających się jedzeniem. W trakcie tego poradnika zaczniesz korzystać także z GraphQL i wsparcia Markdown Gatsby'ego.

Otwórz nowe okno terminala i uruchom następujące komendy, aby utworzyć nową stronę Gatsby w folderze nazwanym `tutorial-part-four`. Następnie, przejdź do nowo utworzonego katalogu:

```shell
gatsby new tutorial-part-four https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-four
```

Zainstaluj kilka innych potrzebnych zależności w źródle projektu. Będziesz potrzebować motywu Typography "Kirkham" oraz wypróbujesz biblioteki CSS-in-JS, ["Emotion"](https://emotion.sh/):

```shell
npm install --save gatsby-plugin-typography typography react-typography typography-theme-kirkham gatsby-plugin-emotion @emotion/core
```

Ustaw stronę podobnie, do tej, z którą skonczyłeś/aś w [Części Trzeciej](/tutorial/part-three). Będzie ona miała komponent layout (układ) oraz dwa inne komponenty stron:

```jsx:title=src/components/layout.js
import React from "react"
import { css } from "@emotion/core"
import { Link } from "gatsby"

import { rhythm } from "../utils/typography"

export default ({ children }) => (
  <div
    css={css`
      margin: 0 auto;
      max-width: 700px;
      padding: ${rhythm(2)};
      padding-top: ${rhythm(1.5)};
    `}
  >
    <Link to={`/`}>
      <h3
        css={css`
          margin-bottom: ${rhythm(2)};
          display: inline-block;
          font-style: normal;
        `}
      >
        Pandas Eating Lots
      </h3>
    </Link>
    <Link
      to={`/about/`}
      css={css`
        float: right;
      `}
    >
      About
    </Link>
    {children}
  </div>
)
```

```jsx:title=src/pages/index.js
import React from "react"
import Layout from "../components/layout"

export default () => (
  <Layout>
    <h1>Amazing Pandas Eating Things</h1>
    <div>
      <img
        src="https://2.bp.blogspot.com/-BMP2l6Hwvp4/TiAxeGx4CTI/AAAAAAAAD_M/XlC_mY3SoEw/s1600/panda-group-eating-bamboo.jpg"
        alt="Group of pandas eating bamboo"
      />
    </div>
  </Layout>
)
```

```jsx:title=src/pages/about.js
import React from "react"
import Layout from "../components/layout"

export default () => (
  <Layout>
    <h1>About Pandas Eating Lots</h1>
    <p>
      We're the only site running on your computer dedicated to showing the best
      photos and videos of pandas eating lots of food.
    </p>
  </Layout>
)
```

```javascript:title=src/utils/typography.js
import Typography from "typography"
import kirkhamTheme from "typography-theme-kirkham"

const typography = new Typography(kirkhamTheme)

export default typography
export const rhythm = typography.rhythm
```

`gatsby-config.js` (musi znajdować się w ścieżce źródłowej Twojego projektu, nie w katalogu src)

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
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

Dodaj powyższe pliki i uruchom komendę `gatsby develop`, tak jak wcześniej. Następnie, powinieneś/powinnaś ujrzeć:

![start](start.png)

Udało Ci się stworzyć kolejną witrynę z layoutem i dwoma stronami.

Teraz możesz zacząc tworzyć zapytania o dane 😋

## Twoje pierwsze query (zapytanie) GraphQL

Kiedy tworzysz strony internetowe, prawdopodobnie zaczniesz chcieć ponownie wykorzystywać wspólne części danych — na przykład _tytuł strony_. Spójrz na stronę `/about/`. Zauważysz, że Twój tytuł (`Pandas Eating Lots`) występuję naraz w komponencie layout (nagłówek witryny), a także w `<h1 />` strony `about.js` (nagłówek strony).

Ale co jeśli chcesz zmienić tytuł witryny w przyszłości? Musiałbyś/Musiałabyś przeszukać wszystkie komponenty i edytować każdy z nich. Staje się to niewygodne i podatne na błędy, w szczególności dla większych, bardziej zaawansowanych stron. Zamiast tego, możesz przechowywać tytuł w jednym miejscu i odnosić się do niego z innych plików; zmień tytuł w jednym miejscu, a Gatsby _ściągnie_ Twój nowy tytuł do plików, które się do niego odnoszą.

Miejsce na tego typu wspólne dane to obiekt `siteMetadata` w pliku `gatsby-config.js`. Dodaj swój nowy tytuł do pliku `gatsby-config.js`:

```javascript:title=gatsby-config.js
module.exports = {
  // highlight-start
  siteMetadata: {
    title: `Title from siteMetadata`,
  },
  // highlight-end
  plugins: [
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

Uruchom ponownie serwer deweloperski.

### Zapytaj o nowy tytuł

Od teraz, Twój tytuł strony jest możliwy do ściągnięcia; Dodaj zapytanie do pliku `about.js` korzystając z [page query](/docs/page-query):

```jsx:title=src/pages/about.js
import React from "react"
import { graphql } from "gatsby" // highlight-line
import Layout from "../components/layout"

// highlight-next-line
export default ({ data }) => (
  <Layout>
    <h1>About {data.site.siteMetadata.title}</h1> {/* highlight-line */}
    <p>
      We're the only site running on your computer dedicated to showing the best
      photos and videos of pandas eating lots of food.
    </p>
  </Layout>
)

// highlight-start
export const query = graphql`
  query {
    site {
      siteMetadata {
        title
      }
    }
  }
`
// highlight-end
```

Udało się! 🎉

![Strona z nowym tytułem pobranym z siteMetadata](site-metadata-title.png)

Podstawowe zapytanie GraphQL, które otrzymuje `title` (tytuł) w Twoim pliku `about.js` to:

```graphql:title=src/pages/about.js
{
  site {
    siteMetadata {
      title
    }
  }
}
```

> 💡 W [części piątej](/tutorial/part-five/#introducing-graphiql), poznasz narzędzie, które pozwoli Ci interaktywnie przeszukiwać dane dostępne poprzez GraphQL i pomoże Ci formułować zapytania, takie jak te wyżej.

Zapytania stron (page queries) znajdują się poza definicją komponentu — według konwencji umieszczane na końcu pliku komponentu — i możliwe są do użycia tylko dla komponentów stron.

### Użyj StaticQuery

[StaticQuery (zapytanie statyczne)](/docs/static-query/) to nowe API wprowadzone w Gatsby v2 które pozwala komponentom innym niż strony (takim jak Twój komponent `layout.js`), na otrzymywanie danych poprzez zapytania GraphQL.
Użyjmy nowo prowadzonej wersji hookowej — [`useStaticQuery`](/docs/use-static-query/).

Śmiało, dokonaj kilku zmian w swoim pliku `src/components/layout.js` aby użyć hooka `useStaticQuery` i odnieś się do `{data.site.siteMetadata.title}`, który wskazuje na Twój tytuł strony. Kiedy skończysz, Twój komponent będzie wyglądał tak:

```jsx:title=src/components/layout.js
import React from "react"
import { css } from "@emotion/core"
// highlight-next-line
import { useStaticQuery, Link, graphql } from "gatsby"

import { rhythm } from "../utils/typography"
// highlight-start
export default ({ children }) => {
  const data = useStaticQuery(
    graphql`
      query {
        site {
          siteMetadata {
            title
          }
        }
      }
    `
  )
  return (
    // highlight-end
    <div
      css={css`
        margin: 0 auto;
        max-width: 700px;
        padding: ${rhythm(2)};
        padding-top: ${rhythm(1.5)};
      `}
    >
      <Link to={`/`}>
        <h3
          css={css`
            margin-bottom: ${rhythm(2)};
            display: inline-block;
            font-style: normal;
          `}
        >
          {data.site.siteMetadata.title} {/* highlight-line */}
        </h3>
      </Link>
      <Link
        to={`/about/`}
        css={css`
          float: right;
        `}
      >
        About
      </Link>
      {children}
    </div>
    // highlight-start
  )
}
// highlight-end
```

Kolejny Sukces! 🎉

![Tytuł strony i tytuł layout razem pobierają dane z siteMetadata](site-metadata-two-titles.png)

Dlaczego użyliśmy dwóch różnych zapytań? Te przykłady były szybkim wprowadzeniem do różnych rodzajów zapytań, jak są formatowane i gdzie można z nich skorzystać. Zapamiętaj, że tylko strony mogą tworzyć zapytania stronowe (page queries).
Komponenty, które nie są stronami (non-page components), takie jak Layout, powinny korzystać z zapytania statycznego (StaticQuery).
[Część siódma](/tutorial/part-seven) poradnika wytłumaczy Ci te rodzaje zapytań bardziej dogłębnie.

Przywróćmy teraz oryginalny tytuł.

Jedną z głównych zasad Gatsby jest to, że _twórcy potrzebują błyskawicznej więzi z tym, co tworzą_ ([ukłon dla Breta Victora](http://blog.ezyang.com/2012/02/transcript-of-inventing-on-principle/)). W innych słowach, kiedy zrobisz jakąkolwiek zmianę w kodzie, powinieneś/powinnaś natychmiastowo widzieć efekt tej zmiany. To Ty zmieniasz dane wejściowe, a Gatsby zajmie się ich wyświetleniem.

Więc praktycznie wszędzie, Twoje zmiany będą miały natychmiastowy efekt. Zedytuj plik `gatsby-config.js` ponownie, tym razem zmieniając `title` (tytuł) z powrotem na "Pandas Eating Lots". Zmiana powinna pojawić się błyskawicznie na twoich stronach.

![Oba tytuły zawierające Pandas Eating Lots](pandas-eating-lots-titles.png)

## Co czeka Cię w kolejnym poradniku?

Następnie, w [części piątej](/tutorial/part-five/) samouczka, dowiesz się jak pobierać dane do Twoich stron Gatsby używając GraphQL z wtyczkami źródłowymi.
