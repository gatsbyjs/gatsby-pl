---
title: Programatyczne tworzenie stron z danych
typora-copy-images-to: ./
disableTableOfContents: true
---

> Ten poradnik jest częścią serii na temat Warstwy Danych Gatsby. Upewnij się, że zapoznałeś się wcześniej z [częścią 4](/tutorial/part-four/), [częścią 5](/tutorial/part-five/), oraz [częścią 6](/tutorial/part-six/) zanim będziesz kontynuował.

## Czego dowiesz się w tym poradniku?

W poprzednim poradniku, stworzyłeś niezłą stronę główną, która wykonywała zapytania do plików 
Markdown i wyświetlała listę tytułów oraz fragmenty blog postów. Jednak nie chcemy widzieć tylko fragmentów, chcielibyśmy właściwych, pełnych stron dla naszych plików Markdown.

Moglibyśmy kontynuować tworzenie stron poprzez umieszczanie kolejnych komponentów React'a w 
folderze `src/pages`. Jednak teraz nauczymy się jak _programatycznie_ tworzyć strony z 
_danych_. Gatsby, w przeciwieństwie do wielu innych generatorów stron statycznych, _nie_ jest ograniczony do tworzenia stron z plików. Gatsby pozwala Ci używać GraphQL, po to, aby wysyłać zapytania o Twoje _dane_, oraz _map'ować_ wyniki zapytań na _strony_ - wszystko podczas procesu budowania (build). Ta 
koncepcja daje ogromną moc w twoje ręce. Nauczysz się zastosowań i sposobów użycia tej możliwości w dalszej części poradnika.

Zaczynajmy.

## Tworzenie slug'ów stron

Tworzenie nowych stron dzielimy na dwa kroki:

1.  Wygenerowanie "ścieżki" (lub "slug'a") strony.
2.  Tworzenie strony.

_**Notatka**: Często źródła danych dostarczają bezpośrednio slug lub nazwę ścieżki dla treści - kiedy pracujesz z jednym z takich systemów (np. z CMS), nie będziesz musiał tworzyć slug'ów własnoręcznie, w przeciwieństwie do tego jak jest to robione z plikami Markdown._

