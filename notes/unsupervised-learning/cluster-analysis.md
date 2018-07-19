# Cluster Analysis

## Steps to get from data to knowledge
1) Feature selection and feature extraction (from the data)
	- Feature Selection is getting features from data that are important, feature extraction is generating features
2) Clustering Algorithm Selection and Tuning
	- No algorithm is a one size fits all
	- Need to choose a proximity measure (Euclidean distance or something else based on the data).
3) Clustering Validation
	- Validating and scoring the clustering algorithm using clustering indices.
4) Results Interpretation
	- What can we learn from the results? This requires domain knowledge

### Clustering Validation
- Three categories of indices
1) External Indices (scoring done if we have labeled data)
2) Internal Indices (measure the fit between the data and structure with only the data)
3) Relative Indices (Indicate which of two clustering structures are better)
- Most indices are defined by compactness (how close internal points are to each other) and separability (how far are clusters from each other)

#### External Validation Indices
- For data which is labeled
- Some include Adjusted Rand Score, Fawlks and Mallows, NMI measure, Jaccard, F-measure, and purity

##### Adjusted Rand Score
- Between -1 and 1
- Rand = a + b / (n choose 2)
- a = number of pairs of points that are correctly left in the same cluster (so if a pair of points was in one cluster in the labels and in one cluster in the clustering, that counts as one for a)
- b = number of pairs of points that are originally in different clusters that are in different clusters after clustering
- n = number of samples
- Adjusted Rand index is RI - ExpectedIndex / max(RI) - ExpectedIndex

#### Internal Validation Indices
- For unlabeled datasets (often the case, as with unsupervised learning)

##### Silhouette Coefficient
- Between -1 and 1
- Silhouette Coefficient = S_i = b_i - a_i / max(a_i, b_i)
- a = average distance between a point and all other points in that cluster
- b = average distance between a point and all points in the closest neighboring cluster
- Silhouette Score = average(all Silhouette coefficients)
- Is a good measure when clusters are dense, but doesn't work on undense clusters.
- Better alg in that case: http://citeseerx.ist.psu.edu/viewdoc/download;jsessionid=83C3BD5E078B1444CB26E243975507E1?doi=10.1.1.707.9034&rep=rep1&type=pdf
