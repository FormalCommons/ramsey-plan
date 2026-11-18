# Geometry

Points are in the plane; a set of points is in *general position* if no three
of them are on a line, and in *convex position* if none of them is in the
convex hull of the others, as the vertices of a convex polygon are. Erdős and
Szekeres proved that every large enough set of points in general position has
$n$ points in convex position [@ErdosSzekeres1935], starting from the case
$n = 4$, which they credit to Esther Klein.

> [!milestone] Five points in convex position
> Among any five points in the plane in general position, there are four in convex position.
>
> Sources: [@ErdosSzekeres1935].
>
> Proof routes: if four of the points are vertices of their convex hull, those four are in convex position. Otherwise the hull is a triangle $abc$, and the other two points $p$ and $q$ are inside it. The line through $p$ and $q$ passes through none of $a$, $b$, $c$, by general position, so it has two of them, say $a$ and $b$, on the same side. Then $a$, $b$, $p$ and $q$ are in convex position.
>
> Formalization notes:
> - Take the points as `Fin 5 → EuclideanSpace ℝ (Fin 2)` or `Fin 5 → ℝ × ℝ`.
> - General position: `¬ Collinear ℝ {p i, p j, p k}` for distinct `i`, `j`, `k`. Convex position: `ConvexIndependent ℝ` of the points, restricted to the four chosen.
> - Orientation, the sign of $\det(q - p, r - p)$, is a more computational language for both, and a lemma that relates it to `Collinear` and to convex position will serve both milestones on this page. Decide early which language to use.
> - This milestone may need several pull requests: the convexity lemmas first, then the case analysis.
>
> Lean status: Mathlib —.

> [!milestone] The Erdős–Szekeres theorem
> For every $n \geq 3$ there is an $N$ such that among any $N$ points in the plane in general position there are $n$ in convex position.
>
> Sources: [@ErdosSzekeres1935].
>
> Proof routes:
> - Through Ramsey's theorem, as in Erdős and Szekeres's first proof. Colour each set of four of the points by whether it is in convex position. Let $s = \max(n, 5)$ and let $N$ be large enough for Ramsey's theorem for hypergraphs with $r = 4$, two colours and sets of size $s$. A set of $s \geq 5$ points all of whose four-point subsets are not in convex position would contradict *Five points in convex position*, so there are $s \geq n$ points all of whose four-point subsets are in convex position. Such a set is in convex position: by Carathéodory's theorem in the plane, a point in the convex hull of others is in the convex hull of three of them, and then those four are not in convex position.
> - Through cups and caps, Erdős and Szekeres's second proof, which gives $N = \binom{2n - 4}{n - 2} + 1$ and does not need Ramsey's theorem. It needs a combinatorial lemma on sequences of points sorted by their first coordinate, so it first turns the plane so that no two points have the same first coordinate: general position alone does not give that.
>
> Formalization notes: on the first route, this depends on *Five points in convex position* and *Ramsey's theorem for hypergraphs*. Mathlib's Carathéodory theorem is `convexHull_eq_union`; with `AffineIndependent.card_le_finrank_succ` it gives three points in the plane.
>
> Lean status: Mathlib —.
