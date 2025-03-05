# Divide and Conquer Paradigm

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
