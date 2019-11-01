---
title: Wprowadzenie do Stylowania w Gatsby
typora-copy-images-to: ./
disableTableOfContents: true
---

<!-- Idea: Create a glossary to refer to. A lot of these terms get jumbled -->

<!--
  - Global styles
  - Component css
  - CSS-in-JS
  - CSS Modules

-->

Witaj w drugiej części tutorialu Gatsby

## Czego naucysz się w tym tutorialu?

W tej części zapoznasz się z mozliwościami stylizacji stron internetowych Gatsby i przyjrzysz się blizej zastosowaniu komponentów React'a w celu tworzenia witryn.

## Zastosowanie stylów globalnych

Kazda strona posiada jakiś rodzaj globalnego stylu. Zaliczają się do tego rzeczy takie jak typografia i kolory tła. Styly te odpowiadają za ogólny odbiór i odczucie strony internetowej - zupełnie jak kolor i faktura ścian w pokoju definiuje panującą w nim atmosferę.

### Tworzenie stylów globalnych przy urzyciu standarwodych plików CSS

Jednym z najbardziej podstawowych sposobów aby dodać style do strony jest uzycie globalnego stylesheet'u `.css`.

#### ✋ Utwórz nową stronę Gatsby

Rozpocznij poprzez utworzenie nowej strony Gatsby. Najlepiej (szczególnie gdy uzywasz linii komend od niedawna) zamknąć wszystkie okna terminala, których uzywałeś w [części pierwszej](/tutorial/part-one/) i otwórz nowe okno dla drugiej części tutorialu.


Otwórz nowe okno terminala, stwórz nową stronę Gatsby "hello world" oraz uruchom serwer developerski (development server):

```shell
gatsby new tutorial-part-two https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-two
```

Utworzyłeś teraz nową stonę Gatsby (na podstawie gotowego projektu startowego "hello world") o następującej strukturze:

```text
├── package.json
├── src
│   └── pages
│       └── index.js
```

#### ✋ Dodaj style do pliku css

1. Stwórz plik `.css` w swoim nowym projekcie:

```shell
cd src
mkdir styles
cd styles
touch global.css
```

> Uwaga: Mozesz uzyc dowolnego edytora kodu aby utworzyc ponizsza strukturę plików i folderów, wedle własnych preferencji.

Powinieneś mieć teraz tak wyglądającą strukturę:

```text
├── package.json
├── src
│   └── pages
│       └── index.js
│   └── styles
│       └── global.css
```

2. Zdefiniuj jakiś styl w pliku `global.css`:

```css:title=src/styles/global.css
html {
  background-color: lavenderblush;
}
```

> Uwaga: Umiejscowienie przykładowego pliku css w folderze `/src/styles/` jest dowolne.

#### ✋ Dodaj stylesheet do pliku `gatsby-browser.js`

1. Utwórz plik `gatsby-browser.js`

```shell
cd ../..
touch gatsby-browser.js
```

Twoja struktura plików powinna w tym momencie wyglądać tak:

```text
├── package.json
├── src
│   └── pages
│       └── index.js
│   └── styles
│       └── global.css
├── gatsby-browser.js
```

> 💡 Za co odpowiada `gatsby-browser.js`? Nie przejmuj się zbytnio tym na tym etapie, wiedz tylko, ze `gatsby-browser.js` jest jednym z wielu specjalnych plików które Gatsby automatycznie wyszukuje i uzywa (o ile istnieją). W tym wypadku nazwa pliku **ma znaczenie**. Jeśli chcesz dowiedzieć się więcej na ten temat juz teraz, sprawdz [dokumentację](/docs/browser-apis/).

2. Zaimportuj swoje właśnie utworzone stylesheet'y do pliku `gatsby-browser.js`:

```javascript:title=gatsby-browser.js
import "./src/styles/global.css"

// or:
// require('./src/styles/global.css')
```

> Uwaga: Zarówno syntax CommonJS (`require`) jak Moduł ES (`import`), zadziałają w tym wypadku. Jeśli nie jesteś pewien który z nich wybrać, my w większości wypadków uzywamy 'import'.

