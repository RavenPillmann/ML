# Independent Component Analysis
- Another dimension reduction techniques
- Takes a set of features and produces new features
- Assumes features are mixtures of independent sources, and that we want to find the independent sources.

## Paper
http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.322.679&rep=rep1&type=pdf

## Idea
- We have data X, source S, mixing matrix A, and unmixing matrix W. X = AS and S = WX.
- W is the inverse of A.
- ICA is all about approximating W

## SKLearn
- FastICA

## FastICA
1) Center, Whiten X
2) Choose initial random weight matrix W_1, ..., W_n
3) Estimate W, containing vectors
4) Decorrelate W
5) Repeat from step 3 until converged

## Estimating W
-https://cl.ly/2n3R0O1A350s
-Decorrelation is the Let W = (WW^T)^(-1/2)W step

## Assumptions
- Components are statistically independent
- Components must have non-gaussian distributions, this is the key to ICA

## Applications
- The cocktail party problem
- Used in medical scanners, where different sensors collect signals and ICA is used to pick apart what's happening in different parts of the brain
- 
