---
title: Get to Know Gatsby Building Blocks
typora-copy-images-to: ./
disableTableOfContents: true
---

W [**poprzedniej sekcji**](/tutorial/part-zero/), przygotowałeś swoje środowisko deweloperskie poprzez zainstalowanie potrzebnego oprogramowania i stworzenie swojej własnej strony Gatsby [**“hello world” starter**](https://github.com/gatsbyjs/gatsby-starter-hello-world). Spójrzmy teraz dokładniej na kod wygenerowany przez ten starter.

## Korzystanie ze starterów Gatsby

W [**części zerowej poradnika**](/tutorial/part-zero/), stworzyłeś nową stronę bazującą na starterze “hello world” używając komendy:

```shell
gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world
```

Podczas tworzenia nowej strony Gatsby, możesz użyć poniższej komendy, aby stworzyć nową stronę bazując na jakimkolwiek istniejącym już starterze Gatsby:

```shell
gatsby new [NAZWA_FOLDERU_STRONY] [URL_REPO_STARTERA_NA_GITHUB]
```

Jeśli pominiesz ostatni URL, Gatsby automatycznie wygeneruje stronę bazując na [**domyślnym starterze**](https://github.com/gatsbyjs/gatsby-starter-default). Dla tej sekcji, zostań przy stronie “Hello World” którą stworzyłeś wcześniej w części zerowej poradnika. Możesz dowiedzieć się więcej o [modyfikowaniu starterów](/docs/modifying-a-starter) w dokumentacji.

### ✋ Otwórz kod

W edytorze kodu, otwórz kod wygenerowany dla twojej strony “Hello World” i przyjrzyj się folderom i plikom znajdującym się w folderze ‘hello-world’. Powinno to wyglądać mniej więcej tak:

![Projekt Hello World w VS Code](01-hello-world-vscode.png)

_Note: Edytor pokazany tutaj to Visual Studio Code. Jeśli korzystasz z innego edytora, może to wyglądać nieco inaczej._

Rzućmy okiem na kod, który zasila stronę główną.

> 💡 Jeśli zatrzymałeś już swój serwer deweloperski bo uruchomieniu `gatsby develop` w sekcji poprzedniej, wystartuj go znowu — czas wprowadzić kilka zmian na stronie hello-world!

## Zapoznanie ze stronami Gatsby

Otwórz folder `/src` w swoim edytorze kodu. W środku znajduje się pojedynczy folder: `/pages`.

Otwórz plik `src/pages/index.js`. Kod z tego pliku tworzy komponent, który zawiera pojedynczy div i trochę tekstu — “Hello world!”

### ✋ Wprowadź zmiany na stronie głównej “Hello World”

1.  Zmień tekst “Hello World!” na “Hello Gatsby!” i zapisz plik. Jeśli twoje okna znajdują się obok siebie, powinieneś zauważyć, że zmiany w kodzie i zawartości są widoczne niemal natychmiast w przeglądarce, po zapisaniu pliku.

<video controls="controls" autoplay="true" loop="true">
<<<<<<< HEAD
  <source type="video/mp4" src="./02-demo-hot-reloading.mp4"></source>
  <p>Przepraszamy! Twoja przeglądarka nie może wyświetlić tego filmu.</p>
=======
  <source type="video/mp4" src="./02-demo-hot-reloading.mp4" />
  <p>Sorry! Your browser doesn't support this video.</p>
>>>>>>> 8ff6bb09c23261662f47e79a041a92855d517097
</video>

> 💡 Gatsby korzysta z **hot reloading** aby przyspieszyć twój proces dewelopmentu. Zasadniczo, kiedy masz uruchomiony serwer deweloperski Gatsby, pliki twojej strony są cały czas "obserwowane" w tle — za każdym razem kiedy zapiszesz plik, Twoje zmiany zostaną wyświetlone w przeglądarce. Nie potrzebujesz odświeżać całej strony ani restartować serwera — Twoje zmiany po prostu się pojawią.

2.  Teraz możesz sprawić, aby Twoje zmiany były bardziej widoczne. Spróbuj zastąpić kod, który obecnie znajduje się w `src/pages/index.js` kodem, który jest poniżej i zapisz plik. Zauważysz zmiany w tekście — kolor zmieni się na fioletowy, a czcionka będzie większa.

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Hello Gatsby!</div>
)
```

> 💡 Przyjrzymy się stylowaniu lepiej w [**części drugiej**](/tutorial/part-two/) poradnika.

3.  Usuń style odpowiadające za rozmiar czcionki, zamień napis “Hello Gatsby!” na nagłówek poziomu pierwszego i dodaj pod nim paragraf.

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  {/* highlight-start */}
  <div style={{ color: `purple` }}>
    <h1>Hello Gatsby!</h1>
    <p>What a world.</p>
  {/* highlight-end */}
  </div>
)
```

![Więcej zmian z hot reloadingiem](03-more-hot-reloading.png)

4.  Dodaj zdjęcie. (W tym przypadku losowe zdjęcie z Unsplash).

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div style={{ color: `purple` }}>
    <h1>Hello Gatsby!</h1>
    <p>What a world.</p>
    {/* highlight-next-line */}
    <img src="https://source.unsplash.com/random/400x200" alt="" />
  </div>
)
```

![Dodaj zdjęcie](04-add-image.png)

### Zaraz zaraz… HTML wewnątrz JavaScript?

_Jeśli znasz już React i JSX, możesz pominąć tę sekcję._ Jeśli nie pracowałeś jeszcze z Reactem, pewnie zastanawiasz się, co HTML robi w javascriptowej funkcji. Albo dlaczego importujemy `react` w pierwszej linii mimo, że nigdzie go nie używamy. Ta hybryda “HTML-w-JS” to tak właściwie rozszerzenie składni JavaScript dla Reacta zwane JSX. Możesz podążać dalej za poradnikiem, bez wcześniejszego doświadczenia z Reactem, ale jeśli jesteś ciekaw, oto krótkie wprowadzenie…

Weźmy na przykład oryginalną zawartość pliku `src/pages/index.js`:

```jsx:title=src/pages/index.js
import React from "react"

