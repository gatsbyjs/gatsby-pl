---
title: API Gatsby Link
---

Opcje konfiguracji dla strony w Gatsby umieszczone są wewnątrz pliku `gatsby-config.js` umiejscowionego w głównym folderze projektu.

_Uwaga: Wewnątrz [Gatsby Example Websites](https://github.com/gatsbyjs/gatsby/tree/master/examples) znajduje się wiele pomocnych przykładowych konfiguracji, do krotóych dla porównania można się odnieść._

## Opcje konfiguracji

Wewnątrz `gatsby-config.js` można ustawić poniższe opcje:

1.  [siteMetadata](#sitemetadata) (obiekt)
2.  [plugins](#plugins) (array)
3.  [pathPrefix](#pathprefix) (string)
4.  [polyfill](#polyfill) (boolean)
5.  [mapping](#mapping-node-types) (obiekt)
6.  [proxy](#proxy) (obiekt)
7.  [developMiddleware](#advanced-proxying-with-developmiddleware) (funkcja)

## siteMetadata

Chcąc używać pewnych danych w obrębie całej strony (na przykład jej tytuł), dane te można przechowywać wewnątrz `siteMetadata`:

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Gatsby`,
    siteUrl: `https://www.gatsbyjs.org`,
    description: `Blazing fast modern site generator for React`,
  },
}
```

Dzięki temu można je przechowywać w jednym miejscu, a następnie pobierać gdziekolwiek chcemy. Jeżeli kiedykolwiek pojawiła by się potrzeba zmiany tych danych, wystarczy zmienić je tylko w tym miejscu.

Pełen opis i przykładowe użycie w [czwartej części poradnika Gatsby.js](/tutorial/part-four/#data-in-gatsby).

## Pluginy

Pluginy to paczki Node.js, które implementują API Gatsby. Plik konfiguracyjny przyjmuje array z listą pluginów. Niektóre pluginy mogą wymagać tylko ich nazwy, inne z kolei mogą przyjmować różne ustawienia (w celu ich znalezienia, należy odwołać się do dokumentacji poszczególnych pluginów).

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    `gatsby-plugin-react-helmet`,
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `docs`,
        path: `${__dirname}/../docs/`,
      },
    },
  ],
}
```

Sprawdź [Plugins](/docs/plugins/) po więcej informacji na temat ich używania oraz by znależć pluginy oficjalne, jak i te od społeczności.

## pathPrefix

Dość często hostuje się strony w innym miejscu, niż na roocie ich domeny. Powiedzmy że strona w Gatsby znajduje się pod adresem `example.com/blog/`. W tym celu należy dodać prefiks (`/blog`) do wszystkich ścieżek na stronie.

```javascript:title=gatsby-config.js
module.exports = {
  pathPrefix: `/blog`,
}
```

Dowiedz się więcej na temat [dodawania prefiksu ścieżki](/docs/path-prefix/).

## Polyfill

Gatsby korzysta z ES6 Promise API. Ponieważ niektóre przeglądarki go nie wspierają, Gatsby domyślnie dołącza polyfill.

Jeśli chcesz dodać swój własny polyfill do Promisów, możesz ustawić właściwość `polyfill` na `false`.

```javascript:title=gatsby-config.js
module.exports = {
  polyfill: false,
}
```

Dowiedz się więcej na temat [wsparcia przeglądarek](/docs/browser-support/#polyfills) w Gatsby.

## Mapowanie typów nodeów

Gatsby zawiera zaawansowany mechanizm pozwalający tworzyć "mapowania" pomiędzy poszczególnymi typami node'ów GraphQL'a.

> Uwaga: Gatsby v2.2 wprowadził nowy sposób tworzenia relacji typu foreign-key z [rozszerzeniem `@link` pola GraphQL](/docs/schema-customization/#foreign-key-fields).

Na przykład, przyjmijmy że tworzymy bloga opartego o pliki markdown, który posiada wielu autorów, w którym chcielibyśmy "podłączyć" każdy wpis do odpowiedniego autora, którego dane przechowywane są w pliku `author.yaml`:

```markdown
---
title: A blog post
author: Kyle Mathews
---

A treatise on the efficacy of bezoar for treating agricultural pesticide poisoning.
```

```yaml:title=author.yaml
- id: Kyle Mathews
  bio: Founder @ GatsbyJS. Likes tech, reading/writing, founding things. Blogs at bricolage.io.
  twitter: "@kylemathews"
```

Można stworzyć połączenie pomiędzy polem `author` wewnątrz `frontmatter` do `id` obiektu autora wewnątrz pliku `author.yaml`, przez dodanie poniższego kodu do `gatsby-config.js`:

```javascript
module.exports = {
  plugins: [...],
  mapping: {
    "MarkdownRemark.frontmatter.author": `AuthorYaml`,
  },
}
```

Może pojawić się potrzeba zainstalowania odpowiedniego transformer'a pliku (w tym przypadku [YAML](/packages/gatsby-transformer-yaml/)) oraz ustawienia [gatsby-source-filesystem](/packages/gatsby-source-filesystem/) tak, aby Gatsby mógł wykorzystywać pliki do mapowania. Dotyczy to również innych rodzajów plików, o których wspomnimy później w tym segmencie.

Gatsby później korzysta z tego mapowania podczas tworzenia schematu GraphQL, by umożliwić pozyskiwanie danych z obu źródeł.

```graphql
query($slug: String!) {
  markdownRemark(fields: { slug: { eq: $slug } }) {
    html
    fields {
      slug
    }
    frontmatter {
      title
      author {
        # Odnosi się to do obiektu autora!
        id
        bio
        twitter
      }
    }
  }
}
```

Mapowanie może być również wykorzystane, by przypisać array identyfikatorów do dowolnego innego zestawu danych. Na przykład, jeżeli masz 2 pliki JSON `experience.json` oraz `tech.json`:

```json:title=experience.json
[
  {
    "id": "companyA",
    "company": "Company A",
    "position": "Unicorn Developer",
    "from": "Dec 2016",
    "to": "Present",
    "items": [
      {
        "label": "Responsibility",
        "description": "Being an unicorn"
      },
      {
        "label": "Hands on",
        "tech": ["REACT", "NODE"]
      }
    ]
  }
]
```

```json:title=tech.json
[
  {
    "id": "REACT",
    "icon": "facebook",
    "color": "teal",
    "label": "React"
  },
  {
    "id": "NODE",
    "icon": "server",
    "color": "green",
    "label": "NodeJS"
  }
]
```

A następnie dodasz następującą regułę do `gatsby-config.js`:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [...],
  mapping: {
    'ExperienceJson.items.tech': `TechJson`
  },
}
```

To możesz pobierać obiekt `tech` przez przypisany mu `id` wewnątrz pliku `experience`:

```graphql
query {
  experience: allExperienceJson {
    edges {
      node {
        company
        position
        from
        to
        items {
          label
          description
          link
          tech {
            label
            color
            icon
          }
        }
      }
    }
  }
}
```

Mapowanie działa również pomiędzy plikami Markdown. Na przykład, zamiast posiadać listy wszystkich autorów wewnątrz pliku YAML, można umieścić dane każdego z nich w poszczególnych plikach markdown:

```markdown
---
author_id: Kyle Mathews
twitter: "@kylemathews"
---

Founder @ GatsbyJS. Likes tech, reading/writing, founding things. Blogs at bricolage.io.
```

A następnie dodać następującą regułę do pliku `gatsby-config.js`:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [...],
  mapping: {
    'MarkdownRemark.frontmatter.author': `MarkdownRemark.frontmatter.author_id`
  },
}
```

Tak jak w plikach YAML i JSON, mapowanie w plikach markdown można również przeprowadzać celem mapowania zestawu identyfikatowów.

## Proxy

Ustawienie opcji `proxy`, spowoduje że serwer do developmentu będzie przekierowywał jakiekolwiek nieznane zapytania na podany serwer. Na przykład:

```javascript:title=gatsby-config.js
module.exports = {
  proxy: {
    prefix: "/api",
    url: "http://examplesite.com/api/",
  },
}
```

Dowiedz się więcej na temat [Proxying API Requests in Develop](/docs/api-proxy/).

## Zaawansowany proxying z `developMiddleware`

Dowiedz się więcej na temat [dodawania develop middleware](/docs/api-proxy/#advanced-proxying).
