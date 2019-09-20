# CSC263

## Week 1 Complexity & ADTs

### __Abstract Data Types__

1. What are the possible values?
2. What are the operations?

_Integers_\
value: ...-2, -1, 0...\
operations: + -...\
Implementations: 32bit binary, ASCII

_Stack_\
value: finite ordered list sequence of elements\
operations:

* push - insert element at top
* pop - remove and return element at top
* peek - look at top element

Implementations:

* As array

  ```python
  let A = [];
  peak(A)
    return A[1]
  pop(A)
    val = A[1]
    for i = 1 to A.size-1
        A[i] = A[i+1]
    return val
  A[A.size] is the top
  peek(A)
    return A[A.size]
  push(A,x)
    A[A.size+1] = x
  pop(A)
    val = peek(A)
    return val
  ```

* As linked list

    ```python
    node.val
    node.next
    head pointer

    head node = top
    peek(head)
        assert(head != null)
        return head.val

    push(head, x)
        oldhead = head
        head = new node
        head.val = x
        head.next = oldhead

    pop(head)
        val = head.val
        head = head.next
        return val
    ```

### __Complexity__

_input size_:

* number of items in the input\
`e.g. the array size n for sorting`

* total number of bits needed to represent the input in ordinary binary notation.\
`e.g. multiplying two integers`

* describe the size of the input with two numbers rather than one.\
`e.g. input is a graph. input size can be described by the numbers of vertices and edges in the graph`

_running time_: number of primitive operations executed.

```python
INSERTION-SORT(A)
1 for i = 2 to A.length
2   key = A[j]
3     //Insert A[j] into the sorted
          sequence A[1..j-1]
4   i = j - 1
5   while i > 0 and A[i] > key
6     A[i+1] = key
7     i = i -1
8   A[i+1] = key
```

The best case occurs if the array is already sorted. _Linear function_ \
The worst case occurs if it is in decreasing order. _Quadratic function_

_`Worst-case and average-case analysis`_

The reason concentrate on finding only the worst-case running time:

* The worst case running time of an algorithm gives an upper bound on the running time for any input. Provides a guarantee that the algorithm will never take any longer.
* For some algorithms, the worst case ocuurs fairly often.
* The "average case" is often roughly as bad as the worst case.

_`Order of growth`_

Simplifying abstractions:

1. use the constants $c$ to represent actual cost of each statement.
2. only consider the leading term. also ignore the leading term's constant coefficient.

_`Asymptotic notation`_

How the running time of an algorithm increases with the size of the input in the limit, as the size of the input increases without bound.

Functions whose domains are the set of natural numbers.

_$\Theta$`-notation`_

$\Theta(g(n))=\{f(n):$ there exist positive constants $c_1,c_2$ and $n_0$ such that $0 \le c_1g(n)\le f(n)\le c_2g(n)$ for all $n\ge n_0\}$

* $\Theta$-notation bounds a function to within constant factors.
* $O$-notation gives an upper bound for a function to within a constant factor.
* $\Omega$-notation gives a lower bound for a function to within a constant factor

> A function $f(n)$ belongs to the set $\Theta(g(n))$ if there exist positive constants $c_1,c_2$ such that it can be "sandwiched" between $c_1g(n)$ and $c_2g(n)$ for sufficiently large n.

> $f(n)=\Theta(g(n))$ to express belongs to

For all $n\ge n_0$, the function $f(n)$ is equal to $g(n)$ within a constant factor. We say that $g(n)$ is an __asymptotically tight bound__ for $f(n)$

_`O-notation`_

When we have only an __asymptotic uppper bound__\
$O(g(n))=\{f(n):$ there exist positive constants $c$ and $n_0$ such that $0 \le f(n)\le cg(n)$ for all $n\ge n_0\}$

Note: $f(n) = \Theta(g(n))$ implies $f(n) = O(g(n))$

_$\Omega$`-notation`_

Provides an __asymptotic lower bound__ on a function\
$\Omega(g(n))=\{f(n):$ there exist positive constants $c$ and $n_0$ such that $0 \le cg(n)\le f(n)$ for all $n\ge n_0\}$

___Theorem 3.1___
For any two functions $f(n)\ and\ g(n)$, we have $f(n)=\Theta(g(n))$ iff $f(n)=O(g(n))$ and $f(n)=\Omega(g(n))$
>p48

```python
1 LinkedSearch(L, k)
2     z = L.head
3     while z != null and z.key != k
4         z = z.next
5     return z
```

Suppose list has n elements\
T(n) = 1 + 2(n + 1) + n = 3n + 3 (worst case cost) n: size of input

$T(n) \in O(f(n))\\
\exists B,C \hspace{0.5cm} \forall n>B \hspace{0.5cm} T(n) > cf(n)\\
T(n)=3n+3\\
suppose\ B=3\ C=4\\
\forall B<n\ 3n+3 \leq Cn\\
3 \leq (c-3)n$

---

$O(n) \subset O(n^2)\\
T(n) = 3m+3\\
show\ T(n) \in O(n^2)\\
\exists B,C \hspace{0.5cm} \forall B<n \hspace{0.5cm} T(n) < Cn^2$

---

$T(n) \in \Omega(F(n))\\
\exists B,c \hspace{0.5cm} \forall B<n \hspace{0.5cm} cf(n) \leq T(n)\\ 3n+3 \in \Omega (n)\\
supppose\ B=0\ c=3\\
\forall B<n\ cn \leq 3n+3\\
(c-3)n\leq 3\\
3n\leq3n+3\\
T(n) \in O(n)\\
T(n) \notin \Omega(n^2)\\
T(n) \in O(f(n)) \land T(n) \in \Omega(f(n)) \rightarrow T(n) \in \Phi(f(n))\\
\Phi(f(n)) = O(f(n)) \cap \Omega(f(n))$

---

## Week 2 Priority Queues, Heaps, Dictionaries, BSTs

### __Binomial Heaps__

_mergeable heaps_:

```
MAKE-HEAP()
  creates and returns a new heap containing no elements

INSERT(H, x)
  inserts node x, whose key field has already been
  filled in, into heap H

MINIMUM(H)
  returns a pointer to the node in heap H whose key is
  minimum

EXTRACT-MIN(H)
  deletes the node from heap H whose key is minimum,
  returning a pointer to the node.

UNION(H1, H2)
  creates and returns a new heap that contains all the
  nodes of heaps H1 and H2. Heaps H1 and H2 are
  "destroyed" by this operation.

DECREASE-KEY(H, x, k)
  assigns to node x within heap H the new key value k,
  which is assumed to be no greater than its current
  key value.

DELETE(H, x)
  deletes node x from heap H
```

Note: ignores issues of allocating nodes prior to insertion and freeing nodes following deletion

### __Binomial trees and binomial heaps__

A binomial heap is a collection of binomial trees.

_`Binomial trees`_:

$B_k$ is an ordered tree. Binomial tree $B_0$ consists of a single node. The binomial tree $B_k$ consists of two binomial trees $B_{k-1}$ that are linked together: the root of one is the leftmost child of the root of the other.

`Properties of binomial trees`:
For the binomial tree $B_k$

1. there are $2^k$ nodes
2. the height of the tree is $k$
3. there are exactly ${k \choose i}$ nodes at depth $i$ for $i=0,1,...,k$ and
4. the root has degree k, which is greater than that of any other node; moreover if the children of the root are numbered from left to right by $k-1,k-2,...0$, child $i$ is the root of a subtree $B_i$
