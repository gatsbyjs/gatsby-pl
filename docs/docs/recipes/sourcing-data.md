w ---
title: "Przepisy: Pozyskiwanie danych"
tableOfContentsDepth: 1
---


Do pozyskania danych w Gatsby używa się wtyczek, które pozyskują dane z określonych źrodeł (np. `gatsby-source-filesystem` pobiera dane z plików systemowych, `gatsby-source-wordpress` pobiera dane z WordPress Api, itp.). Możesz również pozyskać dane samodzielnie samodzielnie.

## Dodawanie danych do GraphQL

[Warstwa danych GraphQL](/docs/graphql-concepts/) wykorzystuje node'y, by zmodelować porcje danych. Wtyczki źródeł dodają źródłowe node'y, które możesz pozyskać poprzez wykonanie odpowiedniego zpaytania. Możesz także stworzyć źródłowe node'y samodzielnie - Gatsby zapewnia metody, które możesz wykorzystać by dodać niestandardowe dane do warstwy danych GraphQL.

Ten przepis pokazuje w jaki sposób dodać własne dane poprzez użycie `createNode()`.

### Wskazówki

1. W pliku `gatsby-node.js` użyj `sourceNodes()` i `actions.createNode()` by stworzyć i wyeksportować node'y niezbędne do wykonania zapytania.

```javascript:title=gatsby-node.js
exports.sourceNodes = ({ actions, createNodeId, createContentDigest }) => {
  const pokemons = [
    { name: "Pikachu", type: "electric" },
    { name: "Squirtle", type: "water" },
  ]

  pokemons.forEach(pokemon => {
    const node = {
      name: pokemon.name,
      type: pokemon.type,
      id: createNodeId(`Pokemon-${pokemon.name}`),
      internal: {
        type: "Pokemon",
        contentDigest: createContentDigest(pokemon),
      },
    }
    actions.createNode(node)
  })
}
```

2. Uruchom komendę `gatsby develop`.

   > _Uwaga: Po dokonaniu zmian w `gatsby-node.js` muisz ponownie uruchomić komendę `gatsby develop` aby zaobserwować te zmiany._

3. Wykonaj zapytanie (w GraphiQL lub w Twoim komponencie).

```graphql
query MyPokemonQuery {
  allPokemon {
    nodes {
      name
      type
      id
    }
  }
}
```


### Dodatkowe źródła

- Użycie pluginu `gatsby-source-filesystem` krok po kroku w [części piątej samouczka](/tutorial/part-five/#source-plugins)
- Wyszukaj dostępne wtyczki źródeł w [bibliotece wtyczek Gatsby](/plugins/?=source)
- Zrozum wtyczki źródeł poprzez stworzenie jednej z pomocą [samouczka Pixabay](/tutorial/pixabay-source-plugin-tutorial/)
- [Dokumentacja funkcji createNode](/docs/actions/#createNode)

## Pozyskiwanie danych z Markdownu dla blog postów i stron z GraphQL

Możesz pozyskać dane z Markdownu i użyć [`createPages` API](/docs/actions/#createPage) by dynamicznie stworzyć strony.
Ten przepis pokazuje w jaki sposób z plikow Markdown ulokowanych na twoim lokalnym systemie stworzyć strony z użyciem warstwy danych GraphQl.

### Wymagania

- [Projekt Gatsby](/docs/quick-start) z plikiem `gatsby-config.js`
- Zainstalowane [Gatsby CLI](/docs/gatsby-cli)
- Zainstalowana wtyczka [gatsby-source-filesystem](/packages/gatsby-source-filesystem)
- Zainstalowana wtyczka [gatsby-transformer-remark](/packages/gatsby-transformer-remark)
- Plik `gatsby-node.js`

### Wskazówki

1. Do pliku `gatsby-config.js`, dodaj `gatsby-transformer-remark` i `gatsby-source-filesystem` by pobrać dane z plików Markdown znajdujących się w folderze `source`. Będzie to dodatkiem do poprzednich wpisów w `gatsby-source-filesystem` jak na przykład dla obrazków:

```js:title=gatsby-config.js
module.exports = {
  plugins: [
    `gatsby-transformer-remark`,
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `content`,
        path: `${__dirname}/src/content`,
      },
    },
  ]
```

2. Dodaj post z użyciem Markdownu w `src/content`, zawżyj frontmatter dla: tytułu, daty i ścieżki. Dodaj początkowy kontent do body posta:

```markdown:title=src/content/my-first-post.md
---
title: My First Post
date: 2019-07-10
path: /my-first-post
---

This is my first Gatsby post written in Markdown!
```

3. Uruchom serwer developerski komendą `gatsby develop`. Następnie przejdź to exploratora GraphiQL `http://localhost:8000/___graphql` i wykonaj zapytanie o dane z plikow Markdown:

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        frontmatter {
          path
        }
      }
    }
  }
}
```

<iframe
  title="Query for all markdown"
  src="https://q4xpb.sse.codesandbox.io/___graphql?explorerIsOpen=false&query=%7B%0A%20%20allMarkdownRemark%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20frontmatter%20%7B%0A%20%20%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D"
  width="600"
  height="300"
/>

4. Aby w czasie kompilacji wygenerować strony na postawie postów Markdown, skopiuj zapytanie GraphQL do pliku `gatsby-node.js`, a następnie przeiteruj otrzymane wyniki:

```js:title=gatsby-node.js
const path = require(`path`)

