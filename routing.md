# Routing

## What is React Router?

We use React Router to handle routing in our applications. From the [React Router GitHub README](https://github.com/reactjs/react-router) file:

> React Router is a complete routing library for React.

## Why do we use it?

At the highest level we use React Router because it is the de-facto routing library for React, which comes with the benefit of huge community support and strong documentation. The JSX syntax and relatively simple API also make React Router easy to pick up and understand for new hires.

We’ve found React Router to be flexible to our needs in supporting everything from simple apps (that need minimal client-side routing) to isomorphic apps (that make use of server-side rendering). React Router also supports all of the same browsers and environments that React supports, making it a reliable and stable choice.

## What would make us switch to a different library?

React Router currently gives us the following features and we would expect new routing library candidates to do the same:

- Handle cross-browser inconsistencies with the native HTML5 History API
- Solid browser support (at least the same as supported by React itself)
- Support for server-side rendering
- Pre/Post transition hooks that allow for user authentication logic
- Support for route transition animations
- Play nice with Redux
- Play nice with Mocha

## JavaScript Guild Open discussions

### State: How much belongs in the URL?

How does React Router fit in the big picture alongside Redux? React Router naturally owns the state derived from the URL, but this is in conflict with the “single state tree” style that Redux offers and presents some challenges, [see this article](https://formidable.com/blog/2016/07/11/let-the-url-do-the-talking-part-1-the-pain-of-react-router-in-redux/).

### App structure

We’re currently in the process of defining how we structure applications that use React Router. For now it’s best to follow the [examples on the React Router docs](https://github.com/reactjs/react-router/tree/master/examples) as a starting point.

Options: https://gist.github.com/bradparker/679ad96aac848ca916facc450a8f91dc

## Examples of usage

- **Boiler Room Runner**
  - Client-side and server-side rendering
  - [https://github.com/everydayhero/boiler-room-runner/blob/master/source/client/index.js](https://github.com/everydayhero/boiler-room-runner/blob/master/source/client/index.js)
  - [https://github.com/everydayhero/boiler-room-runner/blob/master/source/server/index.js](https://github.com/everydayhero/boiler-room-runner/blob/master/source/server/index.js)
- **Eventbrite**
  - Small app using client-side rendering
  - [https://github.com/everydayhero/eventbrite/blob/master/src/application.js](https://github.com/everydayhero/eventbrite/blob/master/src/application.js)
  - [https://github.com/everydayhero/eventbrite/blob/master/src/components/Router/index.js](https://github.com/everydayhero/eventbrite/blob/master/src/components/Router/index.js)
- **Nexus**
  - Large app using client-side rendering, Rails back-end
  - [https://github.com/everydayhero/nexus/blob/master/ui/src/components/Router/index.js](https://github.com/everydayhero/nexus/blob/master/ui/src/components/Router/index.js)
  - [https://github.com/everydayhero/nexus/blob/master/ui/src/application.js](https://github.com/everydayhero/nexus/blob/master/ui/src/application.js)
