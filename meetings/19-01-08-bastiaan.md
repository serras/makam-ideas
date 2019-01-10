Let M be a model program and S and student program. A student program can have holes: each hole can be identified uniquely. Model programs cannot have holes.

## Question 1: are M and S semantically equivalent (i.e., are their behaviors the same at run-time)?

This question is in general undecidable, because of recursion. Property-based testing can be used to find a counter-example showing that the programs are not equivalent.

Let E be as set of equalities (identities, laws, etc.) on programs/expressions that preserve semantic equivalence. Examples of such equalities are:
- alpha conversion, beta reduction, eta expansion
- definitions, such as id = \x -> x
- desugarings, such as [x] = x:[]
- laws on function and operators, such as x+(y+z) = (x+y)+z

Equivalences are by definition undirected. Each set of equivalences gives an equivalence relation (written =E).

## Question 2: is M =E S (i.e., are the programs the same under equivalences E)?

This is still an undecidable problem, but careful engineering the set of equivalences and deciding which equalities to use (and which to drop) can make it computable, especially for smaller programs. Let us assume we have an algorithmic decision procedure A. This procedure should ideally be sound and complete with respect to =E (the specification).

Algorithm A can be approached by turning E into a Term Rewrite System: all equalities are directed in one way or the other, termination is guaranteed by defining a weight function that shows that each step makes the term strictly smaller, and the rewrite system must be completed (by the Knuth-Bendix completion algorithm) to arrive at a confluent set of rules. Relation =E can then be checked by calculating normal forms. The TRS approach has a strong theory, but is very restricted in the rule sets it can handle. Recursion in programs is problematic, and so is associativity/commutativity and equalities that cannot (easily) be expressed as rewrite rules (e.g. equalities involving binding/scoping). An ad-hoc decision procedure to deal with these cases might be useful, or a combination of a TRS with some ad-hoc rules.

Suppose student program S has holes. This turns the decision procedure for question 2 into a matching problem: given M and S, find a refinement sub (a mapping from holes to programs) such that M  =E  sub(S). There can be multiple substitutions. Matching can still be approached using a TRS or an ad-hoc algorithm, but the holes seem to further complicate (or restrict) the analysis.

## Question 3: how can S be refactored into M (using equalities E as steps)?

Such a refactoring is in fact a proof that S and M are equivalent under E. This question seems to be relevant in particular for student programs without holes. One such refactoring is to follow the steps for reaching the normal form (and to go back). This refactoring, however, is not intuitive: the steps will be mechanical, with superfluous steps, on a too low level of granularity. Ideally, we search for short-cuts and use steps at an appropriate level of granularity. What ‘intuitive’ means and what the right step-size would be has to be further examined (and specified). There are some similarities here with the logical equivalence proofs (with short-cuts) by Lodder.

# Response by Johan

Bastiaan mentioned the possibility to use recursive structures as some kind of normal forms. For a while I though this paper might be a source to use for the normal form:

https://dl.acm.org/citation.cfm?id=2676989

but I’ve had at least two master students looking at it, and it isn’t clear that the paper is useful for this purpose at all.

Some observations: 
- if I remember correctly from the above paper, to find a recursion scheme, you need to distinguish several algebras and coalgebras, and it is not obvious how you find them. But maybe there is a limited amount of possibilities for every exercise?
- calculating this normal form might be less useful for giving hints to students, but once we have a suitable normal form we might `fuse’ some of the steps that prove the normal form to obtain useful hints
- Alejandro’s approach might be this fused form, who knows