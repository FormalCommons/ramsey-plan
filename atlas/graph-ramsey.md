# Ramsey numbers of graphs

For graphs $H_1$ and $H_2$, $R(H_1, H_2)$ is the least $n$ such that for every
graph $G$ on $n$ vertices, $G$ contains a copy of $H_1$ or its complement
contains a copy of $H_2$ (see the conventions on the front page). This page
has the first lower bound for $R(k, k)$, and exact values for trees against
complete graphs, for paths, for stars and for the four-cycle. Radziszowski's
survey lists these with their sources [@Radziszowski2026].

> [!milestone] Erdős's lower bound
> For every $k \geq 3$, $R(k, k) > 2^{k/2}$: there is a graph on $\lfloor 2^{k/2} \rfloor$ vertices with no clique of $k$ vertices and no independent set of $k$ vertices. More precisely, if $\binom{n}{k}\, 2^{1 - \binom{k}{2}} < 1$, then some graph on $n$ vertices has neither.
>
> Sources: [@Erdos1947]; [@AlonSpencer2008, Proposition 1.1.1].
>
> Proof routes: counting. Fix $n$ vertices. There are $2^{\binom{n}{2}}$ graphs on them, and for a given set of $k$ vertices, exactly $2 \cdot 2^{\binom{n}{2} - \binom{k}{2}}$ of them make it a clique or an independent set. Summing over the $\binom{n}{k}$ sets, fewer than $2^{\binom{n}{2}}$ graphs have a clique or an independent set of size $k$ when $\binom{n}{k}\, 2^{1 - \binom{k}{2}} < 1$. For $n = \lfloor 2^{k/2} \rfloor$ and $k \geq 3$, $\binom{n}{k} \leq n^k / k! \leq 2^{k^2/2} / k!$, so $\binom{n}{k}\, 2^{1 - \binom{k}{2}} \leq 2^{1 + k/2} / k! < 1$.
>
> Formalization notes: count with `Finset.card` over `Finset.univ : Finset (SimpleGraph (Fin n))`, or over two-colourings, where `SimpleGraph.card_topEdgeLabeling` gives the total. The union bound is `Finset.card_biUnion_le`. Keep the inequality on natural numbers, $2 \binom{n}{k} < 2^{\binom{k}{2}}$, or on reals; for the real powers $2^{k/2}$, Mathlib has `Real.rpow`, but the statement can avoid them as $n^2 \le 2^k$.
>
> Lean status: Mathlib —.

> [!milestone] Trees versus complete graphs
> Let $T$ be a tree with $n \geq 1$ vertices and let $m \geq 1$. Then $R(T, K_m) = (n - 1)(m - 1) + 1$: every graph on $(n - 1)(m - 1) + 1$ vertices contains a copy of $T$ or has an independent set of $m$ vertices, and the disjoint union of $m - 1$ copies of $K_{n-1}$ does neither.
>
> Sources: [@Chvatal1977]; [@Radziszowski2026, §5.10].
>
> Proof routes:
> - Lower half: each component of $(m - 1) K_{n-1}$ has $n - 1$ vertices, too few for the connected graph $T$, and an independent set has at most one vertex in each of the $m - 1$ components.
> - Upper half: the case $n = 1$ is trivial, since $T$ is a single vertex and $N \geq 1$. For $n \geq 2$, two steps.
>   1. A nonempty graph whose minimum degree is at least $n - 1$ contains a copy of every tree with $n$ vertices. By induction on $n$: remove a leaf of $T$, embed the rest, and attach the leaf to an unused neighbour of the image of its neighbour, which has at least $n - 1$ neighbours and at most $n - 2$ of them used.
>   2. Let $G$ have $N = (n - 1)(m - 1) + 1$ vertices and no independent set of $m$ vertices. If some nonempty induced subgraph of $G$ has minimum degree at least $n - 1$, step 1 finds $T$ in it. Otherwise every nonempty induced subgraph has a vertex with fewer than $n - 1$ neighbours in it, so $G$ can be coloured with $n - 1$ colours (remove such a vertex, colour the rest, and give it a colour none of its neighbours has). One colour class then has at least $N / (n - 1) > m - 1$ vertices, and is independent.
>
> Formalization notes: `SimpleGraph.IsTree` for $T$, and `SimpleGraph.IsContained` (`T ⊑ G`) for containing a copy. Mathlib's `SimpleGraph.Coloring` and `Finset.exists_lt_card_fiber_of_mul_lt_card_of_maps_to` give the large colour class. Step 1 is the substantial part; it may be its own pull request.
>
> Lean status: Mathlib —.

