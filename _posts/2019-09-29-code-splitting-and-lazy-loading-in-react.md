---
layout: post
title: Code Splitting & Lazy Loading a Server Side Rendered React app
tags:
  - react
  - code splitting
  - lazy loading
---

## Reasoning, approach, and goals

### Goals

1. Faster intitial load times. Users only download the code they need for the features they are using. This leads to fast load times and more efficient use of resources.
2. Faster subsequent loads. Code splitting enables more efficient caching. Developers can now update isolated parts of the app without forcing users to redownload the entire application code.

### Approach

My initial approach is going to be to agressively split all of my components and node_modules. This will result in 10s if not 100s of js files, which is okay due to HTTP/2 enabling parallel loading of multiple files over the same connection. Gone are the days of round trips to your server for each file.

This approach is not the "100% most optimal" however, because there are other things to consider. Compression is better on larger files, and there is still some downsides to serving many smaller files. [This article](https://medium.com/webpack/webpack-http-2-7083ec3f3ce6) is a good summary.

## The library

**Use [@loadable/components](https://github.com/smooth-code/loadable-components)**. This library is recomended by the React team, is actively maintained, and supports all the features we need (namely SSR).

[react-loadable](https://github.com/jamiebuilds/react-loadable) is no longer maintained, despite the myraid of tutorials that exist for it. I started implementing this lib before realizing this.

There are [other options](https://itnext.io/react-code-splitting-in-2019-9a5d2776c502) and other ways to optimize your app (worth learning about, but outside the scope of this post).

## Install & config

We'll need 4 packages.

```
npm i -S @loadable/component @loadable/babel-plugin
npm i -D @loadable/component @loadable/webpack-plugin
```

Configure the plugins.

1. Add `@loadable/babel-plugin` to `.babelrc`.

```json
{
  "plugins": [
    "@loadable/babel-plugin"
  ]
}
```

2. Add the Webpack plugin.
  I customized the loadable json output file name. This file is referenced on the server.

```js
const LoadablePlugin = require('@loadable/webpack-plugin')

new LoadablePlugin({ filename: 'loadable.json', writeToDisk: { filename: `${paths.serverBuild}` } })
```

## Server setup

_TODO: Link to full examples_

Extract the JavaScript chunks.

```js
// server/render.js
import { ChunkExtractor } from '@loadable/server'

const publicPath = process.env.NODE_ENV === 'production' ? `${paths.cdn}/build/` : paths.publicPath
const statsFile = process.env.NODE_ENV === 'production'
  ? path.resolve('build/server/loadable.json')
  : `${paths.cloudFunctions}/build/server/loadable.json`

const extractor = new ChunkExtractor({
  statsFile,
  entrypoints: ['bundle'],
  outputPath: paths.clientBuild,
  publicPath
})

const content = renderToString(
  extractor.collectChunks(
    sheet.collectStyles(
      <Provider store={req.store}>
        <Router location={req.url} context={{}}>
          { renderRoutes(routes) }
        </Router>
      </Provider>
    )
  )
)

const scriptTags = extractor.getScriptTags()
```

Add the script tags to your HTML.
```js
// server/components/HTML.js
<div dangerouslySetInnerHTML={{ __html: scriptTags }} />
```

## Client setup

```js
import { loadableReady } from '@loadable/component'

loadableReady(() => {
  hydrate(
    <Provider store={store}>
      <Router history={browserHistory}>
        <ScrollToTop>
          { renderRoutes(routes) }
        </ScrollToTop>
      </Router>
    </Provider>,
    document.getElementById('app')
  )
})
```

## Start code splitting

```js
import loadable from '@loadable/component'

export const MyComponent = loadable(() => import('./MyComponent'))
```

## Sources, further reading

- [Reduce JavaScript Payloads with Code Splitting](https://developers.google.com/web/fundamentals/performance/optimizing-javascript/code-splitting)
- [Webpack docs](https://webpack.js.org/guides/code-splitting/)
- [webpack & HTTP/2](https://medium.com/webpack/webpack-http-2-7083ec3f3ce6)
- [The Right Way to Bundle Your Assets for Faster Sites over HTTP/2](https://medium.com/@asyncmax/the-right-way-to-bundle-your-assets-for-faster-sites-over-http-2-437c37efe3ff)

## Things I still want to explore, questions I have

- Group / concatenate similar bundles to reduce overall number of requests.
- [AggressiveSplittingPlugin](https://github.com/webpack/webpack/tree/master/examples/http2-aggressive-splitting)
- Is it best to have a single entrypoint file for 3rd party libs (node_modules), so that you only have to implement @loadable in one place?
