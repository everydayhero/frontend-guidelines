# React Component File Naming & Directory Structure

The naming of files and the moving of components into separate files has the intent of removing the noise commonly found in React component files.

### React components

Most components should have their own folder titled using Pascal Casing and have the component defined in the `index.js` file for that folder.

* The final component wrapped with any higher order components should be the default export
* The component without any higher order wrappings should be a named export for testing purposes

```
`HelloWorld/index.js` // Good

`HelloWorld.js`       // Bad for most components, ok for internal components... see below
`helloWorld.js`       // Bad for a React component, but good for a non React component JavaScript module
`hello-world.js`      // Bad
```

### Internal components

Private internal components used only in their parent component:

* Can be defined inline with their parent component's `index.js` file
* Can be moved to a separate file under their parent's directory

This is up to the discresion of the developers. If there is any code branching or significant lines of code (20+), then it probably is a good candidate to move to a separate file.

The goal is to eliminate noise in the parent component's file.

### Stylized Components using stranger

Stylized components created using `@everydayhero/stranger` should be moved to a separate file called `styles.js`.

* This module will then have multiple named exports
* This module will not have a default export

### Tests for Components

Tests for a component should be defined close to the component in a `__tests__` directory. The test file for a component should be the name of the component with a `-test.js` suffix.

```
__tests__/HelloWorld-test.js // Good

__tests__/index-test.js      // Bad
specs/HelloWorld-spec.js     // Bad
./HelloWorld--test.js        // Bad
```

### Example File and Directory

The resulting file and directory structure should look like:

```
HelloWorld/
├── Message.js
├── __tests__
│   ├── HelloWorld-test.js
│   └── Message-test.js
├── index.js
└── styles.js
```