> [!milestone] Paths
> Let $P_k$ be the path with $k$ vertices. For $n \geq m \geq 2$, $R(P_n, P_m) = n + \lfloor m/2 \rfloor - 1$.
>
> Sources: [@GerencserGyarfas1967]; [@Radziszowski2026, §5.1].
>
> Proof routes:
> - Lower half: take a set $A$ of $n - 1$ vertices and a set $B$ of $\lfloor m/2 \rfloor - 1$ vertices, and let $G$ be the complete graph on $A$, with $B$ isolated. Every component of $G$ has at most $n - 1$ vertices, so $G$ has no $P_n$. In the complement, $A$ is independent, so a path in it never has two consecutive vertices in $A$, and has at most $2|B| + 1 = 2\lfloor m/2 \rfloor - 1 \leq m - 1$ vertices. So the complement has no $P_m$.
> - Upper half: Gerencsér and Gyárfás's induction, which follows a longest path of one colour and the vertices off it. The argument is short on paper and has several cases; follow the source.
>
> Formalization notes: `SimpleGraph.pathGraph k` is $P_k$. Containing a copy of $P_k$ is the same as having a path (`SimpleGraph.Walk.IsPath`) with $k$ vertices; proving the two agree is a useful first lemma. This milestone is likely to need several pull requests.
>
> Lean status: Mathlib —.

> [!milestone] Stars
> Let $K_{1,n}$ be the star with $n$ edges. For $n, m \geq 1$,
> $$R(K_{1,n}, K_{1,m}) = n + m - \varepsilon, \qquad \varepsilon = \begin{cases} 1 & \text{if } n \text{ and } m \text{ are both even,} \\ 0 & \text{otherwise.} \end{cases}$$
>
> Sources: [@Harary1972]; [@BurrRoberts1973] for more colours; [@Radziszowski2026, §5.5].
>
> Proof routes: a graph contains $K_{1,n}$ exactly when some vertex has degree at least $n$, and in a graph on $N$ vertices, every vertex has degree $N - 1$ in $G$ and its complement together.
> - Upper half: if $N = n + m$, every vertex has degree at least $n$ in $G$ or at least $m$ in the complement. If $n$ and $m$ are both even and $N = n + m - 1$, a graph with neither would have every vertex of degree exactly $n - 1$, an odd number, on an odd number of vertices, which contradicts the handshake lemma.
> - Lower half: a graph on $N = n + m - 1 - \varepsilon$ vertices in which every vertex has degree $n - 1$. Then the complement is $(m - 1 - \varepsilon)$-regular, and neither contains its star. Such a graph exists because $(n - 1) N$ is even in each case; a circulant graph on $\mathbb{Z}/N$ is one, with connection set $\{\pm 1, \dots, \pm (n-1)/2\}$ for $n$ odd, and $\{\pm 1, \dots, \pm (n-2)/2\} \cup \{N/2\}$ for $n$ even, when $N$ is even.
>
> Formalization notes: Mathlib's `SimpleGraph.IsRegularOfDegree` and `SimpleGraph.circulantGraph`. Take $K_{1,n}$ as `SimpleGraph.starGraph` on `Fin (n + 1)` (a tree, `isTree_starGraph`) or `SimpleGraph.completeBipartiteGraph (Fin 1) (Fin n)`, or state the milestone with degrees and prove the equivalence separately.
>
> Lean status: Mathlib —.

> [!milestone] Four-cycles
> $R(C_4, C_4) = 6$: every graph on 6 vertices contains a 4-cycle or its complement does, and the 5-cycle, whose complement is again a 5-cycle, contains none.
>
> Sources: [@ChvatalHarary1972]; [@Radziszowski2026, §4.1].
>
> Proof routes:
> - Lower half: any four vertices of the 5-cycle span a path with three edges, not a 4-cycle.
> - Upper half, using the upper half of $R(3,3) = 6$. Let $G$ on 6 vertices have no 4-cycle, and neither have its complement. By $R(3,3) = 6$, and replacing $G$ by its complement if needed, $G$ has a triangle $abc$. Each other vertex $x$ has at most one neighbour among $a, b, c$: if it had two, say $a$ and $b$, then $x a c b$ would be a 4-cycle. So each of the other three vertices $x, y, z$ has a set $N_x \subseteq \{a, b, c\}$ of at least two non-neighbours. If two of these sets shared two elements $p, q$, then $x p y q$ would be a 4-cycle in the complement. So they are the three different pairs, say $N_x = \{a, b\}$, $N_y = \{b, c\}$, $N_z = \{a, c\}$, and $x \sim c$, $y \sim a$, $z \sim b$ in $G$. An edge $xy$ would give the 4-cycle $x y a c$ in $G$, and similarly $yz$ and $xz$ give $y z b a$ and $x z b c$. So $x, y, z$ are pairwise adjacent in the complement, and $x y c z$ is a 4-cycle there. Either way there is a contradiction.
>
> Formalization notes: `SimpleGraph.cycleGraph 4` is $C_4$. A graph contains $C_4$ exactly when it has four distinct vertices $p, q, r, s$ with $p \sim q \sim r \sim s \sim p$; prove that first, and work with it.
>
> Lean status: Mathlib —.
