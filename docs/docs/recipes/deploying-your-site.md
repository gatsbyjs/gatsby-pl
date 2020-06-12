---
title: "Przepisy: Wdrożenie twojej strony"
tableOfContentsDepth: 1
---

Czas na przedstawienie. Jeśli jesteś już zadowolony ze swojej strony, jesteś również gotowy by pokazać ją światu!

## Przygotowanie do wdrożenia

### Wymagania

- Strona [Gatsby](/docs/quick-start)
- Zainstalowany [Gatsby CLI](/docs/gatsby-cli)

### Wskazówki

1. Jeśli serwer deweloperski jest uruchomiony, zatrzymaj go (w większości przypadków wystarczy `Ctrl + C` w konsoli)

2. Dla standardowej ścieżki strony z głównym katalogiem (`/`), korzystając z Gatsby CLI uruchom w konsoli `gatsby build`. Zbudowane pliki będą się znajdowały w folderze `public`.

```shell
gatsby build
```

3. Aby zawrzeć na stonie inną ścieżkę niż `/` (jak np. `/tytul-strony/`), ustaw przedrostek dodając poniższy kod do swojego `gatsby-config.js` i zamień ścieżkę `yourpathprefix` na swoją własną:

```js:title=gatsby-config.js
module.exports = {
  pathPrefix: `/yourpathprefix`,
}
```

Istnieje kilka powodów by użyć -- ,dla przykładu hosting bloga zbudowanego za pomocą Gatsby na domenie z inną stroną, która nie jest zbudowana w Gatsby. Główna strona mogłaby przekierowywać do `przyklad.com`, a strona wykorzystująca Gatsby byłaby dostępna pod adresem `przyklad.com/blog`.

4. Jeśli ustawiłeś przedrostek w `gatsby-config.js`, uruchom `gatsby build` z flagą `--prefix-paths` aby automatycznie dodać przedrostek do początku adresu URL każdej ze stron oraz do tagów `<Link>`.

```shell
gatsby build --prefix-paths
```

5. Upewnij się, że twoja strona wygląda tak samo używając komendy `gatsby build` jak i `gatsby develop`. Kiedy strona została zbudowana uruchom `gatsby serve`, możesz teraz przetestować jej finalny efekt (i jeśli to potrzebne - poprawić) tuż przed upublicznieniem.

```shell
gatsby build && gatsby serve
```

### Dodatkowe materiały

- Przejdź przez samouczek dotyczący budowania i wdrażania przykładowej strony w [części drugiej](/tutorial/part-one/#deploying-a-gatsby-site)
- Dowiedz się więcej o [optymalizacji wydajności](/docs/performance/)
- Przeczytaj więcej o [innych możliwościach wdrażania strony](/docs/preparing-for-deployment/)
- Sprawdź [dokumenty wdrożeniowe](/docs/deploying-and-hosting/), aby odnaleźć więcej informacji na temat konkretnych platform hostingowych oraz jak wykorzystując je - wdrożyć stronę.

## Wdrożenie na platformie Netlify

Wykorzystaj [`netlify-cli`](https://www.netlify.com/docs/cli/), aby wdrożyć swoją aplikację Gatsby bez opuszczania interfejsu konsoli.

### Wymagania

- [Strona Gatsby](/docs/quick-start) z pojedynczym komponentem `index.js`
- Zainstalowana paczka [netlify-cli](https://www.npmjs.com/package/netlify-cli)
- Zainstalowane [Gatsby CLI](/docs/gatsby-cli)

### Wskazówki

1. Zbuduj swoją aplikację Gatsby uruchamiając `gatsby build`

2. Zaloguj się do Netlify wpisując w konsoli `netlify login`

3. Uruchom komendę `netlify init`. Wybierz opcję "Create & configure a new site".

4. Wpisz swoją nazwę strony lub jeśli chcesz kliknij enter aby otrzymać losowo wybraną.

5. Wybierz swój [zespół](https://www.netlify.com/docs/teams/).

6. Zmień ścieżkę wdrożeniową na `public/`

7. Upewnij się, że wszystko wygląda dobrze przed wdrożeniem strony na produkcję. Aby to zrobić uruchom `netlify deploy -d . --prod`

### Dodatkowe źródła

- [Hosting na Netlify](/docs/hosting-on-netlify)
- [gatsby-plugin-netlify](/packages/gatsby-plugin-netlify)

## Wdrożenie na platformie ZEIT Now

Wykorzystaj [Now CLI](https://zeit.co/download), aby wdrożyć swoją aplikację Gatsby bez opuszczania interfejsu konsoli.

### Wymagania

- Konto [ZEIT Now](https://zeit.co/signup)
- [Strona Gatsby](/docs/quick-start) z pojedyńczym komponentem `index.js`
- Zainstalowana paczka [Now CLI](https://zeit.co/download)
- Zainstalowane [Gatsby CLI](/docs/gatsby-cli)

### Wskazówki

1. Zaloguj się do Now CLI wykorzystując `now login`

2. Zmień katalog swojej aplikacji Gatsby w Terminalu jeśli jeszcze tam nie widnieje.

3. Uruchom `now` aby ją wdrożyć

### Dodatkowe źródła

- [Wdrożenie na ZEIT Now](/docs/deploying-to-zeit-now/)

## Wdrożenie na platformie Cloudflare Workers

Wykorzystaj [`wrangler`](https://developers.cloudflare.com/workers/tooling/wrangler/), aby wdrożyć swoją aplikację Gatsby bez opuszczania interfejsu konsoli.

### Wymagania

- Konto [Cloudflare](https://dash.cloudflare.com/sign-up)
- [Plan Workers Unlimited](https://developers.cloudflare.com/workers/about/pricing/) za \$5/miesięcznie aby włączyć KV, które jest wymagane aby serwować pliki Gatsby.
- [Strona Gatsby](/docs/quick-start) ze skonfigurowanym Gatsby CLI
- Zainstalowany globalnie(`npm i -g @cloudflare/wrangler`) [wrangler](https://developers.cloudflare.com/workers/tooling/wrangler/install/)

### Wskazówki

1. Zbuduj swoją aplikację Gatsby używając `gatsby build`
2. Uruchom `wrangler config`, zostaniesz poproszony o swój [Cloudflare API token](https://developers.cloudflare.com/workers/quickstart/#api-token)
3. Uruchom `wrangler init --site`
4. Skonfiguruj `wrangler.toml`. Na początku dodaj pole [account ID](https://developers.cloudflare.com/workers/quickstart/#account-id-and-zone-id) a następnie
   1. Darmową domenę workers.dev poprzez ustawienie `workers_dev = true`
   2. Niestandardową domenę na Cloudflare poprzez ustawienie `workers_dev = false`, `zone_id = "abdc..` oraz `route = przykladowadomena.com/*`
5. W pliku `wrangler.toml` ustaw `bucket = "./public"`
6. Uruchom `wrangler publish`, twoja strona zostanie opublikowania w ciągu kilku sekund!

### Dodatkowe źródła

- [Hosting na Cloudflare](/docs/deploying-to-cloudflare-workers)
