---
title: Plik API gatsby-ssr.js
---

Plik `gatsby-ssr.js` pozwala na zmianę zawartości statycznych plików HTML, tak jakby były renderowane po stronie serwera (SSR) przez Gatsby'ego i Node.js. Aby korzystać z [API SSR Gatsby'ego](/docs/ssr-apis/), stwórz plik o nazwie `gatsby-ssr.js` w katalogu głównym projektu. Wyeksportuj każde API, które chcesz użyć w tym pliku.

API `wrapPageElement` i `wrapRootElement` istnieją zarówno w SSR, jak i [API przeglądarki](/docs/browser-apis). Jeśli używasz któregoś z nich, rozważ, czy powinieneś zaimplementować zarówno w `gatsby-ssr.js`, jak i `gatsby-browser.js`, aby strony generowane przez SSR z Node.js były takie same po [Hydratacji](/docs/glossary#hydration) przez JavaScript w przeglądarce.

```jsx:title=gatsby-ssr.js
const React = require("react")
const Layout = require("./src/components/layout")

// Adds a class name to the body element
exports.onRenderBody = ({ setBodyAttributes }, pluginOptions) => {
  setBodyAttributes({
    className: "my-body-class",
  })
}

// Wraps every page in a component
exports.wrapPageElement = ({ element, props }) => {
  return <Layout {...props}>{element}</Layout>
}
```
