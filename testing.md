# Testing

## Test plans and slices

Be very clear on what you are and are not testing. Split your testing out into tests slices and have an expressed plan for each. Think of the [test pyramid](http://martinfowler.com/bliki/TestPyramid.html).

For instance, a JavaScript project using React might have:

- Many unit tests which cover modules only. Mock as much as you like for dependencies
- Many component tests with a preference to selecting components by type (not class or id) where possible and asserting on those components. Assertions are made against text, labels and visibility, not by interrogating a components state. Only mock when you have to (API, DB, S3 etc)
- A few browser based integration tests along a handful of critical paths with a preference for making as few changes as possible (mock etc) to the code under test. Plan upfront how asynchronous operations like animations and modal display will be catered for.

In general our preference is for more unit tests and less integration tests. While being very valuable, integration tests are slower and more expensive in CI. So each integration test needs to be important and justified.

Remember that it’s OK to fail. If you realise something in test approach lead to a bug creeping through… review your plan, fix it and profit.

## Tools

- We use [Mocha](https://mochajs.org/) as a test framework for Node and in the browser
- We use [Chai](http://chaijs.com/) as an assertion library
  - We are careful when including Chai extensions. For instance, chai-as-promised was used in Postoffice but was found to only allow some of the language chains possible in Chai and, combined with poor documentation, turned out to be more of a burden than a help. See Testing asynchronously below.
- We like [Enzyme](https://github.com/airbnb/enzyme) to help working with React components
- We like [Sinon](http://sinonjs.org/) for spying, stubbing, mocking and matching
- We like what for browser integration tests?
- We like what for cross browser tests?

### Choosing tools

- Value expectation feedback. “Expectation failed” is useless. “Expected ‘banana’ but got ‘apple’” is helpful.

## Testing asynchronously

Testing asynchronously can be hard. It’s easy to get it wrong. Following the below approach should help you.

### Promises

With Mocha, to test a promise you expect to resolve

    it('waits for the promise to resolve and will fail if rejected', () => {
      return runPromise().then((result) => {
        expect(result).to.be.OK
      })
    })

With Mocha, to test a promise you expect to reject we use `done` to avoid the test passing in the event that the promise resolves.

    it('waits for the promise to reject and will fail with timeout if resolved', (done) => {
      runPromise().catch((err) => {
        expect(err).to.be.OK
        done()
      })
    })

Examples of this approach can be found in [Postoffice](https://github.com/everydayhero/postoffice/blob/master/tests/templates/tasks/supporter/recipient/user.js).

## Miscellaneous

- Set up code coverage reports as part of your dev or CI process. If you use `boiler-room-builder` you get `istanbul`  code coverage as batteries included.
- You do not need 100% code coverage. Developers can decide what does and does not need to covered.
- Include as few globals as possible in your unit tests. For instance, having lodash as a global will almost certainly lead to bugs.
- Be consistent in your assertions. If you like assert, use it everywhere in your slice. If you like expect, use it everywhere in your slice. Don’t mix. Inconsistent teams are generally slowly digging holes for themselves
- Do your best to minimise redundant coverage. For example, if one of your higher level test slices is nicely covering a block of code, maybe you don’t need all those lower level tests.
- Have an upfront plan for asynchronous
- https://medium.freecodecamp.com/the-right-way-to-test-react-components-548a4736ab22#.k0xuxxprz

## Instrumentation

## Measurements/Visibility

To elaborate on: StatsC, Sentry, MixPanel
