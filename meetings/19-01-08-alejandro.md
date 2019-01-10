I think during the meeting we discussed two different issues, which are related in the case of comparing students against model programs, but have different answers.

## Question 1: can we extend "syntactic equivalence" a bit more?

From my point of view, the equalities we care about can be of two different forms. Some of them are "internal" to the theory of lambda-calculus. In this group I include:
- alpha conversion;
- beta reduction, (\x. e) t = e [x/t];
- eta expansion, t = \x. t x;
- identity function, id x = x, or id = \x. x;
- composition function, (f . g) x = f (g x), or f . g = \x. f (g x);
- behavior of lets.

Higher-order unification deals with the first three, and for some subsets of the problem we have a deterministic algorithm for doing so. What I proposed in the first half of the meeting was an algorithm to extend matching to cover the rest of the things, by combining two approaches:
- lazy eta expansion and beta reduction, by comparing the two programs we want to look at;
- the matching procedure may return a set of "yet-to-be-proven" equalities, for which we try to generalize a shape of the equality and then try to prove that this equality was OK.

This question just concerns with treating the following programs as equivalent:

> f xs = map (\x. x+1) xs
> f = map g where { g = \x. x+n ; n = 1 }

This would be posed as a unification problem (a unification variable represents a hole). This means that the result is a substitution theta such that theta S = M.

## Question 2: extending that algorithm with further equivalences (in addition to what is discussed by Bastiaan)

We have discussed two approaches for this:
1. compute a normal form for both S and M, and then compare them;
2. compare both programs and have a more ad-hoc procedure about rewriting S and M.

The main trade-offs between both approaches are:
- completeness and predictability: hopefully we could define a subset of programs for which (1) always gives the right answer. Option (2) requires the teacher to also define a heuristic about when a rule fires, which could lead to surprising results (a rule doesn't fire, or fires infinitely often).
- explainability: option (1) is a mechanical process, but some of the steps could be very different to what a programmer would do to refactor the program. Option (2) allows for the possibility to apply a rule only when it is "natural" or "intuitive" (but those are very ill-defined terms).