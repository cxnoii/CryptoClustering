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
  
```python
#Applying a standard scaler to the data
scaler = StandardScaler()
scaled_data = scaler.fit_transform(df_market_data)

#Creating a dataframe of the scaled data
column_names = df_market_data.columns
df_market_data_scaled = pd.DataFrame(scaled_data, columns=column_names)

#Setting the index as the name of the coin
df_market_data_scaled['coin_id'] = coin_ids
df_market_data_scaled = df_market_data_scaled.set_index('coin_id')

#Displaying the first 5 rows
df_market_data_scaled.head()
```

| coin_id      |   price_change_percentage_24h |   price_change_percentage_7d |   price_change_percentage_14d |   price_change_percentage_30d |   price_change_percentage_60d |   price_change_percentage_200d |   price_change_percentage_1y |
|:-------------|------------------------------:|-----------------------------:|------------------------------:|------------------------------:|------------------------------:|-------------------------------:|-----------------------------:|
| bitcoin      |                     0.508529  |                     0.493193 |                     0.7722    |                     0.23546   |                   -0.0674951  |                      -0.355953 |                    -0.251637 |
| ethereum     |                     0.185446  |                     0.934445 |                     0.558692  |                    -0.0543409 |                   -0.273483   |                      -0.115759 |                    -0.199352 |
| tether       |                     0.021774  |                    -0.706337 |                    -0.0216804 |                    -0.0610302 |                    0.00800452 |                      -0.550247 |                    -0.282061 |
| ripple       |                    -0.0407644 |                    -0.810928 |                     0.249458  |                    -0.050388  |                   -0.373164   |                      -0.458259 |                    -0.295546 |
| bitcoin-cash |                     1.19304   |                     2.00096  |                     1.76061   |                     0.545842  |                   -0.291203   |                      -0.499848 |                    -0.270317 |

2. **Find the optimal value for k.**
    * The KMeans algorithm requires the specification of the value k, the number of clusters to be created. The best value for this variable is determined using the elbow method, which was done by comparing the inertia value of each k within a range between 1 and 12. The optimal value is then chosed based on where the inertia values begin to flatten out on the graph comparing k values to ienrtia. Python's SkLearn package was used in order to calculate the inertia values for each value of k. 

```python
#Creating an empty list to store inertia values
k = list(range(1,12))
inertia = []

#Computing the inertia with each possible value of k
for i in k:
    k_model = KMeans(n_clusters=i, random_state=42)
    k_model.fit(df_market_data_scaled)
    inertia.append(k_model.inertia_)

#Creating dictionary with k and inertia values to plot
elbow_dict = {"k": k, "inertia": inertia}

#Creating dataframe to plot
elbow_df = pd.DataFrame(elbow_dict)
elbow_df.head()
```

|   k |   inertia |
|----:|----------:|
|   1 |  256.874  |
|   2 |  168.812  |
|   3 |   93.7746 |
|   4 |   49.6655 |
|   5 |   37.8787 |
|   6 |   27.619  |
|   7 |   21.1828 |

With the calculated inertia values, we can plot these on a graph in order to manually determine the elbow of the graph. Based on the graph, the best value for k=4.
![image](https://github.com/cxnoii/CryptoClustering/assets/114107454/5e093dc2-ff24-4aa7-b898-0e45216dd153)



3. **Cluster Cryptocurrencies using the KMeans Algorithm.**
    * Now that the data is preprocessed and the optimal value of k is determined, the KMeans clustering algorithm can be fitted to the scaled data. The model will then make predictions on which cluster group each cryptocurrency belongs to based on the data available, which in this case is the price change percentage over the intervals listed above.
  
```python
#Initializing the KMeans model and fitting to the scaled data.
k_model = KMeans(n_clusters=4, random_state=42)
k_model.fit(df_market_data_scaled)

#Predicting cluster groups
predictions = k_model.predict(df_market_data_scaled)
df_market_data_predictions = df_market_data_scaled.copy()

#Creating a new dataframe with our clustergroup predictions
df_market_data_predictions['ClusterGroup'] = predictions
df_market_data_predictions.head(3)
```

| coin_id      |   price_change_percentage_24h |   price_change_percentage_7d |   price_change_percentage_14d |   price_change_percentage_30d |   price_change_percentage_60d |   price_change_percentage_200d |   price_change_percentage_1y |   ClusterGroup |
|:-------------|------------------------------:|-----------------------------:|------------------------------:|------------------------------:|------------------------------:|-------------------------------:|-----------------------------:|---------------:|
| bitcoin      |                     0.508529  |                     0.493193 |                     0.7722    |                     0.23546   |                   -0.0674951  |                      -0.355953 |                    -0.251637 |              2 |
| ethereum     |                     0.185446  |                     0.934445 |                     0.558692  |                    -0.0543409 |                   -0.273483   |                      -0.115759 |                    -0.199352 |              2 |
| tether       |                     0.021774  |                    -0.706337 |                    -0.0216804 |                    -0.0610302 |                    0.00800452 |                      -0.550247 |                    -0.282061 |              0 |
| ripple       |                    -0.0407644 |                    -0.810928 |                     0.249458  |                    -0.050388  |                   -0.373164   |                      -0.458259 |                    -0.295546 |              0 |
| bitcoin-cash |                     1.19304   |                     2.00096  |                     1.76061   |                     0.545842  |                   -0.291203   |                      -0.499848 |                    -0.270317 |              2 |


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
