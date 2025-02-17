# Algorithm Paradigms

`global approach` you look at the overall problem to solve. 

Example: If you want to find a route from LA to SF, a global approach would be to look at all roads from LA to SF and decide what is the most `optimal` path.

`optimal` is described by what parameters you want to minimize or maximize.

## Greedy Algorithm

`greedy algorithm` you take a local approach. Greedy algorithms are quick but we may be unsure if it is optimal or not. Usually this takes `O(nlogn)`

Example: You are given a set of tasks. Give a subset of the tasks such that no tasks overlap.

The simplest solution is to return any 1 task.

Example: You are given a set of tasks. Give a subset of non-overlapping tasks such taht the number of tasks are maximized.

Greedy Solutions:

* pick tasks with shortest length
* pick tasks with the least number of scheduling conflicts
* pick tasks with first finish time and delete conflicting tasks.

Say `our` algorithm outputs 5 intervals. `Their` algorithm outputs outputs more than 5 intervals. For some `i` in the beginning, our algorithm outputs the same output as theirs. For some interval `A` in our algorithm and some interval `B` in their algorithm, there are two possibilities:

$$ f_B < f_A $$
$$ f_B > f_A $$

The first one will never happen by our definition because the algorithm always outputs the interval with the lowest finish time.

```
    Our Algorithm:
          i       i+1 f_A
        ----| ... ---|              // stays ahead
    Their Algorithm:
          i       i+1 f_B
        ----| ... -----|
```

Our first `i+1` interval stays ahead of their first `i+1` by our algorithm  definition. So by induction, our first `k` intervals is ahead of their `k` intervals.

There can never be an interval `X` such that they choose that we cannot choose.

```
    Our Algorithm:
          i       i+1 f_A
        ----| ... ---|              
    Their Algorithm:
          i       i+1 f_B   X
        ----| ... -----| |---|      // this can never happen because we can choose X
```

Therefore, Our greedy algorithm is optimal.

To run the algorithm, we need to sort the it in ascending finish times `O(nlogn)`

```mermaid
    graph LR
        f1 --- f2 --- f3 --- ... --- fn
        style f1 fill:#489
        style f2 fill:#489
        style f3 fill:#489
        style ... fill:#489
        style fn fill:#489
```

```
    for n elements in the sorted finish times:
        if s_{i+1} > f_i
            output f_{i+1}
```

This runs in `O(n)` time.

Total time complexity: `O(nlogn) + O(n)` = `O(nlogn)`

## Divide and Conquer Paradigm

Given this list, sort it in non-decreasing order. We are going to use `merge sort`.

```mermaid
    graph TB
        subgraph 2nd
            direction TB
            e@{label: "4"}
            f@{label: "1"}
            g@{label: "6"}
            h@{label: "2"}
        end
        subgraph 1st
            direction TB
            a@{label: "7"}
            b@{label: "4"}
            c@{label: "2"}
            d@{label: "3"}
        end
        style a fill:#940
        style b fill:#940
        style c fill:#940
        style d fill:#940
        style e fill:#940
        style f fill:#940
        style g fill:#940
        style h fill:#940
```

$$
\text{Divide until you can do comparisons}
$$

```mermaid
    graph TB
        subgraph 4th
            direction TB
            h@{label: "2"}
            g@{label: "6"}
        end
        subgraph 3rd
            direction TB
            f@{label: "1"}
            e@{label: "4"}
        end
        subgraph 2nd
            direction TB
            c@{label: "2"}
            d@{label: "3"}
        end
        
        subgraph 1st
            direction TB
            b@{label: "4"}
            a@{label: "7"}
        end
        style a fill:#940
        style b fill:#940
        style c fill:#940
        style d fill:#940
        style e fill:#940
        style f fill:#940
        style g fill:#940
        style h fill:#940
```

$$
\text{merge}
$$

```mermaid
    graph TB
        subgraph 2nd
            direction TB
            e@{label: "1"}
            f@{label: "2"}
            g@{label: "4"}
            h@{label: "6"}
        end
        subgraph 1st
            direction TB
            a@{label: "2"}
            b@{label: "3"}
            c@{label: "4"}
            d@{label: "7"}
        end
        style a fill:#049
        style b fill:#049
        style c fill:#049
        style d fill:#049
        style e fill:#049
        style f fill:#049
        style g fill:#049
        style h fill:#049
```

$$
\text{merge}
$$

```mermaid
    graph TB
        subgraph Final List
            direction TB
            e@{label: "1"}
            f@{label: "2"}
            g@{label: "2"}
            h@{label: "3"}
            a@{label: "4"}
            b@{label: "4"}
            c@{label: "6"}
            d@{label: "7"}
        end
        style a fill:#049
        style b fill:#049
        style c fill:#049
        style d fill:#049
        style e fill:#049
        style f fill:#049
        style g fill:#049
        style h fill:#049
```

