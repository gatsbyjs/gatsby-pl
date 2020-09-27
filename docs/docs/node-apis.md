---
title: Gatsby Node APIs
description: Dokumentacja Node API wykorzystywanego podczas budowania powszechnych zastosowań, takich jak generowanie stron
jsdoc: ["gatsby/src/utils/api-node-docs.js"]
apiCalls: NodeAPI
---

Gatsby udostępnia wtyczkom i kreatorom stron wiele interfejsów API umożliwiających kontrolowanie przepływu danych za pośrednictwem GraphQL.

## Wtyczki asynchroniczne

Jeśli twoja wtyczka wykonuje operacje asynchroniczne (dysk I/O, dostęp do bazy danych, wywoływanie zdalnych interfejsów API, etc.) musisz zwrócić obietnicę albo użyć funkcji zwrotnej przekazanej jako trzeci argument. Gatsby musi wiedzieć kiedy wtyczki są gotowe, tak samo jak niektóre API aby zadziałać poprawnie - wymagają zakończenia pracy poprzednich interfejsów API. Sprawdź [Debugowanie asynchronicznych cykli życia](/docs/debugging-async-lifecycles/) aby dowiedzieć się więcej.

```javascript
// Promise API
exports.createPages = () => {
  return new Promise((resolve, reject) => {
    // wykonaj kod asynchroniczne
  })
}

// Callback API
exports.createPages = (_, pluginOptions, cb) => {
  // wykonaj kod asynchroniczne
  cb()
}
```

Jeśli twoja wtyczka nie wykonuje kodu asynchronicznie - funkcję możesz po prostu zwrócić.

## Zastosowanie

Każde z tych API implementuj poprzez export z pliku `gatsby-node.js` umiejscowionego w głównym folderze projektu.
