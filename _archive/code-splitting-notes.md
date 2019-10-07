I started as any senior developer should: Googling my problem.

> "react webpack code splitting"

I typed in to the search bar. Plenty of promising results popped up, and so I started sifting through the articles.

Many of the top hits like [this one](https://itnext.io/react-router-and-webpack-v4-code-splitting-using-splitchunksplugin-f0a48f110312) recommend using [react-loadable](https://github.com/jamiebuilds/react-loadable), so I did.

I learn best by doing, so I booted up one of my client projects, and started implementing react-loadable for some of the app's admin components - components that only admins should have to load. And it worked! I inspected the output of my webpack build, and saw my main bundle.js file size get smaller, and some new bundle.js files which contain the admin code.

It felt good to make progress, but I knew there was still more to do. Preloading, SSR, smartly separating out more chunks... I still had hours to go. Furthermore, there was a warning in my JavaScript console, stating that this library is using unsafe lifecycle methods, `componentWillMount` which will be deprecated in a future version of React.

I headed back to the Git repository to see if there was an open PR or issue filed to address this. To my surprise, the repo had no "Issues" tab. This confused me, so I clicked on the Pull Requests tab only to find out that this lirbray is no longer maintained!

> This project is dead.

[Source](https://github.com/jamiebuilds/react-loadable/pull/195#issuecomment-520439454)

[https://www.smooth-code.com/open-source/loadable-components/docs/loadable-vs-react-lazy/#note-about-react-loadable](https://www.smooth-code.com/open-source/loadable-components/docs/loadable-vs-react-lazy/#note-about-react-loadable)

Further down in the comments, I found suggestions to use React Suspense and React Lazy. Then I read that these native solutions don't support SSR. _Sigh_.

I then found a link to [loadable-components](https://github.com/smooth-code/loadable-components). Could this be what I'm looking for?

I replaced react-loadable without much friction. Following the readme, I implemented `const OtherComponent = loadable(() => import('./OtherComponent'))` and successfully saw webpack split this code into its own chunk. Now on to the hard stuff.

### SSR

I followed the [docs]()

`npm i -S @loadable/server && npm i -D @loadable/babel-plugin @loadable/webpack-plugin`

I had to add some configuration.

Looking at the example project was very helpful, especially [the main client App.js file](https://github.com/smooth-code/loadable-components/blob/master/examples/server-side-rendering/src/client/App.js).

You don't need `NamedModulesPlugin` anymore. See [this](https://github.com/webpack/webpack.js.org/issues/2279)

Use contenthash over chunkhash. See [this](https://github.com/webpack/webpack.js.org/issues/2096)

The chunk name is determined by webpack. I got rid of the component name because of the amount of characters.

If I have issues with HMR: https://github.com/smooth-code/loadable-components/issues/282#issuecomment-492162507


## Thoughts

Is it best to have a single entrypoint file for 3rd party libs (node_modules), so that you only have to implment @loadable in one place?
