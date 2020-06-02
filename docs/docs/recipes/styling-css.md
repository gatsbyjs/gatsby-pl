---
title: "Przepisy: Stylowanie za pomocą CSS"
tableOfContentsDepth: 1
---

Istnieje wiele sposobów na dodanie stylów do swojej strony; Gatsby wspiera prawie każdy z nich, dzięki wtyczkom oficjalnym i społecznościowym.

## Używanie globalnych plików CSS bez komponentu układu

### Wymagania

- Gotowa [strona Gatsby](/docs/quick-start/) z komponentem strony głównej
- Plik `gatsby-browser`

### Kroki

1. Utwórz globalny plik CSS pod nazwą `src/styles/global.css` i wklej do niego następujący kod:

```css:title=src/styles/global.css
html {
  background-color: lavenderblush;
}

p {
  color: maroon;
}
```

2. Zaimportuj globalny plik CSS w pliku `gatsby-browser.js` następującym sposobem:

```javascript:title=gatsby-browser.js
import "./src/styles/global.css"
```

> **Uwaga:** Możesz także użyć `require('./src/styles/global.css')`, aby zaimportować globalny plik CSS w pliku `gatsby-config.js`

3. Uruchom `gatsby-develop`, aby zaobserwować zaaplikowane style globalne na swojej stronie.

> **Uwaga:** Ten sposób może nie być najlepszym rozwiązaniem, jeżeli używasz stylowania CSS-in-JS, w takim przypadku powinieneś utworzyć stronę układu zawierającą wszystkie współdzielone komponenty. Jest to opisane w kolejnym przepisie.

### Dodatkowe materiały

