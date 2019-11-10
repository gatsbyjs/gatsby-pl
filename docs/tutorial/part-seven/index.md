---
title: Programatyczne tworzenie stron z danych
typora-copy-images-to: ./
disableTableOfContents: true
---

> Ten poradnik jest częścią serii na temat Warstwy Danych Gatsby. Upewnij się, że zapoznałeś się wcześniej z [częścią 4](/tutorial/part-four/), [częścią 5](/tutorial/part-five/), oraz [częścią 6](/tutorial/part-six/) zanim będziesz kontynuował.

## Czego dowiesz się w tym poradniku?

W poprzednim poradniku, tworzyłeś niezłą stronę index ktora wykonywała zapytania do plików 
Markdown i wyświetalała listę tytułów i fragmentów blog postów. Jednak nie chcemy widzieć samych fragmentów, chcielibyśmy właściwych stron dla naszych plików Markdown.

Moglibyśmy kontynuować tworzyć strony poprzez umieszczanie kolejnych komponentów React'a w 
folderze `src/pages`. Jednak teraz nauczymy się jak _programatycznie_ tworzyć strony z 
_danych_. Gatsby _nie_ jest ograniczony do tworzenia stron z plików, jak wiele innych 
generatorów stron statycznych. Gatsby pozwala Toboie używać GraphQL, by pytać o twoje _dane_ i _map'ować_ wyniki zpaytań na _strony_-wszystko podczas procesu budowania strony. Taka 
koncepcja daje potężne możliwości. Nauczysz się zastosowań i sposobów na użycie tych możliwości w dalszej części poradnika.

Zaczynajmy.

## Tworzenie slug'ów dla stron

Tworzenie nowych stron ma dwa kroki:

1.  Generowanie "ścieżki" lub "slug'a" dla strony.
2.  Tworzenie strony.

_**Nota**: Często źródła danych dostarczą bezpośrednio slug lub nazwę ścieżki dla zawartości - kiedy pracuejsz z jednym z takich systemów (np. z CMS), nie będziesz musiał tworzyć slug'ów własnoręcznie, tak jak jest to robione z plikami markdown._

By tworzyć własne strony Markdown, nauczymy Cię używać dwóch API Gatsby'ego:
[`onCreateNode`](/docs/node-apis/#onCreateNode) oraz
[`createPages`](/docs/node-apis/#createPages). Są to dwa główne API
które zauważysz że są używane na wielu stronach i w pluginach.

Dokładamy wszelkich starań by sprawić Gatsby API łatwym do zastosowania. W celu wykorzystania
API, eksportujesz funkcję z nazwą API z pliku `gatsby-node.js`.

A więc zrobimy to tutaj. W katalogu głównym Twojej strony, stwórz plik o nazwie
`gatsby-node.js`. A potem dopisz do niego poniższy kod.

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  console.log(node.internal.type)
}
```

Funkcja `onCreateNode` będzie wywoływana przez Gatsby za każdym razem gdy tworzony jest nowy Node (lub aktualizowany).

Zatrzymaj i uruchom ponownie serwer deweloperski. Gdy będziesz to robił, zauważysz w konsoli 
terminala kilka wpisów o utworzonych node'ach.

Użyj tego API by dodać slug'i do swoich stron Markdown, do node'ów
`MarkdownRemark`.

Zmień swoją funkcję tak by teraz wyświetlała tylko wpisy node'ów `MarkdownRemark`.

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  // highlight-start
  if (node.internal.type === `MarkdownRemark`) {
    console.log(node.internal.type)
  }
  // highlight-end
}
```

Chcemy użyć wszystkich plików Markdown by stworzyć Slug'i dla stron. Tak więc 
`pandas-and-bananas.md` utworzy `/pandas-and-bananas/`. Ale skąd brać nazwy plików
z Node'a `MarkdownRemark`? Abyt to zrobić, musisz _przenieść_ "node graph" na jego
node'a `File` _rodzica_, jako, że node `File`zawiera dane na temat plików na dysku,
których potrzebujesz. Aby to zrobić, zmodywikuj swoją funkcję ponownie:

```javascript:title=gatsby-node.js
// highlight-next-line
exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    // highlight-start
    const fileNode = getNode(node.parent)
    console.log(`\n`, fileNode.relativePath)
    // highlight-end
  }
}
```

Po ponownym uruchomieniu serwera deweloperskiego, powinienieś ujrzeć 
wyświetlone w terminalu relatywne ścieżki dla Twoich dwóch plików Markdown.

![markdown-relative-path](markdown-relative-path.png)

Teraz musisz utworzyć slug'i. Jako że logika tworzenia slugów z nazw plików może być 
trudna, `gatsby-source-filesystem` przychodzi wraz z wbudowaną funkcją tworzenia
Slug'ów. Użyjmy tego teraz.

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`) // highlight-line

exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    console.log(createFilePath({ node, getNode, basePath: `pages` })) // highlight-line
  }
}
```

Funkcja przejmuje znajdowanie node'a `Pliku` rodzica razem ze stworzeniem
slug'a. Uruchom serwer deweloperski ponownie i powinieneś zobaczyć wyświetlone w terminalu
dwa slug'i, po jednym dla każdego pliku Markdown.

Teraz możesz dodać nowe slugi bezpośrednio do node'ów `MarkdownRemark`. Daje Ci to
ogromne możliwości, ponieważ dane, które dodajesz do node'ów są dostępne dla wszystkich 
późniejszych zapytań w GraphQL. Zatem, będzie bardzo łatwo pobrać slug'i gdy przyjdzie czas na tworzenie stron.

Aby to zrobić, użyjesz funkcji przekazanej do implementacji API, która nazywa się
[`createNodeField`](/docs/actions/#createNodeField). Ta funkcja pozwoli Tobie
tworzyć dodatkowe pola na node'ach stowrzonych przez inne wtyczki. Tylko 
oryginalny twórca node'a może bezpośrednio modyfikować node - wszystkie inne wtyczki
(włączając twoje `gatsby-node.js`) muszą używać tej funkcji by tworzyć 
nowe pola.

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`)
// highlight-next-line
exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions // highlight-line
  if (node.internal.type === `MarkdownRemark`) {
    // highlight-start
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
    // highlight-end
  }
}
```

Zrestartuj serwer deweloperski i odśwież GraphiQL. Potem uruchom to
zapytanie GraphQL by ujrzeć swoje nowe slugi.

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        fields {
          slug
        }
      }
    }
  }
}
```

Teraz gdy slug'i są gotowe, możesz zacząć tworzyć strony.

## Tworzenie stron

W tym samym pliku `gatsby-node.js`, dodaj poniższy kod.

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`)

exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
  if (node.internal.type === `MarkdownRemark`) {
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
  }
}

// highlight-start
exports.createPages = async ({ graphql, actions }) => {
  // **Note:** The graphql function call returns a Promise
  // see: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise for more info
  const result = await graphql(`
    query {
      allMarkdownRemark {
        edges {
          node {
            fields {
              slug
            }
          }
        }
      }
    }
  `)

  console.log(JSON.stringify(result, null, 4))
}
// highlight-end
```

Dodałeś implementację API
[`createPages`](/docs/node-apis/#createPages), które Gatsby wywołuje po to, by wtyczki mogły dodawać
strony.

Jak wspomniano we wstępie do tego poradnika, wymagane kroki by tworzyć strony programatycznie są następujące:

1.  Zapytanie o dane przy użyciu GraphQL
2.  Zmapowanie wyników zapytań na strony

Powyższy kod jest pierwszym krokiem do tworzenia stron z Twoich plików markdown, ponieważ używasz
dostarczonej przez `graphql` funkcji by wykonywać zapytania o slug'i z markdown, które utworzyłeś.
Na końcu wyświetlasz w konsoli wynik zapytania, który powinien wyglądać w ten sposób:

![query-markdown-slugs](query-markdown-slugs.png)

Potrzebujesz jescze jednej rzeczy poza slugiem by tworzyć strony: komponent szablonu
strony. Jak wszystko w Gatsby, programatyczne strony są zasilane komponentami React.
Kiedy tworzysz stronę, musisz wskazać którego komponentu użyć.

Stwórz folder `src/templates`, a potem dodaj następujący kod w
pliku o nazwie `src/templates/blog-post.js`.

```jsx:title=src/templates/blog-post.js
import React from "react"
import Layout from "../components/layout"

export default () => {
  return (
    <Layout>
      <div>Hello blog post</div>
    </Layout>
  )
}
```

Następnie uaktualnij `gatsby-node.js`

```javascript:title=gatsby-node.js
const path = require(`path`) // highlight-line
const { createFilePath } = require(`gatsby-source-filesystem`)

exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
  if (node.internal.type === `MarkdownRemark`) {
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
  }
}

exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions // highlight-line
  const result = await graphql(`
    query {
      allMarkdownRemark {
        edges {
          node {
            fields {
              slug
            }
          }
        }
      }
    }
  `)

  // highlight-start
  result.data.allMarkdownRemark.edges.forEach(({ node }) => {
    createPage({
      path: node.fields.slug,
      component: path.resolve(`./src/templates/blog-post.js`),
      context: {
        // Data passed to context is available
        // in page queries as GraphQL variables.
        slug: node.fields.slug,
      },
    })
  })
  // highlight-end
}
```

Uruchonm ponownie serwer deweloperski, twoje strony zostaną teraz utworzone! Łatwym sposobem
by znaleźć nowe strony które utworzyłeś podczas kodowania, jest odwiedzenie domyślnego adresu pod którym 
Gatsby w pomocny sposób wyświetli Tobie pełną listę dostępnych stron. Jeśli odwiedzisz adres
<http://localhost:8000/sdf>, zobaczysz nowe strony które utworzyłeś.

![new-pages](new-pages.png)

Odwiedź jedną z nich, a zobaczysz:

![hello-world-blog-post](hello-world-blog-post.png)

Wygląda to wciąż nudno, a pewnie chcesz czegoś lepszego. Teraz możesz już pobrać dane ze swojego postu markdown. Zmień
`src/templates/blog-post.js` w taki sposób:

```jsx:title=src/templates/blog-post.js
import React from "react"
import { graphql } from "gatsby" // highlight-line
import Layout from "../components/layout"

// highlight-start
export default ({ data }) => {
  const post = data.markdownRemark
  // highlight-end
  return (
    <Layout>
      {/* highlight-start */}
      <div>
        <h1>{post.frontmatter.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.html }} />
      </div>
      {/* highlight-end */}
    </Layout>
  )
}

// highlight-start
export const query = graphql`
  query($slug: String!) {
    markdownRemark(fields: { slug: { eq: $slug } }) {
      html
      frontmatter {
        title
      }
    }
  }
`
// highlight-end
```

I...

![blog-post](blog-post.png)

Ekstra!

Ostatni krok to podlinkowanie twoich nowych stron do strony index.

Wróć do `src/pages/index.js`, wykonaj zapytanie o slug'i markdown i stwórz
linki.

```jsx:title=src/pages/index.js
import React from "react"
import { css } from "@emotion/core"
import { Link, graphql } from "gatsby" // highlight-line
import { rhythm } from "../utils/typography"
import Layout from "../components/layout"

export default ({ data }) => {
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
            {/* highlight-start */}
            <Link
              to={node.fields.slug}
              css={css`
                text-decoration: none;
                color: inherit;
              `}
            >
              {/* highlight-end */}
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
            </Link> {/* highlight-line */}
          </div>
        ))}
      </div>
    </Layout>
  )
}

export const query = graphql`
  query {
    allMarkdownRemark(sort: { fields: [frontmatter___date], order: DESC }) {
      totalCount
      edges {
        node {
          id
          frontmatter {
            title
            date(formatString: "DD MMMM, YYYY")
          }
          // highlight-start
          fields {
            slug
          }
          // highlight-end
          excerpt
        }
      }
    }
  }
`
```

I proszę bardzo! Działający, choć niewielki, blog!

## Zadanie

Spróbuj pobawić się trochę więcej ze stroną. Spróbuj dodać więcej plików markdown. Zbadaj działanie
wykonywania zapytań o inne dane z node'ów `MarkdownRemark` i dodaj je do strony
frontowej lub stron blog postów.

W tej części poradnika, nauczyłeś się podstaw budowania stron
z użyciem warstwy danych Gatsby. Nauczyłeś wię jak _pobierać_ i _przekształcać_ dane używając
wtyczek, jak używać GraphQL do _mapowania_ danych na strony, a potem jak budować _komponenty szablonów stron_
gdzie wykonujesz zapytania o dane dla każdej ze stron.

## Czego nauczysz się w daleszej części?

Teraz gdy już wiesz jak zbudować stronę w Gatsby, gdzie udać się następnie?

- Podziel się swoją stroną Gatsby na twitterze i sam zobacz co inni ludzie stworzyli wyszukując #gatsbytutorial! Upewnij się, że wspomnisz @gatsbyjs w swoim Tweet'cie i dodasz hasztag #gatsbytutorial :)
- Możesz obejrzeć różne [przykładowe strony](https://github.com/gatsbyjs/gatsby/tree/master/examples#gatsby-example-websites)
- Sprawdź więcej [wtyczek](/docs/plugins/)
- Zobacz co [inni ludzie budują z Gatsby](/showcase/)
- Sprawdź dokumentację [API Gatsby](/docs/api-specification/), [node](/docs/node-interface/), lub [GraphQL](/docs/graphql-reference/)
