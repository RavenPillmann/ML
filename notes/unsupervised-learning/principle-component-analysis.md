# Principle Component Analysis - PCA
- Used in all parts of data analysis

## The Idea
- We can analyze whether we get information from any particular dimension. For example, we can see a straight line of items in a 2D system and imagine rotating/shifting the coordinate system so that the data lies on an axis and only requires knowledge of one dimension.

## What is PCA
- Taking any group of data, PCA finds a new coordinate system by putting the center of the coordinate system at the center of the data, the primary axis to the primary axis of variation in the data, and other axies orthogonal to the new primary axis.
- PCA gives us the new coordinate system, but it also gives us an importance value, which represents the spread of the data in that particular direction. This tends to be high for the primary axis and lower for the other axies.

## Measurable vs. Latent Features

### Measurable
- Things you can measure, like square footage or school ranking

### Latent
- Variables that can't be measured directly but may be driving the phenomenon you're measuring behind the scenes.
- So, for eg, if you're measuring housing prices, latent features might be the characteristic of the neighborhood or the kind of house. You can't really measure these per se, but they drive the cost of a house.

## Dimensionality Reduction
- We can reduce the number of input features if we know that there are some latent features that determine our output.
- Specifically, we can try to create composite features, which are features that are combinations of other features
- We can do this by finding relationships between features, which we can do with principle component analysis.

## How to Determine the Principle Component
- The Principle Component is the direction which has the largest variance in data.
- We do this because it retains the maximum amount of information in the original data

## PCA's power from importance values
- Not only can we group features into PCA to map out measureable to latent features, we can actually input all features into PCA and get a ranking of the importance of components
- In other words, we can put them into PCA and get a ranking of the directions the data goes, and use that ranking to determine new features.
- In this way, we can learn that data is primarily driven by a handful of factors.

## PCA's Role
- Systemized way to transform input features into principle components
- Can use principle components as new features
- PCs are directions in data that maximize variance
- More variance of data along a PC, the higher that PC is ranked
- PCs are orthogonal to each other
- Max number of PCs = no of input features. However, if you use all PCs you aren't necessarily gaining anything

## When to Use PCA
- If you want access to latent features, PCA is extremely useful
- Dimensionality Reduction:
	- helps visualize high-dimensional data (can project down to first two or three dimensions, which can help you visualize k nearest neighbors, for e.g.)
	- Reduces noise
	- Makes other algorithms work better if you use PCA to reduce the number of features (faster performance, more simplistic model)

