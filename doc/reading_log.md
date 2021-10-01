# 2021-09-19
## Mouli, 2021

* The paper proposes a learning framework that is able to extrapolate over symmetric known groups without training evidence while retaining antisymmetric information if observed. Extrapolation is described in the sense of "given training data from one environment, what would have been the output if the training data were from another environment instead". Linear automorphism groups are specifically considered.

* A key difference in the problem context of the paper from previous works is the idea of single-environment extrapolation. For example, IRM requires data from multiple environments in order to have hopes of learning an internal representation invariant to environment changes. The discussion of examples not explicitly observed in infinite independent training examples then refers to observations that may occur in other environments but not in the training environment.

* One assumption the paper makes is that the data were generated economically, i.e., that the data were sampled from an environment that is diverse in a way that matters to the task. Absence of variation in some aspect of the data can be considered as evidence of being non-important to the output. The assumption is not required for the method to work, however.

* The paper frames being able to extrapolate as learning a representation of the data that is invariant to some symmetry (over)group of transformations independent of the data labels. If the model extrapolates, then the distribution of labels given the specific data is equal to the distribution of predicted labels given the representation.

* Theorem 1 says CG-invariance is stronger than G-invariance. What is the nuance here? Theorem 2 says that they are equivalent if the overgroup of independent transformations is a normal subgroup of the overgroup of independent and dependent transformations. The proof relies on the fact that in the case of a normal subgroup, any transformation in the composed overgroup can be decomposed into the composition of an independent transformation and a dependent transformation. Why is this important? As a consequence, the learning framework in the paper assumes that the group of independent transformations is a normal subgroup.

* In practice, the group of independent transformations is unknown. The proposed learning framework uses a regularized objective that encourages learning the strongest invariances compatible with the data that are found and partially ordered by exploiting properties of the Reynolds operator for finite linear automorphism groups (Theorem 3).

    * Reynolds operator being a projection operator follows from compositions of transformations being another transformation in the same group.
	
	* Invariance strength is defined in terms of the number of groups.

* Empirical results on image and sequence data generally show that (1) standard neural networks do well when interpolating but fail when extrapolating, (2) neural networks with forced G-invariance (assumed G-invariance that contradicts the training data) do poorly when interpolating but do well when extrapolating, and (3) CG-regularized neural networks do well when interpolating and when extrapolating.

# 2021-09-21
## Mouli, 2021

* Appendix B.1 (generating sequence of transformations): where does the interleaving process come from? References for known group theory result?

# 2021-09-22
## Mouli, 2021

* For all sets M in power set of indices:

    1. Compute intersection of 1-eigenspace.
	
	2. Direct sum 1-eigenspaces of supersets.
	
	3. Remove projection of (1) onto (2) from (1) to get the invariant subspace for set M.
	
* For each invariant subspace, any vector in subspace can be expressed as a linear combination of an orthogonal basis. Let the coefficients of the linear combination for each subspace be the learnable parameters of a neural network, collected in a matrix where rows correspond to subspaces and columns correspond to a particular neuron. A subspace is used in a neuron if the corresponding coefficient is not 0.

* Let optimization objective consist of a loss term and a regularization term. Regularization term consists of two parts: (1) the number of sets M that are invariant to more groups than the least invariant subspace used by any neuron (encourages using subspaces that are invariant to more groups), and (2) the number of subspaces that are used and have the same level of invariance as the least invariant subspace used by any neuron (what does this encourage? "The larger the second term, farther away the optimization is from increasing the least level of invariance...").

# 2021-09-28
## Elesedy, 2021

* Group with Haar measure acting on input space. Given a function, the function can be made invariant by averaging over the Haar measure.
* Lemma 3: RKHS decomposes into space of G-invariant functions and space of functions that vanish when averaged over the Haar measure.

# 2021-09-30
## Elesedy & Zaidi, 2021

* Introduce generalization gap for quantifying benefits of invariance. Previous work focus on worst-case performance and don't show provably non-zero benefits.

## Steinwart, 2008

* For kernel and RKHS background.
