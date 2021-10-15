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

# 2021-10-01

* Save for later:

    * Gonen (2011): [Multiple Kernel Learning Algorithms](https://jmlr.csail.mit.edu/papers/volume12/gonen11a/gonen11a.pdf)
    * Ma (2019): [Learning Invariant Representations with Kernel Warping](http://proceedings.mlr.press/v89/ma19a.html)
	* Ghiasi-Shirazi (2010): [Learning Translation Invariant Kernels for Classification](https://www.jmlr.org/papers/volume11/ghiasi-shirazi10a/ghiasi-shirazi10a.pdf)
	* Walder (2007): [Learning with Transformation Invariant Kernels](https://papers.nips.cc/paper/2007/hash/c0e190d8267e36708f955d7ab048990d-Abstract.html)
	* Gong (2012): [Geodesic flow kernel for unsupervised domain adaptation](https://ieeexplore.ieee.org/document/6247911), [slides](http://adas.cvc.uab.es/task-cv2014/wp-content/uploads/2014/10/Gong_talk.pdf)
	
# 2021-10-07
## van der Wilk, 2018

* Uses notion of *insensitivity* in place of invariance where function output does not change "too much" after transformation on the input.
* Proposes incorporating invariance into marginal likelihood as models on likelihood can fit the training data equally well but have generalization characteristics.
* For insensitivity, prior on invariant functions has a double-sum/integral kernel over *augmentation* set of points that function should be insensitive to. Parameters of base kernel and augmentation distribution are treated as hyperparameters.
* GP inference only tractable for Gaussian likelihoods. Approximate inference possible using *inducing variables* that shape a variational distribution. Variational parameters obtained by maximizing ELBO. This approach is also scalable.
* TODO: inter-domain inducing variables?

# 2021-10-08
## Schwobel, 2021

* Extends van der Wilk (2018) ideas to neural networks by making last layer a GP.
* GP a distribution on functions s.t. vector of function values f=(f(x1),...,f(xn)) is Gaussian distributed. Predictive distribution requires inverting covariance matrix (kernel evaluations) which is intractable for large datasets, and so is approximated by a variational distribution.
* Invariant GPs can be constructed by summing/integrating function evaluation over orbit of input with an augmentation distribution (TODO: invariances are specified through specification of augmentation distribution?). Resulting function is also a GP with a particular kernel. Expected log likelihood in ELBO is not tractable but can be estimated by using unbiased estimators of the parameters.
* TODO: transformations are restricted to affine (image) transformations in paper, but generating orbit distributions is not restricted to affine transformations? Does this mean "unstructured" invariances are learned (ones that try to approximate the affine transformations)?
* Covariance functions are closed under well-defined transformations on their input. Deep kernels that represent covariance take neural network-transformed outputs where network parameters are hyperparameters. GP prior becomes mean 0 with deep kernel covariance Gram matrix.
* Invariant Deep Kernel GP (InvDKGP) is constructed by averaging function from GP prior over augmentation distribution.

## Mroueh, 2015

* Introduces a random feature map of discretized normalized empirical CDFs that in the limit of bins has expectation equal to a group-invariant kernel. The advantage of the feature map is that only transformed templates need to be stored in order to compute it, whereas computing the invariant kernel requires explicitly transforming the points.
* Theoretical properties of feature map and induced invariant kernel are the main results of paper.
* TODO: it is assumed the group is known and has a Haar measure?

# 2021-10-09

* Save for later:

    * Li (2018): [Domain Generalization via Conditional Invariant Representation](https://arxiv.org/abs/1807.08479)
	* Li (2018): [Deep Domain Generalization via Conditional Invariant Adversarial Networks](https://openaccess.thecvf.com/content_ECCV_2018/papers/Ya_Li_Deep_Domain_Generalization_ECCV_2018_paper.pdf)
	* Henze (2003): [Invariant tests for symmetry about an unspecified point based on the empirical characteristic function](https://www.sciencedirect.com/science/article/pii/S0047259X03000447)

## [Harchaoui, 2013](https://www.di.ens.fr/~fbach/hbcm_2013_kertest_spm.pdf)

## [Jia, 2021](http://proceedings.mlr.press/v139/jia21a/jia21a.pdf)

* Proposes a shift-invariant convolutional neural tangent kernel (SCNTK) for two-sample tests and out-of-distribution detection with MMD.
* Purpose of shift-invariance is so that the NTK is characteristic, which is required for [MMD](https://jmlr.csail.mit.edu/papers/volume13/gretton12a/gretton12a.pdf) to be a proper metric.

# 2021-10-10

## [Mei, 2021](https://arxiv.org/abs/2102.13219)

Focuses mainly on statistical learning results of invariant random feature models (activations with fixed, randomly drawn weights) and invariant kernel machines.

## [Haasdonk, 2005](https://www.researchgate.net/publication/225489883_Invariance_in_Kernel_Methods_by_Haar-Integration_Kernels)

Work that first introduced Haar-integration kernels? Definition of HI-kernel involves measurable subset of group rather than group itself. Allows for small transformations of input.

# 2021-10-11

## [Muandet, 2013](http://proceedings.mlr.press/v28/muandet13.pdf)

* Introduces Domain-Invariant Component Analysis (DICA), which learns an invariant transformation of the inputs by minimizing "distributional variance". The feature map (induced by the kernel) is projected onto a (eigen)subspace that minimizes the distributional variance between samples.
* Assumes data from multiple domains and aims to generalize classifier to unseen domains.

## [Zhang, 2013](http://proceedings.mlr.press/v28/zhang13d.pdf)

* Proposes domain adaptation of models to differences caused by target shifts (change in output marginal), conditional shifts (change in input given output), and generalized target shifts (changes in both output marginal and input given output). A causal interpretation is given and a practical algorithm for learning the corresponding distributions based on kernel mean matching is proposed.
* Causal perspective differs in that the joint is factorized into output marginal and input given output rather than input marginal and output given input? Claimed reason is that handling the change in the joint is more convenient this way.

## [Henze, 2005](https://www.sciencedirect.com/science/article/pii/S0047259X03000447)

* Proposes a test for testing affine (reflection?) invariance. Test statistic has a representation in terms of distance between nonparametric kernel density estimators.
* Very technical paper. Context appears relevant but method does not seem to be based on kernels and feature maps.

## Save for later

* Gretton (2012): [A Kernel Two-Sample Test](https://www.jmlr.org/papers/volume13/gretton12a/gretton12a.pdf)
* Gretton (2007): [A Kernel Approach to Comparing Distributions](https://www.aaai.org/Papers/AAAI/2007/AAAI07-262.pdf)
* Gretton (2006): [A Kernel Method for the Two-Sample-Problem](https://proceedings.neurips.cc/paper/2006/file/e9fb2eda3d9c55a0d89c98d6c54b5b3e-Paper.pdf)
* Chwialkowski (2016): [A Kernel Test of Goodness of Fit](http://proceedings.mlr.press/v48/chwialkowski16.pdf)
* Kellner (2015): [A One-Sample Test for Normality with Kernel Methods](https://arxiv.org/pdf/1507.02904.pdf)
* Ancona (2006): [An Invariance Property of Predictors in Kernel-Induced Hypothesis Spaces](https://www.semanticscholar.org/paper/An-Invariance-Property-of-Predictors-in-Hypothesis-Ancona-Stramaglia/90c9cdf3e19e05699b8af7a94862fa1b45a811b3)

# 2021-10-12

## [Gretton, 2007](https://www.aaai.org/Papers/AAAI/2007/AAAI07-262.pdf)

* Standard two-sample kernel test based on MMD? Under alternative (different distributions), MMD^2 converges to Gaussian (see Serfling 1980).

## [Kellner, 2015](https://arxiv.org/pdf/1507.02904.pdf)

* One-sample kernel test for normality where MMD compares empirical embedding to Gaussian embedding. Critical value estimated via parametric bootstrap.

## [Chwialkowski, 2016](http://proceedings.mlr.press/v48/chwialkowski16.pdf)

* Kernel goodness-of-fit test based on Stein discrepancy and bootstrapping. Requires target distribution to be known up to normalization constant.

## [Song, 2013](http://www.gatsby.ucl.ac.uk/~gretton/papers/SonFukGre13.pdf)

* Conditional embedding is not a single point RKHS but a family of points. Fixing the conditioned value, a single point is obtained. A conditional embedding operator that outputs the conditional embedding given a feature map of a conditioning point exists, but relies on an assumption that does not always hold. When assumption does not hold, operator is used to get an approximate mean embedding.

# 2021-10-15

## [Li, 2018](https://www.aaai.org/ocs/index.php/AAAI/AAAI18/paper/view/16595/16558)

* Assumes both marginal X and condition Y|X change across domains.
* Proposed method aims to find conditional invariant representation h(X) that reduces variance of conditional distribution h(X)|Y across source domains. Assuming that P(Y) does not change across domains, joint distribution is invariant to changes in X.

## [Jitkrittum, 2020](https://proceedings.mlr.press/v124/jitkrittum20a/jitkrittum20a.pdf)

* Tests hypotheses p(.|x)=r(.|x) given (empirical) marginal r(x) using a proposed Kernel Conditional Stein Discrepancy (requires gradient of log density of p).

## [Doran, 2014](https://auai.org/uai2014/proceedings/individuals/194.pdf)

* Proposes kernel conditional independence test based on permutation sampling.