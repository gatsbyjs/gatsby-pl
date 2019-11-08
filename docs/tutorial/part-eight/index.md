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

W tej ostatniej sekcji przeprowadzisz kilka typowych kroków przygotowujących witrynę do uruchomienia i wdrożenia, wprowadzając potężne narzędzie diagnostyczne o nazwie [Lighthouse](https://developers.google.com/web/tools/lighthouse/). Po drodze poznamy jeszcze kilka wtyczek, których często będziesz chciał użyć w swoich witrynach Gatsby.

## Audyt Lighthouse

Cytując [stronę Lighthouse](https://developers.google.com/web/tools/lighthouse/):

> Lighthouse to open-source'owe, zautomatyzowane narzędzie do poprawy jakości stron internetowych. Możesz je uruchomić na dowolnej stronie internetowej, publicznej lub wymagającej uwierzytelnienia. Posiada audyty wydajności, dostępności, progresywnych aplikacji internetowych (PWA) i nie tylko.

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

To wszystko czego potrzebujesz aby dodać manifest do swojej strony Gatsby. Podany przykład ilustruje bazową konfigurację -- Sprawdź [dokumentację wtyczki](/packages/gatsby-plugin-manifest/?=gatsby-plugin-manifest#automatic-mode) aby zobaczyć więcej opcji.

## Dodaj obsługę offline

Kolejnym wymogiem, aby strona internetowa mogła zostać zakwalifikowana jako PWA, jest użycie [service worker](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API). Service worker działa w tle, decydując się na udostępnianie treści sieciowych lub buforowanych w zależności od dostępności połączenia internetowego, umożliwiając bezproblemowe zarządzanie offline.

[Wtyczka Gatsby offline](/packages/gatsby-plugin-offline/) sprawia, że witryna Gatsby działa offline i jest bardziej odporna na słabe połączenie internetowe, tworząc service worker'a dla Twojej witryny.

### ✋ Używanie `gatsby-plugin-offline`

1.  Zainstaluj wtyczkę

```shell
npm install --save gatsby-plugin-offline
```

2.  Dodaj wtyczkę do Array `plugins` w pliku `gatsby-config.js`.

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

To wszystko, czego potrzebujesz, aby rozpocząć pracę z service worker'ami w Gatsby.

> 💡 Wtyczka offline powinna być umieszczona _po_ bo wtyczce manifestu aby wtyczka offline mogła zapisać utworzony plik `manifest.webmanifest` w cache.

## Dodaj metadane strony

Dodanie metadanych do stron (jak np. tytuł lub opis) ma kluczowe znaczenie dla wyszukiwarek, takich jak Google, w zrozumieniu treści i podjęciu decyzji, kiedy pokazać ją w wynikach wyszukiwania.

[React Helmet](https://github.com/nfl/react-helmet) to paczka która daje Ci do dyspozycji komponent React do zarządzania [head'em dokumentu/strony](https://developer.mozilla.org/pl/docs/Web/HTML/Element/head).

[Wtyczka react helmet](/packages/gatsby-plugin-react-helmet/) zapewnia wsparcie dla renderowania danych dodanych za pomocą React Helmet. Za pomocą wtyczki atrybuty dodane do React Helmet zostaną dodane do statycznych stron HTML tworzonych przez Gatsby.

### ✋ Używanie `React Helmet` oraz `gatsby-plugin-react-helmet`

1.  Zainstaluj obie paczki:

```shell
npm install --save gatsby-plugin-react-helmet react-helmet
```

2. Upewnij się, że w obiekcie `siteMetadata` masz ustawione pola `description` i `author`. Ponadto, dodaj wtyczkę `gatsby-plugin-react-helmet` do Array `plugins` w pliku `gatsby-config.js`.

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

3. W folderze `src/components`, utwórz plik `seo.js` i dodaj:

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

Powyższy kod ustawia wartości domyślne dla najpopularniejszych tagów metadanych i daje Ci do dyspozycji komponent `<SEO>` do pracy w pozostałej części projektu. Całkiem fajnie, prawda?

4.  Teraz możesz używać komponentu `<SEO>` w swoich szablonach i stronach i przekazywać do niego props'y. Na przykład dodaj go do szablonu `blog-post.js` w następujący sposób:

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

Powyższy przykład oparty jest na [Gatsby Starter Blog](/starters/gatsbyjs/gatsby-starter-blog/). Przesyłając props'y do komponentu `<SEO>`, możesz dynamicznie zmieniać metadane dla postu. W tym wypadku zamiast domyślnych właściwości `siteMetadata` z pliku `gatsby-config.js`, zostaną użyte `title` i `excerpt` bloga (jeśli istnieje w pliku markdown).

Teraz, jeśli ponownie uruchomisz audyt Lighthouse, zgodnie z powyższym opisem, powinieneś zbliżyć się do - jeśli nie osiągnąć - 100 punktów!

> 💡 Po więcej informacji i przykładów zobacz [Dodawania komponentu SEO](/docs/add-seo-component/) oraz [dokumentację React Helmet](https://github.com/nfl/react-helmet#example)!

## Cały czas polepszaj wynik

W tej sekcji pokazaliśmy kilka narzędzi specyficznych dla Gatsby, które mają poprawić wydajność witryny i przygotować ją do uruchomienia/wdrożenia.

Lighthouse to świetne narzędzie do ulepszania strony oraz do nauki -- Kontynuuj przeglądanie szczegółowych raportów i ulepszaj swoją witrynę!

## Następne kroki

### Oficjalna dokumentacja

- [Oficjalna dokumentacja](https://www.gatsbyjs.org/docs/): Zobcz naszą oficjalnę dokumentację dla _[Szybkiego startu](https://www.gatsbyjs.org/docs/quick-start/)_, _[Szczegółowych poradników](https://www.gatsbyjs.org/docs/preparing-your-environment/)_, _[Referencji do API](https://www.gatsbyjs.org/docs/gatsby-link/)_, i wielu innych.

### Oficjalne wtyczki

- [Oficjalne wtyczki](https://github.com/gatsbyjs/gatsby/tree/master/packages): Kompletna lista Oficjalnych wtyczek utrzymywanych przez zespół Gatsby.

### Oficjalne startery

1.  [Gatsby's Domyślny Starter](https://github.com/gatsbyjs/gatsby-starter-default): Rozpocznij projekt z tym domyślnym szablonem. Ten prosty starter jest dostarczany z głównymi plikami konfiguracyjnymi Gatsby, których możesz potrzebować. _[działający przykład](http://gatsbyjs.github.io/gatsby-starter-default/)_
2.  [Gatsby's Blog Starter](https://github.com/gatsbyjs/gatsby-starter-blog): Starter Gatsby do stworzenia wspaniałego i niesamowicie szybkiego bloga. _[działający przykład](http://gatsbyjs.github.io/gatsby-starter-blog/)_
3.  [Gatsby's Hello-World Starter](https://github.com/gatsbyjs/gatsby-starter-hello-world): Starter Gatsby Starter z niezbędnym minimum potrzebnym do rozpoczęcia strony Gatsby. _[działający przykład](https://gatsby-starter-hello-world-demo.netlify.com/)_

## To by było na tyle

Cóż, niezupełnie; tylko dla tego poradnika. Sprawdż [Dodatkowe Poradniki](/tutorial/additional-tutorials/) aby zobaczyć więcej przykładów użycia Gatsby.

To dopiero początek. Tak trzymaj!

- Zbudowałeś coś fajnego? Udostępnij to nw Twiterze z hashtagiem [#buildwithgatsby](https://twitter.com/search?q=%23buildwithgatsby), i [@oznacz nas](https://twitter.com/gatsbyjs)!
- Czy napisałeś fajny artykuł na blogu o tym, czego się nauczyłeś? To też udostępnij!
- Współtwórz Gatsby! Przejrzyj [otwarte zagadnienia](https://github.com/gatsbyjs/gatsby/issues?q=is%3Aissue+is%3Aopen+label%3A%22help+wanted%22) w repozytorium Gatsby i [zostań współtwórcą](/contributing/how-to-contribute/).

Sprawdź dokumentację na temat tego ["jak współtworzyć"](/contributing/how-to-contribute/) po więcej pomysłów.

Nie możemy się doczekać, aby zobaczyć, co zrobisz 😄.
