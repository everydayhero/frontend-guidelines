# Styles

## Pain points

- Diverging styles between projects (or even on the same project)
- Too little reuse
- Code bloat (Are these styles still being used elsewhere?)
- CSS & preprocessors lack power of “real” code
- Weird class name DSLs to avoid name conflicts (Global CSS)
- Dependency management
- Dead code elimination (80% of the styles aren’t being used by this page)
- Specificity problems
- Overriding default component styles

## Elements of Good CSS

A key to maintainable CSS is…

> As little CSS as possible

In order to achieve this, these are some principles to follow:


## Reset default styles to all look as similar as possible

Make everything look like basic text and inherit as many properties as appropriate from `root`. This allows for very little "reseting" on a per component basis, meaning the CSS that is used on a component should be as little as possible. It also helps you think about how much you are able to inherit.

To do this we use [minimal.css](https://www.npmjs.com/package/minimal.css). Install it with yarn/npm and include it at the root of your js with `import 'minimal.css'` . Using it this way will also ensure it is only included once if dependent components also import it.


## Layout

**Keep Margins off a Components Root**

To make components as flexible as possible, they should be able to be placed in an environment and let the parent decide how they are laid out. They margin can either be passed in as a prop or controlled with a parent selector.


    .parent > * + * {
        margin-top: rhythm(1)
    }

This is one of the only times a child selector should be used. It should always be a direct descendent. This avoids overly scoped selectors.


## Sizing (It’s All Relative)

- **Almost never use px values** **(1px borders are the only exception)**
- Default to `rems`
- Use `ems` for breakpoints (Because [reasons](https://zellwk.com/blog/media-query-units/))
- Use `ems` if you want to adjust a components size using the components root font size
- When doing layout (widths, heights) use percentages or viewport units
- When using rems or ems *nearly* always base it on a size() value in our traits (powers of 2)


## Write As Little CSS As Possible

**or Don't Over-engineer It**

These are some features of CSS that will help you write terse, yet flexible CSS:


- Inheritance
- currentColor
- Opacity
- Flexbox
- Box-shadow
- Relative sizes

Here is an example of refactoring (and simplifying) a component, by using these features.

Before: http://codepen.io/lukebrooker/pen/gMMEMB
After: http://codepen.io/lukebrooker/pen/mEEoBQ


## Avoid the Cascade

By keeping the specificity of your classes low, the less you need to worry about the order of these classes or why something is being overridden.


## Codify Design Constraints (Traits)

This gives each new design a coherence with the rest of the system. Traits can be broken down into **values** and **effects**.

### Values

Any value that does not have a limited set of values in CSS **should be given a limited set**. Here are some examples of values:

- Type scale (Based on a modular scale)
  - e.g.
- Sizes
  - size() based on exponents of 2. e.g. 2, 4, 8, 16, 32
  - percentages (i.e. grid width)
- Colors
  - Standard set of colours
  - Opacity levels (This should be the default way to lighten a colour)
  - Colour functions that limited to specific amounts of tint and shade when it’s not possible to use opacity.
- Mesure
  - A small set of ideal line-lengths for optimum legibility
- Leading
- Font Weight
- z-indexes
  - A single place to keep track of all z-indexes across a project
- Shadows
- Borders
- Transitions

### Effects

These are used to consolidate a sets of values and styles that represent a single “style”. Think of them like a set of layer effects in Photoshop or Sketch. Here are some examples of effects:

- Type Treatments
  - e.g. scale, leading, weight together in one composed effect
- Ellipsis
- Stretched Box
- Animations

All of these can be kept in a separate "traits" file or module and referenced by each component. There is an npm module “rug” in the works that will include a standard set of traits for all everydayhero projects.

> …really tied the room together, did it not?

## CSS in JS

Prefer all CSS be written in Javascript. This solves many of the pain points of CSS [written above](https://paper.dropbox.com/doc/Front-End-Guidelines-r5f5s2OK9p9SgNsZHs8Mg#:uid=765148768&h2=Pain-points). Our current tool of choice is [cxsync](https://www.npmjs.com/package/cxsync), it’s not perfect but it’s closest to the API we want for CSS in JS.

### Advantages of CSS in JS

- All the power of JS in CSS, no limited pre-processors
- With React we now have co-located HTML, javascript **and styles**.
- Keeps components self contained
- No new class name DSLs
- CSS stays local to the component
- https://twitter.com/rauchg/status/802328362341384192

### Example (WIP)

    import {
      color,
      opacity,
      radius,
      scale,
      shadow,
      size
    } from 'rug'

    const styles = {
      button: {
        fontSize: scale(1),
        borderRadius: size(4)
        backgroundColor: color.green,
        color: color.lightest,
        padding: `${size(2)} ${size(4)}`,
        shadow: shadow[2]
        ':hover': {
          opacity: opacity.lightest
        }
      }
    }

## Tools

- A reset - [minimal.css](https://www.npmjs.com/package/minimal.css) (Details included in the readme and demo site)
- A traits library [rug](https://www.npmjs.com/package/@everydayhero/rug)
- CSS in JS - [stranger](https://www.npmjs.com/package/@everydayhero/stranger)

*WIP (To be continued)*

## Further Reading

- [Minimising CSS Complexity](https://www.youtube.com/watch?v=EqOCcpH0caQ)
- [Mathematical Web Typography](http://jxnblk.com/writing/posts/mathematical-web-typography/) (Lots of reasoning around why I have scale & size function)
- [Scalable CSS](http://mrmrs.io/writing/2016/03/24/scalable-css/)
- [Patterns for Style Composition in React](http://jxnblk.com/writing/posts/patterns-for-style-composition-in-react/)
