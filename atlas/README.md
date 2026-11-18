# Ramsey theory

Complete disorder is impossible: every large enough structure, coloured with
finitely many colours, contains a large structure of one colour. Ramsey's
theorem says this for graphs and hypergraphs [@Ramsey1930]; the theorems of
Schur, van der Waerden and Rado say it for the integers; and the
Erdős–Szekeres theorem says it for points in the plane. This plan formalizes
these theorems, the exact values of the small Ramsey numbers, and the first
bounds of the theory. The textbook is Graham, Rothschild and Spencer
[@GRS1990]; Radziszowski's survey [@Radziszowski2026] collects the known
values of Ramsey numbers with their references.

## Scope

In scope: Ramsey's theorem in its four classical forms with the standard
upper bounds; the Ramsey numbers $R(3,3)$, $R(3,4)$, $R(3,5)$, $R(4,4)$ and
$R(3,3,3)$; Erdős's exponential lower bound; the Ramsey numbers of trees
against complete graphs, of paths, of stars and of the four-cycle; Schur's
theorem with $S(2)$ and a lower bound for $S(3)$; van der Waerden's theorem
and $W(3;2)$; Rado's theorem for one equation, as a stretch goal; and the
Erdős–Szekeres theorem on points in convex position.

Out of scope: computer-assisted results, such as $R(4,5) = 25$ or
$S(5) = 160$; the upper bound $S(3) \le 13$, a longer case analysis; asymptotic improvements of the bounds, such as
the recent exponential improvement of the diagonal upper bound; the
Hales–Jewett theorem, which Mathlib has; Rado's theorem for systems of
equations; and the canonical Ramsey theorem.

## Conventions

These conventions keep the milestones independent of each other, so that
they can be worked on at the same time.

- A *graph on $n$ vertices* is a `SimpleGraph V` with `Fintype.card V = n`,
  such as a `SimpleGraph (Fin n)`. A *clique of size $s$* is a set of $s$
  pairwise adjacent vertices, and an *independent set of size $t$* a set of
  $t$ pairwise non-adjacent vertices. In Mathlib, $G$ has a clique of size
  $s$ when `¬ G.CliqueFree s`, and an independent set of size $t$ when
  `¬ G.IndepSetFree t`. Taking complements swaps the two
  (`SimpleGraph.cliqueFree_compl`, `SimpleGraph.indepSetFree_compl`).
- A colouring of the edges of the complete graph on $V$ with colours in $K$
  is a `SimpleGraph.TopEdgeLabeling V K`. Its edges of colour $k$ form the
  graph `C.labelGraph k`, and a *monochromatic $K_s$ in colour $k$* is a
  clique of size $s$ in that graph. A colouring with two colours is the same
  as a graph: `SimpleGraph.toTopEdgeLabeling` gives the edges of $G$ label 1
  and its non-edges label 0.
- The *Ramsey number* $R(s, t)$ is the least $n$ such that every graph on
  $n$ vertices has a clique of size $s$ or an independent set of size $t$.
  To say that $R(s, t) = N$ is to say two things: every graph on $N$
  vertices has one or the other (the upper half), and some graph on $N - 1$
  vertices has neither (the lower half). The milestones about particular
  Ramsey numbers state the two halves in these terms, which need no general
  definition of $R(s, t)$, so they need not wait for one. The milestone
  *Ramsey's theorem for graphs* defines $R(s, t)$ and a lemma that turns the
  two halves into the equation.
- For graphs $H_1$ and $H_2$, $R(H_1, H_2)$ is the least $n$ such that for
  every graph $G$ on $n$ vertices, $G$ contains a copy of $H_1$ or its
  complement contains a copy of $H_2$: `H₁ ⊑ G ∨ H₂ ⊑ Gᶜ`
  (`SimpleGraph.IsContained`). So $R(s, t) = R(K_s, K_t)$.
- $[n]$ is $\{1, \dots, n\}$. A colouring of $[n]$ with $r$ colours is a
  function $[n] \to$ `Fin r`, or on `Fin n` after a shift; say which.

## Milestones

