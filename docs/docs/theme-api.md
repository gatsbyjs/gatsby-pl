---
title: API Motywów Gatsby
---

## Gatsby Core API

Motywy są spakowanymi stronami Gatsby dostarczanymi jako pluginy, dzięki czemu masz dostęp do całego API Gatsby oraz możliwości modyfikowania domyślnej konfiguracji i funkcjonalności.

- [Konfiguracja Gatsby](https://www.gatsbyjs.org/docs/gatsby-config/)
- [Akcje](https://www.gatsbyjs.org/docs/actions/)
- [Interfejs Node](https://www.gatsbyjs.org/docs/node-interface/)
- ... [i wiele więcej](https://www.gatsbyjs.org/docs/api-specification/)

Jeżeli dopiero zaczynasz swoją przygodę z Gatsby, możesz zacząć od [przewodników](https://www.gatsbyjs.org/tutorial/) jak budować strony. Poźniejsze konwertowanie jej do motywu będzie proste i przyjemne, gdyż motywy są wcześniej spakowanymi stronami Gatsby.

## Konfiguracja

 Poza pozostałymi plikami `gatsby-*`, pluginy mogą zawierać teraz także `gatsby-config`. Jeżeli taki plugin będzie zawierał `gatsby-config.js` to zazwyczaj nazwiemy go motywem (więcej na ten temat w [kompozycji motywów](#kompozycja-motywów)). Typowy `gatsby-config.js` strony użytkownika, która używa twojego motywu może wyglądać tak:

Dodatkowo, w tym przykładzie podajemy dwie opcje do `gatsby-theme-name`: `postsPath` oraz `colors`.

```js:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: "gatsby-theme-name",
      options: {
        postsPath: "/blog",
        colors: {
          primary: "tomato",
        },
      },
    },
  ],
}
```

Masz dostęp do opcji przekazanych twojemu motywowi, w jego pliku `gatsby-config`. Dzięki tej możliwości, można na przykład używać ich do konfiguracji lokalizacji źródeł w systemie plików, przyjmowania różnych elementów menu nawigacyjnego strony, zmiany domyślnych kolorów na stronie, czy czegokolwiek innego co może być konfigurowalne.

Aby skorzystać z opcji podanych twojemu motywowi przez konfigurację strony użytkownika, który ten motyw wykorzystuje, w pliku `gatsby-config.js` twojego motywu należy zwrócić funkcję. Argument, który zostanie podany tej funkcji, to opcje które użytkownik poda przy konfiguracji motywu.

```js:title=gatsby-config.js
module.exports = themeOptions => {
  console.log(themeOptions)
  // wyświetli `postsPath` oraz `colors`

  return {
    plugins: [
      // ...
    ],
  }
}
```

Używanie tradycyjnego eksportu obiektu w `gatsby-config` twojego motywu (`module.exports = {}`), oznacza że może on działać jako samodzielna strona w nim zawarta, natomiast jeżeli korzystamy z eksportu funkcji, motyw musi być używany jako część innej strony. Warto zobaczyć jak [theme authoring starter](https://github.com/gatsbyjs/gatsby-starter-theme-workspace) wykorzystuje Yarn Workspaces we współpracy z motywami Gatsby.

### Dostęp do opcji konfiguracji w innych miejscach

Warto zauważyć, że skoro motywy są pluginami, to można uzyskać dostęp do opcji konfiguracji z każdej z funkcji cyklu życia Gatsby. Na przykład, wewnątrz `gatsby-node.js` twojego motywu, opcje zostaną przekazane jako drugi argument do `createPages`:

```js:title=gatsby-node.js
exports.createPages = async ({ graphql, actions }, themeOptions) => {
  console.log(themeOptions)
  // wyświetli przekazane opcje - `postsPath` oraz `colors`
}
```

## Shadowing

Ze względu na fakt, że motywy są publikowane jako paczki na npm, które inni użytkownicy wykorzystują w swoich projektach, należy zapewnić metodę modyfikowania poszczególnych plików (takich jak komponenty Reacta), bez wprowadzania zmian do kodu źródłowego motywu. Metoda ta, nazywa się _Shadowing_ (nadpisywanie).

_Shadowing_ to oparte na systemie plików API, które pozwala na zamianę jednego pliku innym, w trakie budowania strony. Na przykład, jeżeli motyw ma komponent `Header` to użytkownik, który ten motyw wykorzystuje, może zamienić ten komponent swoim przez umieszczenie jego pliku w odpowiedniej lokalizacji tak, aby _Shadowing_ mógł go odnaleźć.

### Nadpisywanie

Spójrzmy na przykład `Header` - powiedzmy, że stworzyłeś motyw o nazwie `gatsby-theme-amazing`. Motyw ten używa komponentu `Header`, aby wyrenderować nawigację i inne elementy z nią związane. Ścieżka do komponentu z katalogu głównego paczki na npm to: `gatsby-theme-amazing/src/components/header.js`.

Jeżeli chciałbyś, aby komponent `Header` robił coś innego (np. chciałbyś zmienić kolory, dodać dodatkową część, czy cokolwiek innego co możnaby zmodyfikować), wystarczy stworzyć plik w projekcie strony, która ten motyw wykorzystuje. Plik ten powinien znaleźć się w lokalizacji `src/gatsby-theme-amazing/components/header.js`. Teraz wystarczy wyeksportować dowolny komponent Reacta, a Gatsby użyje go zamiast odpowiadającego komponentu z motywu.

> 💡 Można również używać w ten sposób komponentów z innych motywów. Dowiedz się więcej na temat zaawansowanych aplikacji w [latent shadowing](https://johno.com/latent-component-shadowing).

### Rozszerzanie

W poprzedniej sekcji opisaliśmy sposób nadpisywania komponentów własnymi. Ale co jeżeli chcielibyśmy tylko dodać coś do zapewnionych nam już komponentów, bez potrzeby kopiowania ich kodu do naszych? W tym celu, można skorzystać z możliwości **rozszerzania** komponentów.

Biorąc przykład `Header` z wcześniejszych sekcji, jeżeli stworzymy plik `src/gatsby-theme-amazing/components/header.js`, można zaimportować oryginalny komponent, a następnie wyeksportować go ponownie, nadpisując jakieś właściwości, czy korzystając z takich technik jak [Komponenty Wyższego Rzędu (HOC)](https://pl.reactjs.org/docs/higher-order-components.html)

```js
import Header from "gatsby-theme-amazing/src/components/header"

// Tutaj właściwości będą takie same, jakie otrzymałby oryginalny komponent, z tym że jedną z nich nadpisujemy/dodajemy ręcznie
export default props => <Header {...props} myProp="true" />
```

Idąc tą drogą, jeżeli później autor motywu go zaaktualizuje, to użytkownik nadal będzie mógł korzystać z wprowadzonych zmian, gdyż istniejący komponent będzie tylko rozszerzony, a nie wymieniony.

### Jaka ścieżka powinna zostać wybrana, aby nadpisać plik?

Ze względu na fakt, że Gatsby posiada zestaw narzędzi do automatycznego nadpisywania plików, musimy ręcznie wyszukiwać ścieżki do komponentów w kodzie źródłowym motywu, a następnie tworzyć odpowiadające im pliki w projekcie strony, która ten motyw wykorzystuje.

Na szczęście, jest to tylko kilka kroków:

1. Znajdź lokalizację `src` wewnątrz kodu źródłowego danego motywu w `node_modules`
2. Przesuń ją na początek ścieżki
3. Znajdź plik, który chcesz nadpisać, i dodaj jego lokalizację względem katalogu src do końca ścieżki

Biorąc pod uwagę przykład komponentu `Header`, to jest jego ścieżka wewnątrz motywu:

```text
gatsby-theme-amazing/src/components/header.js
```

a to jest ścieżka pliku, wewnątrz którego powinno znaleźć się jego nadpisanie

```text
<twoja-strona>/src/gatsby-theme-amazing/components/header.js
```

Nadpisywanie działa tylko na plikach zaimportowanych wewnątrz lokalizacji `src` motywu, gdyż zbudowane jest ono na Webpacku - wykres zależności musi zawierać nadpisywalny plik.

Skoro można używać wielu motywów wewnątrz jednej strony, potencjalnie może powstać sytuacja gdy jeden plik będzie nadpisany w wielu miejscach. W sytuacji, gdy wiele motywów będzie chciało nadpisać plik `gatsby-theme-amazing/src/components/header.js` wewnątrz innego motywu, Gatsby wybierze nadpisanie z ostatniego motywu wewnątrz arraya pluginów w projekcie strony, która z nich korzysta. Jeżeli nadpisanie pojawi się również wewnątrz projektu samej strony, to ono będzie miało zawsze największy priorytet.

## Kompozycja motywów

Motywy Gatsby mogą przyjmować kompozycję wertykalną i horyzontalną. Kompozycja wertykalna odnosi się do klasycznej relacji "rodzic->dziecko". Motyw-dziecko deklaruje motyw-rodzica, wewnątrz swojej array pluginów.

```js:title=gatsby-theme-child/gatsby-config.js
module.exports = {
  plugins: [`gatsby-theme-parent`],
}
```

Kompozycja horyzontalna jest wtedy, gdy dwa motywy są użyte razem, obok siebie. Na przykład użycie `gatsby-theme-blog` oraz `gatsby-theme-notes` wewnątrz jednej strony.

```js:title=my-site/gatsby-config.js
module.exports = {
  plugins: [`gatsby-theme-blog`, `gatsby-theme-notes`],
}
```

Motywy od środka są algorytmem, który scala wiele plików `gatsby-config.js`, do pojedynczej konfiguracji, którą twoja strona może użyć aby się zbudować. Aby mogło się to stać, trzeba zdefiniować sposób w jaki łączone jest wiele plików `gatsby-config.js`. Zanim to się stanie, trzeba wyrównać relacje rodzic->dziecko do pojedynczego arraya. Rezultat tego ma znaczenie, gdy trzeba określić z którego motywu użyć plik do nadpisywania, gdy dostępnych jest wiele.

Rezultat pierwszego przykładu to `['gatsby-theme-parent', 'gatsby-theme-child']` (rodzice zawsze muszą być przed ich dziećmi, tak aby one mogły nadpisać ich funkcjonalność, bo to one deklarują użycie ich rodzica), a rezultatem drugiego przykładu jest `['gatsby-theme-blog', 'gatsby-theme-notes']`.

Gdy wszystkie motywy są ułożone w końcowej kolejności, zostają scalone za pomocą funkcji [reduce](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby/src/utils/merge-gatsby-config.js) - określa ona sposób, w jaki każdy klucz wewnątrz `gatsby-config.js` zostanie połączony. Poza wymienionymi poniżej, ostatnia wartość wygrywa.

- `siteMetadata` i `mapping` są scalane głęboko, za pomocą funkcji `merge` lodash'a. Oznacza to, że motyw może ustawić domyślne wartości `siteMetadata`, a strona go wykorzystująca, może nadpisać część z nich, korzystając z obiektu `siteMetadata` wewnątrz jej `gatsby-config.js`.
- `plugins` są normalizowane aby usunąć duplikaty, a następnie łączone.
