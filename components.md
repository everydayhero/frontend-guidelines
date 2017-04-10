# Components

## What is a component?

Components describe a discrete portion of the UI. Components colocate all their unique requirements within a single directory (behaviour and display logic, tests, styles, localisations, images, etc). Components allow us to compose interfaces from modular, often re-usable, pieces, increasing consistency across our applications and decreasing development time/complexity.

## Types of components

While the terminology is still all over the place, our applications care primarily about three distinct types of components: Stateless View, Stateful View, and Store Adaptor, defined primarily by their implementation/relationship with UI and application state.

- **Stateless View**: Have no internal state, and their API is purely received props. They can transform and pass these props to child components. These do the vast majority of the work of building your UI and its interactions. These are purely functional components, defined as a render function and propType declaration (`render = (props) => {}` ). By far the most common component type.
  - Example of a checkbox input which can display a label, hint tooltip, and error message:
    import React from 'react'
    import Errors from '../InputErrors'
    import Tooltip from '../../atoms/Tooltip'
    import css from 'cxsync'
    import styles from './styles'

    const DEFAULT_NAME = 'checkbox'

    const Label = ({
      id,
      label,
      clickableLabel
    }) => (
      <label
        htmlFor={clickableLabel && (id || name || DEFAULT_NAME)}
        className={css(styles.label)}>
        {label}
      </label>
    )

    const Input = ({
      id,
      name,
      disabled,
      value: checked,
      onBlur,
      onChange
    }) => (
      <input
        checked
        disabled
        id={id || name || DEFAULT_NAME}
        name={name || id || DEFAULT_NAME}
        className={css(styles.input)}
        type='checkbox'
        onBlur={({target: {checked}}) => onBlur(checked)}
        onChange={({target: {checked}}) => onChange(checked)}
        autoComplete='off'
      />
    )

    export default ({
      id,
      name,
      disabled = false,
      clickableLabel = true,
      onChange = () => {},
      onBlur = () => {},
      value = false,
      label = '',
      hint = '',
      errors = []
    }) => (
      <div className={css(styles.checkbox, !!errors.length && styles.error)}>
        {!!hint && <Tooltip className={css(styles.hint)} content={hint} />}
        <Input
          id={id}
          name={name}
          disabled={disabled}
          value={value}
          onBlur={onBlur}
          onChange={onChange}
        />
        <Label
          id={id}
          label={label}
          clickableLabel={clickableLabel}
        />
        {!!errors.length && <Errors errors={errors} />}
      </div>
    )
  - Patterns worth noting:
    - No state. This component is a pure functional transform of provided data.
    - Destructuring object arguments (`props` , for components, or `events` for event handlers) both clearly defines the shape of the object that is required, and effectively filters out extraneous data so that it can go no further, making it impossible to invisibly pass data through multiple intermediary children.
    - Render complex child nodes (input and label, here) with separate functions in the same file. No need to extract these into their own components until it makes sense (no premature abstraction).
    - Always return JSX, and where there are 3 or more props being passed to a child, or the props logic is long/complex, split each prop onto its own line for readability.
    - All inputs have a consistent API, so even though this is a checkbox, its `checked` status is passed down as `value` . Similarly, `id`s and `name`s fallback to sensible defaults if not provided. This makes it **much** easier to manage the state and data flow of complex forms because you don’t have to implement different state serializers and i18n conventions for each input type.
    - Defaults props/args are specified always and only at the component boundary.
    - *One thing missing from this example is PropTypes. It would be simple to append them separately, but it feels redundant and clunky. I’d like to explore how Flow might be used to Type the component’s props inline with their destructuring/defaults.*
- **Stateful View**: Have idiomatic internal (local) state. This state *cannot* be important for the application, and must be purely concerned with UI state which we we can afford to lose. These are responsible for non-critical display branching (such as showing and hiding menus), and are the rarest types of component. Can also receive props, and implemented via `class X extends Component` method (not `React.createClass()` ). Rare as hen’s teeth.
- **Store Adaptor**: Connect a Stateless View or Stateful View to the application state. Do not implement their own behaviour or display logic. Pass scoped app state as props to children. These are responsible for connecting major sections of the UI to the central application state, and serve as the entry point where actions are injected as props to allow child components restricted access to manipulate that state (eg: `StoreAdaptedComponent = connect(mapStateToProps, mapDispatchToProps)(Component)` ). Orchestrate data flow and so are more common the more deeply nested and complex a page/route becomes.

## Naming conventions

