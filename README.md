# CryptoClustering

## Overview
The purpose of this project is to analyze cryptocurrencies and create cluster groups based on price change percentages over a variety of different time spans. Cluster groups will be determined using the KMeans algorithm where the optimal number of groups, k, will be determined using the elbow method. This project will also utilize Principal Component Analysis (PCA) in order to find the optimal number of features to include in the KMeans clustering algorithm.

## Data
The dataset includes the price change percentage of each cryptocurrency over the following intervals:
* 24 hours
* 7 days
* 14 days
* 30 days 
* 60 days

## Machine Learning Process
1. Preprocessing the data using a standard scalar. The application of standard scalar removes any bias that are potentially created by variables measured with different scales. Each column will have a value between 0 and 1, with a mean of 0 and a standard deviation of 1. 

![image](https://github.com/cxnoii/CryptoClustering/assets/114107454/fd621a7f-d246-4217-a1dc-7ec6ff4ba629)
