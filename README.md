# CryptoClustering

## Overview
The purpose of this project is to analyze cryptocurrencies and create cluster groups based on price change percentages over a variety of different time spans. Cluster groups will be determined using the KMeans algorithm where the optimal number of groups, k, will be determined using the elbow method. This project will also utilize Principal Component Analysis (PCA) in order to find the optimal number of features to include in the KMeans clustering algorithm.

## Data
The dataset includes the _price change percentage_ of each cryptocurrency over the following intervals:
* 24 hours
* 7 days
* 14 days
* 30 days 
* 60 days

## Machine Learning Process
1. **Preprocess the data.** 
    * This was done using Scikit-Learn's standard scalar. The application of standard scalar removes any bias that are potentially created by variables measured with different scales. Each column will have a value between 0 and 1, with a mean of 0 and a standard deviation of 1. 

![image](https://github.com/cxnoii/CryptoClustering/assets/114107454/fd621a7f-d246-4217-a1dc-7ec6ff4ba629)


2. **Find the optimal value for k.**
    * The KMeans algorithm requires the specification of the value k, the number of clusters to be created. The best value for this variable is determined using the elbow method, which was done by comparing the inertia value of each k within a range between 1 and 12. The optimal value is then chosed based on where the "elbow" of the graph comparing k values to ienrtia. From the graph below, we can see that this elbow is present when k = 4.  

![image](https://github.com/cxnoii/CryptoClustering/assets/114107454/5e093dc2-ff24-4aa7-b898-0e45216dd153)



3. **Cluster Cryptocurrencies using the KMeans Algorithm.**
    * Now that the data is preprocessed and the optimal value of k is determined, the KMeans clustering algorithm can be fitted to the scaled data. The model will then make predictions on which cluster group each cryptocurrency belongs to based on the data available, which in this case is the price change percentage over the intervals listed above. 

![image](https://github.com/cxnoii/CryptoClustering/assets/114107454/27079bb3-e255-4dd4-9eb4-75b4006a79b6)



4. **Plot the results.**
 
![image](https://github.com/cxnoii/CryptoClustering/assets/114107454/83b8ba98-4544-435b-bc45-88307de602cc)

From the graph, there are two cluster groups with only one cryptocurrency. Celcius degree token is the only coin in cluster group 1 and ethlend is the only coin in cluster group 3. 


## Principal Component Analysis (PCA) 
Next, a PCA model will be created in order to reduce the amount of features that will be considered in the KMeans algorithm, while maximizing the explained variance. Because we are reducing the number of features, this will help reduce the noise in the dataset that could potentially cause the model to be overfitted to a certain feature. Additionally, because there are less features to consider, the performance of the model will also be improved. The PCA model was intilized with n_components = 3.

<img width="613" alt="Screenshot 2023-05-11 at 6 44 40 PM" src="https://github.com/cxnoii/CryptoClustering/assets/114107454/e3e36f40-2db0-465d-a6da-d40c0a696324">


5. Repeat 2 - 4 with **PCA data.**
    * The optimal value of k was still k = 4, when using inertia values associated with the PCA model. 
    * Principal Components:
        * price_change_percentage_24h
        * price_change_percentage_7d
        * price_change_percentage_14d


## PCA Results

<img width="653" alt="Screenshot 2023-05-11 at 6 55 44 PM" src="https://github.com/cxnoii/CryptoClustering/assets/114107454/34989b78-29c3-4c4e-8b33-340ea684439c">

Based on the results of the KMeans clustering algorithm using PCA data, we can see that the cluster groups are more clearly defined. The two coins that have been placed in the their own cluster group, celcius and ethlend, are now very apart from the two cluster groups where the majority lie. With the original scaled data, ethlend was placed quite close to one of the larger cluster groups, however, now considering only PC's we can see the distinction. Upon further inspection we can also see the clear divide between the larger cluster groups.

<img width="653" alt="Screenshot 2023-05-11 at 6 58 30 PM" src="https://github.com/cxnoii/CryptoClustering/assets/114107454/c7ec628d-d160-4ad0-91fc-4dffe8eea7c8">
