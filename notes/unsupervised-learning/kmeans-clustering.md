# K-Means Clustering

## Unsuperised Learning
- Learning that involves data without labels
- One example could be clustering, where we want to extract the groupings of data.
- Another example could be finding a trend in the data, like dimensionality reduction

## Clustering Motives
- The act of finding groupings within data
- Netflix might use clustering to determin what kind of movies someone might prefer so they can recommend more

### K-Means Clustering
- In k-means, our goal is to find the centers of k different clusters
- We start by randomly placing k points in the data, then adjust those points to get to the center of different clusters.
- K-means works in two steps: assign and optimize. First, we assign the closest points to each center. In other words, go through all points, and assign them to the center that's closest.
- For optimization, we want to minimize the total quadratic distance between the points and the center they're assigned to. We move the center to the location that has a minimized distance to those points. This is the mean of all the points.

#### Notes about K-Means
- Initial placement of centroids is usually random and can be pretty important.
- The initial conditions can give us pretty different results in the end.
- We usually get around this issue by repeating the algorithm a number of times and seeing which one performs best.

#### Challenges of K-Means
- The clusters for K-means are very dependent on where we put the initial cluster centers
- We can have non-intuitive clusters as a result if for example we fall into a local minimum
- Local minima can be suboptimal and can even be pretty bad.
- Generally, the more cluster centers there are, the more potential local minima. This forces us to run the algorithm multiple times.
- By nature, K-means does well with circular, spherical, or hyperspherical clusters, but clusters do exist in other forms, and k means isn't optimal for finding these.

#### Choosing the number of clusters
- A Wiki page on choosing the right number of clusters: https://en.wikipedia.org/wiki/Determining_the_number_of_clusters_in_a_data_set 
