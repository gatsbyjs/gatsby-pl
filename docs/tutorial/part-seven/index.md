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

You want to use each markdown file name to create the page slug. So
`pandas-and-bananas.md` will become `/pandas-and-bananas/`. But how do you get
the file name from the `MarkdownRemark` node? To get it, you need to _traverse_
the "node graph" to its _parent_ `File` node, as `File` nodes contain data you
need about files on disk. To do that, modify your function again:

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

After restarting your development server, you should see the relative paths for your two markdown
files print to the terminal screen.

![markdown-relative-path](markdown-relative-path.png)

Now you'll have to create slugs. As the logic for creating slugs from file names can get
tricky, the `gatsby-source-filesystem` plugin ships with a function for creating
slugs. Let's use that.

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`) // highlight-line

exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    console.log(createFilePath({ node, getNode, basePath: `pages` })) // highlight-line
  }
}
```

The function handles finding the parent `File` node along with creating the
slug. Run the development server again and you should see logged to the terminal
two slugs, one for each markdown file.

Now you can add your new slugs directly onto the `MarkdownRemark` nodes. This is
powerful, as any data you add to nodes is available to query later with GraphQL.
So, it'll be easy to get the slug when it comes time to create the pages.

To do so, you'll use a function passed to your API implementation called
[`createNodeField`](/docs/actions/#createNodeField). This function
allows you to create additional fields on nodes created by other plugins. Only
the original creator of a node can directly modify the node—all other plugins
(including your `gatsby-node.js`) must use this function to create additional
fields.

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

Restart the development server and open or refresh GraphiQL. Then run this
GraphQL query to see your new slugs.

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

Now that the slugs are created, you can create the pages.

## Creating pages

In the same `gatsby-node.js` file, add the following.

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

You've added an implementation of the
[`createPages`](/docs/node-apis/#createPages) API which Gatsby calls so plugins can add
pages.

As mentioned in the intro to this part of the tutorial, the steps to programmatically creating pages are:

1.  Query data with GraphQL
2.  Map the query results to pages

The above code is the first step for creating pages from your markdown as you're
using the supplied `graphql` function to query the markdown slugs you created.
Then you're logging out the result of the query which should look like:

![query-markdown-slugs](query-markdown-slugs.png)

You need one additional thing beyond a slug to create pages: a page template
component. Like everything in Gatsby, programmatic pages are powered by React
components. When creating a page, you need to specify which component to use.

Create a directory at `src/templates`, and then add the following in a file named
`src/templates/blog-post.js`.

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

Then update `gatsby-node.js`

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

Restart the development server and your pages will be created! An easy way to
find new pages you create while developing is to go to a random path where
Gatsby will helpfully show you a list of pages on the site. If you go to
<http://localhost:8000/sdf>, you'll see the new pages you created.

![new-pages](new-pages.png)

Visit one of them and you see:

![hello-world-blog-post](hello-world-blog-post.png)

Which is a bit boring and not what you want. Now you can pull in data from your markdown post. Change
`src/templates/blog-post.js` to:

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

And…

![blog-post](blog-post.png)

Sweet!

The last step is to link to your new pages from the index page.

Return to `src/pages/index.js`, query for your markdown slugs, and create
links.

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

And there you go! A working, albeit small, blog!

## Challenge

Try playing more with the site. Try adding some more markdown files. Explore
querying other data from the `MarkdownRemark` nodes and adding them to the
frontpage or blog posts pages.

In this part of the tutorial, you've learned the foundations of building with
Gatsby's data layer. You've learned how to _source_ and _transform_ data using
plugins, how to use GraphQL to _map_ data to pages, and then how to build _page
template components_ where you query for data for each page.

## What's coming next?

Now that you've built a Gatsby site, where do you go next?

- Share your Gatsby site on Twitter and see what other people have created by searching for #gatsbytutorial! Make sure to mention @gatsbyjs in your Tweet and include the hashtag #gatsbytutorial :)
- You could take a look at some [example sites](https://github.com/gatsbyjs/gatsby/tree/master/examples#gatsby-example-websites)
- Explore more [plugins](/docs/plugins/)
- See what [other people are building with Gatsby](/showcase/)
- Check out the documentation on [Gatsby's APIs](/docs/api-specification/), [nodes](/docs/node-interface/), or [GraphQL](/docs/graphql-reference/)
