---
title: The gatsby-browser.js API file
---

Plik `gatsby-browser.js` pozwala reagować na to co się dzieje w przeglądarce i na dodawaniu dodatkowych komponentów (głównie rodziców) do Twojej strony. [API Gatsby Browser](/docs/browser-apis) daje wiele opcji interakcji z [client-side](/docs/glossary#client-side) Gatsby.

Interfejsy API `wrapPageElement` oraz `wrapRootElement` istnieją zarówno w przeglądarce, jak i [interfejsach API renderowania po stronie serwera (SSR)](/docs/ssr-apis). Jeśli używasz jednego z nich, zastanów się, czy nie powinieneś wdrożyć go w `gatsby-ssr.js` oraz `gatsby-browser.js`, żeby strony wygenerowane przez SSR z Node.js były takie same po procesie [hydration](/docs/glossary#hydration) przeglądarkowym JavaScript.

Aby użyć interfejsów Browser API, stwórz plik w katalogu głównym witryny pod adresem `gatsby-browser.js`. Wyeksportuj każdy interfejs API, który chcesz użyć, z tego pliku.

```jsx:title=gatsby-browser.js
const React = require("react")
const Layout = require("./src/components/layout")

// Logs when the client route changes
exports.onRouteUpdate = ({ location, prevLocation }) => {
  console.log("new pathname", location.pathname)
  console.log("old pathname", prevLocation ? prevLocation.pathname : null)
}

// Wraps every page in a component
exports.wrapPageElement = ({ element, props }) => {
  return <Layout {...props}>{element}</Layout>
}
```
