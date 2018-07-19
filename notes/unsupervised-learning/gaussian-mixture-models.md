# Gaussian Mixture Models
- An example of a soft clustering algorithm, in which each point is assigned to each cluster but has a probability of truly belonging to each cluster associated with it.
- Assumes that each cluster has a specific gaussian distribution.

## Concept
- Essentially, this method assumes that all of the data belongs to one or more gaussian distributions. We attempt to find distributions in the data and assign each point to the distribution it has the highest probability of belonging to.

## How to cluster
- We use the Expectation - Maximization algorithm
1) Initialize k gaussian distributions
	- Give a mean and sd for each distribution
	- A few ways to do this, including using k-means to find initial clusters and checking distribution on them
	- Another method is just grabbing random mean and variance
2) Soft clustering of data points ('Expectation')
	- We can calculate this using N(X_i | µ_a, var_a) / N(X_i | µ_a, var_a) + N(X_i | µ_b, var_b)
	- Here^ we assume there are clusters a and b, and we are basically finding the prob that the point belongs to each cluster. Then we assign the point to whichever cluster is the max and move on to the next point. We do this with all points.
3) Reestimate Gaussians ('Maximization')
	- Find new mean and variance for each cluster with the points we've softly clustered.
	- Find µ by taking the weighted average https://cl.ly/2H1C140Y0f3v
	- Estimate the weighted variance https://cl.ly/1l2e332R0O3v
4) Evaluate the log likelihood https://cl.ly/0J1E2e3F4245
	- The higher the log likelihood, the more likely the distribution fits the dataset
	- We repeat these entire 4 steps until this log likelihood doesn't get any higher/much higher

## Algorithm Input (EM)
- Method of initializing the k gaussians and covariance type
- Covariance type can define the shape (not the right word but from a visual perspective makes sense) of the gaussian (how much it's distributed in each dimension)

## Advantages of GMM
- Provides a soft-clustering, which can be helpful when we have a situation where each object can belong to multiple categories.
- Cluster shape flexibility

## Disadvantages
- Sensitive to initial values
- Possible to converge to local optimum
- Slow convergence

## CV Applications
- One cool application of Gaussian Mixture Models is to extract the foreground or background from an image: http://www.ai.mit.edu/projects/vsam/Publications/stauffer_cvpr98_track.pdf


