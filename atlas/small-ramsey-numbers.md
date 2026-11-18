# Small Ramsey numbers

Greenwood and Gleason found the values of $R(3,4)$, $R(3,5)$, $R(4,4)$ and
$R(3,3,3)$ [@GreenwoodGleason1955]; $R(3,3) = 6$ was known before them
[@Radziszowski2026, §2.1]. Each milestone here states two halves (see the conventions on the front page):

- an **upper half**: every graph, or colouring, of the given size has the
  monochromatic clique;
- a **lower half**: an explicit graph, or colouring, one vertex smaller, has
  none.

The lower halves are finite checks on explicit, very symmetric graphs, and
depend on nothing else. Each upper half uses the one before it, through one
vertex and its neighbours.

Most finite checks here are too large for `decide` to do naively: there are
$2^{15}$ graphs on 6 vertices, and $\binom{17}{4} = 2380$ sets of four
vertices in a graph on 17. Use the symmetry of the graphs to make the checks
small, prove the upper halves by the arguments below rather than by search,
and do not use `native_decide`, which `AGENTS.md` forbids.

> [!milestone] R(3,3) = 6
> Every graph on 6 vertices has a triangle or an independent set of 3 vertices. The 5-cycle has neither. So $R(3,3) = 6$.
>
> Sources: [@GreenwoodGleason1955]; [@Radziszowski2026, §2.1].
>
> Proof routes: upper half: a vertex $v$ has five others, so at least three neighbours or at least three non-neighbours. If two of three neighbours are adjacent, they form a triangle with $v$; if none are, they are independent. The case of non-neighbours is the same with the roles swapped. Lower half: the 5-cycle has no triangle, and its complement is again a 5-cycle.
>
> Formalization notes: Mathlib's `SimpleGraph.cycleGraph 5` is the 5-cycle. State the upper half for every graph on a vertex type with at least 6 elements, or for every `Finset` of at least 6 vertices, since the other upper halves apply it to sets of at least 6 vertices around a vertex: its neighbours, its non-neighbours, or the far ends of its edges of one colour. State the other upper halves the same way.
>
> Lean status: Mathlib —.

> [!milestone] R(3,4) = 9
> Every graph on 9 vertices has a triangle or an independent set of 4 vertices. The graph on $\mathbb{Z}/8$ in which $x$ and $y$ are adjacent when $x - y \in \{1, 4, 7\}$ (the Wagner graph, an 8-cycle with its four long diagonals) has neither. So $R(3,4) = R(4,3) = 9$.
>
> Sources: [@GreenwoodGleason1955]; [@Radziszowski2026, §2.1].
>
> Proof routes:
> - Upper half, using the upper half of $R(3,3) = 6$. Suppose $G$ on 9 vertices has neither. The neighbours of a vertex are pairwise non-adjacent, since $G$ has no triangle, so every vertex has at most 3 neighbours. Its non-neighbours, $8 - \deg v$ of them, contain no triangle and no independent set of 3, which with $v$ would make one of 4; so there are at most 5 of them, and $\deg v \geq 3$. So every vertex has degree 3, and the degrees sum to 27, which is odd. This contradicts the handshake lemma.
> - Lower half: no three elements of $\{1, 4, 7\}$, repetitions allowed, sum to $0$ modulo 8, so the graph has no triangle. The vertices not adjacent to $0$ are $2, 3, 5, 6$, and no three of them are pairwise non-adjacent; by translation, no vertex is in an independent set of 4.
>
> Formalization notes: `SimpleGraph.circulantGraph` on `ZMod 8` with the set `{1, 4, 7}` is this graph, and its translations are automorphisms (`circulantGraph_adj_translate`). Mathlib's handshake lemma is `SimpleGraph.sum_degrees_eq_twice_card_edges`.
>
> Lean status: Mathlib —.

> [!milestone] R(3,5) = 14
> Every graph on 14 vertices has a triangle or an independent set of 5 vertices. The graph on $\mathbb{Z}/13$ in which $x$ and $y$ are adjacent when $x - y \in \{1, 5, 8, 12\}$, the nonzero cubes modulo 13, has neither. So $R(3,5) = 14$.
>
> Sources: [@GreenwoodGleason1955]; [@Radziszowski2026, §2.1].
>
> Proof routes:
> - Upper half, using the upper half of $R(3,4) = 9$. In a graph on 14 vertices with neither, the neighbours of a vertex are pairwise non-adjacent, so there are at most 4. Its $13 - \deg v$ non-neighbours have no triangle and no independent set of 4, so there are at most 8, and $\deg v \geq 5$. That is a contradiction.
> - Lower half: no three elements of $\{1, 5, 8, 12\}$, repetitions allowed, sum to $0$ modulo 13, so there is no triangle. By translation, an independent set of 5 could be moved to contain $0$. Its other four vertices would be pairwise non-adjacent non-neighbours of $0$, that is, non-cubes. A check of the $\binom{8}{4} = 70$ sets of four non-cubes shows that each contains two that differ by a cube.
>
> Formalization notes: as for $R(3,4)$, a circulant graph on `ZMod 13`.
>
> Lean status: Mathlib —.

