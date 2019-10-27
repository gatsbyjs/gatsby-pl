# Style Guide

Ten dokument zawiera zasady specyfyczne dla języka, których należy przestrzegać podczas tłumaczenia.

## Zasady

### Tekst w Blokach Kodu

Tekst w blokach kodu, z wyjątkiem komentarzy, nie powinien być tłumaczony. Opcjonalnie możesz przetłumaczyć łańcuchy znaków, ale uważaj, aby nie odnosiły się one do kodu!

Przykład:

```js
// Example
import React from "react"
export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Hello Gatsby!</div>
)
```

✅ DOBRZE:

```js
// Przykład
import React from "react"
export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Hello Gatsby!</div>
)
```

✅ RÓWNIEŻ DOBRZE:

```js
// Przykład
import React from "react"
export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Witaj, Gatsby!</div>
)
```

❌ ŹLE:

```js
// Przykład
import React from "react"
export default () => (
  // 'purple' jest słowem kluczowym CSS
  <div style={{ color: `fioletowy`, fontSize: `72px` }}>Witaj, Gatsby!</div>
)
```

❌ ZDECYDOWANIE ŹLE:

```js
importuj React z "react"
eksportuj domyślnie () => (
   <div styl={{ kolor: `fioletowy`, fontSize:` 72px` }}>Witaj, Gatsby!</div>
)
```

### Zewnętrzne Linki

Jeśli zewnętrzny link prowadzi do artykułu w np. [MDN] lub [Wikipedia], a istnieje jego dobrze przetłumaczona wersja, weź pod uwagę, czy nie lepiej byłoby użyć jej.

[mdn]: https://developer.mozilla.org/en-US/
[wikipedia]: https://en.wikipedia.org/wiki/Main_Page

Przykład:

```md
React is a JavaScript [library](https://en.wikipedia.org/wiki/Library_(computing)).
```

✅ DOBRZE:

```md
React jest javascriptową [biblioteką](https://pl.wikipedia.org/wiki/Biblioteka_programistyczna).
```

Dla linków, które nie mają polskiego odpowiednika (Stack Overflow, filmy na YouTube , itd.), po prostu użyj wersji angielskiej.

## Słowniczek

Użyj tej sekcji, jako spisu tłumaczeń dla często używanych, technicznych pojęć.

| Term   | Translation |
| ------ | ----------- |
| Plugin | Wtyczka     |
| Theme  | Motyw       |
| Query  | Zapytanie   |
| String | Łańcuch znaków   |
