---
title: Glossary
disableTableOfContents: true
---

Kiedy dopiero zaczynasz z Gatsby, odkrywasz jak wiele jest nowych słów do poznania. W tym słowniczku staramy się stworzyć ogólny przegląd najpopularniejszych pojęć i ich znaczenia dla stron Gatsby.

<HorizontalNavList
  items={"ABCDEFGHIJKLMNOPQRSTUVWXYZ".split("")}
  slug={props.slug}
/>

## A

### AST

Drzewo składniowe (Abstract Syntax Tree): Reprezentacja kodu źródłowego w formie drzewa, którą znaleźć można podczas [kompilacji](#compiler) między dwoma językami. Na przykład, [gatsby-transformer-remark](/packages/gatsby-transformer-remark/) stworzy AST ze składni [Markdown](#markdown) aby opisać dokumenty Markdown w formie drzewa korzystając z parsera [Remark](#remark).

### API

Interfejs programowania aplikacji (Application Programming Interface): Sposób w jaki jedna aplikacja porozumiewa się z drugą. Na przykład, [plugin źródłowy](#source-plugin) będzie często korzystał z API, aby uzyskać swoje dane.

### Accessibility

Praktyka mająca na celu przełamanie barier, które uniemożliwiają osobom niepełnosprawnym interakcję lub dostęp do stron internetowych. Strony, które są projektowane i budowane z myślą o dostępności umożliwają wszystkim ludziom równy dostęp do informacji i funkcjonalności. Przeczytaj o [Zobowiązaniu Gatsby wobec dostępności](/blog/2019-04-18-gatsby-commitment-to-accessibility/).

## B

### Babel

Narzędzie, które umożliwia Ci korzystanie z najnowocześniejszych funkcji [JavaScript](#javascript), a podczas procesu [budowania](#build) [kompiluje](#compiler) kod tak, aby był on zrozumiały dla wiekszości przeglądarek.

### Backend

To co dzieje się za kulisami i nie jest [publiczne](#public). Często odnosi się to do panelu kontrolnego twojego systemu [CMS](#cms). Są one często obsługiwane przez języki programowania po stronie serwera, takie jak Node.js, PHP, Go, ASP.net, Ruby lub Java.

### [Build](/docs/glossary/build/)

W Gatsby, jest to proces przemiany kodu i zawartości w stronę internetową, która może być hostowana i dostępna z zewnątrz. Zobacz również: [backend](#backend) i [serwerowy](#server-side).

## C

### Cache

Informacje, które są przechowywane lokalnie do kolejnego użytku, aby przyspieszyć obliczenia i pobieranie informacji z jednego miejsca. Gatsby korzysta z cache do przechowywania danych, dzięki czemu Twoja strona budowana jest szybciej i bez wykonywania dwukrotnie tej samej pracy.

### CLI

Interfejs wiersza poleceń (Command Line Interface): Aplikacja, która uruchamiana jest na Twoim komputerze dzięki [wierszowi poleceń](#command-line) i sterowana przy pomocy klawiatury.

Gatsby posiada dwa interfejsy wiersza poleceń. Pierwszy, [`gatsby`](/docs/gatsby-cli/), do codziennej pracy deweloperskiej z Gatsby, oraz drugi, [`gatsby-dev`](/contributing/setting-up-your-local-dev-environment/#gatsby-repo-install-instructions), dla osób, które chcą przyczynić się bezpośrednio do rozwoju projektu Gatsby.

### Client-side

Po stronie klienta (Client-side) odnosi się do operacji, które odbywają się po stronie przeglądarki użytkownika w [relacji klient-serwer](https://en.wikipedia.org/wiki/Client%E2%80%93server_model) w sieci komputerowej. W Gatsby jest to szczególnie ważne podczas [pracy z paczkami](/docs/using-client-side-only-packages/), które korzystają z obiektów [przeglądarkowego DOM](#dom), takich jak `window` czy `navigator`. Zobacz także: [Po stronie serwera](#server-side), [frontend](#frontend) i [backend](#backend).

### CMS

System zarządzania treścią (Content Management System): aplikacja, w której możesz zarządzać swoją zawartością oraz zapisywać ją w bazie danych lub pliku, do późniejszego dostępu. Przykłady systemów zarządzania treścią to WordPress, Drupal, Contentful, i Netlify CMS.

### Command Line

Interfejs tekstowy do uruchamiania poleceń na komputerze. Domyślnymi aplikacjami wiersza poleceń dla komputerów Mac i Windows są odpowiednio `Terminal` oraz `Wiersz poleceń`.

### Compiler

Kompilator jest programem, który tłumaczy kod napisany w jednym języku na inny. Na przykład [Gatsby](#gatsby) kompiluje aplikacje napisane w [React](#react) do statycznych plików [HTML](#html).

### Component

Komponenty to niezależne fragmenty kodu wielokrotnego użytku zasilane przez [React](#react), które gdy są połączone, tworzą Twoją stronę lub aplikację.

Komponent może zawierać inne komponenty. W rzeczywistości, [strony](#page) i [szablony](#template) są przykładami komponentów.

### Config

Plik konfiguracyjny, `gatsby-config.js` przekazuje Gatsby informacje o Twojej stronie internetowej. Popularną opcją ustawianą w pliku konfiguracyjnym są metadane Twojej strony, które następnie zasilają meta tagi SEO.

### [Content Delivery Network](/docs/glossary/content-delivery-network)

A content delivery network (CDN) is a highly distributed network of servers that stores copies of your content in locations that are closer to your site's visitors. Content delivery networks improve your site's performance by reducing the time needed to complete a network request.

### [Continuous Deployment](/docs/glossary/continuous-deployment)

Continuous deployment (CD) automates the process of releasing changes to your project. A continuous deployment workflow automatically builds and tests your project, and publishes your changes only when they pass the required tests.

### CSS

[CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) to kaskadowe arkusze stylów (Cascading Style Sheets), które razem z [HTML](#html) i [JavaScript](#javascript) stanowią podstawę platformy internetowej. CSS to język służący do stylizowania stron internetowych, które w zamyśle mają być wysoce kompatybilne wstecz. W miarę wprowadzania nowych funkcji dla użytkowników końcowych, [parsery CSS](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/#CSS_parsing) mogą bezpiecznie ignorować nieobsługiwane funkcje i ulepszać je za pomocą obsługiwanych przez nie właściwości. CSS osiąga to dzięki _kaskadowalnemu_ designowi, który jest niezbędny do stylowania przy użyciu nowych technik takich jak [CSS Grid](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout/CSS_Grid_and_Progressive_Enhancement) zapewniając jednoczeście wsparcie dla starych przeglądarek. Gatsby obsługuje wiele [sposobów stylowania](/docs/styling/), włącznie ze zwykłymi plikami CSS, modułami CSS, oraz CSS-in-JS.

## D

### Data Source

Miejsce pochodzenia treści i danych, najczęściej zintegrowane z Gatsby przy pomocy [pluginów źródłowych](#source-plugin). Źródłem informacji jest zazwyczaj [Headless CMS](#headless-cms), ale mogą być to równie dobrze pliki Markdown, JSON, czy YAML.

### Database

Baza danych to uporządkowany zbiór danych lub treści. [CMS](#cms) często zapisuje informacje do bazy danych przy pomocy [technologii backend](#backend). Zazwyczaj dostęp do nich w Gatsby odbywa się poprzez [pluginy źródłowe](#source-plugin).

### Decoupled

Oddzielenie opisuje podział różnych problemów. W przypadku [Gatsby](#gatsby) najczęściej oznacza to oddzielenie [frontendu](#frontend) od [backendu](#backend), podobnie jak w przypadku [Decoupled Drupal](https://dri.es/how-to-decouple-drupal-in-2019) lub [Headless WordPress](https://www.smashingmagazine.com/2018/10/headless-wordpress-decoupled/).

### [Decoupled Drupal](/docs/glossary/decoupled-drupal)

Decoupling refers to the practice of using Drupal as a [headless CMS](#headless-cms). A decoupled Drupal instance functions as a content API that returns JSON for your [frontend](#frontend) to consume.

### Deploy

Proces [budowania](#build) Twojej strony lub aplikacji i przesyłania jej na [hosting](#hosting).

### Development Environment

[Środowisko](#environment) podczas pracy nad kodem. Jest ono obsługiwane poprzez [CLI](#cli) za pomocą komendy `gatsby develop` i zapewnia specjalistyczne raportowanie błędów oraz wiele dodatkowych funkcji, które przydadzą się podczas debugowania kodu przed zbudowaniem projektu dla [produkcji](#production-environment).

### DOM

Document Object Model, powszechnie nazywany „DOM”, to standardowy interfejs API przeglądarki, który łączy strony internetowe ze skryptami lub językami programowania, reprezentując strukturę dokumentu HTML w pamięci. Programiści często wchodzą w interakcje z DOM za pomocą znaczników [HTML](#html) (w Gatsby napisanych korzystając z [JSX](#jsx)), a także [React](https://reactjs.org/docs/react-dom.html) i [zwykły JavaScript](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction#DOM_and_JavaScript). Innym ważnym aspektem wykorzystania DOM do pełnego potencjału jest pisanie [dostępnych](#accessibility) znaczników HTML, aby umożliwić korzystanie ze strony przy pomocy technologii wspomagającej.

## E

### ECMAScript

ECMAScript (często nazywany po prostu ES) jest specyfikacją dla języków skryptowych. [JavaScript](#javascript) jest implementacją ECMAScript'u. Bardzo często deweloperzy korzystają z biblioteki [Babel](#babel), aby [kompilować](#compiler) najnowszy kod ECMAScript do bardziej powszechnej wersji JavaScript'u.

### Environment

Środowisko w którym działa Gatsby. Przykładowo, podczas pisania kodu prawdopodobnie oczekujesz jak najwięcej funkcji, które Ci to ułatwią, ale w wersji końcowej są one już zbędne. W związku z tym Gatsby może zmieniać swoje zachowanie w zależności od środowiska, w którym się znajduje.

Gatsby domyślnie wspiera dwa środowiska, [środowisko deweloperskie](#development-environment) i [środowisko produkcyjne](#production-environment).

### Environment Variables

[Zmienne środowiskowe](/docs/environment-variables/) pozwalają na dostosowywanie zachowania Twojej aplikacji w zależności od jej [środowiska](#environment). Przykładowo podczas rozwijania oprogramowania możesz chcieć pobierać swoją zawartość z przejściowego systemu CMS i połączyć się z właściwym CMS, gdy już [zbudujesz](#build) swoją stronę. Za pomocą zmiennych środowiskowych możesz ustawić inny adres URL dla każdego środowiska.

## F

### Filesystem

Sposób w jaki pliki są zorganizowane. W przypadku Gatsby oznacza to, że pliki znajdują się w tym samym miejscu co kod witryny lub aplikacji, zamiast pobierać dane z zewnętrznego [źródła](#data-source). Najczęstsze użycie systemu plików w Gatsby obejmuje zawartość zapisaną w plikach Markdown, obrazy, pliki danych i inne zasoby.

### Frontend

[Publiczny](#public) interfejs  dla Twojej witryny lub aplikacji, dostarczany z wykorzystaniem technologii internetowych: HTML, CSS i JavaScript. Aby dowiedzieć się więcej o tym, jak platforma internetowa łączy te technologie, zapoznaj się z tym artykułem na temat [Jak działają przeglądarki](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/).

## G

### Gatsby

Gatsby to nowoczesna platforma internetowa, która zwiększa wydajność każdej witryny lub aplikacji, wykorzystując najnowsze technologie sieciowe, takie jak [React](#react), [GraphQL](#graphql) i nowoczesny [JavaScript](#javascript). Gatsby ułatwia tworzenie niesamowicie szybkich i atrakcyjnych stron internetowych bez konieczności bycia ekspertem od wydajności.

### [GraphQL](/docs/glossary/graphql)

Język [zapytań](#query), który pozwala na pobieranie danych do swojej strony lub aplikacji. [Gatsby używa go](/docs/graphql/) jako interfejsu do zarządania danymi strony.

## H

### HTML

Język znaczników, który jest w stanie zrozumieć każda przeglądarka. Skrót oznacza hipertekstowy język znaczników (Hypertext Markup Language). [HTML](https://developer.mozilla.org/en-US/docs/Web/HTML) nadaje treści internetowej uniwersalną strukturę, definiując takie elementy, jak nagłówki, akapity i inne. Jest to kluczowe do tego, aby strona internetowa była dostępna dla wszystkich.

### [Headless CMS](/docs/glossary/headless-cms)

System [CMS](#cms), który obsługuje jedynie [backend](#backend) zarządzania treścią zamiast obsługiwać zarówno backend jak i [frontend](#frontend). Ten typ konfiguracji jest również określany jako [oddzielny](#decoupled).

### [Headless WordPress](/docs/glossary/headless-wordpress)

Sposób w którym korzysta się z plików JSON otrzymanych z REST API WordPressa jako [headless CMS](#headless-cms). Pozwala Ci to używać WordPressa do pisania i edytowania zawartości, która następnie może być użyta przez jakiegokolwiek klienta zdolnego do parsowania formatu JSON

### Hosting

Dostawca hostingu przechowuje kopię witryny lub aplikacji i udostępnia ją [publicznie](#public). [Typowi dostawcy hostingu dla Gatsby](/docs/deploying-and-hosting/) to Netlify, AWS, S3, Surge, Heroku i inni.

### Hot module replacement

Funkcja uruchamiana po użyciu komendy `gatsby develop`, która na żywo aktualizuje twoją stronę po zapisaniu kodu w edytorze tekstów automatycznie zastępując moduły lub fragmenty kodu w otwartym oknie przeglądarki.

### Hydration

Gdy witryna zostanie [zbudowana](#build) przez Gatsby i załadowana do przeglądarki internetowej, zasoby JavaScript [po stronie klienta](#client-side) JavaScript pobiorą i przekształcą witrynę w pełną aplikację React, która może manipulować [DOM](#dom). Ten proces jest często nazywany re-hydration, ponieważ uruchamia ten sam kod JavaScript, który służy do generowania stron Gatsby, ale tym razem z dostępnymi interfejsami API DOM przeglądarki, takimi jak `window`.

## I

### Inference

As part of its data layer and [build](#build) process, Gatsby will automatically **infer** a [schema](#schema), or type-based structure, based on available data sources (e.g. Markdown file nodes, WordPress posts, etc.). More control can be gained over this structure by using Gatsby's [Schema Customization API](/docs/schema-customization/).

## J

### [JAMStack](/docs/glossary/jamstack)

JAMStack odnosi się do nowoczesnej architektury internetowej z wykorzystaniem [JavaScript](#javascript), [API](#api) i  znaczników ([HTML](#html)). Z [JAMStack.org](https://jamstack.org): „Jest to nowy sposób tworzenia witryn i aplikacji, który zapewnia lepszą wydajność, wyższe bezpieczeństwo, niższe koszty skalowania i lepsze wrażenia programistyczne”.

### JavaScript

Język programowania, który pomaga nam uczynić sieć dynamiczną i interaktywną. [JavaScript](https://developer.mozilla.org/en-US/docs/Web/Javascript) to technologia internetowa szeroko stosowana w przeglądarkach. Jest również używany po stronie serwera z [Node.js](#node). Jest to implementacja specyfikacji [ECMAScript](#ECMAScript).

### JSX

JSX to rozszerzenie JavaScript, które pozwala programistom pisać HTML i niestandardowe komponenty w tym samym kodzie. [Zespół React zaleca](https://reactjs.org/docs/introducing-jsx.html) korzystać z niego do opisywania, jak powinno wyglądać [UI](#UI). JSX może przypominać język szablonów, ale ma pełną moc JavaScript. Należy zwrócić uwagę na kilka ważnych szczegółów, ponieważ ponieważ JSX używa JavaScript, niektóre atrybuty HTML w znacznikach muszą zostać zamienione, aby uniknąć zarezerwowanych słów w JavaScript (takie jak `htmlFor` i` className`).

## K

## L

### Linting

Linting to proces uruchamiania programu, który analizuje kod pod kątem potencjalnych błędów. Projekt Gatsby używa [prettier](https://prettier.io/) do identyfikowania i rozwiązywania typowych problemów ze stylem. Innym przykładem lintera powszechnie stosowanego w projektach React jest [eslint-jsx-plugin-a11y](https://github.com/evcohen/eslint-plugin-jsx-a11y), który sprawdza standardowe problemy z [dostępnością](#accessibility).

## M

### [MDX](/docs/glossary/mdx/)

Rozszerzenie [Markdown](#markdown) dodające wsparcie dla [komponentów](#component) [Reactowych](#react) wewnątrz Twojej zawartości.

### [Markdown](/docs/glossary/markdown/)

Sposób pisania treści HTML przy użyciu zwykłego tekstu korzystając ze znaków specjalnych w celu oznaczenia typów treści, takich jak symbole [nagłówków](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements) oraz podkreślenia i gwiazdki dla wyróżnienia tekstu.

## N

### NPM

Manager [Paczek](#package) [Node](#node) (Node Package Manager). Służy do instalowania i uaktualniania paczek, na których polega Twój projekt. [Gatsby](#gatsby) i [React](#react) są ich przykładami. Zobacz również: [Yarn](#yarn).

### Node

Gatsby korzysta z [węzłów danych](/docs/node-interface/) do reprezentowania pojedynczego elementu danych. [Źródło danych](#data-source) tworzy wiele węzłów.

### [Node.js](/docs/glossary/node)

Program, który pozwala uruchamiać [JavaScript](#javascript) na twoim komputerze. Gatsby jest zbudowany przy użyciu Node.

## O

## P

### Package

Paczką zazwyczaj nazywamy [javascriptowy](#javascript) program zawierający dodatkowe informacje odnośnie tego, w jaki sposób powinien być on używany i dystrybuowany, np. numer jego wersji. [NPM](#npm) i [Yarn](#yarn) zarządzają i instalują paczki używane przez Twój projekt. [Gatsby](#gatsby) sam w sobie jest paczką.

### Page

Strona [HTML](#html).

Często odnosi się to również do [komponentów](#component) umieszczonych w `/src/pages/`, które [Gatsby](#gatsby) przekształca w strony, oraz do [dynamicznie tworzonych stron](/docs/creating-and-modifying-pages/#creating-pages-in-gatsby-nodejs) w Twoim pliku `gatsby-node.js`.

### Plugin

Kod, który pozwala Gatsby korzystać z nowych funkcji. Najpopularniejsze [pluginy Gatsby](/plugins/) to pluginy [źródłowe](#source-plugins) oraz [transformujące](#transformer), które służą odpowiednio do pobierania i manipulowania danymi.

### Production Environment

[Środowisko](#environment) dla [zbudowanej](#build) już witryny lub aplikacji, z której korzystać będą użytkownicy po [wdrożeniu](#deploy). Można uzyskać do niego dostęp poprzez [CLI](#cli) korzystając z `gatsby build` lub `gatsby serve`.

### Programmatically

Coś, co dzieje się automatycznie na podstawie kodu i konfiguracji. Możesz na przykład [skonfigurować](#config) swój projekt, aby dla każdego napisanego posta na blogu tworzyć osobną [stronę](#page), lub wyświetlać w stopce rok, który zawsze będzie aktualny.

### Progressive enhancement

Progresywne ulepszanie to strategia internetowa, która polega na załadowaniu ​​zawartości strony głównej z serwera przed czymkolwiek innym, bez [JavaScript](#javascript) jako wymogu ładowania. Strategia ta następnie stopniowo dodaje bardziej złożone warstwy prezentacji i funkcje nad treścią, w stopniu na jaki pozwala przeglądarka/połączenie sieciowe użytkownika końcowego. Domyślne podejście Gatsby do [budowania](#build) stron z wyprzedzeniem oznacza, że ​​zawartość zostanie załadowana jako pierwsza i ulepszona w miarę pobierania i wykonywania skryptów.

### Public

Zwykle odnosi się to do kogoś spoza Twojego zespołu lub do folderu `/public`, w którym [zbudowana](#build) już strona lub aplikacja jest zapisana.

## Q

### Query

Proces pozyskiwania określonych danych. W Gatsby zapytania zwykle wykonuje się korzystając z [GraphQL](#graphql).

## R

### [React](/docs/glossary/react)

Biblioteka (napisana w [JavaScript](#javascript)) przeznaczona do budowania interfejsów użytkownika. [Gatsby](#gatsby) korzysta z niej do budowania stron oraz strukturyzacji treści

### Remark

Parser służący do tłumaczenia [Markdown](#markdown) na inny format taki jak [HTML](#html) lub kod [React](#react).

### Runtime

Środowisko wykonawcze odbywa się, gdy program jest uruchomiony (lub jest wykonywany); może odnosić się to do kilku rzeczy. [Node.js](#nodejs) to środowisko wykonawcze [po stronie serwera](#server-side), które wykonuje kod JavaScript. [JavaScript po stronie klienta](#client-side) odnosi się do środowiska wykonawczego przeglądarki, w którym wykonuje się tradycyjny kod JavaScript. Gatsby kompiluje witrynę w [czasie budowania](#build) i korzysta z [rehydratacji przy użyciu środowiska wykonawczego React](#hydration), aby zapewnić szybkie, interaktywne i dynamiczne doświadczenia dla użytkownika.

### Routing

Routing to mechanizm służący do ładowania prawidłowej zawartości w witrynie lub aplikacji na podstawie żądania sieciowego - zazwyczaj adresu URL. Na przykład umożliwia kierowanie adresów URL, takich jak `/about-us`, do odpowiedniej [strony](#page), [szablonu](#template) lub [komponentu](#component).

## S

### Schema

Dokładna reprezentacja sposobu przechowywania danych w systemie, na przykład tabele i pola w bazie danych lub struktura plików JSON. W Gatsby schemat GraphQL wyraża wszystkie kwerendy danych - lub dane, o które komponenty mogą prosić w ramach warstwy danych Gatsby.

### Server-side

Część serwerowa [relacji klient-serwer](https://en.wikipedia.org/wiki/Client%E2%80%93server_model) odnosi się do operacji wykonywanych przez program komputerowy zarządzający dostępem do scentralizowanego zasobu lub usługi w sieci komputerowej. Gatsby używa serwerowej technologii [Node.js](#nodejs) do kompilowania stron w czasie budowania, w przeciwieństwie do udostępniania ich w [środowisku uruchomieniowym przeglądarki](#runtime) za pomocą JavaScriptu [po stronie klienta](#client-side). Zobacz także: [frontend](#frontend) i [backend](#backend).

### [Server-side rendering](/docs/glossary/server-side-rendering/)

Użycie serwera opartego na [Node.js](#nodejs) do wygenerowania HTML w odpowiedzi na żądanie od klienta użytkownika, takiego jak przeglądarka. Gatsby używa technologii po stronie serwera [Node.js](#nodejs) do kompilowania stron w czasie ich budowania, w przeciwieństwie do udostępniania ich w [środowisku uruchomieniowym](#runtime) za pomocą javascriptu [po stronie klienta](#client-side).

### Source Code

Kod źródłowy to kod, który znajduje się w folderze `/src/` i tworzy unikalne aspekty Twojej witryny lub aplikacji. Składa się z [JavaScript](#javascript), a czasem [CSS](#css) oraz innych plików.

Kod źródłowy zostaje [przebudowany](#build) w stronę, która następnie będzie dostępna [publicznie](#public).

### Source Plugin

[Wtyczka](#plugin), która dodaje dodatkowe [źródła danych](#data-source) do Gatsby, o które mogą następnie [zapytać](#query) Twoje [strony](#page) i [komponenty](#component).

### Starter

Wstępnie skonfigurowany projekt Gatsby, który może być wykorzystany jako punkt wyjścia dla Twojego projektu. Można je odkryć za pomocą [biblioteki starterów Gatsby](/starters/) i zainstalować przy pomocy [Gatsby CLI](/docs/starters/).

### Static

Gatsby [buduje](#build) statyczne wersje strony, które można następnie łatwo [hostować](#hosting). Jest to przeciwieństwo dynamicznych systemów, w których każda strona jest generowana w locie. Statyczna strona zapewnia znaczny wzrost wydajności, ponieważ praca jest wykonywana tylko raz co każdą zmianę treści lub kodu.

Odnosi się to także do folderu `/static`, który jest automatycznie kopiowany do `/public` z każdym [budowaniem](#build) dla plików, które nie muszą być przetwarzane przez Gatsby, ale muszą istnieć w [public](#public).

### [Static Site Generator](/docs/glossary/static-site-generator)

A software application that creates HTML pages from templates or [components](#component) and a given content source.

## T

### Template

[Komponent](#component), który jest [programowo](#programmatically) zamieniany w stronę przez Gatsby.

### Theme

Motyw Gatsby jest podobny do motywu WordPress, który można komponować (z innymi motywami), rozszerzać (dodając więcej logiki) i zastępować ([shadowing](/blog/2019-04-29-component-shadowing/)). Motywy Gatsby mogą zawierać w sobie dowolny aspekt aplikacji Gatsby, a także oferować dowolną liczbę funkcji, które można następnie włączać i wyłączać.

### Transformer

[Wtyczka] (#plugin), która przekształca jeden typ danych w inny. Na przykład możesz przekształcić arkusz kalkulacyjny w tablicę [JavaScript](#javascript).

## U

### UI

Skrót UI odnosi się do interfejsu użytkownika. W dziedzinie interakcji człowiek-komputer interfejs użytkownika to przestrzeń, w której zachodzą interakcje między ludźmi a maszynami. Celem tej interakcji jest umożliwienie skutecznego działania i sterowania maszyną z ludzkiego punktu widzenia, podczas gdy maszyna w tym samym czasie zwraca informacje, które pomagają w podejmowaniu decyzji przez użytkownika (takie jak komunikaty o błędach lub powiadomienia).

## V

## W

### [webpack](/docs/glossary/webpack)

[Javascriptowa](#javascript) aplikacja, z której korzysta Gatsby aby spakować kod Twojej strony. Dzieje się to automatycznie podczas [budowania](#build).

### [WPGraphQL](/docs/glossary/wpgraphql)

A WordPress plugin that adds [GraphQL](#graphql) capabilities to WordPress. It's another way that you can use WordPress as a content source for Gatsby.

## X

## Y

### Yarn

Manager [paczek](#package), z którego część użytkowników korzysta zamiast [NPM](#npm). Jest on wymagany do [rozwijania Gatsby](/contributing/setting-up-your-local-dev-environment/#using-yarn).

## Z
