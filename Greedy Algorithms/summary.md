# Summary & Patterns

## Finding the Greedy Solution

> Find the **local best** choice first, and keep making these choices if they are **compatible** with the previous ones.

### Greedy strategies from the problems in this section

| Problem            | Local Best is...                 | Compatible if...                                  |
| ------------------ | -------------------------------- | ------------------------------------------------- |
| Activity Selection | the class that ends the earliest | the class doesn't conflict with existing schedule |
| Driving Problem    | the farthest gas station         | the station is within our range                   |

## The Greedy Exchange Argument

1. Show that there’s an arbitrary optimal solution $\text{OPT}$ different from greedy solution $A$.
2. Find the “first difference” between $\text{OPT}$ and $A$.
   - For a more rigorous proof, you also need to show that this difference actually exists
3. Argue that exchanging this optimal choice with a greedy choice will **NOT** make the solution worse. Doesn’t have to make it better.
4. Use induction to show that the entirety of $\text{OPT}$ can be swapped with $A$
   - This step is technically implicit from step 2, but it depends.
