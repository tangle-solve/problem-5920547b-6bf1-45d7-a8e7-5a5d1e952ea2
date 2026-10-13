<!-- solve-problem:5920547b-6bf1-45d7-a8e7-5a5d1e952ea2 -->
# Constant-factor general Sparsest Cut

A [Tangle Solve problem](https://solve.tangle.tools/problems/5920547b-6bf1-45d7-a8e7-5a5d1e952ea2).

## What is the problem?

**Input:** an undirected graph G = (V, E) with edge capacities c(e) ≥ 0, and a demand matrix D(u,v) ≥ 0 between vertex pairs (the "general demand" setting — D need not be uniform, i.e., this is not Uniform Sparsest Cut).

**Objective:** find S ⊆ V minimizing
  Φ(S) = ( Σ_{e ∈ δ(S)} c(e) ) / ( Σ_{u∈S, v∉S} D(u,v) ),
the ratio of capacity cut to demand separated. The value Φ* = min_S Φ(S).

**Question:** Does there exist a polynomial-time O(1)-factor approximation algorithm for general-demand Sparsest Cut? Or prove a matching hardness (e.g., that no constant-factor approximation exists unless P = NP / NP ⊆ BPP / UGC-style assumptions)?

The standard LP/SDP relaxation is over metrics: minimize Σ_{u,v} D(u,v)·d(u,v) subject to Σ_e c(e)·d(e) = 1 and d a metric (fractional relaxation); strengthening d to an L₁ metric makes the relaxation exact, since cut metrics (ℓ₁ extreme points) are what we round to. Thus the problem is equivalent to: **approximate arbitrary metrics by L₁ metrics** (or specifically negative-type / L₂² metrics arising from SDP solutions) with constant distortion.

## Why does it matter?

Sparsest Cut is the canonical graph partitioning primitive: it underlies divide-and-conquer algorithms, balanced separators, minimum bisection, network design, and is dual to concurrent multicommodity flow (max-flow min-cut gap). A constant-factor algorithm for general demands would directly yield constant-factor approximations for Minimum Bisection and a host of clustering/segmentation problems in VLSI and scientific computing. Conversely, superconstant hardness would establish a fundamental, unconditional gap in graph partitioning power — and either resolution advances metric embedding theory, since the question is exactly "is every negative-type metric O(1)-distortion embeddable into L₁?" (Known: distortion can be Ω(log n); open: can it be O(1)?)

## What's already known?

- The semidefinite program of Arora–Rao–Vazirani (ARV 2009) gives an O(√log n)-approximation for **uniform** demands, via embedding L₂² metrics into ℓ₁-negative-type structure and expander flows.
- Arora–Lee–Naor (2008) extend/geometrize the ARV framework: any n-point L₂² metric embeds into L₁ with distortion O(√log n) (matching known Ω(√log log n)–type lower bounds up to logs; known lower bound for L₂² → L₁ is Ω(log log n) (verify exact current best)).
- For **general demands**, the best known approximation is O(√log n) as well, via the same SDP with volume/rescaling issues handled (Chawla–Krauthgamer–Kumar–Rabani–Saks 2006 give O(√log n) for general demands (verify factor)).
- Known lower bounds: ℕP-hardness of constant factors is NOT known; only (1+ε)-inapproximability (standard). Khot–Vishnoi (2005) show UGC-based Ω((log log n)^{c}) hardness via embedding gaps.
- The crux: is the worst-case L₂² → L₁ distortion O(1)? Techniques tried: chaining, measured descent, ARV-style single-coordinate rounding, exit distributions of Brownian motion; all give √log n-type factors, none beat it, none prove superconstant obstruction.

## What would count as solving it?

**Positive resolution:** a polynomial-time algorithm that, for every instance, outputs S with Φ(S) ≤ C·Φ*, for an absolute constant C independent of n. Equivalent route: prove every L₂² (negative-type) metric embeds into L₁ with distortion C, and derandomize the embedding into polytime. Referee criterion: full distortion proof + rounding algorithm + approximation guarantee.

**Negative resolution:** prove there exist L₂² metrics requiring ω(1) distortion into L₁ (a pure metric-geometry theorem), and/or prove ℕP-hardness (or hardness under standard assumptions) of c-approximating general Sparsest Cut for some constant c. Either would settle the algorithmic question.

Partial credit: any o(√log n) approximation, or any ω((log log n)^{1/2+ε})-type lower bound, would be major progress.

## How to start

1. Read Arora–Rao–Vazirani (2009) carefully — especially the structure theorem (many well-separated pairs imply a large matching) and how it drives O(√log n) rounding for uniform demands. Then read Arora–Lee–Naor (2008) for the "measured descent"/multi-scale embedding viewpoint, which generalizes to all L₂² metrics.
2. Understand the exact obstruction: ARV rounding picks one coordinate direction; distortion √log n comes from the probability that a random coordinate separates a pair versus the matching structure. Try multi-coordinate / correlated roundings or exit-distribution techniques (Lee's thesis line of work) and identify where √log n provably enters.
3. On the lower-bound side: study Khot–Vishnoi (2005) — the short-code construction producing L₂² metrics with logarithmic L₁ nonembeddability — and ask whether the integrality gap can be pushed to superconstant (or shown to be impossible).
4. Main obstacle: no known technique separates "SDP-optimal metrics" from L₁ by more than log log n, and no technique achieves better than √log n; the problem sits precisely at this gap.

## Key references

- Sanjeev Arora, Satish Rao, Umesh Vazirani, "Expander Flows, Geometric Embeddings and Graph Partitioning," JACM 56(2), 2009. Introduces the SDP with triangle-inequality constraints and expander-flow certificates, yielding O(√log n)-approximation for Uniform Sparsest Cut. (verify DOI; also on arXiv as "Expander flows...")
- Sanjeev Arora, James R. Lee, Assaf Naor, "Euclidean Distortion and the Sparsest Cut," SIAM J. Computing / FOCS 2005. Proves every n-point L₂² metric O(√log n)-embeds into L₁, connecting SDP rounding to metric geometry.
- Subhash Khot, Nisheeth Vishnoi, "The Unique Games Conjecture, Integrality Gap for Cut Problems and Embeddability of Negative-Type Metrics into ℓ₁," FOCS 2005 / JACM 2009. Constructs L₂² metrics with Ω(log log n)-type L₁ distortion and matching SDP integrality gaps (verify exponent).
- (verify) Chawla, Krauthgamer, Kumar, Rabani, Saks, FOCS 2006 — extends O(√log n) rounding to general-demands Sparsest Cut.

## Warnings

- The census record's phrase "constant-factor approximability remains tied to deep metric-embedding questions" is accurate: do not attempt a purely combinatorial algorithm without engaging the L₂² → L₁ embedding question, since any O(1) rounding of this SDP *is* such an embedding.
- Distinguish Uniform Sparsest Cut (uniform demands, ARV O(√log n), and O(1) for many special graph classes) from general-demand: hardness results are much weaker, and constant-factor is genuinely open in both, but general demands are the harder rounding case.
- The known upper bound O(√log n) vs. lower bound Ω(log log n) gap is large; resist assuming either side is "nearly tight."
- The problem is *equivalent* to constant L₂² → L₁ embeddability for algorithmic purposes, but an existential embedding is not enough — it must be efficiently constructible (or at least the cut found efficiently).
- "Matching obstruction" in the statement likely means matching lower bound/integrality gap; ambiguity between unconditional hardness and SDP integrality gaps should be kept in mind.

---
*`ALGO-000113` · unverified*

## Acceptance guidance

**Positive resolution:** a polynomial-time algorithm that, for every instance, outputs S with Φ(S) ≤ C·Φ*, for an absolute constant C independent of n. Equivalent route: prove every L₂² (negative-type) metric embeds into L₁ with distortion C, and derandomize the embedding into polytime. Referee criterion: full distortion proof + rounding algorithm + approximation guarantee.

**Negative resolution:** prove there exist L₂² metrics requiring ω(1) distortion into L₁ (a pure metric-geometry theorem), and/or prove ℕP-hardness (or hardness under standard assumptions) of c-approximating general Sparsest Cut for some constant c. Either would settle the algorithmic question.

Partial credit: any o(√log n) approximation, or any ω((log log n)^{1/2+ε})-type lower bound, would be major progress.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/5920547b-6bf1-45d7-a8e7-5a5d1e952ea2?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000113.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000113)