To do the `merge` operation, we initialize two pointers at the start of the lists we are trying to merge. Put the minimum of the pointer into the output list and move the pointer to the next element. This takes `O(mn)` since we compare each m element with at most n elements. But this is pretty pessimistic. If we change our accounting method, this takes `O(m+n)`. If `m_i` is compared with every n, it would take `O(n)` since all values after `m_i` is greater than all n. Likewise it takes `O(m)` to compare `n_i` with all values of m. Therefore it take `O(m+n)`.

### Runtime Analysis for Recursive Algorithms:

`merge sort`

$$
\begin{align*} 
T(n) &= T(\frac{n}{2}) + T(\frac{n}{2}) + O(n)\\
&= 2T(\frac{n}{2}) + cn\\
&= 2(2T(\frac{n}{4}) + \frac{cn}{2})+ cn\\
&= 2^2T(\frac{n}{2^2})+ 2cn\\
&\vdots\\
&=2^iT(\frac{n}{2^i})+icn\\
T(1) = 1\\
\frac{n}{2^i} = 1\\
2^i = n \implies i = \log n\\
&= 2^{\log n}T(\frac{n}{2^{\log n}}) + cn \log n\\
&= nT(1) + cn\log n\\
&= O(n \log n)
\end{align*}
$$

`binary search`

$$
\begin{align*} 
T(n) &= T(\frac{n}{2}) + c\\
&= (T(\frac{n}{4}) + c) + c\\
&= T(\frac{n}{2^i})+ ic\\
T(1) = 1\\
\frac{n}{2^i} = 1\\
2^i = n \implies i = \log n\\
&= T(\frac{n}{2^{\log n}}) + c\log n\\
&= O(\log n)
\end{align*}
$$

### Crossing Number Problem

`crossing number` the number of elements that are out of order in a nondecreasing list. The minimum crossing number for any list is 0. The maximum crossing number in any list is ${n\choose 2}$.

Say you have a list `1 4 3 5 7 6`. The crossing number is `2`.

To solve this problem we will:

* run merge sort
* the crossing number of output is incremented by `x+1` whenever we add an element from the right side to the output. x + 1 is the number of indices on the left partition from current index to the end of the list. If there is a output from the right, that means it crosses all elements on the left side's current index to the end.

## Dynamic Programming (DP)

### Weighted Interval Scheduling

In a weighted interval scheduling problem, we want to maximize the total weight of intervals while not picking overlapping intervals.

We can do this by looking for the optimal solution from time $0$ to time $t$.

```
                  Wx
    |         |-------| |
              Sx      Fx

    | --------------->| |
                    t-1

    | ----------------->|
    0                   t
```

What is the best interval from $0$ to $t$? The most optimal solution can be denoted as `opt(t)`. 

$$
\begin{align*}
&x \in \text{ solution} \implies \text{opt}(t) = \text{opt}(S_x - 1) + W_x\\
&x \notin \text{ solution} \implies \text{opt}(t) = \text{opt}(t-1)
\end{align*}
$$

`opt(t)` is the max of these cases and can be denoted as:

$$
\text{opt}(t) = \text{max}(\text{opt}(S_x-1) + W_x, \text{ opt}(t-1))
$$

If the problem asks for the intervals that contribute to the optimal solution, we must backtrack from the end. The optimal solution is only determined once we have found `opt(T)` where T is the total period of time we want to run the algorithm on.

### Knapsack Problem

Suppose we have `n` number of items, `I` each with two attributes, `S` for size, and `v`, for value. For the Knapsack problem, we want to maximize the value of items such that the items fit into the bag. We will first consider the variation where there is an infinite supply of each item.

The sum of items must be less than or equal to the capacity of the knapsack such that the total value is maximized.

| | $1$ | $2$ | $...$ | $S$ |
| --- | --- | --- | ---| --- |
| $I_1$ | $v1$ or 0|  | ... |
| $I_2I_1$ | |  | ... | |
| $\vdots$ | |  | ... | |
| $I_n...I_2I_1$ | | | | final solution |

The square $(I_1, 1)$ can either contain `v1` or `0` because a knapsack of size 1 can contain items with size 1 or less. If `v1` happens to be smaller than the capacity of this knapsack, we can say opt(1, 1) is `v1`.

Generally, for any opt(i,j):

$$
I_i \in \text{ solution} \implies \text{opt}(i, j) = \text{opt}(i, S_j - S_i) + V_i
$$

because $S_j - S_i$ denotes the space remaining if we put $I_i$ in the knapsack.

$$
I_i \notin \text{ solution} \implies \text{opt}(i, j) = \text{opt}(i - 1, S_j)
$$

$$
\text{opt}(t) = \text{max}(\text{opt}(i, S_j - S_i) + V_i, \text{ opt}(i - 1, S_j))
$$

For our implementation:

```
    for 1 <= i <= n
        for 1 <= j <= S
            do main equation
```

Time Complexity:

There are `n` rows and `S` columns in the table. It takes `O(ns)`. THis is not polynomial time. We call this `pseudo polynomial` because everytime we add one to `S`, the number of rows grows by `n`. The runtime is proportional to the value of `S`, but it is exponential to the memory needed to store that value.
