---
title: Node Interface
---

"Node" jest centrum systemu danych Gatsby. Wszystkie dane które są dodane do Gatsby są modelowane wykorzystując node'y.

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

Klucz zazrezerwowany dla wtyczek które chcą rozszerzyć swoje node'y.

### `contentDigest`

Skrót "Hash", lub krótkie cyfrowe streszczenie zawartości tego node'a (np. `md5sum`).

Skrót powinien być unikatowy dla zawartości tego node'a, ponieważ jest on używany do buforowania. Jeśli zawartość zmieni się, skrót również powinien ulec zmianie. Istnieje funkcja pomocnicza o nazwie [createContentDigest](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-core-utils/src/create-content-digest.js), która służy do utworzenia skrótu `md5`.

### `mediaType`

Opcjonalny [typ nośnika](https://pl.wikipedia.org/wiki/Typ_MIME) do wskazania wtyczek przekształcających ten node oraz jego dane, które mogą być dalej przetwarzane.

### `type`

Globalny, unikatowy typ node'a wybrany przez właściciela wtyczki.

### `owner`

Wtyczka przez którą stworzony został node. To pole jest dodane przez Gatsby(a nie przez wtyczkę).

### `fieldOwners`

Magazyn informacji które wtyczki utworzyły poszczególne pola. To pole jest dodane przez Gatsby(a nie przez wtyczkę).

### `content`

Opcjonalne pole dostarczające surową zawartość dla tego node'a którą mogą pobierać wtyczki przekształcające w celu dalszego przetwarzania.

## Wtyczki źródłowe

Nowe node'y dodane do Gatsby poprzez "źródłowe" wtyczki. Powszechnie stosowaną przez wiele witryn Gatsby wtyczką źródłową jest [wtyczka źródłowa systemu plików](/packages/gatsby-source-filesystem/) która przekształca pliki na dysku na node'y pliku.

Inne wtyczki źródłowe zaciągają dane z zewnętrznych interfejsów API jak np. [Drupal](/packages/gatsby-source-drupal/) czy [Hacker News](/packages/gatsby-source-hacker-news/).

## Wtyczki przekształcające

Wtyczki przekształcające mogą również tworzyć node'y poprzez przekształcenie źródeł node'ów w nowe typy node'ów. Instalowanie obu wtyczek - źródłowych i przekształcających jest powszechną praktyką podczas budowania stron Gatsby.

Node'y utworzone przez wtyczki przekształcające są ustawione jako "dzieci" swoich "rodziców".

- The
  [Remark (Markdown library) transformer plugin](/packages/gatsby-transformer-remark/)
  looks for new nodes that are created with a `mediaType` of `text/markdown` and
  then transforms these nodes into `MarkdownRemark` nodes with an `html` field.
- The [YAML transformer plugin](/packages/gatsby-transformer-yaml/) looks for
  new nodes with a media type of `text/yaml` (e.g. a `.yaml` file) and creates
  new YAML child node(s) by parsing the YAML source into JavaScript objects.

## GraphQL

Gatsby automatycznie wprowadza strukturę node'ów Twojej strony i tworzy schemat GraphQL, który można następnie odpytywać z komponentów twojej strony.

## Tworzenie node'ów

Aby dowiedzieć się więcej na temat tego jak tworzone i łączone są ze sobą node'y sprawdź dokumentację [tworzenia node'ów](/docs/node-creation/) w sekcji "Za kulisami"
