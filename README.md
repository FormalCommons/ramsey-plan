# Ramsey theory: a formalization plan

A formalization plan, in the shape of an atlas: the milestone results of a
piece of mathematics, where they are proved, and what a formalizer should
know. It covers classical Ramsey theory: Ramsey's theorem in its graph,
many-colour, hypergraph and infinite forms; the exact small Ramsey numbers;
Ramsey numbers of trees, paths, stars and cycles; Erdős's lower bound; the
theorems of Schur, van der Waerden and Rado on the integers; and the
Erdős–Szekeres theorem on convex polygons.

The formalization builds on [Mathlib](https://github.com/leanprover-community/mathlib4),
which has graphs, cliques and independent sets, edge colourings of complete
graphs, circulant graphs, the Hales–Jewett theorem and convexity, but no
Ramsey theorem.

This plan was written by an AI agent, Claude, for the second experiment with a
pipeline of agents that formalize a plan through GitHub issues and pull
requests ([fc-worker](https://github.com/FormalCommons/fc-worker)). It was
checked against its sources, but it has not been reviewed by experts.

| Path | Contents |
|---|---|
| [`atlas/README.md`](atlas/README.md) | The front page: scope, conventions, how the milestones depend on each other, and the state of the libraries |
| [`atlas/ramsey-theorem.md`](atlas/ramsey-theorem.md) | Ramsey's theorem for graphs, for many colours, for hypergraphs, and for infinite sets |
| [`atlas/small-ramsey-numbers.md`](atlas/small-ramsey-numbers.md) | The exact values $R(3,3)$, $R(3,4)$, $R(3,5)$, $R(4,4)$ and $R(3,3,3)$ |
| [`atlas/graph-ramsey.md`](atlas/graph-ramsey.md) | Erdős's lower bound, and Ramsey numbers of trees, paths, stars and four-cycles |
| [`atlas/integers.md`](atlas/integers.md) | Schur's theorem, small Schur numbers, van der Waerden's theorem, $W(3;2)$, and Rado's theorem for one equation |
| [`atlas/geometry.md`](atlas/geometry.md) | Convex position: five points, and the Erdős–Szekeres theorem |
| [`atlas/references.bib`](atlas/references.bib) | The sources |

The pages are Markdown, with math between `$…$` and `$$…$$`. Each milestone
is a callout whose first line is its name, `> [!milestone] Name`; the callout
gives a precise statement, the sources, routes to a proof, and the milestone's
status in Mathlib. Citations such as `[@Erdos1947]` refer to entries of
`atlas/references.bib`.
