# The-Projection-Transfers-Spectral-Geometry


Abstract

Transfer learning is commonly treated as an empirical phenomenon, with effectiveness assessed through downstream performance and attributed to representation similarity, model scale, or task relatedness. In contrast, we formulate transfer learning as a rank-limited spectral projection between learned representations, isolating structural compatibility from optimization effects. We define transfer fidelity as the fraction of target variance captured by a rank- projector constructed from the source representation’s covariance spectrum.

Using controlled experiments on synthetic manifolds and high-dimensional subspaces, we show that transfer fidelity is governed by shared intrinsic dimensionality, exhibiting a characteristic knee as projection rank increases. Below this knee, transfer is limited by missing spectral directions; above it, gains diminish until a spanning regime is reached, in which sufficiently high rank yields near-perfect fidelity independent of detailed alignment. We further identify an inherent asymmetry in transfer behavior: representations learned on broader domains can fully span narrower targets given sufficient rank, whereas the converse remains fundamentally constrained.

Together, these results provide a geometric account of transfer learning based on spectral structure, reinterpret rank as a form of memory bandwidth, and introduce a diagnostic for assessing transfer compatibility prior to training, independent of task semantics, parameter count, or downstream loss.



1 Introduction

Transfer learning is a central mechanism in modern machine learning, enabling models trained on one task or domain to accelerate learning or improve performance on another. In practice, transfer effectiveness is evaluated empirically through downstream loss or accuracy, and success is commonly attributed to representation similarity, task relatedness, or model scale. Despite extensive empirical study, however, there remains no mechanistic account of when and why transfer succeeds or fails that is independent of optimization dynamics.

A core difficulty is that transfer learning is typically conflated with parameter reuse or fine-tuning performance. Large, overparameterized models frequently exhibit positive transfer across diverse and weakly related tasks, obscuring the structural conditions that make such transfer possible. As a result, transfer is often treated as an empirical phenomenon to be observed rather than a mechanism that can be analyzed or controlled.

In this work, we propose a geometric formulation of transfer learning that separates structural compatibility from optimization effects. Rather than asking whether two tasks are similar, we ask a more fundamental question:

> How much of the structure learned by a source representation is accessible to a target task under an explicit rank constraint?



We show that this question admits a precise spectral answer.


---

1.1 Classical Axes of Transfer Learning

Existing transfer learning methods are commonly organized along axes describing what is transferred and how it is reused. Table 1 summarizes this prevailing perspective.

Table 1: Classical Axes of Transfer Learning

Axis	Description

Transferred object	Parameters, features, embeddings, activations
Transfer stage	Pretraining, fine-tuning, multi-task learning
Evaluation criterion	Downstream loss, accuracy, sample efficiency
Capacity control	Implicit (model size, regularization)
Similarity notion	Task semantics, feature correlation, representation distance
Failure mode	Negative transfer (empirical degradation)


Under this view, dimensionality and rank are treated indirectly as proxies for model capacity. Spectral structure, when considered, is used primarily for post hoc representation analysis rather than as a component of the transfer mechanism itself.


---

1.2 Spectral–Geometric Axes of Transfer (This Work)

We introduce an alternative organization in which transfer is governed by rank-limited access to learned spectral structure. Table 2 summarizes the axes introduced in this work.

Table 2: Spectral–Geometric Axes of Transfer Learning

Axis	Description

Transferred object	Spectral structure of learned trajectories
Transfer operator	Rank- spectral projector derived from the source
Control variable	Projection rank  (memory bandwidth)
Compatibility measure	Transfer fidelity (captured target variance)
Structural driver	Shared intrinsic dimensionality
Failure mode	Structural incompatibility (low or zero fidelity)
Degenerate regime	Spanning regime induced by overparameterization


This formulation makes explicit a quantity that is typically implicit: the number of spectral directions through which transfer is permitted. As we show, this single control variable induces distinct and interpretable transfer regimes, including guaranteed failure, partial transfer, and a trivial spanning regime in which sufficiently high rank yields near-perfect fidelity independent of detailed alignment.


---

1.3 Relation to Prior Algorithms

Several existing methods touch subsets of these axes, but none complete them. Table 3 situates representative prior approaches relative to the spectral–geometric formulation developed here.

Table 3: Prior Methods and Axis Coverage

Method / Family	Uses spectral structure	Controls rank at transfer	Defines fidelity	Predicts failure pre-training	Mechanistic transfer operator

PCA / Truncated SVD	✓	✗ (analysis only)	✗	✗	✗
CCA	✓	✗	✗	✗	✗
SVCCA / PWCCA / CKA	✓	✗	✗	✗	✗
Random features / NTK	✓ (implicit)	✗	✗	✗	✗
LoRA / Adapters	✗ (implicit only)	✓ (capacity)	✗	✗	✗
Linear probing	✗	✗	✗	✗	✗
This work	✓	✓	✓	✓	✓


Prior work has introduced spectral tools for representation analysis and low-rank parameterizations for efficient adaptation. However, these approaches treat spectral structure either as a diagnostic or as an implicit capacity constraint. To our knowledge, no existing method formulates transfer learning itself as a rank-limited spectral projection, nor defines a fidelity measure that isolates structural compatibility from optimization dynamics.


---

1.4 Contributions

Building on this formulation, we make the following contributions:

1. Geometric formulation of transfer
We formalize transfer learning as rank-limited spectral projection between learned representations and define transfer fidelity as a structure-preserving compatibility measure.


2. Rank–fidelity transfer law
We show that transfer fidelity is governed by shared intrinsic dimensionality, exhibiting a characteristic knee as projection rank increases.


3. Asymmetry of transfer
We demonstrate that transfer is inherently asymmetric: representations learned on broader domains can span narrower targets given sufficient rank, while the converse is fundamentally limited.


4. Spanning regime in overparameterized models
We identify a degenerate regime in which high rank yields near-perfect fidelity independent of structural alignment, explaining the apparent universality of large pretrained models.


5. Pre-training diagnostic
We introduce a geometry-based diagnostic for predicting transfer compatibility prior to optimization.




