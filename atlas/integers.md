# The integers

Colour the positive integers with finitely many colours: some colour class
contains a solution of $x + y = z$ (Schur), arbitrarily long arithmetic
progressions (van der Waerden), and a solution of any linear equation whose
coefficients have a subset summing to zero (Rado). Throughout, $[n]$ is
$\{1, \dots, n\}$, a set $A$ of integers is *sum-free* if there are no
$x, y, z \in A$ with $x + y = z$ ($x = y$ allowed), and an *arithmetic
progression of length $k$* is $a, a + d, \dots, a + (k - 1)d$ with $d \geq 1$.

> [!milestone] Schur's theorem
> For every $k \geq 1$ there is an $N$ such that every colouring of $[N]$ with $k$ colours has $x$, $y$ and $z$ of the same colour with $x + y = z$. One can take $N = R_k(3) - 1$, where $R_k(3) = R(3, \dots, 3)$ with $k$ threes. Equivalently, $[N]$ is not the union of $k$ sum-free sets.
>
> Sources: [@Schur1916]; [@GRS1990, Ch. 3]; [@LandmanRobertson2014, Ch. 8].
>
> Proof routes: given a colouring $\chi$ of $[N]$, colour the edge $\{i, j\}$ of the complete graph on $\{0, 1, \dots, N\}$ with $\chi(|i - j|)$. This graph has $N + 1 = R_k(3)$ vertices, so by Ramsey's theorem for many colours it has a monochromatic triangle $i < j < l$. Then $x = j - i$, $y = l - j$ and $z = l - i$ have the same colour and $x + y = z$.
>
> Formalization notes: this uses the milestone *Ramsey's theorem for many colours*. The vertex set `Fin (N + 1)` matches $\{0, \dots, N\}$.
>
> Lean status: Mathlib —.

> [!milestone] Small Schur numbers
> 1. $[4]$ is the union of two sum-free sets, $\{1, 4\}$ and $\{2, 3\}$, but $[5]$ is not the union of two sum-free sets.
> 2. $[13]$ is the union of three sum-free sets: $\{1, 4, 10, 13\}$, $\{2, 3, 11, 12\}$ and $\{5, 6, 7, 8, 9\}$.
>
> In terms of the Schur number $S(k)$, the largest $n$ such that $[n]$ is the union of $k$ sum-free sets, this says $S(2) = 4$ and $S(3) \geq 13$. Conventions differ by one: some sources define the Schur number as the least $n$ for which every colouring of $[n]$ with $k$ colours has a monochromatic solution. The upper bound $S(3) \leq 13$ is out of scope; its case analysis is much longer.
>
> Sources: [@LandmanRobertson2014, Ch. 8].
>
> Proof routes: finite checks. For $[5]$: say $1$ is red. Then $2 = 1 + 1$ is blue, so $4 = 2 + 2$ is red, so $3 = 4 - 1$ is blue, and $5$ can be neither red ($1 + 4$) nor blue ($2 + 3$). For (2), check the sums in each set.
>
> Formalization notes: both parts are small enough for `decide` once sum-freeness is stated on a `Finset ℕ` with bounded quantifiers.
>
> Lean status: Mathlib —.

> [!milestone] Van der Waerden's theorem
> For all $k, r \geq 1$ there is a $W$ such that every colouring of $[W]$ with $r$ colours has a monochromatic arithmetic progression of length $k$.
>
> Sources: [@vanderWaerden1927]; [@GRS1990, Ch. 2].
>
> Proof routes: from the Hales–Jewett theorem, which Mathlib has; do not reprove it.
> - Mathlib's `Combinatorics.Line.exists_mono_in_high_dimension` gives, for an alphabet $\alpha$ of size $k$ and $r$ colours, a finite type $\iota$ such that every colouring of $\iota \to \alpha$ has a monochromatic combinatorial line.
> - Take $\alpha = \{0, \dots, k - 1\}$ and colour a word $v$ by the colour of $1 + \sum_i v_i \in [W]$, where $W = (k - 1)\,|\iota| + 1$. A monochromatic line gives $a$, the number of its moving coordinates, and $b$, the sum of its fixed ones, such that $1 + b + a s$ has one colour for $s = 0, \dots, k - 1$.
> - Mathlib's `Combinatorics.exists_mono_homothetic_copy` does this argument for a colouring of a whole commutative monoid. Its statement does not give a finite $W$, but its proof is the model.
>
> Formalization notes: state the colouring as `ℕ → Fin r` and the progression as $a + d\,i \in [W]$ for $i < k$, or as a colouring of `Fin W`; `Fin W` makes the finiteness explicit.
>
> Lean status: Mathlib: the form for colourings of a whole monoid, `Combinatorics.exists_mono_homothetic_copy`. The statement for $[W]$ —.

