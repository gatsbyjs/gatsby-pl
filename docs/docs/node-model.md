---
title: Node Model
description: Dokumentacja tłumacząca działanie modelu node'ów w warstwie danych Gatsby GraphQL
jsdoc: ["gatsby/src/schema/node-model.js"]
apiCalls: NodeModel
contentsHeading: Metody
---

Gatsby udostępnia swój wewnętrzny magazyn danych oraz możliwości zapytań do pól resolwerów GraphQL poprzez `context.nodeModel`.

## Przykładowe zastosowanie

```javascript:title=gatsby-node.js
createResolvers({
  Query: {
    mood: {
      type: `String`,
      resolve(source, args, context, info) {
        const coffee = context.nodeModel.getAllNodes({ type: `Coffee` })
        if (!coffee.length) {
          return 😞
        }
        return 😊
      },
    },
  },
})
```
