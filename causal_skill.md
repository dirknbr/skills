# Pearl's Causal Inference Theorems & Rules

This document outlines the foundational theorems, criteria, and calculus rules developed by Judea Pearl for causal inference, represented through Directed Acyclic Graphs (DAGs) and Structural Causal Models (SCMs).

---

## 1. The Causal Hierarchy (Ladder of Causation)

Pearl categorizes causal queries into three distinct levels of complexity. A higher level cannot be answered using only data and methods from a lower level without making causal assumptions.

*   **L1: Association (Seeing):** What is? Involves statistical correlations. Denoted by $P(y | x)$.
*   **L2: Intervention (Doing):** What if we do? Involves changing the system. Denoted by $P(y | \text{do}(x))$.
*   **L3: Counterfactuals (Imagining):** What if we had done? Involves retrospective reasoning. Denoted by $P(y_x | x', y')$.

---

## 2. Structural Causal Models (SCM) & Fundamental Law

An SCM consists of endogenous (observed) variables $V$, exogenous (unobserved) variables $U$, and a set of functions $F$ determining $V$ from $U$.

*   **The Fundamental Law of Counterfactuals:** 
    $$Y_x(u) = Y_{M_x}(u)$$
    This theorem states that the counterfactual value of $Y$ under intervention $\text{do}(X=x)$ for individual $u$ is exactly the solution for $Y$ in the modified submodel $M_x$ (where the structural equation for $X$ is replaced by the constant $x$).

---

## 3. d-Separation Theorem

d-separation (directional separation) allows us to read conditional independencies directly from a causal graph $G$. If two sets of nodes $X$ and $Y$ are d-separated by $Z$ in $G$, then they are statistically independent conditional on $Z$.

A path $p$ is **blocked** by a set of nodes $Z$ if and only if:
1.  $p$ contains a **chain** ($A \rightarrow B \rightarrow C$) or a **fork** ($A \leftarrow B \rightarrow C$) such that the middle node $B$ is in $Z$.
2.  $p$ contains a **collider** ($A \rightarrow B \leftarrow C$) such that the middle node $B$ is NOT in $Z$, and no descendant of $B$ is in $Z$.

**Theorem:** If sets $X$ and $Y$ are d-separated by $Z$ in a DAG $G$, then:
$$(X \perp\!\!\!\perp Y | Z)_G \implies P(x, y | z) = P(x | z) P(y | z)$$

---

## 4. The Backdoor Criterion

The backdoor criterion identifies which variables we must adjust for to isolate the pure causal effect of $X$ on $Y$, removing confounding bias.

A set of variables $Z$ satisfies the backdoor criterion relative to an ordered pair of variables $(X, Y)$ in a DAG $G$ if:
1.  No node in $Z$ is a descendant of $X$.
2.  $Z$ blocks every path between $X$ and $Y$ that contains an arrow pointing into $X$ (these are the "backdoor" paths).

**Backdoor Adjustment Theorem:** If $Z$ satisfies the backdoor criterion for $(X, Y)$, the causal effect of $X$ on $Y$ is given by:
$$P(y | \text{do}(x)) = \sum_z P(y | x, z) P(z)$$

---

## 5. The Frontdoor Criterion

The frontdoor criterion allows for the estimation of causal effects even in the presence of unmeasured confounders, provided there is a suitable mediating variable.

A set of variables $Z$ satisfies the frontdoor criterion relative to $(X, Y)$ if:
1.  $Z$ intercepts all directed paths from $X$ to $Y$.
2.  There is no unblocked backdoor path from $X$ to $Z$.
3.  All backdoor paths from $Z$ to $Y$ are blocked by $X$.

