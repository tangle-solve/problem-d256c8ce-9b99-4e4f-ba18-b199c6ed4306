<!-- solve-problem:d256c8ce-9b99-4e4f-ba18-b199c6ed4306 -->
# Improve the Densest-k-Subgraph exponent

A [Tangle Solve problem](https://solve.tangle.tools/problems/d256c8ce-9b99-4e4f-ba18-b199c6ed4306).

## What is the problem?

**Densest k-Subgraph (DkS):** Given an undirected graph G = (V, E) with n = |V| vertices and an integer k, find a subset S ⊆ V with |S| = k maximizing the number of edges induced by S, i.e., maximize e(S). The decision version ("is there a k-subset with at least K edges?") is NP-complete (contains Clique).

**Approximation:** An α-approximation returns a k-subset inducing at least OPT/α edges. The best known polynomial-time approximation ratio is n^{1/4+ε} for some small ε > 0, from the algorithm of Bhaskara, Charikar, Chlamtac, Feige, and Vijayaraghavan (2010), which achieves roughly O(n^{1/4}). Trivially, greedy/local-search gives O(n/k) or so, and LP-based approaches give O(√n) — all worse.

**The open problem:** Achieve an n^{1/4−δ}-approximation for some fixed constant δ > 0 in polynomial time. Note the asymmetry: improving the *constant after 1/4* does not suffice; one must beat the exponent 1/4 itself. The output must be a k-vertex subgraph (exact cardinality, or within standard slack if you define the variant explicitly).

**Best-known hardness (weak):** Feige (2002) conjectured no n^{ε}-approximation exists for some ε > 0 (based on a random 3SAT assumption); Manurangsi (2017) proved quasi-polynomial-time hardness of approximating beyond n^{1/(log log n)^{O(1)}}-type ratios under ETH-type assumptions. Nothing rules out a polynomial n^δ-approximation for every δ > 0 — the problem is genuinely wide open in both directions.

## Why does it matters

An improved approximation would directly upgrade algorithms for dense community detection, motivated by network science, fraud/anomaly detection, and bioinformatics. Technically, DkS is the canonical problem that escapes the standard LP/SDP/sum-of-squares toolbox — any improvement likely requires a fundamentally new algorithmic technique (or a new structural characterization of dense subgraphs), which would ripple across constraint satisfaction and graph approximation more broadly. Hardness progress would be equally significant: it would settle Feige's conjecture in spirit and separate polynomial from quasi-polynomial approximation.

## What's already known?

- **O(n^{1/4+ε}) algorithm (Feige, Kortsarz, Peleg 1997; refined by Feige, Langberg 2001):** The classical greedy/peeling-based algorithm of Feige–Kortsarz–Peleg achieves n^{1/4−o(1)} in expectation; the log-density framework of Bhaskara et al. (2010) gives O(n^{1/4}) and is analyzed via the "log-density" parameter — the algorithm recursively samples vertices proportional to degree and the analysis buckets instances by their optimum's log-density.
- **LP/SDP failures:** Standard Sherali–Adams and Lasserre hierarchies do not beat n^{1/4}-type ratios; integrality gaps of n^{Ω(1)} persist even at fairly high levels (Bhaskara et al. 2010 construct log-density gap instances; later work shows SA/Lasserre gaps).
- **Hardness:** Under Feige's random 3SAT assumption, no n^{ε}-approximation (Feige 2002). Under Gap-ETH/ETH, n^{1/(log log n)^{O(1)}}-hardness in quasi-polynomial time (Manurangsi 2017). Chlamtáč, Dinitz, Makarychev et al. have related lower-bound programs (e.g., hardness for small-set expansion connections, hypergraph variants like Min DkS).
- **Why approaches fall short:** The log-density framework is tight against its own analysis — the n^{1/4} barrier appears exactly where the recursive sampling analysis loses, and no known convex relaxation captures the missing structure.

## What would count as solving it?

- **Primary (upper bound):** A randomized or deterministic polynomial-time algorithm and proof that, on every n-vertex graph, it outputs a k-subset S with E[e(S)] ≥ OPT/n^{1/4−δ} for a fixed constant δ > 0. Any δ > 0, however tiny, counts.
- **Secondary (lower bound):** A proof, under a well-established assumption (e.g., Gap-ETH, or Feige's assumption), that no polynomial-time n^{1/4−δ}-approximation exists — this *resolves* the problem in the negative direction and explains the barrier.
- **Partial credit / referee-worthy intermediate results:** (a) n^{1/4−δ} for restricted graph families (e.g., bounded log-density, triangle-free graphs); (b) beating n^{1/4} on the log-density ladder at fixed levels with explicit tradeoffs; (c) integrality gaps showing specific new relaxations fail.

## How to start

1. **Read Bhaskara–Charikar–Chlamtáč–Feige–Vijayaraghavan (2010), Sections on the log-density framework and the O(n^{1/4}) algorithm.** Understand precisely where the n^{1/4} loss arises in the recursive sampling analysis; this is where any improvement must come from.
2. **Read Feige–Kortsarz–Peleg (1997)** for the original greedy algorithm, and Manurangsi (2017) for the hardness landscape. The gap between n^{1/4} and quasi-poly hardness is the target zone.
3. **Natural approach:** try to combine log-density-based sampling with an SDP/soS certificate of local density, or analyze the existing algorithm on structured instances (bounded degree, excluded minors) to see if the analysis can be tightened — the known analysis may be lossy on many regimes.
4. **Main obstacle:** there is no known relaxation or combinatorial argument that distinguishes "hard" gap instances from general instances at ratios below n^{1/4}; a solution likely needs a genuinely new idea, not tuning.

## Key references

- A. Bhaskara, M. Charikar, E. Chlamtáč, U. Feige, A. Vijayaraghavan, "Detecting High Log-Densities: An O(n^{1/4}) Approximation for Densest k-Subgraph," STOC 2010. Introduces the log-density framework and gives the best-known n^{1/4+ε} approximation. (verify exact ε and journal version details)
- U. Feige, G. Kortsarz, D. Peleg, "The Dense k-Subgraph Problem," Algorithmica 29(3), 2001 (STOC 1997). Original greedy n^{1/4−o(1)}-style approximation and problem formulation.
- U. Feige, "Relations between average case complexity and approximation complexity," STOC 2002. Introduces the conjecture that DkS is hard to approximate within n^{ε} under a random 3SAT assumption.
- P. Manurangsi, "Almost-Polynomial Ratio ETH-Hardness of Approximating Densest k-Subgraph," STOC 2017. Proves n^{1/(log log n)^{O(1)}} quasi-polynomial-time hardness under Gap-ETH.
- (verify) Work on Lasserre/Sherali–Adams integrality gaps for DkS by Bhaskara et al. and subsequent papers by Chlamtáč, Dinitz, and coauthors on hypergraph/min-DkS variants.

## Warnings

- **Don't chase the constant:** beating O(n^{1/4}) by improving constants or the +ε does not solve the problem; the exponent itself must drop below 1/4.
- **Output constraint:** the solution must return exactly k vertices (or state your slack convention explicitly); algorithms returning arbitrary-size dense subgraphs solve a different (easier, O(1)-approximable) problem — the densest subgraph problem without the k constraint.
- The census record's n^{1/4+ε} phrasing is slightly off: the correct statement is that O(n^{1/4+ε}) is known and n^{1/4−δ} is the target.
- Beware claimed improvements in the literature to O(n^{1/4}) "exactly" — check whether the analysis holds for all n and k or only in regimes; many claimed tightenings are folklore or restricted.
- Hardness results under nonstandard assumptions are plentiful; don't mistake a conditional lower bound for resolving the algorithmic question.

---
*`ALGO-000114` · unverified*

## Acceptance guidance

- **Primary (upper bound):** A randomized or deterministic polynomial-time algorithm and proof that, on every n-vertex graph, it outputs a k-subset S with E[e(S)] ≥ OPT/n^{1/4−δ} for a fixed constant δ > 0. Any δ > 0, however tiny, counts.
- **Secondary (lower bound):** A proof, under a well-established assumption (e.g., Gap-ETH, or Feige's assumption), that no polynomial-time n^{1/4−δ}-approximation exists — this *resolves* the problem in the negative direction and explains the barrier.
- **Partial credit / referee-worthy intermediate results:** (a) n^{1/4−δ} for restricted graph families (e.g., bounded log-density, triangle-free graphs); (b) beating n^{1/4} on the log-density ladder at fixed levels with explicit tradeoffs; (c) integrality gaps showing specific new relaxations fail.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/d256c8ce-9b99-4e4f-ba18-b199c6ed4306?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000114.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000114)
