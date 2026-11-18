# Ramsey's theorem

Ramsey proved his theorem for colourings of the $r$-element subsets of a set,
first for infinite sets and then for finite ones [@Ramsey1930]. The case of
graphs, $r = 2$, is the one most of this plan uses. See the front page for the
conventions on graphs and colourings.

> [!milestone] Ramsey's theorem for graphs
> For all positive integers $s$ and $t$ there is an $n$ such that every graph on $n$ vertices has a clique of size $s$ or an independent set of size $t$. The least such $n$ is the Ramsey number $R(s, t)$, and:
> 1. $R(s, t) = R(t, s)$, $R(1, t) = 1$ and $R(2, t) = t$.
> 2. For $s, t \geq 2$, $R(s, t) \leq R(s - 1, t) + R(s, t - 1)$, and the inequality is strict when $R(s - 1, t)$ and $R(s, t - 1)$ are both even.
> 3. $R(s, t) \leq \binom{s + t - 2}{s - 1}$.
> 4. If every graph on $N$ vertices has a clique of size $s$ or an independent set of size $t$, and some graph on $N - 1$ vertices has neither, then $R(s, t) = N$.
>
> Sources: [@GRS1990, Ch. 1]; [@ErdosSzekeres1935] for the bound (3); [@GreenwoodGleason1955] for the recurrence (2) and its strict form, as [@Radziszowski2026, §2.3(a)] records.
>
> Proof routes:
> - (2), and with it existence, by induction on $s + t$. Let $G$ have $N = R(s - 1, t) + R(s, t - 1)$ vertices and pick a vertex $v$. Of the other $N - 1$ vertices, either at least $R(s - 1, t)$ are neighbours of $v$ or at least $R(s, t - 1)$ are not. In the first case the neighbours contain a clique of size $s - 1$, which $v$ extends to one of size $s$, or an independent set of size $t$. The second case is symmetric.
> - For the strict form, let $G$ have $N - 1$ vertices, an odd number, and suppose it has neither. Then, by the same argument, every vertex has exactly $R(s - 1, t) - 1$ neighbours, an odd number. So the sum of the degrees is odd, which contradicts the handshake lemma.
> - (3) follows from (2) and Pascal's rule, by induction on $s + t$.
> - (1): the complement swaps cliques and independent sets. A graph on $t$ vertices has an edge or is independent, and the empty graph on $t - 1$ vertices has neither an edge nor an independent set of size $t$.
>
> Formalization notes:
> - Define $R(s, t)$ with `Nat.find` on the existence statement.
> - State the existence on any `Fintype` vertex type of the right size. The induction restricts to the neighbours of a vertex, a subtype, and a statement only about `Fin n` would have to be transported along an equivalence at every step.
> - (4) is what the milestones about particular Ramsey numbers need to turn their two halves into an equation, so state it as a lemma of its own.
>
> Lean status: Mathlib —.

> [!milestone] Ramsey's theorem for many colours
> For every $k \geq 1$ and all $s_1, \dots, s_k \geq 1$ there is an $n$ such that every colouring of the edges of the complete graph on $n$ vertices with colours $1, \dots, k$ has, for some $i$, a monochromatic $K_{s_i}$ in colour $i$. The least such $n$ is $R(s_1, \dots, s_k)$, and if all $s_i \geq 2$,
> $$R(s_1, \dots, s_k) \leq 2 - k + \sum_{i=1}^{k} R(s_1, \dots, s_{i-1}, s_i - 1, s_{i+1}, \dots, s_k).$$
> In particular, for $k \geq 2$, $R_k(3) = R(3, \dots, 3)$, with $k$ threes, satisfies $R_k(3) \leq k\,(R_{k-1}(3) - 1) + 2$.
>
> Sources: [@GRS1990, Ch. 1]; [@GreenwoodGleason1955].
>
> Proof routes: the same induction as for two colours. With $N$ vertices as on the right, a vertex $v$ has $N - 1 = 1 + \sum_i (R_i - 1)$ edges, where $R_i$ is the $i$-th term of the sum. By pigeonhole, for some $i$ at least $R_i$ of them have colour $i$, and their other ends contain a monochromatic $K_{s_j}$ in some colour $j \neq i$, or a $K_{s_i - 1}$ in colour $i$, which $v$ extends. For the bound on $R_k(3)$, note that $R(2, 3, \dots, 3) = R_{k-1}(3)$: a single edge of colour $1$ is a $K_2$.
>
> Formalization notes: colourings are `SimpleGraph.TopEdgeLabeling V (Fin k)`; `SimpleGraph.TopEdgeLabeling.pullback`, along an embedding `V' ↪ V`, restricts one to a subset of the vertices. For $k = 2$, show that the statement agrees with the one for graphs, through `SimpleGraph.toTopEdgeLabeling`.
>
> Lean status: Mathlib —.

