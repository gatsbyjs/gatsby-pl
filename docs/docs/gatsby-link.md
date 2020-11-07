---
title: API Gatsby Link
---

Do nawigacji wewnątrz strony, Gatsby dostarcza wbudowany komponent `<Link>` oraz funkcję `navigate` służącą do nawigacji programowej.

Komponent `<Link>` umożliwia linkowanie do stron wewnętrznych oraz korzystanie z ładowania wstępnego (_preloading_). Ładowanie wstępne wykorzystywane jest do ładowania zasobów strony, zanim użytkownik na nią przejdzie, co zwiększa wydajność i ogólną responsywność strony. W tym celu używany jest `IntersectionObserver`, który wstępnie wykonuje zapytanie z niskim priorytetem, gdy `Link` znajdzie się w polu widzenia użytkownika, a następnie wykonuje zapytanie o wyższym priorytecie, gdy uruchomiony zostanie event `onMouseOver` elementu `Link` - w tym momencie użytkownik najprawdopodobniej będzie chciał przejść na stronę, na której pobierane zasoby będą się znajdować.

Komponent `Link` zbudowany jest na bazie [komponentu link z @reach/router](https://reach.tech/router/api/Link), dodając do niego użyteczne usprawnienia specyficzne dla Gatsby. Wszystkie propsy przekazywane są do komponentu `Link` z @reach/router.

## Jak używać Gatsby Link?

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-why-and-how-to-use-gatsby-s-link-component"
  lessonTitle="Why and How to Use Gatsby’s Link Component"
/>

### Zamień tagi `a` na `Link` do lokalnych przekierowań

W jakiejkolwiek sytuacji, kiedy chcesz stworzyć przekierowanie wewnątrz tej samej strony, korzystaj z komponentu `Link` zamiast tagu `a`.

```jsx
import React from "react"
// highlight-next-line
import { Link } from "gatsby"

const Page = () => (
  <div>
    <p>
      {/* highlight-next-line */}
      Check out my <Link to="/blog">blog</Link>!
    </p>
    <p>
      {/* Zauważ, że zewnętrzne linki nadal korzystają z tagu `a`. */}
      Follow me on <a href="https://twitter.com/gatsbyjs">Twitter</a>!
    </p>
  </div>
)
```

### Dodawanie własnych styli do aktualnie aktywnego linku

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-add-custom-styles-for-the-active-link-using-gatsby-s-link-component"
  lessonTitle="Add Custom Styles for the Active Link Using Gatsby’s Link Component"
/>

Zazwyczaj dobrym pomysłem jest wyróżnianie, która strona jest aktualnie wyświetlana, przez zmianę wyglądu linku odnoszącego się do tej strony.

`Link` zapewnia dwa sposoby na dodawanie styli do aktywnego odnośnika:

- `activeStyle` — obiekt styli aplikowany tylko wtedy, gdy link jest aktywny
- `activeClassName` — klasa dodawana do komponentu `Link`, gdy jest on aktywny

Na przykład, aby zmienić kolor aktywnego linku na czerwony, oba podejścia są poprawne

```jsx
import React from "react"
import { Link } from "gatsby"

const SiteNavigation = () => (
  <nav>
    <Link
      to="/"
      {/* highlight-start */}
      {/* Zakładamy że klasa `active` jest obecna w stylach CSS */}
      activeClassName="active"
      {/* highlight-end */}
    >
      Home
    </Link>
    <Link
      to="/about/"
      {/* highlight-next-line */}
      activeStyle={{ color: "red" }}
    >
      About
    </Link>
  </nav>
)
```

### Wykorzystywanie `getProps` do zaawansowanego stylowania linków

Komponent Gatsby `<Link>` dostarcza props `getProps`, która może być użyteczna przy zaawansowanym stylowaniu. Dostarcza obiekt, z następującymi propsami:

- `isCurrent` — prawda, jeżeli `location.pathname` jest dokładnie taka sama jak props `to` komponentu `Link`
- `isPartiallyCurrent` — prawda, jeżeli `location.pathname` zaczyna się tak samo jak props `to` komponentu `Link`
- `href` — wartość props `to`
- `location` — aktualny obiekt `location`

Więcej na ten temat w [dokumentacji `@reach/router`](https://reach.tech/router/api/Link).

### Style aktywnego linku dla ścieżek częściowo pasujących lub linków do rodzica

Domyślnie propsy `activeStyles` i `activeClassName` będą aplikowane do komponentu `Link` tylko jeżeli obecna ścieżka jest _dokładnie_ taka sama jak ścieżka podana we propsie `to`. Czasami jednak przydatne mogłoby być stylowanie linku tak jakby był aktywny, ale gdy props `to` tylko częściowo pasuje do obecnego URL'u. Na przykład:

- Można by chcieć aby `/blog/hello-world` pasował do `<Link to="/blog">`
- Lub `/gatsby-link/#passing-state-through-link-and-navigate` pasował do `<Link to="/gatsby-link">`

W takich sytuacjach wystarczy dodać props `partiallyActive` do komponentu `<Link>`, a style zostaną zaaplikowane nawet wtedy, gdy ścieżka pasuje tylko częściowo.

```jsx
import React from "react"
import { Link } from "gatsby"

const Header = <>
  <Link
    to="/articles/"
    activeStyle={{ color: "red" }}
    {/* highlight-next-line */}
    partiallyActive={true}
  >
    Articles
  </Link>
</>;
```

_**Uwaga:** Ta funkcjonalność jest dostępna od Gatsby w wersji 2.1.31, jeżeli obserwujesz jakieś błędy z tym związane, sprawdź twoją wersję Gatsby i/lub go zaaktualizuj_

### Przekazywanie stanu jako propsa do linkowanych stron

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-include-information-about-state-in-navigation-with-gatsby-s-link-component"
  lessonTitle="Include Information About State in Navigation With Gatsby’s Link Component"
/>

Może pojawić się potrzeba przekazywania danych ze strony, na której znajduje się link, a strony do której on przekierowuje. Można zrobić to, przez przekazanie propsa `state` do komponentu `Link` lub przez wywołanie funkcji `navigate`. Podlinkowana strona otrzyma props `location`, która będzie zawierała w sobie obiekt `state` zawierający przekazane dane

```jsx
const PhotoFeedItem = ({ id }) => (
  <div>
    {/* (dla zwięzłości pominięto element feed) */}
    <Link
      to={`/photos/${id}`}
      {/* highlight-next-line */}
      state={{ fromFeed: true }}
    >
      View Photo
    </Link>
  </div>
)

// highlight-start
const Photo = ({ location, photoId }) => {
  if (location.state.fromFeed) {
    // highlight-end
    return <FromFeedPhoto id={photoId} />
  } else {
    return <Photo id={photoId} />
  }
}
```

### Podmiana historii, aby zmieniać zachowanie przycisku "wstecz"

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-replace-navigation-history-items-with-gatsby-s-link-component"
  lessonTitle="Replace Navigation History Items with Gatsby’s Link Component"
/>

Jest kilka sytuacji gdzie rzeczywiście sens miałoby zmienianie domyślnego zachowania przycisku "wstecz" - na przykład jeżeli tworzymy stronę, gdzie użytkownik coś wybiera, a następnie zostaje przekierowany do podstrony "Czy jesteś pewien?", z której przechodzi do strony z podsumowaniem, to znajdując się na niej, chcąc jednak coś zmienić, logicznym wydawałoby się aby po kliknięciu przycisku "wstecz" strona z potwierdzeniem została pominięta.

W takiej sytuacji używamy propsa `replace`, aby na przekierowaniu obecny URL w historii został zamieniony na docelowy danego linku.

```jsx
import React from "react"
import { Link } from "gatsby"

const AreYouSureLink = () => (
  <Link
    to="/confirmation/"
    {/* highlight-next-line */}
    replace
  >
    Yes, I’m sure
  </Link>
)
```

## Jak używać funkcji pomocniczej `navigate`?

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-navigate-to-a-new-page-programmatically-in-gatsby"
  lessonTitle="Navigate to a New Page Programmatically in Gatsby"
/>

Czasami powstaje potrzeba przekierowania użytkownika programowo - na przykład przy podsumowywaniu formularza. W takiej sytuacji komponent `Link` nie zadziała.

_**Uwaga:** funkcja `navigate` wcześniej nazywała się `navigateTo`. `navigateTo` jest zdeprecjonowane w Gatsby v2 i zostanie usunięte w najbliższym głównym wydaniu_

Gatsby eksportuje funkcję pomocniczą `navigate` która akceptuje argumenty `to` oraz `options`.

| Argument          | Wymagany | Opis                                                                                                                   |
| ----------------- | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| `to`              | tak      | Strona do której ma zostać wykonane przekierowanie (np. `/blog/`)                                                      |
| `options.state`   | nie      | Obiekt. Wartości przekazane w tym miejscu, dostępne będą wewnątrz `location.state` w propsach strony docelowej. |
| `options.replace` | nie      | Wartość logiczna. Jeżeli `true`, to podmienia obecny URL w historii przeglądarki.                                      |

Domyślnie, `navigate` operuje w taki sam sposób jak kliknięty komponent `Link`

```jsx
import React from "react"
import { navigate } from "gatsby" // highlight-line

const Form = () => (
  <form
    onSubmit={event => {
      event.preventDefault()

      // TODO: operacje na wartościach z formularza
      // highlight-next-line
      navigate("/form-submitted/")
    }}
  >
    {/* (pominięto inputy dla zwięzłości) */}
  </form>
)
```

### Dodawanie stanu do nawigacji programowej

Aby zawrzeć informacje o stanie, wystarczy dodać obiekt `options` z wartością `state` zawierającą wszystkie dane które potrzebujemy.

```jsx
import React from "react"
import { navigate } from "gatsby"

const Form = () => (
  <form
    onSubmit={event => {
      event.preventDefault()

      // Implementacja tej funkcji to zadanie dla czytelnika ;)
      const formValues = getFormValues()

      navigate(
        "/form-submitted/",
        // highlight-start
        {
          state: { formValues },
        }
        // highlight-end
      )
    }}
  >
    {/* (pominięto inputy dla zwięzłości) */}
  </form>
)
```

Następnie aby na stronie docelowej można uzyskać stan `location` tak, jak zaprezentowano w [Przekazywanie stanu jako propsa do linkowanych stron](#przekazywanie-stanu-jako-propsa-do-linkowanych-stron).

### Podmiana historii podczas nawigacji programowej

Jeżeli nawigacja ma podmienić aktualny wpis w historii, zamiast dodawania nowego, dodaj props `replace` z wartością `true` do argumentu `options` funkcji navigate `navigate`.

```jsx
import React from "react"
import { navigate } from "gatsby"

const Form = () => (
  <form
    onSubmit={event => {
      event.preventDefault()

      // TODO: operacje na wartościach z formularza
      navigate(
        "/form-submitted/",
        // highlight-next-line
        { replace: true }
      )
    }}
  >
    {/* (pominięto inputy dla zwięzłości) */}
  </form>
)
```

## Dodawanie prefiksu do ścieżki, korzystając z `withPrefix`

Popularną praktyką jest hostowanie stron w pod-ścieżkach strony. Gatsby pozwala [ustawić prefiks ścieżki dla twojej strony](/docs/path-prefix/). Po skonfigurowaniu prefiksu ścieżki, komponent `Link` będzie automatycznie konstruował odpowiedni URL w fazie developmentu, jak i na produkcji.

Dla nazw ścieżek konstruowanych ręcznie, istnieje funkcja pomocnicza - `withPrefix`, która dodaje prefiks do ścieżki na produkcji (ale nie w fazie developmentu, gdzie nie istnieje taka potrzeba).

```jsx
import { withPrefix } from "gatsby"

const IndexLayout = ({ children, location }) => {
  const isHomepage = location.pathname === withPrefix("/")

  return (
    <div>
      <h1>Welcome {isHomepage ? "home" : "aboard"}!</h1>
      {children}
    </div>
  )
}
```

## Przypomnienie: używaj komponentu `Link` tylko do przekierowań wewnątrz strony!

Komponent ten jest stworzony z myślą _tylko_ o linkach do stron stworzonych przez Gatsby. Do przekierowań na strony w innych domenach, czy w tej samej domenie ale nie należących do obecnej strony Gatsby, należy używać tradycyjnego elementu `a`.

Czasami nie jesteśmy w stanie przewidzieć, czy przekierowanie będzie wewnętrzne lub zewnętrzne, na przykład wtedy gdy dane przychodzą z CMS'a. W takiej sytacji, przydatne może być utworzenie komponentu, który sprawdza link, a następnie renderuje komponent Gatsby `Link` lub element `a`, w zależności od tego czy jest on wewnętrzny czy zewnętrzny.

Ze względu na fakt, że określenie czy link jest wewnętrzny lub zewnętrzny zależy od strony, którą budujemy, może pojawić się potrzeba zmodyfikowania poniższego przykładu do własnych potrzeb. Mimo to może on stanowić dobry punkt wyjścia do budowania takiego mechanizmu:

```jsx
import { Link as GatsbyLink } from "gatsby"

// Elementy DOM (w tym `a`) nie przyjmują takich propsów jak
// activeClassName, partiallyActive, to, itp.
// więc trzeba je zdestrukturyzować, oraz podać bezpośrednio do komponentu `Link`
const Link = ({ children, to, activeClassName, partiallyActive, ...other }) => {
  // Dostosuj poniższy test do twojego środowiska.
  // Poniższy przykład zakłada, że linki wewnętrzne (przeznaczone dla Gatsby)
  // będą zaczynać się dokładnie jednym slashem,
  // a wszystkie inne zostaną potraktowane jako zewnętrzne
  const internal = /^\/(?!\/)/.test(to)

  // Gatsby `Link` do wewnętrznych, `a` do zewnętrznych
  if (internal) {
    return (
      <GatsbyLink
        to={to}
        activeClassName={activeClassName}
        partiallyActive={partiallyActive}
        {...other}
      >
        {children}
      </GatsbyLink>
    )
  }
  return (
    <a href={to} {...other}>
      {children}
    </a>
  )
}

export default Link
```

### Pobieranie plików

Podobny mechanizm można zaimplementować do pobierania plików:

```jsx
  const file = /\.[0-9a-z]+$/i.test(to)

  ...

  if (internal) {
    if (file) {
        return (
          <a href={to} {...other}>
            {children}
          </a>
      )
    }
    return (
      <GatsbyLink to={to} {...other}>
        {children}
      </GatsbyLink>
    )
  }
```

## Zalecenia do programowej nawigacji, wewnątrz tej samej strony

`<Link>` i `navigate` nie mogą być wykorzystywane do nawigacji wewnątrz tej samej ścieżki, z hashem lub parametrem query. Jeżeli potrzebujesz takiego zachowania, należy wykorzystać tag `a` lub zaimportować funkcję `navigate` z paczki `@reach/router`, którą wewnątrz wykorzystuje Gatsby.

```jsx
import { navigate } from '@reach/router';

...

onClick = () => {
  navigate('#some-link');
  // lub
  navigate('?foo=bar');
}
```

## Obsługa nieaktualnych stron po stronie klienta

Komponent `Link` będzie pobierał zasoby każdej strony tylko raz. Aktualizacje do stron nie są bezpośrednio odzwierciedlane w przeglądarce, ponieważ są w pewien sposób "zamrożone w czasie". Mimo że będzie to zmiejszać czas ładowania strony, to może mieć to wpływ na niechcianą sytuację, gdy inni użytkownicy będą widzieć na stronie różną treść.

Aby temu zapobiec, Gatsby wykonuje zapytanie o dodatkowy zasób na każdym nowym załadowaniu strony - `app-data.json`. Zawiera on wygenerowany podczas budowania strony hash; jeżeli cokolwiek zmieni się wewnątrz lokalizacji `src`, hash się zmieni. Gdy podczas ładowania, Gatsby zauważy że hash się zmienił od czasu gdy pobrał go po raz pierwszy, przeglądarka w celu nawigacji użyje `window.location`. W ten sposób pobierze nową stronę od zera, a jakiekolwiek zasoby znajdujące się w pamięci podręcznej zostaną utracone.

Jeżeli natomiast strona została już wcześniej załadowana, `app-data.json` nie zostanie pobrany. W tej sytuacji nie dojdzie do porównywania hash'ów, a użyta zostanie wcześniej pobrana treść.

> **Uwaga:** Jakikolwiek stan zostanie utracony podczas przekierowania `window.location`. Może mieć to wpływ, jeżeli opieramy się na zarządzaniu stanem, np. śledzenie stanu wewnątrz [wrapPageElement](/docs/browser-apis/#wrapPageElement) czy przez biblioteke taką jak Redux

## Dodatkowe źródła

- [Authentication tutorial for client-only routes](/tutorial/authentication-tutorial/)
- [Routing: Getting Location Data from Props](/docs/location-data-from-props/)
- [`gatsby-plugin-catch-links`](https://www.gatsbyjs.org/packages/gatsby-plugin-catch-links/) aby automatycznie przechwycić linki wewnętrzne z plików markdown czy CMS'ów, dla zachowania podobnego do `gatsby-link`
