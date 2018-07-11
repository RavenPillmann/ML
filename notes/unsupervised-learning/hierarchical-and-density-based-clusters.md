# Hierarchical and Density Based Clustering
- Heirarchical and density-based clustering are different clusterins algorithms that can deal with k-mean's shortcomings.

## Heirarchical Clustering
- Creates a heirarchy of clusters that makes the clustering easy to interpret

### Single Link
- Say we have a bunch of different data points. We start by finding the two points that are closest together and putting them into a cluster. Then we look for the next two points outside the cluster and make a new cluster between those.
- If one of the closest two points is already in a cluster, we make a cluster out of that cluster and the other point.
- https://cl.ly/0c1d0K2c0J0l
- We can do this to build out a tree until we have a cluster encompassing everything (called a dendrogram).
- We can then cut off the top part of the tree to get the number of clusters we specify in the algorithm.
- Single Link differentiates itself from other hierarchical clustering methods by considering the distance between two clusters to be the distance between the closest points between the two clusters.

#### Reprocussions of single link
- Usually leads us to elongated clusters

### Complete Link
- Very similar to single link, in which we start by assuming each point is a cluster and clustering them together to get a new cluster
- The difference, however, is that when we look at merging two clusters, we measure distance by the distance between the farthest two points in each cluster.
- Complete link produces compact clusters
- However, the problem with complete link is that it only considers one point in each cluster when looking at distances between clusters and ignores all other points in the clusters. This is also an issue with single link.

### Average Link
- The distance measure for average link is the average of the distance between every point and every other point in the two clusters

### Ward's Method
- Default in sklearn
- Attempts to minimize variance when merging two clusters.
- We find the average point between the points in two clusters, get the distance between that average and the points in the two clusters. We take the average point within each cluster and find the distance between the points in those clusters and their average, and add those up. Then we subtract that value from the distance between the two clusters described before. https://cl.ly/3N0N2E302R05

### Advantages of HC
- Resulting hierarchical representations can be very informative
- Especially potent when the data has real hierarchical relationships

### Disadvantages of HC
- Sensitive to noise and outliers
- Computationally intensive (O(N^2))