By tworzyć własne strony z Markdown, nauczymy Cię używać dwóch API Gatsby'ego:
[`onCreateNode`](/docs/node-apis/#onCreateNode),  oraz
[`createPages`](/docs/node-apis/#createPages). Są to dwa główne API,
które zauważysz, że są używane na wielu stronach i we wtyczkach.

Dokładamy wszelkich starań by zrobić API Gatsby jak najprostszym w użyciu. W celu wykorzystania
API, eksportuj funkcję z nazwą API z pliku `gatsby-node.js`.

A zatem zróbmy to tutaj. W katalogu głównym Twojej strony, stwórz plik o nazwie
`gatsby-node.js`. A potem dopisz do niego poniższy kod.

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  console.log(node.internal.type)
}
```

Funkcja `onCreateNode` będzie wywoływana przez Gatsby za każdym razem, gdy będzie tworzony (lub aktualizowany) nowy Node.

Zatrzymaj i uruchom ponownie serwer deweloperski. Gdy to zrobisz, zauważysz w konsoli 
terminala kilka wpisów o utworzonych node'ach.

Użyj tego API by dodawać slug'i do swoich stron Markdown, do node'ów
`MarkdownRemark`.

Zmień teraz swoją funkcję tak, aby wyświetlała tylko wpisy pochodzące z node'ów `MarkdownRemark`.

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  // highlight-start
  if (node.internal.type === `MarkdownRemark`) {
    console.log(node.internal.type)
  }
  // highlight-end
}
```

Chcemy użyć wszystkich plików Markdown, aby tworzyć slug'i dla stron. Tak więc 
`pandas-and-bananas.md` utworzy `/pandas-and-bananas/`. Ale jak uzyskać nazwę pliku
z node'a `MarkdownRemark`? Aby to zrobić, musisz _przenieść_ "node graph" do jego
_nadrzędnego_ node'a `File`, ponieważ node'y `File` zawierą potrzebne informacje o plikach na dysku.
Aby to zrobić, zmodyfikuj ponownie swoją funkcję:

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

Po zrestartowaniu serwera deweloperskiego, powinieneś ujrzeć, 
wyświetlone w oknie terminala, dwie ścieżki względne Twoich plików Markdown.

![markdown-relative-path](markdown-relative-path.png)

Teraz musisz stworzyć slug'i. Ponieważ logika tworzenia slug'ów z nazw plików może być 
trudna, wtyczka `gatsby-source-filesystem` dostarcza nam wbudowaną funkcję tworzenia
Slug'ów. Użyjmy tego.

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`) // highlight-line

exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    console.log(createFilePath({ node, getNode, basePath: `pages` })) // highlight-line
  }
}
```

Funkcja ta obsługuje znajdowanie node'a nadrzędnego `Pliku`, wraz z utworzeniem jego
slug'a. Uruchom ponownie serwer deweloperski, powinieneś zobaczyć wyświetlone w oknie terminala
dwa slug'i, po jednym dla każdego pliku Markdown.

Teraz możesz dodać nowe slug'i bezpośrednio do node'ów `MarkdownRemark`. Daje Ci to
ogromne możliwości, ponieważ dane, które dodajesz do node'ów, będą dostępne we wszystkich 
późniejszych zapytaniach w GraphQL. Dlatego będzie bardzo łatwo pobierać slug'i gdy przyjdzie czas na tworzenie stron.

Aby to zrobić, musisz użyć funkcji przekazanej do implementacji API, która nazywa się
[`createNodeField`](/docs/actions/#createNodeField). Funkcja ta pozwoli Ci
tworzyć dodatkowe pola danych na node'ach stworzonych przez inne wtyczki. Tylko 
oryginalny twórca node'a może bezpośrednio modyfikować node - wszystkie inne wtyczki
(w tym `gatsby-node.js`) muszą używać tej funkcji do tworzenia 
dodatkowych pól.

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

Uruchom ponownie serwer deweloperski i otwórz lub odśwież GraphiQL. Potem uruchom to
zapytanie GraphQL, aby zobaczyć nowe slugi.

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

Po utworzeniu slug'ów możesz zacząć tworzyć strony.

## Tworzenie stron

W tym samym pliku `gatsby-node.js`, dodaj następujące elementy.

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
[`createPages`](/docs/node-apis/#createPages), które Gatsby wywołuje po to, aby wtyczki mogły dodawać
strony.

Jak wspomniano we wstępie do tej części poradnika, wymagane kroki do programatycznego tworzenia stron to:

1.  Zapytanie o dane za pomocą GraphQL
2.  Zmapowanie wyników zapytania na strony

Powyższy kod jest pierwszym krokiem do tworzenia stron z plików markdown, ponieważ używasz
dostarczonej przez `graphql` funkcji, aby wykonywać zapytania o utworzone przez siebie slug'i markdown.
Na końcu wyświetlasz w konsoli wynik zapytania, który powinien wyglądać następująco:

![query-markdown-slugs](query-markdown-slugs.png)

Aby tworzyć strony, potrzebujesz jeszcze jednej rzeczy poza slugiem: komponentu szablonu
strony. Jak wszystko w Gatsby, programatyczne strony są obsługiwane przez komponenty React.
Podczas tworzenia strony musisz określić, którego komponentu użyć.

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

Uruchom ponownie serwer deweloperski, Twoje strony zostaną teraz utworzone! Łatwym sposobem
by znaleźć nowe strony, które utworzyłeś podczas pisania kodu, jest odwiedzenie domyślnego adresu, pod którym 
Gatsby w pomocny sposób wyświetli Tobie pełną listę dostępnych stron. Jeśli odwiedzisz adres
<http://localhost:8000/sdf>, zobaczysz nowe strony które stworzyłeś.

![new-pages](new-pages.png)

Odwiedź jedną z nich, a zobaczysz:

![hello-world-blog-post](hello-world-blog-post.png)

Wygląda to wciąż nudno, i pewnie chcesz zobaczyć coś bardziej rozbudowanego. Teraz możesz już pobierać dane ze swojego postu napisanego w markdown. A więc zmień
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

I mamy to! Działający, choć niewielki, blog!

## Wyzwanie

Spróbuj pobawić się trochę więcej ze stroną. Spróbuj dodać więcej plików markdown. Zbadaj działanie
wykonywania zapytań o inne dane z node'ów `MarkdownRemark` i dodaj je do strony
frontowej lub stron blog postów.

W tej części poradnika, nauczyłeś się podstaw budowania stron
z użyciem warstwy danych Gatsby. Nauczyłeś wię jak _pobierać_ i _przekształcać_ dane używając
wtyczek, jak używać GraphQL do _mapowania_ danych na strony, a potem jak budować _komponenty szablonów stron_
gdzie wykonujesz zapytania o dane dla każdej ze stron.

## Czego nauczysz się w dalszej części?

Teraz gdy już wiesz jak zbudować stronę w Gatsby, gdzie udać się następnie?

- Podziel się swoją stroną Gatsby na twitterze i sam zobacz co inni ludzie stworzyli wyszukując #gatsbytutorial! Upewnij się, że wspomnisz @gatsbyjs w swoim Tweet'cie i dodasz hasztag #gatsbytutorial :)
- Możesz obejrzeć różne [przykładowe strony](https://github.com/gatsbyjs/gatsby/tree/master/examples#gatsby-example-websites)
- Sprawdź więcej [wtyczek](/docs/plugins/)
- Zobacz co [inni ludzie budują z Gatsby](/showcase/)
- Sprawdź dokumentację [API Gatsby](/docs/api-specification/), [node](/docs/node-interface/), lub [GraphQL](/docs/graphql-reference/)
