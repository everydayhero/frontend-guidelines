# Guidelines Divergence/Amendments

- How do we make decisions, or decide on new things (e.g. guild meetings, submitting PR’s)
- How do we decide to diverge/add exceptions from the document

## Why have “guidelines divergence”?

As the javascript community adapts and evolves we do not want to be restricted to an outdated standard. Allowing for divergence from the guidelines provides the flexibility to find new tools, develop products with fewer restrictions and grow our general knowledge. Too much flexibility and we run the risk of having an unmaintainable work environment, too little and we will spend more time fighting the guidelines than working with them.

## How do we decide on when to diverge? (WIP)

- When a new module/tool is found that does not fit in the bounds of the guidelines but appears to meet the requirements of a new project.
  - We need to follow some restraint when adopting new modules. Many are short lived without the prospect of further fixes.
  - We do not want to maintain additional forks where we can avoid it if it doesn’t provide a clear benefit.
- Through open communication amongst guild members through PRs, guild meetings, etc to highlight the need to diverge
  - State your case and reasoning to diverge. Provide an example if you can on how it meets needs not found within the document.
  - Be sure to respond to feedback and don’t be afraid to defend your case. We need to adapt and this is an opportunity for discussion.

If an impasse occurs the power of decision falls to tech leads, assuming they have the knowledge of our systems, how they will be affected and prior knowledge of what risks will occur when diverging.

## Typical concerns when diverging

When you raise your case to diverge there are typical questions raised.

Design choice concerns

- Where are our current policies failing that you are hoping to improve upon?

Tech related concerns

- Is the module you’re planning to adopt at risk of losing support?
- Are there conflicts with existing technologies within our environment?
- How difficult is it to bring the rest of the environment up to date with the divergence?
  - Technical debt is a real cost and needs to be appreciated. That said, technical wealth can be created by maintaining a consistent, well engineered environment.
- “You can accomplish X with Y which we already use, why do it this way?”
  - Lack of knowledge of the currently available toolset is expected, either by new developers or by those working in other environments. This is an opportunity for all to learn.