> [!milestone] Ramsey's theorem for hypergraphs
> For all $r, k, s \geq 1$ there is an $n$ such that for every colouring of the $r$-element subsets of an $n$-element set with $k$ colours, there is a set of $s$ elements all of whose $r$-element subsets have the same colour.
>
> Sources: [@Ramsey1930, Theorem B]; [@GRS1990, Ch. 1].
>
> Proof routes:
> - Induction on $r$; the case $r = 1$ is the pigeonhole principle. For $r + 1$, start from a large set $X_0$. Take its least element $x_1$, and colour each $r$-subset $T$ of $X_0 \setminus \{x_1\}$ by the colour of $T \cup \{x_1\}$. By the case $r$, there is a large $X_1 \subseteq X_0 \setminus \{x_1\}$ on which this colouring is constant. Repeat with $X_1$, and so on. This gives $x_1 < x_2 < \dots < x_m$ such that the colour of an $(r + 1)$-subset of them depends only on its least element. Colour each $x_i$ by that colour; by pigeonhole, $m = k(s - 1) + 1$ of them give $s$ elements of one colour, which form the set wanted. The sizes of the $X_j$ have to be chosen backwards from $m$, which is the fiddly part.
> - Alternatively, derive it from the infinite Ramsey theorem by compactness (König's lemma). This needs the infinite theorem and a compactness argument, neither of which the induction does.
>
> Formalization notes: the $r$-element subsets of a `Finset` are `Finset.powersetCard r`. State the theorem for colourings of `Finset (Fin n)`, or of `Finset V` for a `Fintype V`, restricted to sets of card $r$. Make the bound $n$ explicit or not; nothing in this plan needs its size. The Erdős–Szekeres theorem uses the case $r = 4$, $k = 2$.
>
> Lean status: Mathlib —.

> [!milestone] The infinite Ramsey theorem
> For every $r \geq 1$ and every colouring of the $r$-element subsets of $\mathbb{N}$ with finitely many colours, there is an infinite set $A \subseteq \mathbb{N}$ all of whose $r$-element subsets have the same colour.
>
> Sources: [@Ramsey1930, Theorem A]; [@GRS1990, Ch. 1].
>
> Proof routes: induction on $r$, with the construction of the hypergraph milestone run forever. For $r + 1$, build $x_1 < x_2 < \cdots$ and infinite sets $X_0 \supseteq X_1 \supseteq \cdots$, where $X_j$ is an infinite subset of $X_{j-1} \setminus \{x_j\}$, given by the case $r$, on which $T \mapsto$ colour of $T \cup \{x_j\}$ is constant, and $x_{j+1} = \min X_j$. Then the colour of an $(r+1)$-subset of $\{x_1, x_2, \dots\}$ depends only on its least element, and an infinite pigeonhole gives infinitely many $x_j$ of one colour.
>
> Formalization notes: Mathlib's `Finite.exists_infinite_fiber` is the infinite pigeonhole principle. The construction is a recursion that produces a sequence together with infinite sets; Mathlib's `Nat.rec` or `Stream'` can carry the pair. State the colouring as a function `Finset ℕ → κ` with `[Finite κ]`, applied to sets of card $r$.
>
> Lean status: Mathlib —.
