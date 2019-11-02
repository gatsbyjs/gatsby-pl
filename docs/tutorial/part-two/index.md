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

Witaj w drugiej części poradnika Gatsby

## Czego nauczysz się w tym poradniku?

W tej części zapoznasz się z możliwościami stylowania stron internetowych Gatsby i przyjrzysz się bliżej zastosowaniu komponentów React'a w celu tworzenia witryn.

## Zastosowanie stylów globalnych

Każda strona posiada jakieś style globalne. Zaliczają się do tego rzeczy takie jak typografia i kolory tła. Style te odpowiadają za ogólny odbiór i odczucie strony internetowej - zupełnie jak kolor i faktura ścian w pokoju definiuje panującą w nim atmosferę.

### Tworzenie Stylów Globalnych przy użyciu standardowych plików CSS

Jednym z najbardziej podstawowych sposobów aby dodać style do strony jest użycie globalnego arkusza styli `.css`.

#### ✋ Utwórz nową stronę Gatsby

Rozpocznij poprzez utworzenie nowej strony Gatsby. Najlepiej (szczególnie gdy używasz interfejsu wiersza poleceń od niedawna) zamknij wszystkie okna terminala, których używałeś w [części pierwszej](/tutorial/part-one/) i otwórz nowe okno dla drugiej części tutorialu.

Otwórz nowe okno terminala, stwórz nową stronę Gatsby "hello world" oraz uruchom serwer developerski (development server):

```shell
gatsby new tutorial-part-two https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-two
```

Utworzyłeś teraz nową stronę Gatsby (na podstawie gotowego Projektu Startowego "hello world") o następującej strukturze:

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

> Uwaga: Możesz użyc dowolnego edytora kodu, aby utworzyc poniższą strukturę plików i folderów, wedle własnych preferencji.

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

> Uwaga: Umiejscowienie przykładowego pliku css w folderze `/src/styles/` jest opcjonalne.

#### ✋ Dodaj arkusz styli do pliku `gatsby-browser.js`

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

> 💡 Za co odpowiada `gatsby-browser.js`? Nie przejmuj się tym teraz, wiedz tylko, że `gatsby-browser.js` jest jednym z wielu specjalnych plików które Gatsby automatycznie wyszukuje i używa (o ile istnieją). W tym wypadku nazwa pliku **ma znaczenie**. Jeśli chcesz dowiedzieć się więcej na ten temat już teraz, sprawdź [Dokumentację](/docs/browser-apis/).

2. Zaimportuj swoje właśnie utworzone arkusze styli do pliku `gatsby-browser.js`:

```javascript:title=gatsby-browser.js
import "./src/styles/global.css"

// or:
// require('./src/styles/global.css')
```

> Uwaga: Zarówno składnia CommonJS (`require`), jak i ES Module (`import`), zadziała w tym wypadku. Jeśli nie jesteś pewien którą z nich wybrać, my w większości wypadków używamy 'import'.

3. Uruchom Serwer Deweloperski (development server):

```shell
gatsby develop
```

Jeśli spojrzysz teraz na swój projekt w przeglądarce, powinieneś zobaczyć w Starterze "hello world" zastosowane lawendowe tło:

![Lavender Hello World!](global-css.png)

> Porada: Ta część poradnika skupia się na najszybszym i najprostszym sposobie na rozpoczęcie stylowania strony Gatsby - bezpośrednim zaimportowaniu standardowych plików CSS przy użyciu `gatsby-browser.js`. Jednak w większości przypadków, najlepszą metodą na zastosowanie Stylów Globalnych jest użycie współdzielonego komponentu odpowiadającego za układ strony. [Sprawdź Dokumentację](/docs/global-css/) aby dowiedzieć się więcej na ten temat.

## Użycie CSS o zasięgu komponentu

Do tej pory rozmawialiśmy o bardziej tradycyjnym podejściu do używania standardowych arkuszy stylów css. Teraz porozmawiamy o różnych metodach rozbicia CSS na niezależne moduły w celu zastosowania stylowania w sposób zorientowany na pracę z komponentami.

### Moduły CSS

