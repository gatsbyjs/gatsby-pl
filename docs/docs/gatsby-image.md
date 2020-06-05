---
title: Gatsby Image API
---

Częścią tego, co sprawia, że strony Gatsby są tak szybkie, jest rekomendowane podejście do implementowania obrazów. `gatsby-image` jest komponentem React zaprojektowanym do bezproblemowej współpracy z [natywnym przetwarzaniem obrazu](https://image-processing.gatsbyjs.org/) Gatsby obsługiwanym przez GraphQL i [gatsby-plugin-sharp](/packages/gatsby-plugin-sharp/), tak aby łatwo i całkowicie zoptymalizować ładowanie obrazu na twojej witrynie.

> _Uwaga: gatsby-image **nie jest** zamiennikiem dla `<img />`. Jest zoptymalizowany pod kątem responsywnych obrazów o stałej szerokości / wysokości oraz obrazów rozciągających się na całą szerokość rodzica. Istnieją również inne sposoby [pracy z obrazami](/docs/images-and-files/) w Gatsby, które nie wymagają GraphQL._

Przykład: [https://using-gatsby-image.gatsbyjs.org/](https://using-gatsby-image.gatsbyjs.org/)

## Wstępne ustawienie Gatsby Image

Aby rozpocząć pracę z Gatsby Image, zainstaluj pakiet `gatsby-image` wraz z niezbędnymi pluginami` gatsby-transformer-sharp` i `gatsby-plugin-sharp`. Odwołaj się do pakietów w pliku `gatsby-config.js`. Możesz także podać dodatkowe opcje dla [`gatsby-plugin-sharp`](/packages/gatsby-plugin-sharp/) w pliku konfiguracyjnym.

Typowym sposobem na pozyskiwanie obrazów jest instalacja i używanie `gatsby-source-file` do łączenia plików lokalnych, można również używać innych pluginów źródłowych, takich jak `gatsby-source-contentful`, `gatsby-source-datocms` i `gatsby-source-sanity`.

```bash
npm install --save gatsby-image gatsby-plugin-sharp gatsby-transformer-sharp
```

```js:title=gatsby-config.js
module.exports = {
  plugins: [
    `gatsby-transformer-sharp`,
    `gatsby-plugin-sharp`,
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        path: `${__dirname}/src/data/`,
      },
    },
  ],
}
```

_Dla dogłębnych instrukcji na temat instalacji, sprawdź dokumentację [Using Gatsby Image](/docs/using-gatsby-image/)._

### Gatsby Image zaczyna się od zapytania

Aby wprowadzić dane pliku do Gatsby Image, skonfiguruj [zapytanie GraphQL](/docs/graphql-reference/) i albo przekaż go do komponentu jako propsy, albo zapisz go bezpośrednio w komponencie. Jedną z technik jest wykorzystanie [`useStaticQuery`](/docs/use-static-query/).

Typowe zapytania GraphQL do pozyskiwania obrazów wykorzystują `file` z [gatsby-source-filesystem](/packages/gatsby-source-filesystem/) jak i ` imageSharp` oraz `allImageSharp` z [gatsby-plugin-sharp](/packages/gatsby-plugin-sharp/), ale ostatecznie dostępne opcje będą zależeć od źródeł treści.

> Uwaga: możesz użyć również [skrótów GraphQL](/docs/graphql-reference/#aliasing) do przeszukiwania wielu obrazów tego samego typu.

Zobacz poniżej przykłady kodu zapytań i sposób ich użycia w komponentach.

## Typy obrazów w `gatsby-image`

Obiekty obrazów Gatsby są tworzone metodami GraphQL. Dostępne są dwa typy optymalizacji obrazu, _fixed_ i _fluid_, które tworzą wiele rozmiarów obrazu (1x, 1,5x itp.). Istnieje również metoda _resize_, która zwraca pojedynczy obraz.

### Obrazy z _zablokowaną_ szerokością i wysokością

Automatycznie tworzy obrazy dla różnych rozdzielczości przy określonej szerokości lub wysokości - Gatsby tworzy responsywne obrazy dla powiększenia 1x, 1,5x i 2x pikseli za pomocą elementu `<picture>`.

Po wysłaniu zapytania o obraz typu `fixed` w celu otrzymania jego danych, możesz przekazać te dane do komponentu `Img`:

```jsx
import { useStaticQuery, graphql } from "gatsby"
import Img from "gatsby-image"

export default () => {
  const data = useStaticQuery(graphql`
    query {
      file(relativePath: { eq: "images/default.jpg" }) {
        childImageSharp {
          # Specify a fixed image and fragment.
          # The default width is 400 pixels
          // highlight-start
          fixed {
            ...GatsbyImageSharpFixed
          }
          // highlight-end
        }
      }
    }
  `)
  return (
    <div>
      <h1>Hello gatsby-image</h1>
      <Img
        fixed={data.file.childImageSharp.fixed} {/* highlight-line */}
        alt="Gatsby Docs are awesome"
      />
    </div>
  )
}
```

#### Parametry dla obrazu typu fixed

W zapytaniu możesz ustalić opcje dla obrazów typu fixed.

- `width` (int, default: 400)
- `height` (int)
- `quality` (int, default: 50)

#### Zwraca

- `base64` (string)
- `aspectRatio` (float)
- `width` (float)
- `height` (float)
- `src` (string)
- `srcSet` (string)

Tutaj przydają się fragmenty takie jak `GatsbyImageSharpFixed`, ponieważ zwracają wszystkie powyższe elementy w jednym wierszu bez konieczności wypisywania ich wszystkich:

```graphql
file(relativePath: { eq: "images/default.jpg" }) {
  childImageSharp {
    // highlight-start
    fixed(width: 400, height: 400) {
      ...GatsbyImageSharpFixed
      // highlight-end
    }
  }
}
```

Przeczytaj więcej o zapytaniach dla obrazów typu fixed w [gatsby-plugin-sharp](/packages/gatsby-plugin-sharp/#fixed) README.

### Obrazy, które dopasowują się do rodzica typu _fluid_

Utwórz elastyczne rozmiary obrazu, który rozciąga siędo rozmiarów rodzica. Na przykład: w przypadku rodzica, którego maksymalna szerokość wynosi 800px, automatyczne rozmiary będą wynosić: 200px, 400px, 800px, 1200px i 1600px - wystarczająco, aby zapewnić niemal optymalny rozmiar obrazu dla każdego rozmiaru urządzenia / rozdzielczości ekranu. Jeśli chcesz mieć większą kontrolę nad wyjściowymi rozmiarami, możesz użyć parametru `srcSetBreakpoints`.

Po zapytaniu o obraz typu `fluid` w celu pozyskania jego danych, możesz przekazać te dane do komponentu `Img`:

```jsx
import { useStaticQuery, graphql } from "gatsby"
import Img from "gatsby-image"

export default () => {
  const data = useStaticQuery(graphql`
    query {
      file(relativePath: { eq: "images/default.jpg" }) {
        childImageSharp {
          # Specify a fluid image and fragment
          # The default maxWidth is 800 pixels
          // highlight-start
          fluid {
            ...GatsbyImageSharpFluid
          }
          // highlight-end
        }
      }
    }
  `)
  return (
    <div>
      <h1>Hello gatsby-image</h1>
      <Img
        fluid={data.file.childImageSharp.fluid} {/* highlight-line */}
        alt="Gatsby Docs are awesome"
      />
    </div>
  )
}
```

#### Parametry zapytania o obraz typu fluid

W zapytaniu, możesz ustalić opcje dla obrazów typu fluid.

- `maxWidth` (int, default: 800)
- `maxHeight`(int)
- `quality` (int, default: 50)
- `srcSetBreakpoints` (array of int, default: [])
- `background` (string, default: `rgba(0,0,0,1)`)

#### Zwraca

- `base64` (string)
- `aspectRatio` (float)
- `src` (string)
- `srcSet` (string)
- `srcSetType` (string)
- `sizes` (string)
- `originalImg` (string)

Tutaj przydają się fragmenty takie jak `GatsbyImageSharpFluid`, ponieważ zwracają wszystkie powyższe elementy w jednym wierszu bez konieczności wpisywania ich wszystkich:

```graphql
file(relativePath: { eq: "images/default.jpg" }) {
  childImageSharp {
    // highlight-start
    fluid(maxWidth: 400) {
      ...GatsbyImageSharpFluid
      // highlight-end
    }
  }
}
```

Przeczytaj więcej o zapytaniach dla obrazów typu fluid w [gatsby-plugin-sharp](/packages/gatsby-plugin-sharp/#fluid) README.

### Obrazy o zmienionym rozmiarze

Oprócz obrazów _fixed_ oraz _fluid_, interfejs API gatsby-image pozwala Ci wywołać funkcję `resize` za pomocą `gatsby-plugin-sharp`, aby zwrócić pojedynczy obraz w przeciwieństwie do wielu rozmiarów. Brak dostępnych domyślnych fragmentów dla metody zmiany rozmiaru.

#### Parametry

- `width` (int, default: 400)
- `height` (int)
- `quality` (int, default: 50)
- `jpegProgressive` (bool, default: true)
- `pngCompressionLevel` (int, default: 9)
- `base64`(bool, default: false)

#### Zwraca

Metoda `resize` zwraca obiekt z następującymi elementami:

- `src` (string)
- `width` (int)
- `height` (int)
- `aspectRatio` (float)

```graphql
allImageSharp {
  edges {
    node {
        resize(width: 150, height: 150, grayscale: true) {
          src
        }
    }
  }
}
```

Przeczytaj więcej o zapytaniach dla obrazów o zmienionym rozmiarze w [gatsby-plugin-sharp](/packages/gatsby-plugin-sharp/#resize) README.

### Wspólne parametry zapytania

Oprócz ustawień `gatsby-plugin-sharp` w` gatsby-config.js`, istnieją dodatkowe opcje zapytań, które dotyczą obrazów _fluid_, _fixed_ i _resized_:

- [`grayscale`](/packages/gatsby-plugin-sharp/#grayscale) (bool, default: false)
- [`duotone`](/packages/gatsby-plugin-sharp/#duotone) (bool|obj, default: false)
- [`toFormat`](/packages/gatsby-plugin-sharp/#toformat) (string, default: \`\`)
- [`cropFocus`](/packages/gatsby-plugin-sharp/#cropfocus) (string, default: `ATTENTION`)
- [`fit`](/packages/gatsby-plugin-sharp/#fit) (string, default: `COVER`)
- [`pngCompressionSpeed`](/packages/gatsby-plugin-sharp/#pngcompressionspeed) (int, default: 4)
- [`rotate`](/packages/gatsby-plugin-sharp/#rotate) (int, default: 0)

Tutaj jest przykład użycia opcji `duotone` w obrazie o zmienionym rozmiarze:

```graphql
fixed(
  width: 800,
  duotone: {
    highlight: "#f00e2e",
    shadow: "#192550"
  }
)
```

<figure>
  <img
    alt="Jay Gatsby holding wine class in normal color and duotone."
    src="./images/duotone-before-after.png"
  />
  <figcaption>Duotone | Before - After</figcaption>
</figure>

Z kolei tutaj przykład użycia opcji `grayscale` w obrazie o zmienionym rozmiarze:

```graphql
fixed(
  grayscale: true
)
```

<figure>
  <img
    alt="Jay Gatsby holding wine class in normal color and duotone."
    src="./images/grayscale-before-after.png"
  />
  <figcaption>Grayscale | Before - After</figcaption>
</figure>

Przeczytaj więcej o wspólnych parametrach zapytania w [`gatsby-plugin-sharp`](/packages/gatsby-plugin-sharp/#shared-options) README.

## Fragmenty zapytań o obraz

GraphQL zawiera pojęcie zwane "fragmentami zapytań", które są częścią zapytania, które można ponownie wykorzystać. Aby ułatwić budowanie za pomocą `gatsby-image`, pluginy przetwarzania obrazu Gatsby, które obsługują` gatsby-image`, są dostarczane z fragmentami, które możesz łatwo dołączyć do swoich zapytań.

> Uwaga: używanie fragmentów w zapytaniach zależy od źródeł danych, które skonfigurowałeś. Przeczytaj więcej na temat fragmentów zapytań o obrazy w [gatsby-image](/packages/gatsby-image/#fragments) README.

### Typowe fragmenty przy użyciu `gatsby-transformer-sharp`

#### Obrazy typu fixed

- `GatsbyImageSharpFixed`
- `GatsbyImageSharpFixed_noBase64`
- `GatsbyImageSharpFixed_tracedSVG`
- `GatsbyImageSharpFixed_withWebp`
- `GatsbyImageSharpFixed_withWebp_noBase64`
- `GatsbyImageSharpFixed_withWebp_tracedSVG`

#### Obrazy typu fluid

- `GatsbyImageSharpFluid`
- `GatsbyImageSharpFluid_noBase64`
- `GatsbyImageSharpFluid_tracedSVG`
- `GatsbyImageSharpFluid_withWebp`
- `GatsbyImageSharpFluid_withWebp_noBase64`
- `GatsbyImageSharpFluid_withWebp_tracedSVG`

#### Informacje o `noBase64`

Jeśli nie chcesz używać [efektu rozmycia](https://using-gatsby-image.gatsbyjs.org/blur-up/), wybierz fragment z `noBase64` na końcu.

#### Informacje o `tracedSVG`

Jeśli chcesz użyć [śledzonych plików zastępczych SVG](https://using-gatsby-image.gatsbyjs.org/traced-svg/), wybierz fragment z `tracedSVG` na końcu.

#### Informacje o `withWebP`

Jeśli chcesz automatycznie korzystać z [obrazów WebP](https://developers.google.com/speed/webp/), gdy przeglądarka obsługuje format pliku, użyj fragmentów `withWebp`. Jeśli przeglądarka nie obsługuje WebP, `gatsby-image` powróci do domyślnego formatu obrazu.

Oto przykład użycia niestandardowego fragmentu z `gatsby-transformer-sharp`. Wybierz taki, który pasuje do żądanego typu obrazu (_fixed_ lub _fluid_):

```graphql
file(relativePath: { eq: "images/default.jpg" }) {
  childImageSharp {
    fluid {
      // highlight-next-line
      ...GatsbyImageSharpFluid_tracedSVG
    }
  }
}
```

Aby uzyskać więcej informacji na temat działania tych opcji, zobacz przykład Gatsby Image: [https://using-gatsby-image.gatsbyjs.org/](https://using-gatsby-image.gatsbyjs.org/)

#### Dodatkowe frgamenty pluginów

Dodatkowo, pluginy obsługujące `gatsby-image` obecnie obejmują [gatsby-source-contentful](/packages/gatsby-source-contentful/), [gatsby-source-datocms](https://github.com/datocms/gatsby-source-datocms) i [gatsby-source-sanity](https://github.com/sanity-io/gatsby-source-sanity). Zobacz [gatsby-image](/packages/gatsby-image/#fragments) README, aby uzyskać więcej informacji.

## Propsy Gatsby-image

Po wykonaniu zapytania możesz przekazać dodatkowe opcje do komponentu gatsby-image.

| Name                   | Type                | Description                                                                                                                               |
| ---------------------- | ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `fixed`                | `object`            | Dane zwrócone z zapytania typu `fixed`                                                                                                    |
| `fluid`                | `object`            | Dane zwrócone z zapytania typu `fluid`                                                                                                    |
| `fadeIn`               | `bool`              | Domyślna animacja fade-in obrazu po załadowaniu                                                                                           |
| `durationFadeIn`       | `number`            | Czas trwania animacji fade-in, który jest ustawiony domyślnie na 500ms                                                                    |
| `title`                | `string`            | Przekazany do renderowanego elementu HTML `img`                                                                                           |
| `alt`                  | `string`            | Przekazany do renderowanego elementu HTML `img`. Domyślnie pusty string, np. `alt=""`                                                     |
| `crossOrigin`          | `string`            | Przekazany do renderowanego elementu HTML `img`                                                                                           |
| `className`            | `string` / `object` | Przekazany do rodzica. Obiekt jest potrzebny do wspierania propsa Glamor css                                                              |
| `style`                | `object`            | Przeniesiony do domyśnych stylów rodzica                                                                                                  |
| `imgStyle`             | `object`            | Przeniesiony do domyśnych stylów obecnego elementu `img`                                                                                  |
| `placeholderStyle`     | `object`            | Przeniesiony do domyśnych stylów tymczasowego elementu `img`                                                                              |
| `placeholderClassName` | `string`            | Klasa przekazana do tymczasowego elementu `img`                                                                                           |
| `backgroundColor`      | `string` / `bool`   | Ustawia tło zastępcze o jednolitym kolorze. Ustawione na "true", używa jako kolor "lightgray". Możesz też ustawić każdy kolor jako string |
| `onLoad`               | `func`              | Funkcja zwrotna wywoływana gdy załaduje się obraz o pełnym rozmiarze.                                                                      |
| `onStartLoad`          | `func`              | Funkcja zwrotna wywoływana gdy obraz o pełnym rozmiarze zaczyna się ładować, pobiera podany parametr `{ wasCached: <boolean> }`.           |
| `onError`              | `func`              | Funkcja zwrotna wywoływana gdy nie uda się załaować obrazu.                                                                               |
| `Tag`                  | `string`            | Jaki element HTML jest użyty jako rodzic. Domyślnie jest to `div`.                                                                        |
| `objectFit`            | `string`            | Przekazany do polyfill `object-fit-images` gdy importuje się `gatsby-image/withIEPolyfill`. Domyślnie: `cover`.                           |
| `objectPosition`       | `string`            | Przekazany do polyfill `object-fit-images` gdy importuje się `gatsby-image/withIEPolyfill`. Domyślnie: `50% 50%`.                         |
| `loading`              | `string`            | Ustawia natywny atrybut powolnego ładowania. Jeden z `lazy`, `eager` lub `auto`. Domyślnie `lazy`.                                        |
| `critical`             | `bool`              | Rezygnacja z powolnego ładowania. Domyślnie: `false`. Przestarzałe, użyj w zamian `loading`.                                              |

Oto kilka przykładów użycia:

```jsx
<Img
  fluid={data.file.childImageSharp.fluid}
  alt="Cat taking up an entire chair"
  fadeIn="false"
  className="customImg"
  placeholderStyle={{ `backgroundColor`: `black` }}
  onLoad={() => {
    // do loading stuff
  }}
  onStartLoad={({ wasCached }) => {
    // do stuff on start of loading
    // optionally with the wasCached boolean parameter
  }}
  onError={(error) => {
    // do error stuff
  }}
  Tag="custom-image"
  loading="eager"
/>
```
