<p align="center">
  <img src="https://i.imgur.com/J0jv1Sz.png" width="240" height="240" alt="critters">
  <h1 align="center">Critters</h1>
</p>

> Critters is a plugin that inlines your app's [critical CSS] and lazy-loads the rest.

## critters [![npm](https://img.shields.io/npm/v/critters.svg)](https://www.npmjs.org/package/critters)

It's a little different from [other options](#similar-libraries), because it **doesn't use a headless browser** to render content. This tradeoff allows Critters to be very **fast and lightweight**. It also means Critters inlines all CSS rules used by your document, rather than only those needed for above-the-fold content. For alternatives, see [Similar Libraries](#similar-libraries).

Critters' design makes it a good fit when inlining critical CSS for prerendered/SSR'd Single Page Applications. It was developed to be an excellent compliment to [prerender-loader](https://github.com/GoogleChromeLabs/prerender-loader), combining to dramatically improve first paint time for most Single Page Applications.

## Features

*   Fast - no browser, few dependencies
*   Integrates with Webpack [critters-webpack-plugin]
*   Supports preloading and/or inlining critical fonts
*   Prunes unused CSS keyframes and media queries
*   Removes inlined CSS rules from lazy-loaded stylesheets

## Installation

First, install Critters as a development dependency:

```sh
npm i -D critters
```

or

```sh
yarn add -D critters
```

## Simple Example

```js
import Critters from 'critters';

const critters = new Critters({
  // optional configuration (see below)
});

const html = `
  <style>
    .red { color: red }
    .blue { color: blue }
  </style>
  <div class="blue">I'm Blue</div>
`;

const inlined = await critters.process(html);

console.log(inlined);
// "<style>.blue{color:blue}</style><div class=\"blue\">I'm Blue</div>"
```

## Usage with webpack

Critters is also available as a Webpack plugin called [critters-webpack-plugin](https://www.npmjs.org/package/critters-webpack-plugin). [![npm](https://img.shields.io/npm/v/critters-webpack-plugin.svg)](https://www.npmjs.org/package/critters-webpack-plugin)

The Webpack plugin supports the same configuration options as the main `critters` package:

```diff
// webpack.config.js
+const Critters = require('critters-webpack-plugin');

module.exports = {
  plugins: [
+    new Critters({
+      // optional configuration (see below)
+    })
  ]
}
```

That's it! The resultant html will have its critical CSS inlined and the stylesheets lazy-loaded.

## Usage

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### Critters

All optional. Pass them to `new Critters({ ... })`.

#### Parameters

*   `options`  

#### Properties

*   `path` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Base path location of the CSS files *(default: `''`)*
*   `publicPath` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Public path of the CSS resources. This prefix is removed from the href *(default: `''`)*
*   `external` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Inline styles from external stylesheets *(default: `true`)*
*   `inlineThreshold` **[Number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** Inline external stylesheets smaller than a given size *(default: `0`)*
*   `minimumExternalSize` **[Number](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number)** If the non-critical external stylesheet would be below this size, just inline it *(default: `0`)*
*   `pruneSource` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Remove inlined rules from the external stylesheet *(default: `false`)*
*   `mergeStylesheets` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Merged inlined stylesheets into a single `<style>` tag *(default: `true`)*
*   `additionalStylesheets` **[Array](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array)<[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)>** Glob for matching other stylesheets to be used while looking for critical CSS.
*   `preload` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Which [preload strategy](#preloadstrategy) to use
*   `noscriptFallback` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Add `<noscript>` fallback to JS-based strategies
*   `inlineFonts` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Inline critical font-face rules *(default: `false`)*
*   `preloadFonts` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Preloads critical fonts *(default: `true`)*
*   `fonts` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Shorthand for setting `inlineFonts` + `preloadFonts`*   Values:
    *   `true` to inline critical font-face rules and preload the fonts
    *   `false` to don't inline any font-face rules and don't preload fonts
*   `keyframes` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Controls which keyframes rules are inlined.*   Values:
    *   `"critical"`: *(default)* inline keyframes rules used by the critical CSS
    *   `"all"` inline all keyframes rules
    *   `"none"` remove all keyframes rules
*   `compress` **[Boolean](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** Compress resulting critical CSS *(default: `true`)*
*   `logLevel` **[String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String)** Controls [log level](#loglevel) of the plugin *(default: `"info"`)*
*   `logger` **[object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)** Provide a custom logger interface [logger](#logger)
*   `additionalDataTags` **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)** Search for classes inside data tags. Example the value [animations] search for classes inside data-animations attributes *(default: `[]`)

### Logger

Custom logger interface:

Type: [object](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object)

#### Properties

*   `trace` **function ([String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String))** Prints a trace message
*   `debug` **function ([String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String))** Prints a debug message
*   `info` **function ([String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String))** Prints an information message
*   `warn` **function ([String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String))** Prints a warning message
*   `error` **function ([String](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String))** Prints an error message

### LogLevel

Controls log level of the plugin. Specifies the level the logger should use. A logger will
not produce output for any log level beneath the specified level. Available levels and order
are:

*   **"info"** *(default)*
*   **"warn"**
*   **"error"**
*   **"trace"**
*   **"debug"**
*   **"silent"**

Type: (`"info"` | `"warn"` | `"error"` | `"trace"` | `"debug"` | `"silent"`)

### PreloadStrategy

The mechanism to use for lazy-loading stylesheets.

Note: <kbd>JS</kbd> indicates a strategy requiring JavaScript (falls back to `<noscript>` unless disabled).

*   **default:** Move stylesheet links to the end of the document and insert preload meta tags in their place.
*   **"body":** Move all external stylesheet links to the end of the document.
*   **"media":** Load stylesheets asynchronously by adding `media="not x"` and removing once loaded. <kbd>JS</kbd>
*   **"swap":** Convert stylesheet links to preloads that swap to `rel="stylesheet"` once loaded ([details](https://www.filamentgroup.com/lab/load-css-simpler/#the-code)). <kbd>JS</kbd>
*   **"swap-high":** Use `<link rel="alternate stylesheet preload">` and swap to `rel="stylesheet"` once loaded ([details](http://filamentgroup.github.io/loadCSS/test/new-high.html)). <kbd>JS</kbd>
*   **"js":** Inject an asynchronous CSS loader similar to [LoadCSS](https://github.com/filamentgroup/loadCSS) and use it to load stylesheets. <kbd>JS</kbd>
*   **"js-lazy":** Like `"js"`, but the stylesheet is disabled until fully loaded.
*   **false:** Disables adding preload tags.

Type: (default | `"body"` | `"media"` | `"swap"` | `"swap-high"` | `"js"` | `"js-lazy"`)

## Similar Libraries

There are a number of other libraries that can inline Critical CSS, each with a slightly different approach. Here are a few great options:

*   [Critical](https://github.com/addyosmani/critical)
*   [Penthouse](https://github.com/pocketjoso/penthouse)
*   [webpack-critical](https://github.com/lukeed/webpack-critical)
*   [webpack-plugin-critical](https://github.com/nrwl/webpack-plugin-critical)
*   [html-critical-webpack-plugin](https://github.com/anthonygore/html-critical-webpack-plugin)
*   [react-snap](https://github.com/stereobooster/react-snap)

## License

[Apache 2.0](LICENSE)

This is not an official Google product.

[critters-webpack-plugin]: https://github.com/GoogleChromeLabs/critters/tree/main/packages/critters-webpack-plugin

[critical css]: https://www.smashingmagazine.com/2015/08/understanding-critical-css/

[html-webpack-plugin]: https://github.com/jantimon/html-webpack-plugin