Zajmijmy się **Modułami CSS**. Cytując
[stronę główną Modułów CSS](https://github.com/css-modules/css-modules):

> **Moduł CSS** jest plikiem CSS, w którym wszystkie nazwy klas i nazwy animacji 
>  posiadają domyślny zasięg lokalny.

Moduły CSS są bardzo popularne, ponieważ pozwalają pisać zwykły, lecz bardziej bezpieczny kod  CSS. Narzędzie automatycznie generuje unikalne nazwy klas i animacji, więc nie musisz się martwić o kolizje nazw Selektorów.

Gatsby ma wbudowane wsparcie dla Modułów CSS. Ten sposób jest wysoce zalecany dla osób, które dopiero zaczynają budować strony z Gatsby (i ogólnie z React'em).

#### ✋ Zbuduj nową stronę używając Modułów CSS

W tej sekcji będziesz tworzyć komponent nowej strony oraz ostylujesz ten komponent używając Modułu CSS.

Po pierwsze, utwórz nowy komponent 'Container'.

1. Utwórz nowy katalog w `src/components`, a następnie w tym nowym katalogu utwórz plik o nazwie `container.js` i wklej następujący kod:

```javascript:title=src/components/container.js
import React from "react"
import containerStyles from "./container.module.css"

export default ({ children }) => (
  <div className={containerStyles.container}>{children}</div>
)
```

Zauważ, że zaimportowałeś plik modułu css o nazwie `container.module.css`. Utwórzmy teraz ten plik.

2. W tym samym katalogu (`src/components`), utwórz plik `container.module.css` i kopiuj/wklej następujący kod:

```css:title=src/components/container.module.css
.container {
  margin: 3rem auto;
  max-width: 600px;
}
```

Zauważ, że nazwa pliku kończy się na `.module.css` zamiast zwykłego `.css`. W ten sposób informujesz Gatsby, że ten plik CSS powinien być przetwarzany jako Moduł CSS, zamiast zwykłego CSS.

3. Stwórz komponent nowej strony poprzez utworzenie pliku w:
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

Teraz, gdy odwiedzisz `http://localhost:8000/about-css-modules/`, Twoja strona powinna wyglądać mniej więcej tak:

![Strona ze stylami Modułów CSS](css-modules-basic.png)

#### ✋ Ostyluj komponent używając Modułów CSS

W tej sekcji utworzysz listę osób o imionach, awatarach i krótkich biografiach łacińskich. Utworzysz także Komponent `<User />' i nadasz mu Styl za pomocą Modułu CSS.

1. Utwórz plik dla CSS w `src/pages/about-css-modules.module.css`.

2. Wklej poniższy kod do nowo utworzonego pliku:

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

3. Zaimportuj nowy plik `src/pages/about-css-modules.module.css` do strony `about-css-modules.js`, którą wcześniej utworzyłeś, poprzez edytowanie pierwszych kilku linijek kodu, w ten sposób:

```javascript:title=src/pages/about-css-modules.js
import React from "react"
// highlight-next-line
import styles from "./about-css-modules.module.css"
import Container from "../components/container"

// highlight-next-line
console.log(styles)
```

Kod `console.log(styles)` wyświetli wykonany import, a więc zobaczysz wynik pochodzący z twojego przetworzonego pliku `./about-css-modules.module.css`. Jeśli otworzysz Konsolę Deweloperską (korzystając np. z narzędzi deweloperskich Firefox'a lub Chrome) w swojej przeglądarce, oto co zobaczysz:

![Rezultat zaimportowania Modułu CSS w konsoli](css-modules-console.png)

Jeśli porównasz to ze swoim plikiem CSS, zauważysz, że każda z klas jest teraz kluczem w zaimportowanym obiekcie wskazującym na długi String - np. 'avatar' wskazuje na `src-pages----about-css-modules-module---avatar---2lRF7`. Tak wyglądają nazwy klas wygenerowane przez Moduł CSS. Jest zagwarantowane, że nazwy te będą unikatowe na całej Twojej stronie. I ponieważ musisz zaimportować je, by użyć klas, nie ma nigdy wątpliwości, gdzie jaki CSS został użyty.

4. Stwórz komponent `User`.

Stwórz nowy komponent `<User />` bezpośrednio w kodzie komponentu strony `about-css-modules.js`. 
Zmodyfikuj `about-css-modules.js` tak, by wyglądał w poniższy sposób:

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

> Wskazówka: Generalnie, jeśli użyjesz komponentu w kilku różnych miejscach na stronie, powinien on znajdować się we własnym pliku-module w folderze 'components'. Ale gdy jest użyty tylko w jednym pliku, napisz go bezpośrednio w kodzie.

Skończona strona powinna wyglądać w taki sposób:

![Strona listy użytkowników z Modułami CSS](css-modules-userlist.png)

### CSS-in-JS

CSS-in-JS jest metodą stylowania zorientowaną na komponenty. Najogólniej mówiąc, jest to schemat w którym [CSS jest napisany bezpośrednio w pliku JavaScript](https://reactjs.org/docs/faq-styling.html#what-is-css-in-js).

#### Używanie CSS-in-JS z Gatsby

Jest wiele różnych bibliotek CSS-in-JS i wiele z nich posiada już wtyczki do Gatsby. Nie będziemy omawiać przykładu CSS-in-JS w tym początkowym samouczku, ale zachęcamy Cię do [zbadania](/docs/styling/) co ten ekosystem ma do zaoferowania. Mamy mini-samouczki dla dwóch bibliotek, a konkretnie, [Emotion](/docs/emotion/) oraz [Styled Components](/docs/styled-components/).

#### Zalecane lektury na temat CSS-in-JS

Jeśli jesteś zainteresowany dalszą lekturą, sprawdź [Christopher "vjeux" Chedeau's 2014 presentation that sparked this movement](https://speakerdeck.com/vjeux/react-css-in-js), a także [jeden z nowszych wpisów Mark'a Dalgleish'a "A Unified Styling Language"](https://medium.com/seek-blog/a-unified-styling-language-d0c208de2660).

### Inne opcje CSS

Gatsby wspiera prawie każdą możliwą opcję stylowania (jeśli wciąż nie ma wtyczki do twojego ulubionego sposobu na użycie CSS, [prosimy o twój wkład!](/contributing/how-to-contribute/))

- [Typography.js](/packages/gatsby-plugin-typography/)
- [Sass](/packages/gatsby-plugin-sass/)
- [JSS](/packages/gatsby-plugin-jss/)
- [Stylus](/packages/gatsby-plugin-stylus/)
- [PostCSS](/packages/gatsby-plugin-postcss/)

i więcej!

## Czego nauczysz się następnie?

Przejdź teraz do [części trzeciej samouczka](/tutorial/part-three/), gdzie dowiesz się więcej na temat wtyczek Gatsby i komponentów Układu Stron.