**Frontdoor Adjustment Theorem:** If $Z$ satisfies the frontdoor criterion for $(X, Y)$, and if $P(x, z) > 0$, then the causal effect of $X$ on $Y$ is:
$$P(y | \text{do}(x)) = \sum_z P(z | x) \sum_{x'} P(y | x', z) P(x')$$

---

## 6. The Rules of do-Calculus

The *do*-calculus consists of three algebraic rules used to map interventional distributions to observational distributions. 

**Notation for Modified Graphs:**
*   $G_{\overline{X}}$: The graph obtained by deleting all arrows pointing *into* nodes in $X$.
*   $G_{\underline{X}}$: The graph obtained by deleting all arrows emerging *out of* nodes in $X$.
*   $Z(W)$: The set of nodes in $Z$ that are not ancestors of any node in $W$ in the graph $G_{\overline{X}}$.

### Rule 1: Insertion/Deletion of Observations
We can ignore an observation $Z$ if it is irrelevant to $Y$ given we have intervened on $X$ and observed $W$.
$$P(y | \text{do}(x), z, w) = P(y | \text{do}(x), w) \quad \text{if } (Y \perp\!\!\!\perp Z | X, W)_{G_{\overline{X}}}$$

### Rule 2: Action/Observation Exchange
We can replace an intervention $\text{do}(Z)$ with an observation $z$ if $Z$ acts as an interceptor blocking all backdoors from $Z$ to $Y$.
$$P(y | \text{do}(x), \text{do}(z), w) = P(y | \text{do}(x), z, w) \quad \text{if } (Y \perp\!\!\!\perp Z | X, W)_{G_{\overline{X}\underline{Z}}}$$

### Rule 3: Insertion/Deletion of Actions
We can entirely remove an intervention $\text{do}(Z)$ if $Z$ has no causal path to $Y$ that isn't already blocked by $X$ and $W$.
$$P(y | \text{do}(x), \text{do}(z), w) = P(y | \text{do}(x), w) \quad \text{if } (Y \perp\!\!\!\perp Z | X, W)_{G_{\overline{X}\overline{Z(W)}}}$$

**Completeness Theorem:** The three rules of *do*-calculus are complete. If a causal effect $P(y | \text{do}(x))$ is identifiable from a given DAG and observational data, it can be derived by iterative application of these three rules.

## 7. Instrumental Variables (IV)

When unmeasured confounders block both backdoor and frontdoor adjustments, causal effects can sometimes still be evaluated using an Instrumental Variable ($Z$). 

**Graphical Conditions for an Instrument $Z$:**
In a DAG, a variable $Z$ qualifies as an instrument for the causal effect of $X$ on $Y$ if it satisfies three conditions:
1.  **Relevance:** $Z$ has a causal effect on $X$ ($Z \not\!\perp\!\!\!\perp X$).
2.  **Exclusion Restriction:** $Z$ affects $Y$ *only* through its effect on $X$. There is no direct directed path from $Z$ to $Y$.
3.  **Unconfoundedness:** $Z$ and $Y$ share no unobserved common causes (no open backdoor paths between $Z$ and $Y$).

**Identification via IV:**
*   **Linear Models:** If the structural equations are linear, the causal effect of $X$ on $Y$ ($\beta$) is point-identified as the ratio of covariances (the Wald estimator):
    $$\beta = \frac{\text{Cov}(Z, Y)}{\text{Cov}(Z, X)}$$
*   **Nonparametric Models:** In fully nonparametric SCMs, an IV does not yield point identification of the average causal effect. Instead, it yields strict algebraic upper and lower limits known as the **Balke-Pearl Bounds**.

## Software 

You can use the `is_directed_acyclic_graph` function in the networkx library to understand cycles.

The same package has functions on D separation https://networkx.org/documentation/stable/reference/algorithms/d_separation.html.

- https://github.com/jtextor/dagitty


## References

You can also read these sources to get more info

- Pearl 2010 https://ftp.cs.ucla.edu/pub/stat_ser/r355-corrected-reprint.pdf
- Pearl 2009 https://www.cs.princeton.edu/courses/archive/fall09/cos597A/papers/Pearl2009.pdf
- https://www.statslab.cam.ac.uk/~qz280/teaching/causal-2023/notes-2021.pdf
- https://en.wikipedia.org/wiki/Simpson%27s_paradox
- https://www.pywhy.org/dowhy/v0.11/user_guide/modeling_causal_relations/index.html
- https://mixtape.scunning.com/04-potential_outcomes
- https://pmc.ncbi.nlm.nih.gov/articles/PMC12206152/