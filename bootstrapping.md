# Bootstrapping/Build

## App structure, building, etc.

We are moving towards using Webpack to bundle and compile the source for our JS apps. From the [Webpack docs](http://webpack.github.io/docs/what-is-webpack.html):

> Webpack is a module bundler.
>
> Webpack takes modules with dependencies and generates static assets representing those modules.

We use Webpack because of it’s focus on treating static assets and JS modules as declarative dependencies. Images, CSS and JS modules are all part of the dependency graph of your application.

Webpack has a growing and active community of contributors as well a large eco-system of [plugins](http://webpack.github.io/docs/list-of-plugins.html) and [loaders](http://webpack.github.io/docs/list-of-loaders.html) which fulfil many front-end build requirements.

Another key advantage is the developer experience, with supporter for a watch mode with incremental rebuilding and a dev server with an in-memory cache of your project’s compilation.

Web pack configuration management can be challenging, in response to this [many tools](https://github.com/facebookincubator/create-react-app#alternatives) have been developed around it to help with co-ordination of dependant options and synchronisation of development vs production output and behaviour.

We have our own tool which is specifically geared to work as a static site renderer (for use in Professional services landing pages), as well as a client-side application build-pipeline.

https://github.com/everydayhero/boiler-room-builder

### Example application setups

[Refer to the examples directory in BRB](https://github.com/everydayhero/boiler-room-builder/tree/master/examples)

[Event Log viewer UI setup](https://github.com/everydayhero/kanga/commit/431e7194f4e5b46dd82d7678de8f83b8e8eea6e7)

[Warp-gate converting from create react app to BRB](https://github.com/everydayhero/warp-gate/pull/28)

[Quite a ‘complete’ setup for a professional services site](https://github.com/everydayhero/strava-challenge/commit/29d748aea2d7f30f2966cb7d4eac3a45f51f9832)

## Tools

We’re currently in the process of defining how we structure applications. For now it’s best to follow the [examples on the React Router docs](https://github.com/reactjs/react-router/tree/master/examples) as a starting point.

Options: https://gist.github.com/bradparker/679ad96aac848ca916facc450a8f91dc

### Boilermaker (BRM)

BRM is a tool to generate app structure which follows these guidelines. This is available at https://github.com/everydayhero/boilermaker.
