---
title: Punkty funkcji pomocniczych API
description: Dokumentacja na temat API funkcji pomocniczych do tworzenia punktów Gatsby wewnątrz warstwy danych GraphQL
jsdoc: ["gatsby/src/utils/api-node-helpers-docs.js"]
apiCalls: NodeAPIHelpers
contentsHeading: Shared helpers
showTopLevelSignatures: true
---

Pierwszym argumentem przekazanym do [punktów API Gatsby](/docs/node-apis/) jest obiekt zawierający zestaw pomocniczych funkcji, udostępnionych przez całe API punktów Gatsby, które są udokumentowane w sekcji [Udostępnione funkcje pomocnicze](#shared-helpers).

```javascript
// w gatsby-node.js
exports.createPages = gatsbyNodeHelpers => {
  const { actions, reporter } = gatsbyNodeHelpers
  // Użyj funkcji pomocniczych
}
```

Powszechnie stosowaną konwencją jest destrukturyzowanie funkcji pomocniczych bezpośrednio w liście argumentów:

```javascript
// w gatsby-node.js
exports.createPages = ({ actions, reporter }) => {
  // Użyj funkcji pomocniczych
}
```

## Uwaga

Niektóre interfejsy API zapewniają dodatkowe funkcje pomocnicze. Na przykład `createPages` dostarcza funkcję `graphql`. Aby uzyskać szczegółowe informacje, zapoznaj się z dokumentacją poszczególnych interfejsów API w [punktach API Gatsby'ego](/docs/node-apis/).