export default () => <div>Hello world!</div>
```

W czystym JavaScripcie, wygląda to mniej więcej tak:

```javascript:title=src/pages/index.js
import React from "react"

export default () => React.createElement("div", null, "Hello world!")
```

Teraz widzisz przeznaczenie importu `'react'`! Ale zaraz. Przy pisaniu używasz JSX, a nie czystego HTMLu i JavaScriptu. Jak przeglądarka jest w stanie to przeczytać? Krótko mówiąc: Nie jest. Strony Gatsby są wyposażone w narzędzia skonfigurowane tak, aby konwertować twój kod źródłowy na coś, co przeglądarki potrafią zrozumieć.

## Budowanie przy użyciu komponentów

Strona główna, którą właśnie edytowałeś była stworzona przy użyciu komponentu strony. Czym właściwie jest “komponent”?

Generalnie, komponent jest elementem składowym Twojej strony; Jest to samodzielny fragment kodu, który opisuje jakąś część interfejsu użytkownika (UI - user interface).

Gatsby jest oparty na React. Kiedy mówimy o używaniu i tworzeniu **komponentów**, tak na prawdę chodzi nam o **komponenty Reactowe** — samodzielne fragmenty kodu (napisane zazwyczaj przy użyciu JSX), które potrafią przyjmować dane wejściowe i zwracać Reactowe elementy opisujące część interfejsu użytkownika.

Jedna ważna zmiana, którą zauważysz, gdy zaczniesz budować przy użyciu komponentów (jeśli jesteś już deweloperem) to ta, że Twój CSS, HTML i JavaScript są ściśle powiązane i często mieszczą się nawet w tym samym pliku.

Choć może się wydawać, że nie jest to nic takiego, ma to spory wpływ na to, w jaki sposób myślisz o tworzeniu stron internetowych.

Weźmy za przykład tworzenie niestandardowego przycisku. W przeszłości stworzyłbyś klasę CSS (powiedzmy `.primary-button`) z Twoimi niestandardowymi stylami, a następnie używał jej, kiedy chciałbyś z tych styli skorzystać. Na przykład:

```html
<button class="primary-button">Click me</button>
```

W świecie komponentów, zamiast tego tworzysz komponent `PrimaryButton` z Twoimi stylami przycisku i używasz go na swojej stronie:

<!-- prettier-ignore -->
```jsx
<PrimaryButton>Click me</PrimaryButton>
```

Komponenty stają się podstawowymi elementami do budowy Twojej strony. Zamiast ograniczać się do elementów, których dostarcza przeglądarka, n.p. `<button />`, możesz z łatwością tworzyć nowe elementy, które spełniają wymagania Twoich projektów.

### ✋ Korzystanie z komponentów stron

Każdy Reactowy komponent znajdujący się w `src/pages/*.js` zostanie automatycznie przekształcony w stronę. Zobaczmy to w akcji.

Masz już plik `src/pages/index.js`, który był w starterze “Hello World”. Stwórzmy stronę "O Mnie" (About).

1.  Stwórz nowy plik w `src/pages/about.js`, skopiuj poniższy kod do tego pliku i zapisz zmiany.

```jsx:title=src/pages/about.js
import React from "react"

export default () => (
  <div style={{ color: `teal` }}>
    <h1>About Gatsby</h1>
    <p>Such wow. Very React.</p>
  </div>
)
```

2.  Otwórz `http://localhost:8000/about/`.

![Nowa strona O Mnie](05-about-page.png)

Samo umieszczenie Reactowego komponentu w pliku `src/pages/about.js` wystarczyło, aby strona była dostępna na `/about`.

### ✋ Korzystanie z sub-komponentów

Powiedzmy, że strona główna i o mnie stały się już całkiem spore i musiałeś przepisywać wiele rzeczy. Możesz użyć sub-komponentów aby rozbić interfejs na kawałki, których użyjesz wielokrotnie. Obie Twoje strony mają nagłówki `<h1>` — stwórz komponent, który będzie opisywał `Header`.

1.  Stwórz nowy folder `src/components` a w nim plik `header.js`.
2.  Dodaj poniższy kod do nowo stworzonego pliku `src/components/header.js`.

```jsx:title=src/components/header.js
import React from "react"

export default () => <h1>This is a header.</h1>
```

3.  Zmodyfikuj plik `about.js` tak, aby importował `Header` component. Zastąp znacznik `h1` komponentem `<Header />`:

```jsx:title=src/pages/about.js
import React from "react"
import Header from "../components/header" // highlight-line

export default () => (
  <div style={{ color: `teal` }}>
    <Header /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
)
```

![Dodawanie komponentu Header](06-header-component.png)

W przeglądarce, tekst w nagłówku “About Gatsby” powinien być zastąpiony na “This is a header.” Z tym, że nie chcesz aby strona “O mnie”, również miała w nagłówku “This is a header.” Chcesz, aby tam widniało “About Gatsby”.

4.  Przejdź do `src/components/header.js` i wprowadź następującą zmianę:

```jsx:title=src/components/header.js
import React from "react"

export default props => <h1>{props.headerText}</h1> {/* highlight-line */}
```

5.  Wróć do `src/pages/about.js` i zmień kod, aby wyglądał tak:

```jsx:title=src/pages/about.js
import React from "react"
import Header from "../components/header"

