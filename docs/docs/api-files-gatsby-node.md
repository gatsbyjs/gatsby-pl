---
title: Plik API gatsby-node.js
---

Kod z pliku `gatsby-node.js` jest uruchamiany raz, w trakcie budowania witryny. Możesz go użyć do dynamicznego tworzenia stron, dodawania punktów (nodes) GraphQL lub reagowania na zdarzenia procesu budowania. Aby korzystać z [Node'ów API Gatsby'ego](/docs/node-apis/), stwórz plik o nazwie `gatsby-node.js` w katalogu głównym projektu. Wyeksportuj każde API, które chcesz użyć w tym pliku.

Każdy Node API Gatsby'ego przekazuje [zestaw pomocniczych Punktów API](/docs/node-api-helpers/). Umożliwiają one dostęp do kilku metod, takich jak raportowanie, lub wywoływanie czynności, np. tworzenie nowych stron.

```js:title=gatsby-node.js
const path = require(`path`)
// Log out information after a build is done
exports.onPostBuild = ({ reporter }) => {
  reporter.info(`Your Gatsby site has been built!`)
}
// Create blog pages dynamically
exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions
  const blogPostTemplate = path.resolve(`src/templates/blog-post.js`)
  const result = await graphql(`
    query {
      allSamplePages {
        edges {
          node {
            slug
            title
          }
        }
      }
    }
  `)
  result.data.allSamplePages.edges.forEach((edge) => {
    createPage({
      path: `${edge.node.slug}`,
      component: blogPostTemplate,
      context: {
        title: edge.node.title,
      },
    })
  })
}
```
