---
title: API plików
---

Gatsby używa 4 plików w głownym katalogu projektu do konfiguracji twojej strony oraz kontroli tego jak się zachowuje. Wszytskie z tych plików są opcjonalne.

- [gatsby-config.js](/docs/api-files-gatsby-config) - Pozwala na użycie pluginów, definiuje typowe dane dla strony oraz zawiera inne ustawienia, które pozwalają integrować warstwę danych GraphQL
- [gatsby-browser.js](/docs/api-files-gatsby-browser) - Pozwala kontrolować to jak Gatsby zachowuje się w przeglądarce. Na przykład: reagowanie na zmiane podstrony przez użytkownika, lub wywoływanie funkcji gdy użytkownik wejdzie na jakąkolwiek stronę.
- [gatsby-node.js](/docs/api-files-gatsby-node) - Pozwala reagować na zdarzeneia w procesie budowania strony Gatsby. Dla przykładu: dodawanie stron dynamicznie, edytowanie node-ów GraphQL gdy zostały zaciągnięte, wywoływanie akcji gdy budowanie się zakończy.
- [gatsby-ssr.js](/docs/api-files-gatsby-ssr) - Ujawnia proces renderowania po stronie serwera w Gatsby, abyś mógł kontrolować, w jaki sposób tworzy strony HTML.