> [!milestone] W(3;2) = 9
> Every colouring of $[9]$ with 2 colours has a monochromatic arithmetic progression of length 3. The colouring of $[8]$ as red, red, blue, blue, red, red, blue, blue has none. So the van der Waerden number $W(3;2)$ is 9.
>
> Sources: [@LandmanRobertson2014, Ch. 2].
>
> Proof routes:
> - Lower half: check the 12 progressions of length 3 in $[8]$.
> - Upper half: a case analysis. Swapping the colours, assume $5$ is red. Then branch on the colours of a few more numbers, and at each step colour the numbers that are forced: a number that completes a progression whose other two terms have one colour must have the other colour. Every branch ends in a monochromatic progression after a few steps. Alternatively, check all $2^9 = 512$ colourings, arranged so that the kernel's work stays small, for example by fixing the colour of $5$ first.
>
> Formalization notes: if the check is by `decide`, measure how long it takes; `AGENTS.md` asks for proofs that check quickly.
>
> Lean status: Mathlib —.

> [!milestone] Rado's theorem for one equation
> *A stretch goal: expected to be hard and to need several pull requests.*
>
> Let $n \geq 1$ and let $a_1, \dots, a_n$ be nonzero integers. The equation $a_1 x_1 + \cdots + a_n x_n = 0$ is *partition regular*, that is, every colouring of the positive integers with finitely many colours has a monochromatic solution in positive integers, if and only if some nonempty set of the $a_i$ sums to $0$.
>
> The hypothesis $n \geq 1$ is needed: with no coefficients, the empty tuple is a solution, vacuously of one colour, so the empty equation is partition regular, but there is no nonempty set of coefficients.
>
> Sources: [@Rado1933]; [@GRS1990, Ch. 3]; [@LandmanRobertson2014, Ch. 9].
>
> Proof routes:
> - Only if: let $p$ be a prime larger than $|a_1| + \cdots + |a_n|$, and colour a positive integer by its last nonzero digit in base $p$. In a monochromatic solution, of colour $d$, let $e = \min_i v_p(x_i)$ and $I = \{ i : v_p(x_i) = e \}$. Divide the equation by $p^e$ and reduce it modulo $p$: the terms with $i \notin I$ vanish, and those with $i \in I$ leave $d \sum_{i \in I} a_i \equiv 0 \pmod p$. Since $d \not\equiv 0$, $\sum_{i \in I} a_i \equiv 0 \pmod p$, and since its absolute value is less than $p$, it is $0$.
> - If: let $I$ be a nonempty set with $\sum_{i \in I} a_i = 0$. If $I$ is every index, any $x_1 = \dots = x_n$ is a solution. Otherwise let $A = \sum_{j \notin I} a_j$, $g = \gcd(a_i : i \in I)$ and $s = g$, and choose integers $\lambda_i$ with $\sum_{i \in I} a_i \lambda_i = -sA$, which exist since $g$ divides $sA$. Adding the same integer to every $\lambda_i$ leaves the sum unchanged, because $\sum_{i \in I} a_i = 0$, so take all $\lambda_i$ in $[0, l]$ for some $l$. Now use van der Waerden's theorem in a strengthened form: for all $r$, $l$ and $s$, every colouring of a long enough $[N]$ with $r$ colours has $a, a + d, \dots, a + ld$ and $sd$ all of one colour, which follows from van der Waerden's theorem by induction on $r$. Then $x_i = a + \lambda_i d$ for $i \in I$ and $x_j = sd$ for $j \notin I$ is a monochromatic solution: $\sum_{i \in I} a_i (a + \lambda_i d) + A s d = a \cdot 0 + d(-sA) + Asd = 0$.
>
> Formalization notes: this depends on the milestone *Van der Waerden's theorem*. The colouring by last nonzero digit uses `Nat.factorization` or `padicValNat` and `Nat.digits`.
>
> Lean status: Mathlib —.