3. Uruchom serwer deweloperski (development server):

```shell
gatsby develop
```

Jeli spojrzysz teraz na swój projekt w przeglądarce, powinieneś zobaczyć w starterze "hello world" zastosowane lawendowe tło:

![Lavender Hello World!](global-css.png)

> Porada: Ta część tutorialu skupia się na najszybszym i prostoliniowym sposobie na rozpoczęcie stylowania strony Gatsby - bezpośrednim zaimportowaniu standardowych plików CSS uzywając `gatsby-browser.js`. Jednakl w większości wypadków, najelpszą metodą na zastosowanie stylów globalnych jest uzycie współdzielonego komponentu odpowiadającego za układ strony. [Sprawdź dokumentację](/docs/global-css/) aby dowiedzieć się więcej na ten temat.

## Uzycie CSS o zasęgu kompnentu.

Do tej pory rozmawialiśmy o bardziej tradycyjnym podejściu do używania standardowych arkuszy stylów css. Teraz porozmawiamy o różnych metodach rozbicia CSS na niezalezne moduły w celu zastosowania stylowania w sposób zorientowany na pracę z komponentami.

### Moduły CSS

Zajmijmy się **Modułami CSS**. Cytując
[stronę główną modółów CSS](https://github.com/css-modules/css-modules):

> **Moduł CSS** jest plikiem CSS, w którym wszystkie nazwy klas i nazwy animacji 
>  posiadają domyślny zasięg lokalny.

Moduły CSS są bardzo popularne, ponieważ pozwalają normalnie pisać CSS, ale z większym bezpieczeństwem. Narzędzie automatycznie generuje unikalne nazwy klas i animacji, więc nie musisz się martwić o kolizje nazw selektorów.

Gatsby działa od razu z modułami CSS. Ten sposób jest wysoce zalecany dla osób, które dopiero zaczynają budować z Gatsby (i ogólnie z React).

#### ✋ Zbuduj nową stronę uzywając Modółów CSS

W tej sekcji będziesz tworzyć komponent nowej strony oraz style , you'll create a new page component and style that page component using a CSS Module.

First, create a new `Container` component.

1. Create a new directory at `src/components` and then, in this new directory, create a file named `container.js` and paste the following:

```javascript:title=src/components/container.js
import React from "react"
import containerStyles from "./container.module.css"

export default ({ children }) => (
  <div className={containerStyles.container}>{children}</div>
)
```

You'll notice you imported a css module file named `container.module.css`. Let's create that file now.

2. In the same directory (`src/components`), create a `container.module.css` file and copy/paste the following:

```css:title=src/components/container.module.css
.container {
  margin: 3rem auto;
  max-width: 600px;
}
```

You'll notice that the file name ends with `.module.css` instead of the usual `.css`. This is how you tell Gatsby that this CSS file should be processed as a CSS module rather than plain CSS.

3. Create a new page component by creating a file at
   `src/pages/about-css-modules.js`:

```javascript:title=src/pages/about-css-modules.js
import React from "react"

import Container from "../components/container"

export default () => (
  <Container>
    <h1>About CSS Modules</h1>
    <p>CSS Modules are cool</p>
  </Container>
)
```

Now, if you visit `http://localhost:8000/about-css-modules/`, your page should look something like this:

![Page with CSS module styles](css-modules-basic.png)

#### ✋ Style a component using CSS Modules

In this section, you'll create a list of people with names, avatars, and short Latin biographies. You'll create a `<User />` component and style that component using a CSS module.

1. Create the file for the CSS at `src/pages/about-css-modules.module.css`.

2. Paste the following into the new file:

```css:title=src/pages/about-css-modules.module.css
.user {
  display: flex;
  align-items: center;
  margin: 0 auto 12px auto;
}

.user:last-child {
  margin-bottom: 0;
}

.avatar {
  flex: 0 0 96px;
  width: 96px;
  height: 96px;
  margin: 0;
}

.description {
  flex: 1;
  margin-left: 18px;
  padding: 12px;
}

.username {
  margin: 0 0 12px 0;
  padding: 0;
}

.excerpt {
  margin: 0;
}
```

3. Import the new `src/pages/about-css-modules.module.css` file into the `about-css-modules.js` page you created earlier by editing the first few lines of the file like so:

```javascript:title=src/pages/about-css-modules.js
import React from "react"
// highlight-next-line
import styles from "./about-css-modules.module.css"
import Container from "../components/container"

// highlight-next-line
console.log(styles)
```

The `console.log(styles)` code will log the resulting import so you can see the result of your processed `./about-css-modules.module.css` file. If you open the developer console (using e.g. Firefox or Chrome's developer tools) in your browser, you'll see:

![Import result of CSS module in console](css-modules-console.png)

If you compare that to your CSS file, you'll see that each class is now a key in the imported object pointing to a long string e.g. `avatar` points to `src-pages----about-css-modules-module---avatar---2lRF7`. These are the class names CSS Modules generates. They're guaranteed to be unique across your site. And because you have to import them to use the classes, there's never any question about where some CSS is being used.

4. Create a `User` component.

Create a new `<User />` component inline in the `about-css-modules.js` page
component. Modify `about-css-modules.js` so it looks like the following:

```jsx:title=src/pages/about-css-modules.js
import React from "react"
import styles from "./about-css-modules.module.css"
import Container from "../components/container"

console.log(styles)

// highlight-start
const User = props => (
  <div className={styles.user}>
    <img src={props.avatar} className={styles.avatar} alt="" />
    <div className={styles.description}>
      <h2 className={styles.username}>{props.username}</h2>
      <p className={styles.excerpt}>{props.excerpt}</p>
    </div>
  </div>
)
// highlight-end

export default () => (
  <Container>
    <h1>About CSS Modules</h1>
    <p>CSS Modules are cool</p>
    {/* highlight-start */}
    <User
      username="Jane Doe"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/adellecharles/128.jpg"
      excerpt="I'm Jane Doe. Lorem ipsum dolor sit amet, consectetur adipisicing elit."
    />
    <User
      username="Bob Smith"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/vladarbatov/128.jpg"
      excerpt="I'm Bob Smith, a vertically aligned type of guy. Lorem ipsum dolor sit amet, consectetur adipisicing elit."
    />
    {/* highlight-end */}
  </Container>
)
```

> Tip: Generally, if you use a component in multiple places on a site, it should be in its own module file in the `components` directory. But, if it's used only in one file, create it inline.

The finished page should now look like:

![User list page with CSS modules](css-modules-userlist.png)

### CSS-in-JS

CSS-in-JS is a component-oriented styling approach. Most generally, it is a pattern where [CSS is composed inline using JavaScript](https://reactjs.org/docs/faq-styling.html#what-is-css-in-js).

#### Using CSS-in-JS with Gatsby

There are many different CSS-in-JS libraries and many of them have Gatsby plugins already. We won't cover an example of CSS-in-JS in this initial tutorial, but we encourage you to [explore](/docs/styling/) what the ecosystem has to offer. There are mini-tutorials for two libraries, in particular, [Emotion](/docs/emotion/) and [Styled Components](/docs/styled-components/).

#### Suggested reading on CSS-in-JS

If you're interested in further reading, check out [Christopher "vjeux" Chedeau's 2014 presentation that sparked this movement](https://speakerdeck.com/vjeux/react-css-in-js) as well as [Mark Dalgleish's more recent post "A Unified Styling Language"](https://medium.com/seek-blog/a-unified-styling-language-d0c208de2660).

### Other CSS options

Gatsby supports almost every possible styling option (if there isn't a plugin yet for your favorite CSS option, [please contribute one!](/contributing/how-to-contribute/))

- [Typography.js](/packages/gatsby-plugin-typography/)
- [Sass](/packages/gatsby-plugin-sass/)
- [JSS](/packages/gatsby-plugin-jss/)
- [Stylus](/packages/gatsby-plugin-stylus/)
- [PostCSS](/packages/gatsby-plugin-postcss/)

and more!

## What's coming next?

Now continue on to [part three of the tutorial](/tutorial/part-three/), where you'll learn about Gatsby plugins and layout components.