exports.createPages = async ({ actions, graphql }) => {
  const { createPage } = actions

  const result = await graphql(`
    {
      allMarkdownRemark {
        edges {
          node {
            frontmatter {
              path
            }
          }
        }
      }
    }
  `)
  if (result.errors) {
    console.error(result.errors)
  }

  result.data.allMarkdownRemark.edges.forEach(({ node }) => {
    createPage({
      path: node.frontmatter.path,
      component: path.resolve(`src/templates/post.js`),
    })
  })
}
```

5. Dodaj szablon dla posta w folderze `src/templates` i umieść w nim zapytanie GraphQl by dynamicznie wygenerować strony z Markdownu w trakcie kompilacji:

```jsx:title=src/templates/post.js
import React from "react"
import { graphql } from "gatsby"

export default function Template({ data }) {
  const { markdownRemark } = data // data.markdownRemark holds your post data
  const { frontmatter, html } = markdownRemark
  return (
    <div className="blog-post">
      <h1>{frontmatter.title}</h1>
      <h2>{frontmatter.date}</h2>
      <div
        className="blog-post-content"
        dangerouslySetInnerHTML={{ __html: html }}
      />
    </div>
  )
}

export const pageQuery = graphql`
  query($path: String!) {
    markdownRemark(frontmatter: { path: { eq: $path } }) {
      html
      frontmatter {
        date(formatString: "MMMM DD, YYYY")
        path
        title
      }
    }
  }
`
```

6. Wykonaj komendę `gatsby develop` by uruchomić ponownie serwer developerski. Sprawdź swój post w przeglądarce: `http://localhost:8000/my-first-post`

### Dodatkowe źródła

- [Samouczek: Programowe tworzenie stron z danych](/tutorial/part-seven/)
- [Tworzenie i modyfikowanie stron](/docs/creating-and-modifying-pages/)
- [Dodawanie stron z użyciem Markdown](/docs/adding-markdown-pages/)
- [Przewodnik do programowego tworzenia stron z danych](/docs/programmatically-create-pages-from-data/)
- [Przykładowe repozytorium](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-sourcing-markdown) dla tego przepisu

## Pozyskiwanie danych z Wordpress

### Wymagania

- [Projekt Gatsby](/docs/quick-start) z plikami `gatsby-config.js` i `gatsby-node.js` 
- Instancja Wordpress, hostowana na Wordpress.com lub samodzielnie przez ciebie

### Wskazówki

1. Zainstaluj wtyczkę `gatsby-source-wordpress` poprzez uruchomienie komendy:

```shell
npm install gatsby-source-wordpress --save
```

2. Skonfiguruj wtyczkę w `gatsby-config.js` w następujący sposób:

```javascript:title=gatsby-config.js
module.exports = {
  ...
  plugins: [
    {
      resolve: `gatsby-source-wordpress`,
      options: {
        // baseUrl will need to be updated with your WordPress source
        baseUrl: `wpexample.com`,
        protocol: `https`,
        // is it hosted on wordpress.com, or self-hosted?
        hostingWPCOM: false,
        // does your site use the Advanced Custom Fields Plugin?
        useACF: false
      }
    },
  ]
}
```