- [Ramsey's theorem](ramsey-theorem.md):
  - **Ramsey's theorem for graphs**
  - **Ramsey's theorem for many colours**
  - **Ramsey's theorem for hypergraphs**
  - **The infinite Ramsey theorem**
- [Small Ramsey numbers](small-ramsey-numbers.md):
  - **R(3,3) = 6**
  - **R(3,4) = 9**, whose upper half depends on R(3,3) = 6
  - **R(3,5) = 14**, whose upper half depends on R(3,4) = 9
  - **R(4,4) = 18**, whose upper half depends on R(3,4) = 9
  - **R(3,3,3) = 17**, whose upper half depends on R(3,3) = 6
- [Ramsey numbers of graphs](graph-ramsey.md):
  - **Erdős's lower bound**
  - **Trees versus complete graphs**
  - **Paths**
  - **Stars**
  - **Four-cycles**, whose upper half depends on R(3,3) = 6
- [The integers](integers.md):
  - **Schur's theorem**, which depends on Ramsey's theorem for many colours
  - **Small Schur numbers**
  - **Van der Waerden's theorem**
  - **W(3;2) = 9**
  - **Rado's theorem for one equation** (stretch), which depends on Van der
    Waerden's theorem
- [Geometry](geometry.md):
  - **Five points in convex position**
  - **The Erdős–Szekeres theorem**, which depends on Five points in convex
    position, and, on one route, on Ramsey's theorem for hypergraphs

Everything can start at once except Schur's theorem, Rado's theorem, the
Erdős–Szekeres theorem, and the upper halves of the four small Ramsey numbers
after $R(3,3)$ and of Four-cycles, whose lower halves depend on nothing.

## The libraries

As of 18 November 2026, at Mathlib commit `048fffa`:

- **Mathlib** has simple graphs with cliques, independent sets and their
  `CliqueFree` and `IndepSetFree` predicates; complements; copies of one
  graph in another (`SimpleGraph.Copy`, `IsContained`, `Free`); edge
  labellings of complete graphs with their count
  (`SimpleGraph.TopEdgeLabeling`, `card_topEdgeLabeling`); circulant graphs
  (`SimpleGraph.circulantGraph`), cycle graphs (`SimpleGraph.cycleGraph`),
  path graphs (`SimpleGraph.pathGraph`), complete bipartite graphs, trees
  (`SimpleGraph.IsTree`) and colourings (`SimpleGraph.Coloring`); the
  handshake lemma (`SimpleGraph.sum_degrees_eq_twice_card_edges`);
  pigeonhole principles (`Finset.exists_lt_card_fiber_of_mul_lt_card_of_maps_to`,
  `Finite.exists_infinite_fiber`); the Hales–Jewett theorem
  (`Combinatorics.Line.exists_mono_in_high_dimension`) with a form of van der
  Waerden's theorem for colourings of a whole monoid
  (`Combinatorics.exists_mono_homothetic_copy`); Hindman's theorem; finite
  fields (`GaloisField`, `ZMod`) and quadratic residues; and convexity, with
  `ConvexIndependent`, `Collinear` and Carathéodory's theorem
  (`convexHull_eq_union`).
- Mathlib has no Ramsey theorem and no Ramsey number. Its `Archive`, which
  is not part of the library, proves the Erdős–Szekeres theorem on monotone
  subsequences, a different theorem from the one in this plan.
- **Tau Ceti** has no Ramsey theory either (checked at its commit `08ee344`,
  18 November 2026), and this plan does not use it.

None of the milestones here is in Mathlib, apart from part of van der
Waerden's theorem.

## Sources

Graham, Rothschild and Spencer [@GRS1990] cover Ramsey's theorem, van der
Waerden's and Rado's theorems, and graph Ramsey theory. Radziszowski's survey
[@Radziszowski2026] gives each known Ramsey number with the papers that
established it. Greenwood and Gleason [@GreenwoodGleason1955] found the
small Ramsey numbers of this plan, apart from $R(3,3) = 6$, which was known
before them; Erdős and Szekeres [@ErdosSzekeres1935]
proved the binomial upper bound and the theorem on convex polygons; Erdős
[@Erdos1947] the lower bound, presented as in Alon and Spencer
[@AlonSpencer2008]. The survey's sections are those of its revision 18. Landman and Robertson [@LandmanRobertson2014] treat
Schur numbers, van der Waerden numbers and Rado's theorem at the level of
detail the integer milestones need.
