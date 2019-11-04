---
title: Tworzenie komponentów zagnieżdżonych układów
typora-copy-images-to: ./
disableTableOfContents: true
---

Witaj w części trzeciej!

## Czego nauczysz się w tym poradniku?

W tej części dowiesz się o wtyczkach Gatsby i tworzeniu komponentów „układu”.

Wtyczki Gatsby to paczki JavaScript, które pomagają dodać różne funkcjonalności do strony. Gatsby zaprojektowano tak aby był jak najbardziej elastyczny, co oznacza, że wtyczki mogą rozszerzać i modyfikować niemal wszystko, co robi.

Komponenty układu budują sekcje witryny, które chcesz udostępnić na wielu stronach. Na przykład, witryny zwykle zawierają komponent układu z takim samym nagłówkiem i stopką. Inne typowe elementy układów to pasek boczny i / lub nawigacja. Na przykład na tej stronie nagłówek u góry jest częścią komponentu układu gatsbyjs.org.

Zacznijmy teraz część trzecią poradnika.

## Używanie wtyczek

Prawdopodobnie znasz już idee wtyczek. Wiele systemów obsługuje dodawanie niestandardowych wtyczek w celu dodania nowej funkcjonalności, a nawet zmodyfikowania podstawowych funkcji oprogramowania. Wtyczki Gatsby działają w ten sam sposób.

Członkowie społeczności (tacy jak Ty!) Mogą dodawać wtyczki (niewielkie ilości kodu JavaScript), z których inni mogą korzystać podczas tworzenia witryn Gatsby.

> Istnieją już setki wtyczek! Poznaj [Bibliotekę Wtyczek](/plugins/) Gatsby.

Naszym celem przy tworzeniu wtyczek jest, aby były łatwe do zainstalowania i używania. Prawdopodobnie będziesz używać wtyczek w prawie każdej budowanej witrynie Gatsby. Podczas dalszej części samouczka będziesz mieć wiele okazji do przećwiczenia instalacji i używania wtyczek.

Jako wstęp do korzystania z wtyczek zainstalujemy i wdrożymy wtyczkę Gatsby dla Typography.js.

[Typography.js](https://kyleamathews.github.io/typography.js/) to biblioteka JavaScript, która generuje globalne, bazowe style dla typografii na Twojej stronie. Posiada ona [odpowiednią wtyczkę Gatsby](/packages/gatsby-plugin-typography/), która usprawnia korzystanie z biblioteki wraz z Gatsby.

### ✋ Utwórz nową stronę Gatsby

Jak już wspomnieliśmy w [części drugiej](/tutorial/part-two/), w tym momencie prawdopodobnie dobrym pomysłem będzie zamknięcie okien terminali i plików z poprzednich części samouczka, aby utrzymać porządek na pulpicie. Następnie otwórz nowe okno terminala i uruchom następujące polecenia, aby utworzyć nową witrynę Gatsby w katalogu o nazwie „tutorial-part-three”, a następnie przejdź do nowego katalogu:

```shell
gatsby new tutorial-part-three https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-three
```

### ✋ Zainstaluj i skonfiguruj `gatsby-plugin-typography`

Korzystanie z wtyczek składa się z dwóch głównych kroków: instalacji i konfiguracji.

1. Zainstaluj paczkę NPM `gatsby-plugin-typography`.

```shell
npm install --save gatsby-plugin-typography react-typography typography typography-theme-fairy-gates
```

> Uwaga: Typography.js wymaga kilku dodatkowych paczek, dlatego są one zawarte w instrukcji. Dodatkowe wymagania, są zwykle wymienione w instrukcjach „instalowania” danej wtyczki.

2. W głównym folderze, zmien plik `gatsby-config.js`, aby wyglądał następująco:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    },
  ],
}
```

Plik `gatsby-config.js` to kolejny specjalny plik, który Gatsby automatycznie rozpozna. Możesz tutaj dodać wtyczki i inne elementy konfiguracji witryny.

> Jeśli chcesz dowiedzieć się więcej, sprawdź [Dokumentację Gatsby na temat gatsby-config.js](/docs/gatsby-config/).

3. Typography.js potrzebuje pliku konfiguracyjnego. Utwórz nowy katalog o nazwie `utils` w katalogu` src`. Następnie dodaj nowy plik o nazwie `typography.js` do `utils` i skopiuj do niego poniższy kod:

```javascript:title=src/utils/typography.js
import Typography from "typography"
import fairyGateTheme from "typography-theme-fairy-gates"

const typography = new Typography(fairyGateTheme)

export const { scale, rhythm, options } = typography
export default typography
```

4. Uruchom serwer deweloperski.

```shell
gatsby develop
```

Po załadowaniu witryny, jeśli sprawdzisz wygenerowany kod HTML za pomocą Chrome dev tools, zobaczysz, że wtyczka Typography.js dodała element `<style>` do elementu `<head>` z wygenerowanym kodem CSS:

![style-typograficzne](typography-styles.png)

### ✋ Wprowadź zmiany treści i stylu

Skopiuj poniższy kod do pliku `src/pages/index.js`, aby lepiej zobaczyć
efekt wygenerowanego przez Typography.js kodu CSS.

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div>
    <h1>Cześć! Tworzę przykładową witrynę Gatsby w ramach poradnika!</h1>
    <p>
      Co lubię robić? Oczywiście wiele, ale zdecydowanie lubię budować
       strony internetowe.
    </p>
  </div>
)
```