export default () => (
  <div style={{ color: `teal` }}>
    <Header headerText="About Gatsby" /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
)
```

![Przekazywanie danych do nagłówka](07-pass-data-header.png)

Powinieneś teraz znowu widzieć tekst “About Gatsby” w nagłówku!

### Czym są “props”?

Wcześniej określiliśmy komponenty Reactowe jako elementy wielokrotnego użytku, służące do opisywania interfejsu. Aby sprawić, by były one dynamiczne, musisz być w stanie przekazywać im różne dane. Można to zrobić przy pomocy danych wejściowych zwanych “props". Props są (jak wskazuje nazwa) właściwościami dostarczanymi do komponentów Reactowych.

W `about.js` przekazałeś prop o nazwie `headerText` i wartości `"About Gatsby"` do zaimportowanego sub-komponentu `Header`:

```jsx:title=src/pages/about.js
<Header headerText="About Gatsby" />
```

W pliku `header.js`, komponent nagłówka oczekuje przekazania prop `headerText` (ponieważ, w taki sposób go napisałeś). Dlatego, możesz go użyć w następujący sposób:

```jsx:title=src/components/header.js
<h1>{props.headerText}</h1>
```

> 💡 W JSX możesz osadzić dowolne javascriptowe wyrażenie, zawierając je w `{}`. W ten właśnie sposób korzystasz z właściwości `headerText` (lub “prop!”) z obiektu “props”.

Jeśli przekazałbyś kolejny prop komponentowi `<Header />`, w taki sposób...

```jsx:title=src/pages/about.js
<Header headerText="About Gatsby" arbitraryPhrase="is arbitrary" />
```

...miałbyś także dostęp do prop o nazwie `arbitraryPhrase`: `{props.arbitraryPhrase}`.

6.  Aby podkreślić to, że Twoich komponentów można używać wielokrotnie, dodaj kolejny komponent `<Header />` do strony o mnie, wklej poniższy kod do pliku `src/pages/about.js` i zapisz.

```jsx:title=src/pages/about.js
import React from "react"
import Header from "../components/header"