> **Uwaga:** Sprawdź [dokumentację wtyczki `gatsby-source-wordpress`](/packages/gatsby-source-wordpress/?=wordpre#how-to-use) by dowiedzieć się więcej o tym, jak ją prawidłowo skonfiugurować.

3. Stwórz szablon w `src/templates/post.js` i umieść w nim poniższy kod:

```jsx:title=post.js
import React, { Component } from "react"
import { graphql } from "gatsby"
import PropTypes from "prop-types"

class Post extends Component {
  render() {
    const post = this.props.data.wordpressPost

    return (
      <>
        <h1>{post.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.content }} />
      </>
    )
  }
}

Post.propTypes = {
  data: PropTypes.object.isRequired,
  edges: PropTypes.array,
}

export default Post

export const pageQuery = graphql`
  query($id: String!) {
    wordpressPost(id: { eq: $id }) {
      title
      content
    }
  }
`
```

4. Stwórz dynamiczne strony dla twoich postów poprzez skopiowanie poniższego kodu do pliku `gatsby-node.js`:

```javascript:title=gatsby-node.js
const path = require(`path`)
const { slash } = require(`gatsby-core-utils`)

exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions

  // query content for WordPress posts
  const result = await graphql(`
    query {
      allWordpressPost {
        edges {
          node {
            id
            slug
          }
        }
      }
    }
  `)

  const postTemplate = path.resolve(`./src/templates/post.js`)
  result.data.allWordpressPost.edges.forEach(edge => {
    createPage({
      // `path` will be the url for the page
      path: edge.node.slug,
      // specify the component template of your choice
      component: slash(postTemplate),
      // In the ^template's GraphQL query, 'id' will be available
      // as a GraphQL variable to query for this posts's data.
      context: {
        id: edge.node.id,
      },
    })
  })
}
```

5. Uruchom komendę `gatsby-develop` żeby zobaczyć nowo wygenerowane strony i poruszać się pomiędzy nimi.

6. Otwórz`GraphiQL IDE` pod adresem `http://localhost:8000/__graphql` i otwórz Docs lub Explorer, gdzie znajdziesz pola do opytywania dla `allWordpressPosts`

Stworzone powyżej w `gatsby-node.js` dynamiczne strony mają unikalne ścieżki dla poszczególnych postów i używają szablonu razem z przykładowym zapytaniem GraphQl, które pozyskuje dane z Wordpressa.

### Dodatkowe źródła

- [Pierwsze kroki z WordPressem i Gatsby](/blog/2019-04-26-how-to-build-a-blog-with-wordpress-and-gatsby-part-1/)
- Więcej o [pozyskiwaniu danych z WordPressa](/docs/sourcing-from-wordpress/)
- [Działający przykład pozyskiwania danych z WordPress](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-wordpress)

## Pozyskiwanie danych z Contentful

### Wymagania

- [Projekt Gatsby](/docs/quick-start/)
- [Konto Contentful](https://www.contentful.com/)
- Zainstalowane [Contentful CLI](https://www.npmjs.com/package/contentful-cli) 

### Wskazówki

1. Zaloguj się do Contenful przez CLI i postępuj zgodnie z kolejnymi krokami. Jeśli nie masz jeszcze konta, CLI umożliwi Ci założenie nowego.

```shell
contentful login
```

2. Jeśli jeszcze nie masz żadnego space, stwórz nowy. Upewnij się, że zapisałeś ID podane pod koniec komendy. Jeśli masz już space i space ID, możesz ominąć kroki 2 i 3. 

Uwaga: jeśli stworzyłeś nowe konto, możesz nadpisać domyślny space. Sprawdź [spaces zawarte w Twoim koncie](https://app.contentful.com/account/profile/space_memberships).

```shell
contentful space create --name 'Gatsby example'
```

3. Zainicjuj nowy space przykładową treścią bloga, używając nowego space ID zwróconego z poprzedniej komendy w miejscu `<space ID>`.

```shell
contentful space seed -s '<space ID>' -t blog
```

Na przykład, z podmienionym ID na twoje: `contentful space seed -s '22fzx88spbp7' -t blog`

4. Stwórz nowy klucz dostępu dla twojego space. Zapamiętaj ten klucz, będzie on potrzebny w 6tym kroku.

```shell
contentful space accesstoken create -s '<space ID>' --name 'Example token'
```

5. Zainstaluj wtyczkę `gatsby-source-contentful` dla twojej strony Gatsby:

```shell
npm install --save gatsby-source-contentful
```

6. By aktywować wtyczkę w pliku `gatsby-config.js` dodaj `gatsby-source-contentful` do listy `plugins`. Powinieneś poważnie rozważyć użycie [zmiennych środowiskowych](/docs/environment-variables/) do przechowywania twojego space ID i klucza ze względów bezpieczeństwa.

```javascript:title=gatsby-config.js
plugins: [
   // add to array along with any other installed plugins
   // highlight-start
   {
    resolve: `gatsby-source-contentful`,
    options: {
      spaceId: `<space ID>`, // or process.env.CONTENTFUL_SPACE_ID
      accessToken: `<access token>`, // or process.env.CONTENTFUL_TOKEN
    },
  },
  // highlight-end
],
```

7. Uruchom komendę `gatsby develop` i upewnij się, że zakończyła się ona sukcesem.

8. Wykonaj zapytanie za pomocą [edytora GraphiQL](/docs/introducing-graphiql/) pod adresem `http://localhost:8000/___graphql`. Wtyczka Contentful dodaje kilka nowych typów node'ów, włączając wszystke typy kontentu na twojej stronie Contentful. Twój przykładowy space z typem "Blog Post" tworzy node `allContentfulBlogPost` w GraphQl.

![Interferjs graphQL, z przykładowym zapytaniem przedstawionym poniżej](../images/recipe-sourcing-contentful-graphql.png)

By wykonać zapytanie o tytuły blog postów z Contenful, użyj poniższe zapytanie GraphQL:

```graphql
{
  allContentfulBlogPost {
    edges {
      node {
        title
      }
    }
  }
}
```

Node'y Contentful zawierają także metadane takie jak `createdAt` i `node_locale`.


9. By wyświetlić listę linków do blog postów, stwórz nowy plik `/src/pages/blog.js`. Ta strona wyświetli wszystkie posty, posortowane na podstawie daty aktualizacji.

```jsx:title=src/pages/blog.js
import React from "react"
import { graphql, Link } from "gatsby"

const BlogPage = ({ data }) => (
  <div>
    <h1>Blog</h1>
    <ul>
      {data.allContentfulBlogPost.edges.map(({ node, index }) => (
        <li key={index}>
          <Link to={`/blog/${node.slug}`}>{node.title}</Link>
        </li>
      ))}
    </ul>
  </div>
)

export default BlogPage

export const query = graphql`
  {
    allContentfulBlogPost(sort: { fields: [updatedAt] }) {
      edges {
        node {
          title
          slug
        }
      }
    }
  }
`
```

By dalej tworzyć twoją stronę, w tym podstron ze szczegółami postów, zapoznaj się z resztą [dokumentacji Gatsby](/docs/sourcing-from-contentful/) i dodatkowymi źródłami poniżej.

### Dodatkowe źródła

- [Tworzenie strony z użyciem Reacta o Contentful](/blog/2018-1-25-building-a-site-with-react-and-contentful/)
- [Więcej o pozyskiwaniu danych z Contentful](/docs/sourcing-from-contentful/)
- [Wtyczka źródła Contentful ](/packages/gatsby-source-contentful/)
- [Typy pól z długim tekstem zwracane jako obiekty](/packages/gatsby-source-contentful/#a-note-about-longtext-fields)
- [Przykładowe repozytorium z tym przepisem](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-sourcing-contentful)

## Pozyskiwanie danych z zewnętrznych źródeł i tworzenie stron bez użycia GrapohQL

Nie musisz używać warstwy danych GraphQl, aby uwzględnić dane na stronach, [chociaż są istotne powody, dla których powinieneś rozważyć użycie GraphQL](/docs/why-gatsby-uses-graphql/). Możesz wykorzystać `createPages` API, by pozyskać nieustrukturyzowane dane bezpośrednio do projektu Gatsby, a nie za pośrednictwem GraphQL i wtyczek źródłowych.

W tym przepisie utworzysz dynamiczne strony z danych pobranych z [endpointów PokéAPI’s REST](https://www.pokeapi.co/). [Kompletny przykład](https://github.com/jlengstorf/gatsby-with-unstructured-data/) może być znaleziony na Githubie.

### Wymagania

- Projekt Gatsby z plikiem `gatsby-node.js`
- Zainstalowane [Gatsby CLI](/docs/gatsby-cli)
- Paczka [axios](https://www.npmjs.com/package/axios) zainstalowana poprzez npm

### Wskazówki

1. Dodaj poniższy kod do pliku `gatsby-node.js` by pobrać dane z PokeAPI i programowo stworzyć stronę główną:

```js:title=gatsby-node.js
const axios = require("axios")

const get = endpoint => axios.get(`https://pokeapi.co/api/v2${endpoint}`)

const getPokemonData = names =>
  Promise.all(
    names.map(async name => {
      const { data: pokemon } = await get(`/pokemon/${name}`)
      return { ...pokemon }
    })
  )
exports.createPages = async ({ actions: { createPage } }) => {
  const allPokemon = await getPokemonData(["pikachu", "charizard", "squirtle"])

  // Create a page that lists Pokémon.
  createPage({
    path: `/`,
    component: require.resolve("./src/templates/all-pokemon.js"),
    context: { allPokemon },
  })
}
```

2. Stwórz szablon aby wyświetlić Pokémony na stronie głównej:

```jsx:title=src/templates/all-pokemon.js
import React from "react"

export default ({ pageContext: { allPokemon } }) => (
  <div>
    <h1>Behold, the Pokémon!</h1>
    <ul>
      {allPokemon.map(pokemon => (
        <li key={pokemon.id}>
          <img src={pokemon.sprites.front_default} alt={pokemon.name} />
          <p>{pokemon.name}</p>
        </li>
      ))}
    </ul>
  </div>
)
```

3. Uruchom komendę `gatsby develop` by pozyskać dane, zbudować strony i uruchomić serwer developerski.
4. Sprawdź swoją stronę główną na: `http://localhost:8000`

### Dodatkowe źródła

- [Full Pokemon data repo](https://github.com/jlengstorf/gatsby-with-unstructured-data/)
- Więcej o nieustrukturyzowanych danych w samouczku o [używaniu Gatsby bez GraphQl](/docs/using-gatsby-without-graphql/)
- Kiedy i jak [wykonywać zapytania o dane](/docs/graphql-concepts/)  dla bardziej złożonych witryn Gatsby

## Pozyskiwanie danych z Drupal

### Wymagania

- [Strona Gatsby](/docs/quick-start)
- [Drupal](http://drupal.org)
- [JSON:API moduł](https://www.drupal.org/project/jsonapi) zainstalowany i aktywowany na Drupal

### Wskazówki

1. Zainstaluj wtyczkę `gatsby-source-drupal`.

```shell
npm install --save gatsby-source-drupal
```

2. Dodaj poniższy kod do pliku `gatsby-config.js` by skonfigurować i aktywować wtyczkę.

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-source-drupal`,
      options: {
        baseUrl: `https://your-website/`,
        apiBase: `api`, // optional, defaults to `jsonapi`
      },
    },
  ],
}
```

3. Uruchom serwer developerski przy użyciu komendy `gatsby develop`, otwórz eksplorator GraphiQL pod adresem `http://localhost:8000/___graphql`. 
Poniżej zakładki Eksplorator powinieneś zobaczyć nowe typy node'ów, takie jak `allBlockBlock` dla bloków Drupal i eden dla każdego typu danych w Twoim Drupal.
Na przykład jeśli masz typ danych "Page", będzie on dostępny jako `allNodePage`. By pobrać wszystkie node'y wraz z ich tytułem i body, użyj poniżego zapytania:


```graphql
{
  allNodePage {
    edges {
      node {
        title
        body {
          value
        }
      }
    }
  }
}
```

4. By użyć swoje dane z Drupal, stwórz nową stronę `src/pages/drupal.js`, będzie ona zawierać listę wszystkich node'ów "Page" z Drupal.

_**Uwaga:** dokładna schema GraphQL będzie zależała od tego jak skonfigurowana jest Twoja instancja Drupal._

```jsx:title=src/pages/drupal.js
import React from "react"
import { graphql } from "gatsby"

const DrupalPage = ({ data }) => (
  <div>
    <h1>Drupal pages</h1>
    <ul>
    {data.allNodePage.edges.map(({ node, index }) => (
      <li key={index}>
        <h2>{node.title}</h2>
        <div>
          {node.body.value}
        </div>
      </li>
    ))}
   </ul>
  </div>
)

export default DrupalPage

export const query = graphql`
  {
  allNodePage {
    edges {
      node {
        title
        body {
          value
        }
      }
    }
  }
}
```

5. Mając uruchomiony serwer developerski możesz zobaczyć swoją nową stronę odwiedzając `http://localhost:8000/drupal`

### Dodatkowe źródła

- [Użycie Decoupled Drupal z Gatsby](/blog/2018-08-13-using-decoupled-drupal-with-gatsby/)
- [Więcej o pozyskiwaniu danych z Drupal](/docs/sourcing-from-drupal)
- [Samouczek: Twórz programowo strony z użyciem danych](/tutorial/part-seven/)
