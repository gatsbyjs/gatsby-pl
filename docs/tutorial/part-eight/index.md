---
title: Przygotowanie strony do wdrożenia
typora-copy-images-to: ./
disableTableOfContents: true
---

Wow! Przebyłeś długą drogę! Dotychczas nauczyłeś się jak:

- tworzyć nowe strony Gatsby
- tworzyć nowe strony i komponenty
- stylować komponenty
- dodawać wtyczki do strony
- pobierać i transformować dane
- używać GraphQL do pobierania danych na stronę
- programatycznie tworzyć nowe strony z pobranych danych

W tej ostatniej sekcji przeprowadzisz kilka typowych kroków przygotowujących witrynę do uruchomienia i wdrożenia, wprowadzając potężne narzędzie diagnostyczne o nazwie [Lighthouse](https://developers.google.com/web/tools/lighthouse/). Po drodze wprowadzimy jeszcze kilka wtyczek, których często będziesz chciał użyć w swoich witrynach Gatsby.

## Audyt Lighthouse

Cytując [stronę Lighthouse](https://developers.google.com/web/tools/lighthouse/):

> Lighthouse to open-source'owe, zautomatyzowane narzędzie do poprawy jakości stron internetowych. Możesz je uruchomić na dowolnej stronie internetowej, publicznej lub wymagającej uwierzytelnienia.Posiada audyty wydajności, dostępności, progresywnych aplikacji internetowych (PWA) i nie tylko.

Lighthouse jest częścią Chrome DevTools. Przeprowadzenie audytu - a następnie usunięcie napotkanych błędów i wdrożenie sugerowanych ulepszeń - to świetny sposób na przygotowanie witryny do uruchomienia. Pomaga ci upewnić się, że Twoja strona jest tak szybka i dostępna, jak to tylko możliwe.

Wypróbuj Lighthouse!

Najpierw musisz utworzyć wersję produkcyjną swojej witryny Gatsby. Serwer programistyczny Gatsby jest zoptymalizowany pod kątem szybkiego programowania; Jednak witryna, którą generuje, choć przypomina wersję produkcyjną witryny, nie jest tak zoptymalizowana.

### ✋ Zbuduj wersję produkcyjną

1.  Zatrzymaj serwer programistyczny (jeśli nadal działa) i uruchom następującą komendę:

```shell
gatsby build
```

> 💡 Jak już się nauczyłeś w [części pierwszej](/tutorial/part-one/), ta komenda buduje wersję produkcyjną Twojej strony i tworzy statyczne pliki w folderze `public`.

2. Aby zobaczyć lokalnie wersję produkcyjną swojej strony, uruchom:

```shell
gatsby serve
```

Po uruchomieniu, możesz wyświetlić swoją witrynę pod adresem [`localhost:9000`](http://localhost:9000).

### Przperowadź audyt Lighthouse

Teraz uruchomisz swój pierwszy test Lighthouse.

1.  Jeśli jeszcze tego nie zrobiłeś, otwórz stronę w trybie incognito w Chrome, aby żadne rozszerzenia nie zakłócały testu. Następnie otwórz Chrome DevTools.

2.  Kliknij zakładkę „Audits”, gdzie zobaczysz ekran wyglądający jak:

![Lighthouse audyt](./lighthouse-audit.png)

3.  Kliknij "Perform an audit..."(Wszystkie dostępne typy kontroli powinny być domyślnie wybrane). Następnie kliknij "Run audit”. (Uruchomienie audytu zajmie około minuty). Po zakończeniu audytu powinieneś zobaczyć wyniki, które wyglądają tak:

![Wyniki audytu Lighthouse](./lighthouse-audit-results.png)

As you can see, Gatsby's performance is excellent out of the box but you're missing some things for PWA, Accessibility, Best Practices, and SEO that will improve your scores (and in the process make your site much more friendly to visitors and search engines).

Jak widać, wydajność Gatsby jest znakomita "z automatu", ale brakuje Ci kilku rzeczy związanych z PWA, dostępnością, najlepszymi praktykami i SEO, które poprawią twoje wyniki (a przez to uczynią twoją stronę bardziej przyjazną dla odwiedzających i silinków wyszukiwania).

## Dodaj plik manifestu

Wygląda na to, że masz dość marny wynik w kategorii „Progresywna aplikacja internetowa”(PWA). Zajmijmy się tym.

Ale najpierw, czym właściwie _są_ PWA?

Są to zwykłe strony internetowe, wykorzystujące nowoczesną funkcjonalność przeglądarki, która ma zwiększyć komfort korzystania z Internetu dzięki funkcjom i zaletom podobnym do działania natywnych aplikacji. Sprawdź [dokument Google](https://developers.google.com/web/progressive-web-apps/) na temat tego, co definiuje PWA.

Dołączenie manifestu aplikacji internetowej jest jednym z trzech ogólnie akceptowanych [podstawowych wymagań dla PWA](https://alistapart.com/article/yes-that-web-project-should-be-a-pwa#section1).

Cytująć [Google](https://developers.google.com/web/fundamentals/web-app-manifest/):

> Manifest aplikacji internetowej to prosty plik JSON, który informuje przeglądarkę o Twojej aplikacji internetowej i tym, jak powinna się zachowywać po „zainstalowaniu” na urządzeniu mobilnym lub komputerze użytkownika.

[Wtyczka manifestu Gatsby](/packages/gatsby-plugin-manifest/) konfiguruje Gatsby tak aby tworzył plik `manifest.webmanifest` przy każdej budowie strony.

### ✋ Używanie `gatsby-plugin-manifest`

1.  Zainstaluj wtyczkę:

```shell
npm install --save gatsby-plugin-manifest
```

2. Dodaj favicon swojej aplikacji w `src/images/icon.png`. Jeśli nie posiadasz faviconu, na potrzeby tego poradnika, użyj [tej przykładowej ikony](https://raw.githubusercontent.com/gatsbyjs/gatsby/master/docs/tutorial/part-eight/icon.png). Ikona jest niezbędna do zbudowania wszystkich obrazów manifestu. Aby uzyskać więcej informacji, zapoznaj się z dokumentacją dotyczącą [`gatsby-plugin-manifest`](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-plugin-manifest/README.md).

3. Dodaj wtyczkę do Array `plugins` w pliku `gatsby-config.js`.

```javascript:title=gatsby-config.js
{
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        // Enables "Add to Homescreen" prompt and disables browser UI (including back button)
        // see https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: `standalone`,
        icon: `src/images/icon.png`, // This path is relative to the root of the site.
      },
    },
  ]
}
```

That's all you need to get started with adding a web manifest to a Gatsby site. The example given reflects a base configuration -- Check out the [plugin reference](/packages/gatsby-plugin-manifest/?=gatsby-plugin-manifest#automatic-mode) for more options.

## Add offline support

Another requirement for a website to qualify as a PWA is the use of a [service worker](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API). A service worker runs in the background, deciding to serve network or cached content based on connectivity, allowing for a seamless, managed offline experience.

[Gatsby's offline plugin](/packages/gatsby-plugin-offline/) makes a Gatsby site work offline and more resistant to bad network conditions by creating a service worker for your site.

### ✋ Using `gatsby-plugin-offline`

1.  Install the plugin:

```shell
npm install --save gatsby-plugin-offline
```

2.  Add the plugin to the `plugins` array in your `gatsby-config.js` file.

```javascript:title=gatsby-config.js
{
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        // Enables "Add to Homescreen" prompt and disables browser UI (including back button)
        // see https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: `standalone`,
        icon: `src/images/icon.png`, // This path is relative to the root of the site.
      },
    },
    // highlight-next-line
    `gatsby-plugin-offline`,
  ]
}
```

That's all you need to get started with service workers with Gatsby.

> 💡 The offline plugin should be listed _after_ the manifest plugin so that the offline plugin can cache the created `manifest.webmanifest`.

## Add page metadata

Adding metadata to pages (such as a title or description) is key in helping search engines like Google understand your content and decide when to surface it in search results.

[React Helmet](https://github.com/nfl/react-helmet) is a package that provides a React component interface for you to manage your [document head](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/head).

Gatsby's [react helmet plugin](/packages/gatsby-plugin-react-helmet/) provides drop-in support for server rendering data added with React Helmet. Using the plugin, attributes you add to React Helmet will be added to the static HTML pages that Gatsby builds.

### ✋ Using `React Helmet` and `gatsby-plugin-react-helmet`

1.  Install both packages:

```shell
npm install --save gatsby-plugin-react-helmet react-helmet
```

2.  Make sure you have a `description` and an `author` configured inside your `siteMetadata` object. Also, add the `gatsby-plugin-react-helmet` plugin to the `plugins` array in your `gatsby-config.js` file.

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
    // highlight-start
    description: `A simple description about pandas eating lots...`,
    author: `gatsbyjs`,
    // highlight-end
  },
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        // Enables "Add to Homescreen" prompt and disables browser UI (including back button)
        // see https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: `standalone`,
        icon: `src/images/icon.png`, // This path is relative to the root of the site.
      },
    },
    `gatsby-plugin-offline`,
    // highlight-next-line
    `gatsby-plugin-react-helmet`,
  ],
}
```

3. In the `src/components` directory, create a file called `seo.js` and add the following:

```jsx:title=src/components/seo.js
import React from "react"
import PropTypes from "prop-types"
import Helmet from "react-helmet"
import { useStaticQuery, graphql } from "gatsby"

function SEO({ description, lang, meta, title }) {
  const { site } = useStaticQuery(
    graphql`
      query {
        site {
          siteMetadata {
            title
            description
            author
          }
        }
      }
    `
  )

  const metaDescription = description || site.siteMetadata.description

  return (
    <Helmet
      htmlAttributes={{
        lang,
      }}
      title={title}
      titleTemplate={`%s | ${site.siteMetadata.title}`}
      meta={[
        {
          name: `description`,
          content: metaDescription,
        },
        {
          property: `og:title`,
          content: title,
        },
        {
          property: `og:description`,
          content: metaDescription,
        },
        {
          property: `og:type`,
          content: `website`,
        },
        {
          name: `twitter:card`,
          content: `summary`,
        },
        {
          name: `twitter:creator`,
          content: site.siteMetadata.author,
        },
        {
          name: `twitter:title`,
          content: title,
        },
        {
          name: `twitter:description`,
          content: metaDescription,
        },
      ].concat(meta)}
    />
  )
}

SEO.defaultProps = {
  lang: `en`,
  meta: [],
  description: ``,
}

SEO.propTypes = {
  description: PropTypes.string,
  lang: PropTypes.string,
  meta: PropTypes.arrayOf(PropTypes.object),
  title: PropTypes.string.isRequired,
}

export default SEO
```

The above code sets up defaults for your most common metadata tags and provides you an `<SEO>` component to work with in the rest of your project. Pretty cool, right?

4.  Now, you can use the `<SEO>` component in your templates and pages and pass props to it. For example, add it to your `blog-post.js` template like so:

```jsx:title=src/templates/blog-post.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"
// highlight-next-line
import SEO from "../components/seo"

export default ({ data }) => {
  const post = data.markdownRemark
  return (
    <Layout>
      // highlight-start
      <SEO title={post.frontmatter.title} description={post.excerpt} />
      // highlight-end
      <div>
        <h1>{post.frontmatter.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.html }} />
      </div>
    </Layout>
  )
}

export const query = graphql`
  query($slug: String!) {
    markdownRemark(fields: { slug: { eq: $slug } }) {
      html
      frontmatter {
        title
      }
      // highlight-next-line
      excerpt
    }
  }
`
```

The above example is based off the [Gatsby Starter Blog](/starters/gatsbyjs/gatsby-starter-blog/). By passing props to the `<SEO>` component, you can dynamically change the metadata for a post. In this case, the blog post `title` and `excerpt` (if it exists in the blog post markdown file) will be used instead of the default `siteMetadata` properties in your `gatsby-config.js` file.

Now, if you run the Lighthouse audit again as laid out above, you should get close to--if not a perfect-- 100 score!

> 💡 For further reading and examples, check out [Adding an SEO Component](/docs/add-seo-component/) and the [React Helmet docs](https://github.com/nfl/react-helmet#example)!

## Keep making it better

In this section, we've shown you a few Gatsby-specific tools to improve your site's performance and prepare to go live.

Lighthouse is a great tool for site improvements and learning -- Continue looking through the detailed feedback it provides and keep making your site better!

## Next Steps

### Official Documentation

- [Official Documentation](https://www.gatsbyjs.org/docs/): View our Official Documentation for _[Quick Start](https://www.gatsbyjs.org/docs/quick-start/)_, _[Detailed Guides](https://www.gatsbyjs.org/docs/preparing-your-environment/)_, _[API References](https://www.gatsbyjs.org/docs/gatsby-link/)_, and much more.

### Official Plugins

- [Official Plugins](https://github.com/gatsbyjs/gatsby/tree/master/packages): The complete list of all the Official Plugins maintained by Gatsby.

### Official Starters

1.  [Gatsby's Default Starter](https://github.com/gatsbyjs/gatsby-starter-default): Kick off your project with this default boilerplate. This barebones starter ships with the main Gatsby configuration files you might need. _[working example](http://gatsbyjs.github.io/gatsby-starter-default/)_
2.  [Gatsby's Blog Starter](https://github.com/gatsbyjs/gatsby-starter-blog): Gatsby starter for creating an awesome and blazing-fast blog. _[working example](http://gatsbyjs.github.io/gatsby-starter-blog/)_
3.  [Gatsby's Hello-World Starter](https://github.com/gatsbyjs/gatsby-starter-hello-world): Gatsby Starter with the bare essentials needed for a Gatsby site. _[working example](https://gatsby-starter-hello-world-demo.netlify.com/)_

## That's all, folks

Well, not quite; just for this tutorial. There are [Additional Tutorials](/tutorial/additional-tutorials/) to check out for more guided use cases.

This is just the beginning. Keep going!

- Did you build something cool? Share it on Twitter, tag [#buildwithgatsby](https://twitter.com/search?q=%23buildwithgatsby), and [@mention us](https://twitter.com/gatsbyjs)!
- Did you write a cool blog post about what you learned? Share that, too!
- Contribute! Take a stroll through [open issues](https://github.com/gatsbyjs/gatsby/issues?q=is%3Aissue+is%3Aopen+label%3A%22help+wanted%22) on the gatsby repo and [become a contributor](/contributing/how-to-contribute/).

Check out the ["how to contribute"](/contributing/how-to-contribute/) docs for even more ideas.

We can't wait to see what you do 😄.