export default () => (
  <div style={{ color: `teal` }}>
    <Header headerText="About Gatsby" />
    <Header headerText="It's pretty cool" /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
)
```

![Zduplikowany nagłówek, dla pokazania możliwośći ponownego użycia](08-duplicate-header.png)

I oto jest; Drugi nagłówek — bez przepisywania kodu — a jedynie przez przekazanie różnych danych korzystając z props.

### Korzystanie z komponentów układu strony

Komponenty układu są przeznaczone do sekcji witryny, które chcesz powielać na wielu stronach. Na przykład strony Gatsby często posiadają komponent układu strony ze wspólnym nagłówkiem i stopką. Inne rzeczy, które często tam się znajdują, zawierają pasek boczny i/lub menu nawigacyjne.

Odkryjemy komponentu układu strony w [**części trzeciej**](/tutorial/part-three/).

## Łącza między stronami

Często będziesz chciał łączyć się między stronami - Przyjrzyjmy się routingowi na stronie Gatsby.

### ✋ Korzystanie z komponentu `<Link />`

1.  Otwórz komponent strony index (`src/pages/index.js`), zaimportuj komponent `<Link />` z Gatsby, dodaj komponent `<Link />` nad nagłówkiem, i przekaż mu właściwość `to` o wartości `"/contact/"` dla ścieżki:

```jsx:title=src/pages/index.js
import React from "react"
import { Link } from "gatsby" // highlight-line
import Header from "../components/header"

export default () => (
  <div style={{ color: `purple` }}>
    <Link to="/contact/">Contact</Link> {/* highlight-line */}
    <Header headerText="Hello Gatsby!" />
    <p>What a world.</p>
    <img src="https://source.unsplash.com/random/400x200" alt="" />
  </div>
)
```

Po kliknięciu nowego linku "Contact", na stronie głównej, powinieneś zobaczyć...

![Deweloperska strona 404 Gatsby](09-dev-404.png)

...deweloperską stronę 404 Gatsby. Dlaczego? Ponieważ próbujesz łączyć do strony, która jeszcze nie istnieje.

2.  Musisz teraz stworzyć komponent strony dla strony "Kontakt" w `src/pages/contact.js` i sprawić, aby łączył on z powrotem do strony głównej:

```jsx:title=src/pages/contact.js
import React from "react"
import { Link } from "gatsby"
import Header from "../components/header"

