# Chromeless [![npm version](https://badge.fury.io/js/chromeless.svg)](https://badge.fury.io/js/chromeless)

Chrome automation made simple. Runs locally or headless on AWS Lambda. (**[See Demo](https://chromeless.netlify.com/)**)

## Chromeless can be used to...

* Run 1000s of **browser integration tests in parallel** ⚡️
* Crawl the web & automated screenshots
* Write bots that require a real browser
* *Do pretty much everything you've used __PhantomJS, NightmareJS or Selenium__ before*

### Examples

* [prep](https://github.com/graphcool/prep): Compile-time prerendering for SPA/PWA (like React, Vue...) instead of server-side rendering (SSR)
* See the full [examples list](/examples) for more

## ▶️ Try it out

You can try out Chromeless and explore the API in the browser-based **[demo playground](https://chromeless.netlify.com/)**

[![](http://i.imgur.com/i1gtCzy.png)](https://chromeless.netlify.com/)

## How it works

With Chromeless you can control Chrome (open website, click elements, fill out forms...) using an [elegant API](#api). This is useful for integration tests or any other scenario where you'd need to script a real browser.

### There are 2 ways to use Chromeless

1. Running Chrome on your local computer
2. Running Chrome on AWS Lambda and control it remotely

![](http://imgur.com/2bgTyAi.png)

### 1. Local Setup

For local development purposes where a fast feedback loop is necessary, the easiest way to use Chromeless is by controlling your local Chrome browser. Just follow the [usage guide](#usage) to get started.

### 2. Remote Proxy Setup

You can also run Chrome in [headless-mode](https://developers.google.com/web/updates/2017/04/headless-chrome) on AWS Lambda. This way you can run multiple tests (or other code) at the same time.

## Installation
```sh
npm install chromeless
```

### Remote Setup

The project contains a [Serverless](https://serverless.com/) service for running and driving Chrome remotely on AWS Lambda.

1. Deploy The RemoteChrome service to AWS Lambda. More details [here](https://github.com/graphcool/chromeless/tree/master/serverless#setup)
2. Follow the setup instructions [here](https://github.com/graphcool/chromeless/tree/master/serverless#remotechrome).

```js
const chromeless = new Chromeless({
  remote: {
    endpointUrl: 'https://XXXXXXXXXX.execute-api.eu-west-1.amazonaws.com/dev'
    apiKey: 'your-api-key-here'
  },
})
```

## Usage
```js
const Chromeless = require('chromeless')

async function run() {
  const chromeless = new Chromeless()

  const screenshot = await chromeless
    .goto('https://www.google.com')
    .type('chromeless', 'input[name="q"]')
    .press(13)
    .wait('#resultStats')
    .screenshot()

  console.log(screenshot) // prints local file path or S3 url

  await chromeless.end()
}

run().catch(console.error.bind(console))
```

### Local chrome usage

```sh
alias canary="/Applications/Google\ Chrome\ Canary.app/Contents/MacOS/Google\ Chrome\ Canary"
canary --remote-debugging-port=9222 --disable-gpu http://localhost:9222
canary --remote-debugging-port=9222 --disable-gpu http://localhost:9222 --headless
```

### API

**Chromeless methods**
- [`end()`](#api-end)

**Chrome methods**
- [`goto(url: string)`](#api-goto)
- [`click(selector: string)`](#api-click)
- [`wait(timeout: number)`](#api-wait-timeout)
- [`wait(selector: string)`](#api-goto-selector)
- [`wait(fn: (...args: any[]) => boolean, ...args: any[])`](#api-wait-fn)
- [`wait(firstArg, ...args: any[])`](#api-wait-first-arg)
- [`focus(selector: string)`](#api-focus)
- [`press(keyCode: number, count?: number, modifiers?: any)`](#api-press)
- [`type(input: string, selector?: string)`](#api-type)
- [`back()`](#api-back) - Not implemented yet
- [`forward()`](#api-forward) - Not implemented yet
- [`refresh()`](#api-refresh) - Not implemented yet
- [`mousedown()`](#api-mousedown) - Not implemented yet
- [`mouseup()`](#api-mouseup) - Not implemented yet
- [`scrollTo(x: number, y: number)`](#api-scrollto)
- [`viewport(width: number, height: number)`](#api-viewport)
- [`evaluate<U extends any>(fn: (...args: any[]) => void, ...args: any[])`](#api-evaluate)
- [`inputValue(selector: string)`](#api-inputvalue)
- [`exists(selector: string)`](#api-exists)
- [`screenshot()`](#api-screenshot)
- [`pdf()`](#api-pdf) - Not implemented yet
- [`cookiesGet()`](#api-cookiesget)
- [`cookiesGet(name: string)`](#api-cookiesget-name)
- [`cookiesGet(query: CookieQuery)`](#api-cookiesget-query) - Not implemented yet
- [`cookiesGetAll()`](#api-cookiesgetall)
- [`cookiesSet(name: string, value: string)`](#api-cookiesset)
- [`cookiesSet(cookie: Cookie)`](#api-cookiesset-one)
- [`cookiesSet(cookies: Cookie[])`](#api-cookiesset-many)
- [`cookiesSet(nameOrCookies, value?: string)`](#api-cookiesset-nameorcookies)
- [`cookiesClear(name: string)`](#api-cookiesclear)
- [`cookiesClearAll()`](#api-cookiesclearall)


---------------------------------------

<a name="api-goto" />

### goto(url: string)

Navigate to a URL.

__Arguments__
- `url` - URL to navigate to

__Example__





- goto(url: string)
- click(selector: string)
- wait(timeout: number)
- wait(selector: string)
- wait(fn: (...args: any[]) => boolean, ...args: any[])
- wait(firstArg, ...args: any[])
- focus(selector: string)
- press(keyCode: number, count?: number, modifiers?: any)
- type(input: string, selector?: string)
- back() - Not implemented yet
- forward() - Not implemented yet
- refresh() - Not implemented yet
- mousedown() - Not implemented yet
- mouseup() - Not implemented yet
- scrollTo(x: number, y: number)
- viewport(width: number, height: number)
- evaluate<U extends any>(fn: (...args: any[]) => void, ...args: any[])
- inputValue(selector: string)
- exists(selector: string)
- screenshot()
- pdf() - Not implemented yet
- cookiesGet()
- cookiesGet(name: string)
- cookiesGet(query: CookieQuery) - Not implemented yet
- cookiesGetAll()
- cookiesSet(name: string, value: string)
- cookiesSet(cookie: Cookie)
- cookiesSet(cookies: Cookie[])
- cookiesSet(nameOrCookies, value?: string)
- cookiesClear(name: string)
- cookiesClearAll()
- end()


## FAQ

### How is this different from [NightmareJS](https://github.com/segmentio/nightmare), PhantomJS or Selenium?

### I'm new to AWS Lambda, is this still for me?

### How much does it cost to run Chromeless in production?

### Are there any limitations?

If you're running Chromeless on AWS Lambda, the execution cannot take longer than 5 minutes which is the current limit of Lambda. Besides that, every feature that's supported in Chrome is also working with Chromeless.

## Contributors

A big thank you to all contributors and supporters of this repository 💚

<a href="https://github.com/adieuadieu/" target="_blank">
  <img src="https://github.com/adieuadieu.png?size=64" width="64" height="64" alt="adieuadieu">
</a>
<a href="https://github.com/schickling/" target="_blank">
  <img src="https://github.com/schickling.png?size=64" width="64" height="64" alt="schickling">
</a>
<a href="https://github.com/timsuchanek/" target="_blank">
  <img src="https://github.com/timsuchanek.png?size=64" width="64" height="64" alt="timsuchanek">
</a>
<a href="https://github.com/matthewmueller/" target="_blank">
  <img src="https://github.com/matthewmueller.png?size=64" width="64" height="64" alt="matthewmueller">
</a>


## Credits

* [chrome-remote-interface](https://github.com/cyrus-and/chrome-remote-interface): Chromeless uses this package as an interface to Chrome
* [serverless-chrome](https://github.com/adieuadieu/serverless-chrome): Compiled Chrome binary that runs on AWS Lambda (Azure and GCP soon, too.)
* [NightmareJS](https://github.com/segmentio/nightmare): We draw a lot of inspiration for the API from this great tool


## Help & Community [![Slack Status](https://slack.graph.cool/badge.svg)](https://slack.graph.cool)

Join our [Slack community](http://slack.graph.cool/) if you run into issues or have questions. We love talking to you!

![](http://i.imgur.com/5RHR6Ku.png)