> [!milestone] R(4,4) = 18
> Every graph on 18 vertices has a clique of 4 vertices or an independent set of 4 vertices. The Paley graph on $\mathbb{Z}/17$, in which $x$ and $y$ are adjacent when $x - y$ is a nonzero square modulo 17, has neither. So $R(4,4) = 18$.
>
> Sources: [@GreenwoodGleason1955]; [@Radziszowski2026, §2.1].
>
> Proof routes:
> - Upper half, using the upper half of $R(3,4) = 9$ and its complement form $R(4,3) = 9$. A vertex $v$ has 17 others, so at least 9 neighbours or at least 9 non-neighbours. Nine neighbours contain a triangle, which with $v$ is a clique of 4, or an independent set of 4. Nine non-neighbours contain a clique of 4 or an independent set of 3, which with $v$ is one of 4.
> - Lower half: the nonzero squares modulo 17 are $\{1, 2, 4, 8, 9, 13, 15, 16\} = \{\pm 1, \pm 2, \pm 4, \pm 8\}$; since $-1$ is a square, adjacency is symmetric. Multiplication by $3$, a non-square, maps the graph onto its complement, so it is enough to show there is no clique of 4. Translations and multiplication by nonzero squares are automorphisms, so a clique of 4 could be moved to contain $0$ and $1$. The common neighbours of $0$ and $1$ are $2$, $9$ and $16$, and no two of them are adjacent. So there is no clique of 4.
>
> Formalization notes: a circulant graph on `ZMod 17` with the set of nonzero squares, which `decide` can list. The two automorphisms and the isomorphism with the complement are small lemmas that make every check small.
>
> Lean status: Mathlib —.

> [!milestone] R(3,3,3) = 17
> Every colouring of the edges of the complete graph on 17 vertices with 3 colours has a monochromatic triangle. The complete graph on 16 vertices has a 3-colouring with none. So $R(3,3,3) = 17$.
>
> The colouring: take the 16 vertices to be the vectors in $(\mathbb{Z}/2)^4$, written as the numbers $0, \dots, 15$ in binary, and split the 15 nonzero vectors into the three sets
> $$C_1 = \{1, 8, 10, 12, 15\}, \quad C_2 = \{2, 3, 7, 11, 13\}, \quad C_3 = \{4, 5, 6, 9, 14\}.$$
> Colour the edge $\{x, y\}$ with the $i$ for which $x + y \in C_i$, where $+$ is the sum of vectors, the bitwise exclusive or.
>
> Sources: [@GreenwoodGleason1955]; [@Radziszowski2026, §6 and §6.1].
>
> Proof routes:
> - Upper half, using the upper half of $R(3,3) = 6$. A vertex $v$ has 16 edges, so at least 6 of one colour, say red. If two of their other ends are joined by a red edge, there is a red triangle. Otherwise the edges among those 6 vertices use the two other colours, and $R(3,3) = 6$ gives a triangle in one of them.
> - Lower half: a triangle $x, y, z$ of colour $i$ would have $x + y$, $y + z$ and $x + z$ in $C_i$, and $(x + y) + (y + z) = x + z$. So it is enough that no $C_i$ contains $a$ and $b$ with $a + b \in C_i$; note that $a + a = 0$ is in no $C_i$. That is $3 \times 10$ sums to check.
> - The sets come from the field $\mathbb{F}_{16}$: with $\omega^4 = \omega + 1$, $C_1$ is the set of nonzero cubes, $\{\omega^0, \omega^3, \omega^6, \omega^9, \omega^{12}\}$, and $C_2 = \omega C_1$, $C_3 = \omega^2 C_1$ are its cosets. The proof above does not need the field, only the three sets.
>
> Formalization notes: model the vertices as `Fin 16` with `Nat.xor`, or as `Fin 4 → ZMod 2`; either makes the lower half a small `decide`. The colouring is a `SimpleGraph.TopEdgeLabeling (Fin 16) (Fin 3)`.
>
> Lean status: Mathlib —.