Twoja witryna powinna teraz wyglądać następująco:

![bez-układu](no-layout.png)

Zróbmy szybką poprawkę. Wiele witryn ma jedną kolumnę tekstu wyśrodkowaną na środku strony. Aby utworzyć taki układ, dodaj następujące style do
`<div>` w pliku `src/pages/index.js`.

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  // highlight-next-line
  <div style={{ margin: `3rem auto`, maxWidth: 600 }}>
    <h1>Cześć! Tworzę przykładową witrynę Gatsby w ramach poradnika!</h1>
    <p>
      Co lubię robić? Oczywiście wiele, ale zdecydowanie lubię budować
       strony internetowe.
    </p>
  </div>
)
```

![z-układem](with-layout2.png)

Nieźle. Właśnie zainstalowałeś/aś i skonfigurowałeś/aś swoją pierwszą wtyczkę Gatsby!

## Tworzenie komponentów układu

Teraz przejdźmy do nauki o komponentach układu. Aby przygotować się do tej części, dodaj do projektu kilka nowych stron: stronę o mnie i stronę kontaktową.

```jsx:title=src/pages/about.js
import React from "react"

export default () => (
  <div>
    <h1>O mnie</h1>
    <p>Jestem wystarczająco dobry, jestem wystarczająco inteligentny i do cholery, ludzie mnie lubią!</p>
  </div>
)
```

```jsx:title=src/pages/contact.js
import React from "react"

export default () => (
  <div>
    <h1>Chętnie porozmawiam! Napisz me emaila na poniższy adres</h1>
    <p>
      <a href="mailto:me@example.com">me@example.com</a>
    </p>
  </div>
)
```

Zobaczmy, jak wygląda nowa strona:

![o-mnie-nie-wycentrowane](about-uncentered.png)

Hmm. Byłoby lepiej, gdyby treść dwóch nowych stron była wyśrodkowana tak jak strona główna. Poza tym, byłoby super mieć globalną nawigację, aby odwiedzający mogli łatwo znaleźć i odwiedzić każdą z podstron.

Poradzisz sobie z tymi zmianami, tworząc pierwszy komponent układu.

### ✋ Utwórz swój pierwszy komponent układu

1. Utwórz nowy folder `src/components`.

2. Utwórz bardzo podstawowy komponent układu `src/components/layout.js`:

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    {children}
  </div>
)
```

3. Zaimportuj nowy komponent w komponencie strony `src/pages/index.js`:

```jsx:title=src/pages/index.js
import React from "react"
import Layout from "../components/layout" // highlight-line

export default () => (
  <Layout> {/* highlight-line */}
    <h1>Cześć! Tworzę przykładową witrynę Gatsby w ramach poradnika!</h1>
    <p>
      Co lubię robić? Oczywiście wiele, ale zdecydowanie lubię budować
       strony internetowe.
    </p>
  </Layout> {/* highlight-line */}
)
```

![z-układem](with-layout2.png)

Nieźle, układ działa prawidłowo! Treść strony głównej jest nadal wyśrodkowana.

Spróbuj jednak przejśc do stron `/about/`, albo `/contact/`. Treść na tych stronach nadal nie będzie wyśrodkowana..

4. Zaimportuj komponent układu do `about.js` oraz `contact.js` (tak jak zrobiłeś dla `index.js` w poprzednim kroku).

Zawartość wszystkich trzech stron jest wyśrodkowana dzięki jednemu wspólnemu komponentowi układu!

### ✋ Dodaj tytuł strony

1. Dodaj następujący wiersz do nowego komponentu układu:

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    <h3>MojaŚwietnaStrona</h3> {/* highlight-line */}
    {children}
  </div>
)
```

Jeśli przejdziesz na którąkolwiek ze swoich trzech stron, zobaczysz dodany ten sam tytuł, np. strona `/about/`:

![z-tytułem](with-title.png)

### ✋ Dodaj linki nawigacyjne pomiędzy stronami

1. Skopiuj następujące elementy do pliku komponentu układu:

```jsx:title=src/components/layout.js
import React from "react"
// highlight-start
import { Link } from "gatsby"

const ListLink = props => (
  <li style={{ display: `inline-block`, marginRight: `1rem` }}>
    <Link to={props.to}>{props.children}</Link>
  </li>
)
// highlight-end

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    {/* highlight-start */}
    <header style={{ marginBottom: `1.5rem` }}>
      <Link to="/" style={{ textShadow: `none`, backgroundImage: `none` }}>
        <h3 style={{ display: `inline` }}>MojaŚwietnaStrona</h3>
      </Link>
      <ul style={{ listStyle: `none`, float: `right` }}>
        <ListLink to="/">Strona główna</ListLink>
        <ListLink to="/about/">O mnie</ListLink>
        <ListLink to="/contact/">Kontakt</ListLink>
      </ul>
    </header>
    {/* highlight-end */}
    {children}
  </div>
)
```

![z-nawigacja2](with-navigation2.png)

Mamy to! Trzystronicowa witryna z podstawową globalną nawigacją.

_Wyzwania:_ Dzięki nowym możliwościom „komponentu układu” spróbuj dodać nagłówki, stopki, globalną nawigację, paski boczne itp. na swoich stronach Gatsby!

## Co dalej?

Kontynuuj do [części czwartej poradnika](/tutorial/part-four/) gdzie zaczniesz poznawać warstwę danych Gatsby i programatycznie tworzyć strony!