- Więcej odnośnie [dodawania stylów globalnych bez komponentu układu](/docs/global-css/#adding-global-styles-without-a-layout-component)

## Używanie globalnych stylów w komponencie układu

### Wymagania

- Gotowa [strona Gatsby](/docs/quick-start/) z komponentem strony głównej

### Kroki

Możesz dodać globalne style do [współdzielonego komponentu układu](/tutorial/part-three/#your-first-layout-component). Ten komponent będzie użyty do powtarzających się rzeczy na stronie, takich jak nagłówek lub stopka.

1. Utwórz folder `/src/components`, jeśli jeszcze taki nie istnieje.

2. W środku folderu `components` utwórz dwa pliki pod nazwami: `layout.css` i `layout.js`.

3. Wklej następujący kod do pliku `layout.css`:

```css:title=/src/components/layout.css
body {
  background: red;
}
```

4. Zmodyfikuj plik `layout.js`, aby zaimportować plik CSS i wyeksportować swój układ w następujący sposób:

```jsx:title=/src/components/layout.js
import React from "react"
import "./layout.css"

export default ({ children }) => <div>{children}</div>
```

5. Teraz, zmodyfikuj stronę główną w pliku `/src/pages/index.js` tak, aby korzystałą z komponentu układu:

```jsx:title=/src/pages/index.js
import React from "react"
import Layout from "../components/layout"

export default () => <Layout>Hello world!</Layout>
```

### Dodatkowe materiały

- [Stylowanie za pomocą globalnych plików CSS](/docs/global-css/)
- [Więcej na temat komponentów układu](/tutorial/part-three)

## Korzystanie z Styled Components

### Wymagania

- Gotowa [strona Gatsby](/docs/quick-start/) z komponentem strony głównej
- Zainstalowane w `package.json` wtyczki: [gatsby-plugin-styled-components, styled-components, and babel-plugin-styled-components](/packages/gatsby-plugin-styled-components/)

### Kroki

1. W pliku `gatsby-config.js` dodaj `gatsby-plugin-styled-components`

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-styled-components`],
}
```

1. Otwórz komponent strony głównej (`src/pages/index.js`) i zaimportuj w nim `styled-components`

2. Dodaj style do komponentów poprzez tworzenie bloków stylu dla każdego elementu.

3. Dodaj utworzone komponenty do drzewa JSX, aby je zaaplikować.

```jsx:title=src/pages/index.js
import React from "react"
import styled from "styled-components" //highlight-line

const Container = styled.div`
  margin: 3rem auto;
  max-width: 600px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
`

const Avatar = styled.img`
  flex: 0 0 96px;
  width: 96px;
  height: 96px;
  margin: 0;
`

const Username = styled.h2`
  margin: 0 0 12px 0;
  padding: 0;
`

const User = props => (
  <>
    <Avatar src={props.avatar} alt={props.username} />
    <Username>{props.username}</Username>
  </>
)

export default () => (
  <Container>
    <h1>O Styled Components</h1>
    <p>Styled Components jest cool</p>
    <User
      username="Jan Kowalski"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/adellecharles/128.jpg"
    />
    <User
      username="Alicja Nowak"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/vladarbatov/128.jpg"
    />
  </Container>
)
```

4. Uruchom `gatsby develop`, aby zaobserwować zmiany.

### Dodatkowe materiały

- [Więcej o korzystaniu z Styled Components](/docs/styled-components/)
- [Lekcja Egghead](https://egghead.io/lessons/gatsby-style-gatsby-sites-with-styled-components)

## Korzystanie z Modułów CSS

### Wymagania

- Gotowa [strona Gatsby](/docs/quick-start/) z komponentem strony głównej.

### Kroki

1. Utwórz moduł CSS pod nazwą `src/pages/index.module.css` i wklej do niego następujący kod:

```css:title=src/pages/index.module.css
.feature {
  margin: 2rem auto;
  max-width: 500px;
}
```

2. Zaimportuj utworzony moduł CSS jako `style` w pliku `index.js` i zmodyfikuj stronę, aby wyglądała następująco:

```jsx:title=src/pages/index.js
import React from "react"

// highlight-start
import style from "./index.module.css"

export default () => (
  <section className={style.feature}>
    <h1>Korzystanie z Modułów CSS</h1>
  </section>
)
// highlight-end
```

4. Uruchom `gatsby develop`, aby zaobserwować zmiany.

**Uwaga:**
Pamiętaj o rozszerzeniu pliku `.module.css` zamiast samego `.css`, informuje to Gatsbyiego o tym, że jest on modułem CSS.

### Dodatkowe materiały

- Więcej o [korzystaniu z Modułów CSS](/tutorial/part-two/#css-modules)
- [Przykład wykorzystania Modułów CSS](https://github.com/gatsbyjs/gatsby/blob/master/examples/using-css-modules)

## Korzystanie z Sass/SCSS

Sass to rozszerzenie języka CSS, które umożliwia korzystanie z bardziej zaawansowanych funkcjonalności takich jak zagnieżdżone zasady, zmienne, mixiny i więcej.

Sass korzysta z 2 różnych składni. Najczęściej używaną składnią jest "SCSS" która jest nadzbiorem CSS. Oznacza to, że każda poprawna składnia CSS jest także poprawna w SCSS. Pliki SCSS korzystają z rozszerzenia `.scss`

Sass kompiluje pliki `.scss` i `.sass` do plików `.css` za ciebie, dzięki temu możesz korzystać z bardziej zaawansowanych funkcjonalności podczas pisania swoich arkuszy stylów.

### Wymagania

- Gotowa [strona Gatsby](/docs/quick-start/)

### Kroki

1. Zainstaluj plugin [gatsby-plugin-sass](https://www.gatsbyjs.org/packages/gatsby-plugin-sass/) i `node-sass`.

`npm install --save node-sass gatsby-plugin-sass`

2. Zaimportuj ten plugin w pliku `gatsby-config.js`.

```javascript:title=gatsby-config.js
plugins: [`gatsby-plugin-sass`],
```

1.  Napisz swoje arkusze stylów, zapisz je z rozszerzeniami `.sass` lub `.scss` i zaimportuj do strony. Jeśli nie wiesz jak importować style, odnieś się do [Stylowanie za pomocą CSS](/docs/recipes/#2-styling-with-css)

```css:title=styles.scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

```css:title=styles.sass
$font-stack:    Helvetica, sans-serif
$primary-color: #333

body
  font: 100% $font-stack
  color: $primary-color
```

```javascript
import "./styles.scss"
import "./styles.sass"
```

_Uwaga: Możesz także korzystać z plików Sass/SCSS jako modułów, tak jak już to było wspomniane wcześniej w przepisie o modułach CSS. Jedyną różnicą jest to, że należy korzystać z rozszerzeń `.scss` i `.sass` zamiast `.css`_

### Dodatkowe materiały

- [Różnice pomiędzy .sass a .scss](https://responsivedesign.is/articles/difference-between-sass-and-scss/)
- [Oficjalny poradnik Sass](https://sass-lang.com/guide)
- [Kompletny tutorial o instalacji Sass zawierający więcej wyjaśnień i materiałów.](https://www.gatsbyjs.org/docs/sass/)

## Dodawanie lokalnej czcionki

### Wymagania

- Gotowa [strona Gatsby](/docs/quick-start/)
- Plik z czcionką: `.woff2`, `.ttf`, itd.

### Kroki

1. Skopiuj plik czcionki do projektu Gatsby w następujący sposób: `src/fonts/fontname.woff2`.

```text
src/fonts/fontname.woff2
```

1. Zaimportuj czcionkę do pliku CSS, aby dołączyć ją do swojej strony Gatsby:

```css:title=src/css/typography.css
@font-face {
  font-family: "Font Name";
  src: url("../fonts/fontname.woff2");
}
```

**Uwaga:** Upewnij się, że twoja czcionka jest użyta w odpowiednich sektorach:

```css:title=src/components/layout.css
body {
  font-family: "Font Name", sans-serif;
}
```

Poprzez dodanie czcionki do elementu `HTML`, zaaplikuje się ona do większości tekstów na stronie. Dodatkowe reguły mogą być dodane do innych elementów takich jak `button` lub `textarea`.

Jeżeli czcionki na stornie nie aktualizują się poprawnie, upewnij się, że podmieniłeś istniejące własności font-family w odpowiednich elementach.

### Dodatkowe materiały

- Więcej na temat [importowania zasobów do plików](/docs/importing-assets-into-files/)

## Korzystanie z Emotion

[Emotion](https://emotion.sh) to potężna biblioteka CSS-in-JS, która wspiera zarówno style lokalne, jak i ostylowane komponenty. Można z nich korzystać osobno lub mieszać razem w jednym pliku.

### Wymagania

- Gotowa [strona Gatsby](/docs/quick-start/)

### Kroki

1. Zainstaluj [plugin Gatsby Emotion](/packages/gatsby-plugin-emotion/) i pakiety Emotion.

```shell
npm install --save gatsby-plugin-emotion @emotion/core @emotion/styled
```

1. Dodaj plugin `gatsby-plugin-emotion` do pliku `gatsby-config.js`:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-emotion`],
}
```

3. Jeśli jeszcze tego nie zrobiłeś, utwórz podstronę w swojej stronie Gatsby w folderze `src/pages/emotion-sample.js`.

Zaimportuj `css` z pakietu rdzenia Emotion. Możesz wtedy z niego korzystać, aby dodawać style obiektów Emotion to jakiegokolwiek elementu w komponencie:

```jsx:title=src/pages/emotion-sample.js
import React from "react"
import { css } from "@emotion/core"

export default () => (
  <div>
    <p
      css={{
        background: "pink",
        color: "blue",
      }}
    >
      Ta strona korzysta z Emotion.
    </p>
  </div>
)
```

4. Aby używać [stylowanych komponentów](https://emotion.sh/docs/styled), zaimportuj odpowiedni pakiet i zdefinuj je za pomocą funkcji `styled`.

```jsx:title=src/pages/emotion-sample.js
import React from "react"
import styled from "@emotion/styled"

const Content = styled.div`
  text-align: center;
  margin-top: 10px;
  p {
    font-weight: bold;
  }
`

export default () => (
  <Content>
    <p>Ta strona korzysta z Emotion.</p>
  </Content>
)
```

### Dodatkowe materiały

- [Korzystanie z Emotion w Gatsby](/docs/emotion/)
- [Strona Emotion](https://emotion.sh)
- [Podstawy Emotion i Gatsby](https://egghead.io/lessons/gatsby-getting-started-with-emotion-and-gatsby)

## Korzystanie z Google Fonts

Jeśli hostujemy [Google Fonts](https://fonts.google.com/) lokalnie w swoim projekcie, nasza strona nie będzie wysyłać zapytania o czcionkę przez internet za każdym załadowaniem strony. Może to przyspieszyć twoją stronę aż o ~300 milisekund na komputerze i 1+ sekundę na połączeniu 3G. Rekomenduje się ograniczanie użytku własnych czcionek tylko do tych, które są wymagane, aby zmaksymalizować wydajność strony.

### Wymagania

- Gotowa [strona Gatsby](/docs/quick-start/)
- Zainstalowane [Gatsby CLI](/docs/gatsby-cli/)
- Wybrany pakiet czcionek z [projektu typefaces](https://github.com/KyleAMathews/typefaces)

### Kroki

1. Uruchom `npm install --save typeface-your-chosen-font`, zamieniając `your-chosen-font` na nazwę wybranej czcionki z [projektu typefaces](https://github.com/KyleAMathews/typefaces).

Na przykład, załadowanie popularnej czcionki 'Source Sans Pro' wyglądałoby następująco: `npm install --save typeface-source-sans-pro`.

2. Dodaj linijkę `import "typeface-your-chosen-font"` do twojego komponentu układy, strony, lub pliku `gatsby-browser.js`.

```jsx:title=src/components/layout.js
import "typeface-your-chosen-font"
```

1. Po zaimportowaniu możesz korzystać ze swojej czcionki w arkuszu stylów CSS, module CSS, lub CSS-in-JS.

```css:title=src/components/layout.css
body {
  font-family: "Your Chosen Font";
}
```

_Uwaga: Dla podanego powyżej przykładu, odpowiednią zasadą była by `font-family: 'Source Sans Pro';`_

### Dodatkowe materiały

- [Typography.js](/docs/typography-js/) - Alternatywna opcja umożliwiająca wykorzystywanie Google Fonts na stronie Gatsby.
- [Dokumentacja projektu Typefaces](https://github.com/KyleAMathews/typefaces/blob/master/README.md)
- [Przykład na blogu Kyla Mathewsa](https://www.bricolage.io/typefaces-easiest-way-to-self-host-fonts/)

## Korzystanie z Font Awesome

[Font Awesome](https://fontawesome.com/) umożliwia ci dostęp to tysięcy ikonek których możesz używać na swojej stronie. Ze względu na to że Gatsby korzysta z Reacta, rekomendujemy bibliotekę [react-fontawesome](https://github.com/FortAwesome/react-fontawesome).

### Wymagania

- Gotowa [strona Gatsby](/docs/quick-start/)
- Zainstalowane [Gatsby CLI](/docs/gatsby-cli/)

### Kroki

1. Zainstaluj pakiety `react-fontawesome`.

```shell
npm install @fortawesome/fontawesome-svg-core  @fortawesome/free-brands-svg-icons @fortawesome/react-fontawesome
```

> Pamiętaj że `react-fontawesome` zawiera kilka zbiorów ikonek. Mozliwe że zainteresują cię `free-regular-svg-icons` lub `free-solid-svg-icons`, które instaluje się w taki sam sposób.

2. Zaimportuj komponent `FontAwesomeIcon` i ikonę której chcesz użyć. Potem możesz jej używać jako zwykły komponent w plikach JSX:

```jsx:title=index.js
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome"
import { faReact } from "@fortawesome/free-brands-svg-icons"

const IndexPage = () => (
  <Layout>
    <FontAwesomeIcon icon={faReact} /> //highlight-line
  </Layout>
)

export default IndexPage
```

> W tym przykładzie importowana jest tylko jedna, specyficzna ikonka aby zwiększyć wydajność strony. Alternatywnie możesz także [zimportować kilka ikon i zbudować bibliotekę](https://github.com/FortAwesome/react-fontawesome#build-a-library-to-reference-icons-throughout-your-app-more-conveniently).

### Dodatkowe materiały

- [Font Awesome](https://fontawesome.com/)
- [react-fontawesome](https://github.com/FortAwesome/react-fontawesome)