- Components themselves are named as nouns describing the UI concept they render. Examples include `Input` , `RadioGroup` , `HeaderImage` , `BannerSlideShow`, etc.
- Instance methods (and helper functions for Stateless Views) have intention-revealing names, usually adhering to a few simple conventions:
  - `handleEventName()` : perform the logic to process a user initiated event of any kind
  - `submitDataName()` : post data to some external receiver
  - `renderUIName()`: helper methods to either conditionally render portions of the component’s UI, manage complex logic that would convolute the component’s `render()` method, or render multiple similar components (like inputs)
- Props are named using DOM conventions, so you’d pass a `handleChange` method to the `onChange` prop of a child input, or a `submitForm` method to the `onClick` prop of a child button.


  - How to best use existing

Before creating a new component, check HUI and the rest of your App to ensure there isn’t an existing component which fulfills all/most of your requirements. If such a component exists, use it, even if you need to serialize your data to do so (or consider restructuring how the data flows to this point in a way that would avoid the need for further serialization).

Before using an existing component, review its interface (defined by its `propTypes` ), which define required and optional props, and their types/shapes. Ensure that you are always passing all required props (React will throw a fit if you don’t, but not until you try to render it).

Only ever pass accepted props to child components. Use the spread operator if necessary to extract extraneous data `const { extraneousProp, …rest } = props`. Extraneous props don’t cause any harm, but they make it harder to trace data flow. Be Explicit Always.


  - How to update existing

In the case where you need some partial functionality that isn’t provided by an otherwise useful existing component, evaluate whether your use functionality fits any of the following cases:

- Where does this functionality logically belong (whose concern is it, really)?
- How complex is this functionality?
- How much of the existing component’s functionality suits your use-case (how much overlap is there between what you have and what you need)?
- Is it likely that other applications/consumers will benefit from this new functionality?

Then consider the options (in descending order of preference):

- Implement functionality in parent component
- Create a higher-order-component to wrap the existing component with the desired functionality
- Include this new functionality into the existing component
- Create a new similar component explicitly to address this use-case specifically

In all cases, consider how this change will affect other consumers, and whether it is a breaking change.

  - When to add new

## Things we don’t do

- Define components with `React.createClass()`
- Use mixins
- Synchronise props and state within a Stateful View component
- Use Stateful View components when there is any hint that anything else might care about that state
- Over-use Store Adaptors: these make sense at major junctions in the UI (like forms, pages, navigation), but are ridiculous if used to wrap individual components (like inputs, links, buttons)
- Use context directly. Not even to pass down state/actions to deeply nested children: this is a perfect situation for a Store Adaptor. Context is poorly documented and subject to change, is often abused when it isn’t remotely necessary, and obfuscates data flow.

## Things we avoid where we can but will sometimes resort to until we find a better solution

- Pass/accept className as a prop: this essentially expands the interface of the component to include *every single css property.*
- Pass/accept a style object as a prop: same reason as above
- Use component lifecycle methods. These aren’t bad *per se* but are often used in place of more elegant solutions or to patch over a data-flow problem. Things like binding/unbinding eventListeners are often better and more logically managed through actions and app state (since they are, almost exclusively, global). Animations/Transitions are more difficult but most often better encapsulated within components that focus exclusively on that functionality (which also brings re-usability). shouldComponentUpdate is nearly always an unnecessary premature performance optimisation, that makes more sense (and gives the biggest benefit) to confine to the domain of Store Adaptor components anyway (which is something redux connect() does for us). componentDidMount() is perhaps the least egregious, particularly when dealing with server-side-rendering.

## Things we do

- Define the total interface surface area for all components. If a component renders children with specific prop requirements, those requirements are explicitly declared on the parent component. Nobody should ever have to look deep into the dependency tree to determine all the props required to successfully render a single component.
- Test for the UI result. Assert that text passed to a component appears in the rendered html, as opposed to testing for the presence of the nested component it was passed through to. Assert that event handlers are called with the correct args through simulated interactions with the rendered dom, as opposed to manually calling methods on the component instance. Assert that presentational props result in the correct classes being applied to the correct elements, with no other extraneous classes (way easier with something like CXS).
- Gardening as we go. If you see a component which specifies an unused prop, remove it from propTypes. Conversely for a component using an un-declared prop. Clean up syntax. Refactor to use helper methods (lodash, etc). Fix deprecation warnings. Pull state out into the app store and refactor into a Store Adaptor around a Stateless View. If we clean our code as we use it, our codebase stays fresh and lovely. If we never touch anything except the features we are building, it gets dusty and mouldy.
- Write our JSX as similar as possible to raw HTML. We use named html entities (`&hellip;` `$amp;` `&nbsp;` ) and not unicode characters, string variables instead of literals (`<p>this is a string</p>` and not `<p>{'this is a string'}</p>` ), and only break out of “html-ese” when explicitly necessary for executing/referencing javascript (`<button onClick={handleClick}>{t('namespace.button_text')}</button>)` ).
