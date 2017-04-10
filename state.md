# State (Redux etc)

As we continue on the journey of creating client rendered applications with a redux state store questions will arise regarding how we structure this data. Some of the questions you inevitably end up asking should be answered here.

## Do we ever want to store routing information in the store?

no

## Should we have a single, large, flat structure?

no

## How do we structure the store?

- group by “concept” or “component”, such as authentication, search, selected item, etc.
- inside the grouping have separate keys which represent the data and the data state.
- with lists of resources (like list of donations), maintain a separate list for the actual individual donation details, and a separate one for the current list being shown (reference the individual items by id).

## Example

There is a [gist](https://gist.github.com/nippysaurus/d4b85c98e3fc56f925d2989e60c9da08) with an example of how the store could be structured.

https://gist.github.com/nippysaurus/d4b85c98e3fc56f925d2989e60c9da08


[https://gist.github.com/nippysaurus/d4b85c98e3fc56f925d2989e60c9da08](https://gist.github.com/nippysaurus/d4b85c98e3fc56f925d2989e60c9da08)

## Some things to still figure out

- If we have a separate section of the store for items details (which are referenced from the display list), how do we know what items can be pruned from that list. Without pruning I imaging the list would get pretty large, especially in the case of search results.


## References

- https://hackernoon.com/avoiding-accidental-complexity-when-structuring-your-app-state-6e6d22ad5e2a#.4bevyefj0