export default () => (
  <div style={{ color: `teal` }}>
    <Link to="/">Home</Link>
    <Header headerText="Contact" />
    <p>Send us a message!</p>
  </div>
)
```

Po zapisaniu pliku, powinieneś zobaczyć stronę kontaktową i być w stanie łączyć się między nią a stroną główną.

<video controls="controls" loop="true">
<<<<<<< HEAD
  <source type="video/mp4" src="./10-linking-between-pages.mp4"></source>
  <p>Przepraszamy! Twoja przeglądarka nie mogła wyświetlić tego filmu.</p>
=======
  <source type="video/mp4" src="./10-linking-between-pages.mp4" />
  <p>Sorry! Your browser doesn't support this video.</p>
>>>>>>> 8ff6bb09c23261662f47e79a041a92855d517097
</video>

Komponent `<Link />` Gatsby służy do łączenia pomiędzy stronami na Twojej witrynie. Dla linków zewnętrznych, do stron nie powiązanych z Twoją witryną Gatsby, użyj zwykłego znacznika HTML, `<a>`.

## Wdrażanie strony Gatsby

Gatsby.js jest _nowoczesnym generatorem stron_, co oznacza tyle, że nie potrzebujesz konfigurować żadnych serwerów ani skomplikowanych baz danych, aby go wdrożyć. Zamiast tego, komenda Gatsby `build` tworzy folder ze statycznymi plikami HTML i JavaScript, które możesz umieścić na hostingu statycznych stron.

Wypróbuj [Surge](http://surge.sh/) aby wdrożyć Twoją pierwszą stronę Gatsby. Surge jest jednym z wielu "hostingów stron statycznych", które umożliwiają wdrażanie stron Gatsby.

Jeśli jeszcze nie zainstalowałeś i nie skonfigutowałeś Surge, otwórz nowe okno terminala i zainstaluj jego narzędzie wiersza poleceń:

```shell
npm install --global surge

# Następnie stwórz tam (darmowe) konto
surge login
```

Teraz zbuduj swoją stronę poprzez uruchomienie poniższej komendy w terminalu, w katalogu głównym Twojej strony (porada: upewnij się, że faktycznie uruchamiasz tę komendę w katalogu głównym swojej strony, w tym przypadku jest to folder hello-world, możesz to zrobić otwierając nową kartę w tym samym oknie, w którym uruchamiałeś `gatsby develop`):

```shell
gatsby build
```

Budowanie powinno zająć 15-30 sekund. Kiedy się zakończy możesz rzucić okiem na pliki, które zostały przygotowane do wdrożenia przez komendę `gatsby build`, jeśli Cię to interesuje.

Spójrz na listę wygenerowanych plików poprzez wprowadzenie poniższej komendy w terminalu, w katalogu głównym Twojej strony, co pozwoli Ci zajrzeć do folderu `public`:

```shell
ls public
```

Teraz możesz wreszcie wdrożyć swoją stronę poprzez publikację wygenerowanych plików na surge.sh.

```shell
surge public/
```

> Pamietaj aby nacisnąć przycisk `enter` po zobaczeniu informacji `domain: some-name.surge.sh` w Twoim wierszu wierszu poleceń.

Kiedy to się zakończy, powinieneś w swoim terminalu zobaczyć coś takiego:

![Zrzut ekranu z publikowania strony Gatsby przy pomocy Surge](surge-deployment.png)

Otwórz adres internetowy pokazany na dole (w tym przypadku `lowly-pain.surge.sh`) i powinieneś ujrzeć swoją nowo opublikowaną stronę! Świetna robota!

## ➡️ Co dalej?

W tej części:

- Poznałeś startery Gatsby, oraz to jak z nich korzystać podczas tworzenia nowych projektów
- Poznałeś składnię JSX
- Poznałeś komponenty
- Poznałeś komponenty strony oraz sub-komponenty Gatsby
- Poznałeś Reactowe “props” oraz jak używać wielokrotnie komponentów Reactowych

Przejdź teraz do [**stylowania swojej strony**](/tutorial/part-two/)!
