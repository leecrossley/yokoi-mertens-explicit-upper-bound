# How long can a prime product stay above its Mertens curve?

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.24410811.svg)](https://doi.org/10.5281/zenodo.24410811)

This result is now maintained in [quadratic-mertens-first-crossings](https://github.com/leecrossley/quadratic-mertens-first-crossings/tree/main/results/yokoi-mertens-explicit-upper-bound). This repository is archived and keeps its releases and DOI.

As more prime ideals enter a product, its value rises in jumps. Mertens' theorem predicts a smooth curve for its growth. Daniel R. Johnston asked how far you may have to go before that curve overtakes the product in a family of number fields. He posed the question during Ethan Lee's talk at the 2024 Comparative Prime Number Theory Symposium. [Problem 8, pp. 4–5](https://arxiv.org/pdf/2407.03530#page=4)

The bound for a field of discriminant $d$ assumes GRH for its Dedekind zeta function $\zeta(s)L(s,\chi_d)$. Alpöge's announced Dirichlet GRH theorem supplies this hypothesis for both factors. [Announcement](https://alpo.ge/remark.pdf) [Formal verification](https://math.mit.edu/~drew/grh/)

**For every real quadratic field of discriminant $d$, the product is strictly below the curve at some point before**

$$
U(d)=2^{\thinspace2^{\thinspace d^{30000}}}.
$$

For $K=\mathbb Q(\sqrt d)$ of fundamental discriminant $d$, the product is $P_K(x)=\prod_{N\mathfrak p\le x}(1-1/N\mathfrak p)^{-1}$ and the curve is $e^{\gamma_E}\kappa_K\log x$, where $\kappa_K$ is the residue of the Dedekind zeta function at 1. For the first crossing we take the infimum over $x\ge2$ of points of strict negative error, counting ideals at their norm. If the error is already negative at 2 the infimum is 2. Equality at an interior boundary can be followed immediately by negative values. The proof gives a point of strict inequality $P_K(x)<e^{\gamma_E}\kappa_K\log x$ with $e^2<x<U(d)$, beyond the first ideal norm (at most 4). The same formula covers every member of Yokoi's family, including arbitrarily late first crossings. The choice of Yokoi's family was ours. Johnston asked for a family in general. [Proof](PROOF.md) [Original problem](https://arxiv.org/pdf/2407.03530#page=5)

For one sense of scale, take $9413=97^2+4$. Its first crossing is between 525.474356978253 and 525.474356978254. The general bound is enormous even here. Its purpose is to give a finite deadline from the discriminant alone for every field, including those left outside a density-one conclusion. [Earlier family result and census](https://github.com/leecrossley/yokoi-mertens-crossing-bounds/releases/tag/v1.0.0)

## How the proof works

Start just above $x=1$, before even the first prime ideal could enter. In the prime ideal explicit formula, a known logarithmic term grows without bound there. The sum over zeta zeros has to cancel it. After smoothing, only finitely many frequencies matter to a prescribed accuracy. A counting argument gives a time by which those frequencies return close to their starting positions. By then the logarithmic term has decayed. This forces a deficit in the count of prime powers large enough to put the product below its curve. Tracking the number of frequencies and the error gives the deadline. [Full argument and sharper bound](PROOF.md)

The proof uses the Dirichlet zero-location theorem announced by Levent Alpöge on 20 November 2026. Andrew Sutherland reports independent kernel checks and statement matching. The quadratic factorization reduces the zero-location input to exactly that statement. The rest is a written analytic proof, with the finite simultaneous approximation argument and exponent estimates also checked in Lean. Hathi and Lee had already proved fixed-field oscillation under a zero condition. The contribution here is the explicit discriminant-only deadline. [Upstream audit](https://math.mit.edu/~drew/grh/) · [Hathi and Lee](https://arxiv.org/pdf/2112.02166#page=3) · [Prior work and verification scope](PRIOR-WORK.md)

The later conductor bound gives a smaller explicit deadline under the same field GRH hypothesis. For computed first crossings the census gives the values from 2 and identifies the largest field through discriminant 10,000. [Smaller deadline](https://github.com/leecrossley/quadratic-mertens-conductor-bound) [Finite census](https://github.com/leecrossley/yokoi-mertens-crossing-bounds)

## Verification

Print the sharper bound in exact symbolic form for a positive fundamental discriminant. The calculator takes the integer supplied as its size parameter. Field membership is a hypothesis of the theorem. Python 3.10 or later suffices.

~~~sh
python3 bound.py 9413
~~~

To reproduce the finite interval checks and verify the release manifest, run the commands below in the release directory. The checks evaluate the constant term in the explicit formula independently at zero and one, bound the difference between the two signed errors by 3 at 35 arguments and check them against the empty product for $d=5$ at $x=7/2$, and replay the full first-crossing window calculation for $d=9413$.

~~~sh
python3 -m venv .venv
. .venv/bin/activate
python -m pip install -r requirements.txt
./verify-release.sh
~~~

For the Lean replay, use the official Mathlib v4.33.1 checkout at commit 0df444a360eaa60ab8c11dca51a86af692955474 with its cache. Download the [pinned Statements.lean](https://raw.githubusercontent.com/Deicyde/riemann-hypothesis/40e56bab026552e089d7afcfeff08d2defb9c005/src/Statements.lean). From that Mathlib checkout run

~~~sh
lake env python3 /absolute/path/to/check_lean.py /absolute/path/to/Statements.lean
~~~

The script verifies the statement-file hash, compiles the local proofs and checks their axiom reports. Recorded output is available for both replays. [Finite checks](evidence/check.txt) · [Lean checks](evidence/lean.txt) · [Checksums](SHA256SUMS)
