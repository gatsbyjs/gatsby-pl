---
title: Node Interface
---

"Node" jest centrum systemu danych Gatsby. Wszystkie dane, które są dodane do Gatsby są modelowane wykorzystując node'y.

## Struktura danych node'a

Podstawowa struktura danych node'a przedstawiona jest następująco:

```flow
id: String,
children: Array[String],
parent: String,
fields: Object,
internal: {
  contentDigest: String,
  mediaType: String,
  type: String,
  owner: String,
  fieldOwners: Object,
  content: String,
}
...innne pola określone dla tego typu node'a
```

### `parent`

Klucz zarezerwowany dla wtyczek, które chcą rozszerzyć swoje node'y.

### `contentDigest`

Skrót "Hash", lub krótkie cyfrowe streszczenie zawartości tego node'a (np. `md5sum`).

Skrót powinien być unikatowy dla zawartości tego node'a, ponieważ jest on używany do buforowania. Jeśli zawartość zmieni się, skrót również powinien ulec zmianie. Istnieje funkcja pomocnicza o nazwie [createContentDigest](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-core-utils/src/create-content-digest.js), która służy do utworzenia skrótu `md5`.

### `mediaType`

Opcjonalny [typ nośnika](https://pl.wikipedia.org/wiki/Typ_MIME) do wskazania wtyczek przekształcających ten node oraz jego dane, które mogą być dalej przetwarzane.

### `type`

Globalny, unikatowy typ node'a wybrany przez właściciela wtyczki.

### `owner`

Wtyczka, przez którą stworzony został node. To pole jest dodawane przez Gatsby (a nie przez wtyczkę).

### `fieldOwners`

Magazynuje informacje, które wtyczki utworzyły poszczególne pola. To pole jest dodawane przez Gatsby (a nie przez wtyczkę).

### `content`

Opcjonalne pole dostarczające surową zawartość dla tego node'a, którą mogą pobierać wtyczki przekształcające w celu dalszego przetwarzania.

## Wtyczki źródłowe

Nowe node'y dodawane są do Gatsby poprzez wtyczki "źródłowe". Powszechnie stosowaną przez wiele witryn Gatsby wtyczką źródłową jest [wtyczka źródłowa systemu plików](/packages/gatsby-source-filesystem/), która przekształca pliki na dysku na node'y pliku.

Inne wtyczki źródłowe zaciągają dane z zewnętrznych interfejsów API takich jak np. [Drupal](/packages/gatsby-source-drupal/) czy [Hacker News](/packages/gatsby-source-hacker-news/).

## Wtyczki przekształcające

Wtyczki przekształcające mogą również tworzyć node'y poprzez przekształcenie źródeł node'ów w nowe typy node'ów. Instalowanie obu wtyczek - źródłowych i przekształcających jest powszechną praktyką podczas budowania stron Gatsby.

Node'y utworzone przez wtyczki przekształcające są ustawione jako "dzieci" swoich "rodziców".

- [Wtyczka przekształcająca dane (biblioteka Markdown)](/packages/gatsby-transformer-remark/) wyszukuje nowych node'ów,
  które zostały utworzone z typem `text/markdown` dla `mediaType` i  
  przekształca je na node'y `MarkdownRemark` z polami `html`.
- [Wtyczka przekształcająca YAML](/packages/gatsby-transformer-yaml/) wyszukuje nowe node'y z typem nośnika `text/yaml` (np. pliki `.yaml`) i tworzy nowe YAML dzieci node'y poprzez transformację źródła YAML w obiekt JavaScript.

## GraphQL

Gatsby automatycznie wprowadza strukturę node'ów twojej strony i tworzy schemat GraphQL, który można następnie odpytywać z komponentów twojej strony.

## Tworzenie node'ów

Aby dowiedzieć się więcej na temat tego jak tworzone i łączone są ze sobą node'y sprawdź dokumentację [tworzenia node'ów](/docs/node-creation/) w sekcji "Za kulisami".
