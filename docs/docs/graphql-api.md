---
title: GraphQL API
tableOfContentsDepth: 2
---

Ogromną przewagą Gatsby jest wbudowana warstwa danych która jest w stanie połączyć wiele źródeł danych. Dane zbierane są w [trakcie budowania](/docs/glossary#build) i zostają automatycznie zgromadzone w [schemat](/docs/glossary#schema) który definiuje w jaki sposób mogą one być pobierane przez twoją stronę.

Jest to dokumentacja funkcjonalności GraphQL wbudowanych w Gatsby, włącznie z metodami do zapytań i pozyskiwania danych, oraz dostosowywania GraphQL do swoich potrzeb.

## Pierwsze kroki z GraphQL

GraphQL jest dostępny w Gatsby bez potrzeby dodatkowej instalacji: schemat jest automatycznie budowany kiedy uruchamiasz projekt (`gatsby develop` lub `gatsby build`). Kiedy strona zostanie zbudowana, warstwę danych możesz [ręcznie przeglądać](/docs/running-queries-with-graphiql/) pod adresem: <http://localhost:8000/___graphql>

## Pozyskiwanie danych

Dane muszą zostać [dodane](/docs/content-and-data/) do schematu GraphQL, aby je odpytywać i wykorzystywać na stronie za pomocą GraphQL. Do pobierania danych, Gatsby wykorzystuje [wtyczki źródłowe](/plugins/?=gatsby-source).

**Uwaga**: GraphQL nie jest wymagany: z Gatsby można [korzystać bez GraphQL](/docs/using-gatsby-without-graphql/).

Aby pozyskać dane poprzez istniejącą wtyczkę, musisz zainstalować wszystkie potrzebne paczki i dodać wtyczkę (oraz opcjonalnie jej konfigurację) do tablicy wtyczek w `gatsby-config`. Dane takie jak zdjęcia, pliki Markdown, itp. można pozyskiwać z systemu plików. Po więcej informacji, sprawdź [dokumentację pozyskiwania danych z systemu plików](/docs/sourcing-from-the-filesystem/) oraz [przepisy](/docs/recipes/sourcing-data).

Aby uzyskać instrukcje dotyczące instalacji paczek z npm, zajrzyj do instrukcji w dokumentacji [jak korzystać z wtyczki](/docs/using-a-plugin-in-your-site/).

Możesz również [tworzyć niestandardowe wtyczki](/docs/creating-plugins/) dopasowane do twoich potrzeb, które mogą pobierać dane w dowolny sposób.

## Komponenty i hooki służące do wykonywania zapytań

Dane mogą być odpytywane w stronach, komponentach, oraz w pliku `gatsby-node.js`, wykorzystując jedną z poniższych możliwości:

- Komponent `pageQuery`
- Komponent `StaticQuery`
- Hook `useStaticQuery`

**Uwaga**: Ze względu na to jak Gatsby przetwarza zapytania GraphQL, nie możesz mieszać zapytań stron i zapytań statycznych wewnątrz pliku. Nie możesz również umieszczać wielu zapytań stron wewnątrz jednego pliku.

Po więcej informacji na wykorzystywania przez Gatsby komponentów stron i nie tylko, sprawdź sekcję [budowanie z komponentami](docs/building-with-components/#how-does-gatsby-use-react-components).

### `pageQuery`

`pageQuery` jest wbudowanym komponentem który pobiera informacje z warstwy danych w stronach Gatsby. Na jednej stronie możesz mieć jedno zapytanie. Może ono przyjmować argumenty dla zmiennych w zapytaniach.

[Strona jest utworzona w Gatsby](/docs/page-creation/) z dowolnego komponentu React znajdującego się w folderze `src/pages` lub poprzez wywołanie akcji `createPage` oraz użycie komponentu w opcjach `createPage` - znaczy to, że `pageQuery` nie zadziała w dowolnym komponencie, ale tylko w tych które spełniają te kryteria.

Zapoznaj się również z [przewodnikiem dotyczącym pobierania danych w stronach poprzez zapytania stron](/docs/page-query/)

#### Parametry

Page query nie jest metodą, ale raczej ekportowaną zmienną która ma przypisany string `graphql` oraz poprawny blok zapytania jako jego wartość:

```javascript
export const pageQuery = graphql`
  query HomePageQuery {
    site {
      siteMetadata {
        description
      }
    }
  }
`
```

**Uwaga**: Zapytanie eksportowane jako `const` wcale nie musi być nazwane `pageQuery`. Ważniejszym aspektem jest to, że Gatsby szuka w pliku wyeksportowanego string'a `graphql`.

#### Zwrócone wartości

Kiedy zapytanie jest użyte w pliku komponentu strony, zapytanie strony zwraca obiekt `data` zostaje przekazany do komponentu jako prop.

```jsx
// highlight-start
const HomePage = ({ data }) => {
  // highlight-end
  return (
    <div>
      Hello!
      {data.site.siteMetadata.description} // highlight-line
    </div>
  )
}
```

### `StaticQuery`

StaticQuery jest wbudowanym komponentem który pobiera dane z warstwy danych Gatsby dla komponentów które nie są stronami, jak np. nagłówek, nawigacja czy dowolny inny komponent użyty jako dziecko.

Możesz użyć tylko jedno `StaticQuery` na stronę: aby uwzględnić potrzebne dane z wielu źródeł, możesz użyć jednego zapytania z wieloma [głównymi polami](/docs/graphql-concepts/#query-fields). Komponent ten nie może przyjmować zmiennych jako argumenty.

Zapoznaj się również z [przewodnikiem dotyczącym odpytywania danych w komponentach wykorzystując zapytanie statyczne](/docs/static-query/).

#### Parametry

Komponent `StaticQuery` przyjmuje dwie wartości jako atrybuty w JSX:

- `query`: zapytanie `graphql` jako string
- `render`: komponent który ma dostęp do zwróconych danych

```jsx
<StaticQuery
  query={graphql` //highlight-line
    query HeadingQuery {
      site {
        siteMetadata {
          title
        }
      }
    }
  `}
  render={(
    data //highlight-line
  ) => (
    <header>
      <h1>{data.site.siteMetadata.title}</h1>
    </header>
  )}
/>
```

#### Zwrócone wartości

Komponent StaticQuery zwraca `data` w jako atrybut `render`:

```jsx
<StaticQuery
  // ...
  // highlight-start
  render={data => (
    <header>
      <h1>{data.site.siteMetadata.title}</h1>
    </header>
  )}
  // highlight-end
/>
```

### `useStaticQuery`

Hook `useStaticQuery` może być użyty w podobny sposób do `StaticQuery` w dowolnym komponencie lub stronie, ale nie wymaga użycia dodatkowego komponentu z render propem.

Ponieważ jest to React hook, [zasady korzystania z hooków](https://pl.reactjs.org/docs/hooks-rules.html) muszą zostać zachowane, więc będziesz potrzebować wersji React i ReactDOM 16.8.0 lub wyższej. Wobec tego to jak zapytania obecnie działają w Gatsby, tylko jedna instancja `useStaticQuery` jest wspierana dla danego pliku.

Zapoznaj się również z [poradnikiem dotyczącym odpytywania danych w komponentach z użyciem useStaticQuery](/docs/use-static-query/)

#### Parametry

Hook `useStaticQuery` przyjmuje jeden argument:

- `query`: zapytanie `graphql` jako string

```javascript
const data = useStaticQuery(graphql`
  query HeaderQuery {
    site {
      siteMetadata {
        title
      }
    }
  }
`)
```

#### Zwrócone wartości

Hook `useStaticQuery` zwraca `data` jako obiekt:

```jsx
const data = useStaticQuery(graphql`
  query HeaderQuery {
    site {
      siteMetadata {
        title
      }
    }
  }
`)
return (
  // highlight-start
  <header>
    <h1>{data.site.siteMetadata.title}</h1>
  </header>
  // highlight-end
)
```

## Struktora zapytania

Zapytania są pisane w takim samym kształcie w jakim mają być zwracane dane. Umieszczone dane określą nazwy pól do których możesz tworzyć zapytania, bazując na node'ach które dodają do schematu GraphQL.

Aby lepiej zrozumieć działanie części zapytania, zapoznaj się z [przewodnikiem koncepcyjnym](/docs/graphql-concepts/#understanding-the-parts-of-a-query)

### Argumenty w zapytaniach GraphQL

Zapytania GraphQL mogą przyjmować argumenty które będą definiować jakie dane będą zwracane. Logika dla tych argumentów jest wewnęterznie obsługiwana przez Gatsby. Argumenty mogą być przekazywane do pól na dowolnym szczeblu zapytania.

Różne node'y w zależności od ich budowy, przyjmować mogą różne argumenty

Argumenty które możesz przekazać do kolekcji (jak tablice lub długie listy danych - np. `allFile` czy `allMdx`) to:

- [`filter`](/docs/graphql-reference#filter)
- [`limit`](/docs/graphql-reference#limit)
- [`sort`](/docs/graphql-reference#sort)
- [`skip`](/docs/graphql-reference#skip)

Argumenty które możesz przekazać do pola `date` to:

- [`formatString`](/docs/graphql-reference#dates)
- [`locale`](/docs/graphql-reference#dates)

Argumenty które możesz przekazać do pola `excerpt` to:

- [`pruneLength`](/docs/graphql-reference#excerpt)
- [`truncate`](/docs/graphql-reference#excerpt)

### Operacje zapytań Graphql

Inne wbudowane konfiguracje mogą być użyte w zapytaniach

- [`Alias`](/docs/graphql-reference#alias)
- [`Group`](/docs/graphql-reference#group)

Jeśli chcesz zobaczyć przykłady, zapoznaj się z [recepturami zapytań](/docs/recipes/querying-data) i [przewodnikiem po opcjach zapytań GraphQL](/docs/graphql-reference/).

## Fragmenty zapytań

Fragmenty pozwalają Ci na ponowne używanie części zapytań GraphQL. Pozwalają one również na dzielenie skomplikowanych zapytań na mniejsze, łatwiejsze w zrozumieniu komponenty.

Aby dowiedziec się więcej, sprawdź przewodnik w dokumentacji na temat [używania fragmentów w Gatsby](/docs/using-graphql-fragments/)

### Fragmenty Gatsby

Niektóre fragmenty są już zawarte we wtyczkach Gatsby, jak np. fragmenty do zwracania zoptymalizowanych danych obrazu w różnych formatach z wykorzystaniem `gatsby-image` i `gatsby-transformer-sharp`, lub fragmentów danych - w przypadku `gatsby-source-contentful`. Po więcej informacji które wtyczki zawierają fragmenty, sprawdź [`gatsby-image` README](/packages/gatsby-image#fragments).

## Zaawansowane dostosowywanie

Możesz dostosować pobieranie danych w warstwie GraphQL i stworzyć relację pomiędzy węzłami z pomocą [Gatsby Node APIs](/docs/node-apis/).

Schemat GraphQL może być dostosowany dla bardziej zaawansowanych przypadków: przeczytaj więcej na temat [dostosowywania schematu w dokumentacji API](/docs/schema-customization/).
