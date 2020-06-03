---
title: Plik API gatsby-browser.js
---

Plik `gatsby-browser.js` pozwala Ci reagować na akcję, które dzieją się w przeglądarce oraz zawierać Twoją stronę w dodatkowe komponenty. [Gatsby Browser API](/docs/browser-apis) daje Ci wiele opcji interakcji [po stronie klienta](/docs/glossary#client-side) w Gatsby.

API `wrapPageElement` oraz `wrapRootElement` występują zarówno w przeglądarce jak i w [API renderowania po stronie serwera (SSR)](/docs/ssr-apis). Jeżeli używasz tylko jednego tych dwóch rozwiązań, zastanów się, czy nie powinieneś zaimplementować go w `gatsby-ssr.js` oraz `gatsby-browser.js`, dzięki temu strony generowane przez SSR z Node.js są takie same jak po procesie [hydration](/docs/glossary#hydration) przeglądarkowym JavaScript.

By skorzystać z Browser API, stwórz plik w katalogu głównym witryny pod adresem `gatsby-browser.js`. Wyeksportuj z tego pliku każdy interfejs API którego chcesz użyć.

```jsx:title=gatsby-browser.js
const React = require("react")
const Layout = require("./src/components/layout")

// Wypisuje w konsoli gdy użytkownik zmieni stronę
exports.onRouteUpdate = ({ location, prevLocation }) => {
  console.log("new pathname", location.pathname)
  console.log("old pathname", prevLocation ? prevLocation.pathname : null)
}

// Zawiera całą stronę w komponencie
exports.wrapPageElement = ({ element, props }) => {
  return <Layout {...props}>{element}</Layout>
}
```
