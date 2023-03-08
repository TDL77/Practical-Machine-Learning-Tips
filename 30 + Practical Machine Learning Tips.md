### ML Practical Tip # 1 ###

One of the great qualities of random forests in sklearn is that it allows measuring the relative importance of each feature by looking at how much the tree nodes that use that feature reduce the impurity on a weighted average (across all trees in the forest). The weight of each node is the number of training samples associated with it. 

For example, the code below trains a Random Forest Classifier on the iris dataset and outputs each feature’s importance. It seems that the most important features are the petal length (44%) and width (42%), while sepal length and width are rather unimportant in comparison (11% and 2%, respectively).


![pika-2022-08-15T11_06_41 062Z](https://user-images.githubusercontent.com/72076328/188969870-2e62cc31-309d-4810-8627-088f7cf28000.png)

------------------------------------------------------------------------------------------------------------------------------------------------------------------
### ML Practical Tip # 2 ###
If you are training a deep neural network and the loss is unstable (getting large changes from update to update), the loss goes to Nan during training or the model is not learning from the data this is a sign of exploding gradients. You can make sure by checking the weights if they are very large go to NaN values during training this will sign that there is an exploding gradient.

A simple but effective solution to this problem is 𝗴𝗿𝗮𝗱𝗶𝗲𝗻𝘁 𝗰𝗹𝗶𝗽𝗽𝗶𝗻𝗴, in which you clip the gradient if they pass a certain threshold.

In the Keras deep learning library, you can use gradient clipping by setting the 𝗰𝗹𝗶𝗽𝗻𝗼𝗿𝗺 or 𝗰𝗹𝗶𝗽𝘃𝗮𝗹𝘂𝗲 arguments on your optimizer before training.

Good default values are clipnorm = 1.0 and clipvalue = 0.5.


![gradclip](https://user-images.githubusercontent.com/72076328/188970232-120aed43-28e7-475a-afdf-c4edfab7a8aa.png)

------------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 3 ### 

You should save every model you experiment with, so you can come back easily to any model you want. Make sure you save both the hyperparameters and the trained parameters, as well as the cross-validation scores and perhaps the actual predictions as well. This will allow you to easily compare scores across model types, and compare the types of errors they make. Also, this could be used as a backup if the new model fails for some reason. Likewise, you should keep a backup of the different versions of the datasets so that you can go back to it if the new version got corrupted for any reason.You can easily save Scikit-Learn models by using Python’s pickle module or using the joblib library, which is more efficient at serializing large NumPy arrays as shown in the code below. Also, you can save Keras's deep learning models in a similar way.

![pika-2022-08-17T11_19_16 604Z](https://user-images.githubusercontent.com/72076328/188971012-e4cbd3d2-89cd-4a5d-9073-ba6f65a02ae9.png)

----------------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 4 ###

One of the practical tips that might be a little bit strange is to split the data and leave out the test even before exploration and cleaning. The reason for that is that our brains are amazing pattern detection system, which means that it is highly prone to overfitting.

So if you look at the test set, you may stumble upon some seemingly interesting pattern in the test data that leads you to select a particular kind of Machine Learning model. When you estimate the generalization error using the test set, your estimate will be too optimistic and you will launch a system that will not perform as well as expected. This is called **𝐝𝐚𝐭𝐚 𝐬𝐧𝐨𝐨𝐩𝐢𝐧𝐠 𝐛𝐢𝐚𝐬**

----------------------------------------------------------------------------------------------------------------------------------------------------------------------
### ML Practical Tip # 5 ###
One of the main reasons bagging and pasting are famous, besides their good results, is that they can scale very well in an easy way. since the predictor can be trained in parallel as they are independent, you can train on multiple CPU cores or on multiple servers. The same can also be done in prediction. 

Scikit learn offers a  simple API for bagging and pasting with the  BaggingClassifier class. The parameter n_jobs defines the number of cores that can be used in both training and prediction. The code below n_jobs is -1, meaning use all the available cores.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------
### Tip 6 ###
A big challenge with online learning* is that if bad data is fed to the system, the system performance will gradually decline. if it is a live system, your clients will notice. Bad data could come from a malfunctioning sensor on a robot or someone spamming a search engine to try to rank high in search results. To reduce the risk, you need to monitor your system closely and promptly switch learning off if you detect a drop in performance. You may also want to monitor the input data and react to abnormal data for example using an anomaly detection algorithm. 

*Online learning: You train the system incrementally by feeding it data instances sequentially, either individually or in small groups called mini-batches. Each learning step is fast and cheap, so the system can learn about new data on the fly, as it arrives.

------------------------------------------------------------------------------------------------------------------------------------------------------------------
### ML Practical Tip # # 7 ###

If you would like to increase the speed of PCA you can set the svd_solver hyperparameter to "randomized" in Scikit-Learn. This will make it use a stochastic algorithm called Randomized PCA that quickly finds an approximation of the first d principal components. Its computational complexity is O(m × d2) + O(d3),
instead of O(m × n2) + O(n3) for the full SVD approach, so it is dramatically faster than full SVD when d is much smaller than n:

```
from sklearn.decomposition import PCA

𝐏𝐂𝐀(𝐧_𝐜𝐨𝐦𝐩𝐨𝐧𝐞𝐧𝐭𝐬=𝟏𝟎𝟎, 𝐬𝐯𝐝_𝐬𝐨𝐥𝐯𝐞𝐫="𝐫𝐚𝐧𝐝𝐨𝐦𝐢𝐳𝐞𝐝")

```

By default, svd_solver is actually set to "auto": Scikit-Learn automatically uses the randomized PCA algorithm if m or n is greater than 500 and d is less than 80% of m
or n, or else it uses the full SVD approach. If you want to force Scikit-Learn to use full SVD, you can set the svd_solver hyperparameter to "full".

---------------------------------------------------------------------------------------------------------------------------------------------------------------------
### ML Practical Tip # 8 ###
If you would like to apply PCA to a large dataset you will probably face a memory error since the implementations of PCA requires the whole training set to fit in memory in order for the algorithm to run.

Fortunately, 𝗜𝗻𝗰𝗿𝗲𝗺𝗲𝗻𝘁𝗮𝗹 𝗣𝗖𝗔 (𝗜𝗣𝗖𝗔) algorithms have been developed to solve this problem: you can split the training set into mini-batches and feed an IPCA algorithm one mini-batch at a time. This is useful for large training sets, and also to apply PCA online.

The following code splits the training dataset into 100 mini-batches (using NumPy’s array_split() function) and feeds them to Scikit-Learn’s IncrementalPCA class to reduce the dimensionality of the training dataset down to 100 dimensions. Note that you must call the partial_fit() method with each mini-batch rather than the fit() method with the whole training set:
```
from sklearn.decomposition import IncrementalPCA
n_batches = 100
inc_pca = IncrementalPCA(n_components=100)
for X_batch in np.array_split(X_train, n_batches):
    inc_pca.partial_fit(X_batch)
    
X_reduced = inc_pca.transform(X_train)

```
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 9 ###

If you would like to use a very slow dimension reduction method especially if your data is nonlinear such as Locally Linear Embedding (LLE) you can chain two dimension reduction algorithms to decrease the computational time of the LLE and still have almost the same result.

You can first apply PCA to your data to quickly get rid of a large number of useless dimensions and then use LLE. This will yield a similar performance as just using LLE but in a fraction of its time.

-------------------------------------------------------------------------
### ML Practical Tip # 10 ###

When splitting the dataset into training/ validations and test datasets it is important to avoid sampling bias especially if your dataset is imbalanced or not large enough. One way to avoid this is using stratified sampling: the population is divided into homogeneous subgroups called strata, and the right number of instances is sampled from each stratum to guarantee that the test set is representative of the overall population.

You can apply this through the 𝐬𝐭𝐫𝐚𝐭𝐢𝐟𝐲 parameter in the 𝐬𝐤𝐥𝐞𝐚𝐫𝐧.𝐦𝐨𝐝𝐞𝐥_𝐬𝐞𝐥𝐞𝐜𝐭𝐢𝐨𝐧.𝐭𝐫𝐚𝐢𝐧_𝐭𝐞𝐬𝐭_𝐬𝐩𝐥𝐢𝐭 method as the following:

```
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y,
                                                    stratify=y, 
                                                    test_size=0.25)
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 11 ###

If you would like to visualize data with high dimensions a good method is 𝘁-𝗗𝗶𝘀𝘁𝗿𝗶𝗯𝘂𝘁𝗲𝗱 𝗦𝘁𝗼𝗰𝗵𝗮𝘀𝘁𝗶𝗰 𝗡𝗲𝗶𝗴𝗵𝗯𝗼𝗿 𝗘𝗺𝗯𝗲𝗱𝗱𝗶𝗻𝗴 (𝘁-𝗗𝗦𝗡𝗘).

𝘁-𝗗𝗦𝗡𝗘 reduces dimensionality while trying to keep similar instances close and dissimilar instances apart. 𝘁-𝗗𝗦𝗡𝗘 uses a heavy-tailed Student-t distribution to compute the similarity between two points in the low-dimensional space rather than a Gaussian distribution, which helps to address the crowding and optimization problems.

For example, applying t-DSNE to the MNSIT dataset will results in 9 clusters as shown in the figure below:
![1665215052130](https://user-images.githubusercontent.com/72076328/194696370-00435ffd-69d5-4989-8e34-9218b36c0e4e.jpg)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 12 ###

Although creating a test set is theoretically quite simple: you can just pick some instances randomly of the dataset, and set them aside. There are some tips to make it more efficient one of them is Consistent Splitting.

Consistent splitting of the same instants as train and test will prevent the model from seeing the whole data set which is something we want to avoid to make sure that the models are reliable.

This can be done by using random.seed() when splitting or removing the data from the beginning and put it into a separate file.

However, both of these solutions will break the next time you fetch an updated dataset. A common better, and more reliable solution is to use each instance’s identifier to decide whether or not it should go in the test set (assuming instances have a unique and immutable identifier).

For example, you could compute a hash of each instance’s identifier and put that instance in the test set if the hash is lower or equal to 20% of the maximum hash value. This ensures that the test set will remain consistent across multiple runs, even if you refresh the dataset.

If your data is imbalanced and you will oversample or undersample certain classes in your data to overcome this problem. Do this to the training data only after splitting.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 13 ###

If your data is imbalanced and you will oversample or undersample certain classes in your data to overcome this problem. Do this to the training data only after splitting.

Do not oversample or undersample the test or validation dataset as they will make it nonrepresentative to the real world and will give unrealistic results.  

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 14 ###


If you use SVM this tip can decrease time and computational complexity.

Since 𝐋𝐢𝐧𝐞𝐚𝐫𝐒𝐕𝐂 class is based on the 𝐥𝐢𝐛𝐥𝐢𝐧𝐞𝐚𝐫 library, which implements an optimized algorithm for linear SVMs. It does not support the kernel trick, but it 𝐬𝐜𝐚𝐥𝐞𝐬 𝐚𝐥𝐦𝐨𝐬𝐭 𝐥𝐢𝐧𝐞𝐚𝐫𝐥𝐲 with the number of training instances and the number of features: its training time complexity is roughly O(m × n).

The algorithm takes longer if you require very high precision. This is controlled by the tolerance hyperparameter ϵ (called 𝐭𝐨𝐥 in 𝐒𝐜𝐢𝐤𝐢𝐭-𝐋𝐞𝐚𝐫𝐧). In most classification tasks, the default tolerance is fine. This will be a perfect choice if you have a large dataset but are linearly separable.

On the other hand, the 𝐒𝐕𝐂 class is based on the 𝐥𝐢𝐛𝐬𝐯𝐦 library, which implements an algorithm that supports the kernel trick. The training time complexity is usually between O(m2 × n) and O(m3 × n).
Unfortunately, this means that it gets very slow when the number of training instances gets large (e.g., hundreds of thousands of instances).

This algorithm is perfect for 𝐜𝐨𝐦𝐩𝐥𝐞𝐱 𝐛𝐮𝐭 𝐬𝐦𝐚𝐥𝐥 𝐨𝐫 𝐦𝐞𝐝𝐢𝐮𝐦 𝐭𝐫𝐚𝐢𝐧𝐢𝐧𝐠 𝐬𝐞𝐭𝐬. However, it scales well with the number of features, especially with sparse features (i.e., when each instance has few nonzero features). In this case, the algorithm scales roughly with the average number of nonzero features per instance.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 15 ###

If you would like to apply **K_Means algorithm** to large dataset that you can use **MiniBatchKmeans class**. Instead of using the full dataset at each iteration, the algorithm is capable of using mini-batches, moving the centroids just slightly at each iteration. This speeds up the algorithm typically by a factor of 3 or 4 and makes it possible to cluster huge datasets that do not fit in memory. 

Scikit-Learn implements this algorithm in the MiniBatchKMeans class. You can just use this class like the KMeans class:

```
from sklearn.cluster import MiniBatchKMeans
minibatch_kmeans = MiniBatchKMeans(n_clusters=5)
minibatch_kmeans.fit(X)
```
If the dataset does not fit in memory, the simplest option is to use the **memmap class** or you can use the **partial_fit()** method, but this will require much more work, since you will need to perform multiple initializations and select the best one yourself.


-----------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 16 ###

Using the 𝐞𝐥𝐛𝐨𝐰 𝐝𝐢𝐚𝐠𝐫𝐚𝐦 to find the best value of K for K-Means clustering can be inaccurate and lead to choosing a wrong k-value.

A better and more accurate way is to use the 𝐬𝐢𝐥𝐡𝐨𝐮𝐞𝐭𝐭𝐞 𝐝𝐢𝐚𝐠𝐫𝐚𝐦.
Consider the figure below:

The 𝐯𝐞𝐫𝐭𝐢𝐜𝐚𝐥 𝐝𝐚𝐬𝐡𝐞𝐝 𝐥𝐢𝐧𝐞𝐬 represent the silhouette score for each number of clusters, the width of each bar represents the size of data for each cluster, and the length of each cluster represents its silhouette coefficient.

When most of the instances in a cluster have a lower coefficient than this score (i.e., if many of the instances bars stopped short of the dashed line, ending to the left of it), then the cluster is rather bad since this means its instances are much too close to other clusters.

We can see that when k=3 and when k=6, we get bad clusters. But when k=4 or k=5, the clusters look pretty good as most instances extend beyond the dashed line to the right and closer to 1.0.

When k=4, the cluster at index 1 (the third from the top), is rather big, while when k=5, all clusters have similar sizes, so even though the overall silhouette score from k=4 is slightly greater than for k=5, it seems like a good idea to use k=5 to get clusters of similar sizes.

![1667312052220](https://user-images.githubusercontent.com/72076328/199254875-d378cbaf-7ab6-4754-a008-e3f5db653519.jpg)


-----------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 17 ###
Choosing the type of loss for a deep learning model can be confusing here is a short guide:

We use the 𝐬𝐩𝐚𝐫𝐬𝐞_𝐜𝐚𝐭𝐞𝐠𝐨𝐫𝐢𝐜𝐚𝐥_𝐜𝐫𝐨𝐬𝐬𝐞𝐧𝐭𝐫𝐨𝐩𝐲 loss when we have sparse labels (when for each instance we get a class index for example if we have three classes then labels will be 0,1,2)

We use 𝐜𝐚𝐭𝐞𝐠𝐨𝐫𝐢𝐜𝐚𝐥_𝐜𝐫𝐨𝐬𝐬𝐞𝐧𝐭𝐫𝐨𝐩𝐲 loss when we have one target probability per class instance for example [0,0,1] for class 3.

Finally, we use the 𝐛𝐢𝐧𝐚𝐫𝐲_𝐜𝐫𝐨𝐬𝐬𝐞𝐧𝐭𝐫𝐨𝐩𝐲 loss for binary classification tasks.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 18 ###

If you would like to plot your deep learning model there is an easy way to do this using 𝐩𝐥𝐨𝐭_𝐦𝐨𝐝𝐞𝐥 function from Keras utils. 

Consider the model shown below:  
![1_z5LWYS_mNAxUeGSv1k_TXA](https://user-images.githubusercontent.com/72076328/201094730-6436f026-afa3-4924-abba-a111663ba083.jpeg)


we can define the model and plot it with this code 
```
from tensorflow import keras
from tensorflow.keras.utils import plot_model


input_A = keras.layers.Input(shape=[5], name="wide_input")
input_B = keras.layers.Input(shape=[6], name="deep_input")
hidden1 = keras.layers.Dense(30, activation="relu")(input_B)
hidden2 = keras.layers.Dense(30, activation="relu")(hidden1)
concat = keras.layers.concatenate([input_A, hidden2])
output = keras.layers.Dense(1, name="main_output")(concat)
aux_output = keras.layers.Dense(1, name="aux_output")(hidden2)
model = keras.models.Model(inputs=[input_A, input_B],
                           outputs=[output, aux_output])

keras.utils.plot_model(model, "model.png", show_shapes=True)

```
Here is the output of this code:

![download (4)](https://user-images.githubusercontent.com/72076328/201095601-f6eea602-7059-4e4c-a0d3-bbcda97e1b53.png)

You can see that is similar as the orignal plot of the model


It is mportant to note that if you will be using google colab you can use it this function directly, however if you will use it on your own machine you will have to download some packages as shown in the code below:
```
conda install graphviz
conda install pydot
conda install pydotplus
```
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 19 ###

If you would like to experiment new reseach ideas with flexoblity in Keras a great way to do this is using the subclassing API. It allows you to include dynamic functions such as loops, varying shapes, conditional branching and other dynamic functions in a simple and flexible way.  

Consider the code below we have two inputs and two outputs. We simply create the layers we need in the constructor and define the calcualtions after that in the call() method. 

However this extra flexibility does come at a cost: your model's architecture is hidden within the **call() method**, so Keras cannot easily inspect it; it cannot save or clone it; and when a call the **summary() method**, you only get a list of layers, without any information on how they are connected to each other. 

Moreover, Keras cannot check types and shapes ahead of time, and it is easier to make mistakes. So unless you really need
that extra flexibility, you should probably stick to the Sequential API or the Functional API.

```
class WideAndDeepModel(keras.models.Model):
    def __init__(self, units=30, activation="relu", **kwargs):
        super().__init__(**kwargs)
        self.hidden1 = keras.layers.Dense(units, activation=activation)
        self.hidden2 = keras.layers.Dense(units, activation=activation)
        self.output1 = keras.layers.Dense(1)
        self.output2 = keras.layers.Dense(1)
        
    def call(self, inputs):
        input_A, input_B = inputs
        hidden1 = self.hidden1(input_B)
        hidden2 = self.hidden2(hidden1)
        concat = keras.layers.concatenate([input_A, hidden2])
        output1 = self.main_output(concat)
        output2 = self.aux_output(hidden2)
        return output1, output2

model = WideAndDeepModel(30, activation="relu")

```
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 20 ###

𝐇𝐨𝐰 𝐭𝐨 𝐨𝐩𝐭𝐢𝐦𝐢𝐳𝐞 𝐭𝐡𝐞 𝐧𝐞𝐮𝐫𝐚𝐥 𝐧𝐞𝐭𝐰𝐨𝐫𝐤 𝐥𝐞𝐚𝐫𝐧𝐢𝐧𝐠 𝐫𝐚𝐭𝐞 𝐪𝐮𝐢𝐜𝐤𝐥𝐲?

The 𝐥𝐞𝐚𝐫𝐧𝐢𝐧𝐠 𝐫𝐚𝐭𝐞 is considered to be one of the most important hyperparameters you can optimize in a neural network model.

One way to find a good learning rate is to train the model for a few hundred iterations, starting with a very low learning rate (e.g., 10^-5) and gradually increasing it up to a very large value (e.g., 10). This is done by multiplying the learning rate by a constant factor at each iteration (e.g., by exp(log(10⁶)/500) to go from 10^-5 to 10 in 500 iterations). 

If you plot the loss as a function of the learning rate (using a LIP log scale for the learning rate), you should see it drop at first. But after a while, the learning rate will be too large, so the loss will shoot back up: the optimal learning rate will be a bit lower than the point at which the loss starts to climb (typically about 10 times lower than the turning point). You can then reinitialize the model and train it normally using this good learning rate.
 
Finally, it is important to remember that the optimal learning rate depends on the other hyperparameters, especially the batch size, so if you modify any of the hyperparameters, remember to update the learning rate as well. 


-----------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 21 ###

The choice of activation function has a large impact on the capability and performance of the neural network, and different activation functions may be used in different parts of the model.

Here is a quick guide on how to do it:

A very good starting point is to start with 𝐑𝐞𝐥𝐮 as an activation function for the hidden layers as shown in the figure below.

The activation functions of the output layer will depend mainly on the 𝐭𝐚𝐬𝐤:

1. For 𝐫𝐞𝐠𝐫𝐞𝐬𝐬𝐢𝐨𝐧 𝐭𝐚𝐬𝐤𝐬, you can use the linear activation function as you would like to output from the fully connected layers without changes.

2. If your problem is a 𝐜𝐥𝐚𝐬𝐬𝐢𝐟𝐢𝐜𝐚𝐭𝐢𝐨𝐧 𝐩𝐫𝐨𝐛𝐥𝐞𝐦, then there are three main types of classification problems, and each may use a different activation function:

⏺ If there are two mutually exclusive classes (𝐛𝐢𝐧𝐚𝐫𝐲 𝐜𝐥𝐚𝐬𝐬𝐢𝐟𝐢𝐜𝐚𝐭𝐢𝐨𝐧), then your output layer will have one node, and a 𝐬𝐢𝐠𝐦𝐨𝐢𝐝 𝐚𝐜𝐭𝐢𝐯𝐚𝐭𝐢𝐨𝐧 function should be used.

⏺ If there are more than two mutually exclusive classes (𝐦𝐮𝐥𝐭𝐢𝐜𝐥𝐚𝐬𝐬 𝐜𝐥𝐚𝐬𝐬𝐢𝐟𝐢𝐜𝐚𝐭𝐢𝐨𝐧), then your output layer will have one node per class, and a 𝐬𝐨𝐟𝐭𝐦𝐚𝐱 𝐚𝐜𝐭𝐢𝐯𝐚𝐭𝐢𝐨𝐧 should be used.

⏺ If there are two or more mutually inclusive classes (𝐦𝐮𝐥𝐭𝐢𝐥𝐚𝐛𝐞𝐥 𝐜𝐥𝐚𝐬𝐬𝐢𝐟𝐢𝐜𝐚𝐭𝐢𝐨𝐧), then your output layer will have one node for each class, and a 𝐬𝐢𝐠𝐦𝐨𝐢𝐝 𝐚𝐜𝐭𝐢𝐯𝐚𝐭𝐢𝐨𝐧 function is used.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 22 ###


𝐖𝐡𝐚𝐭 𝐢𝐬 𝐭𝐡𝐞 𝐨𝐩𝐭𝐢𝐦𝐚𝐥 𝐛𝐚𝐭𝐜𝐡 𝐬𝐢𝐳𝐞 𝐭𝐨 𝐮𝐬𝐞 𝐰𝐡𝐞𝐧 𝐭𝐫𝐚𝐢𝐧𝐢𝐧𝐠 𝐚 𝐝𝐞𝐞𝐩 𝐧𝐞𝐮𝐫𝐚𝐥 𝐧𝐞𝐭𝐰𝐨𝐫𝐤?

The batch size can have a significant impact on your model’s performance and training time. You can choose a small batch size such as 32 or 64, or you can use the maximum batch size that will fit in your memory how to decide which to choose, or are there any other options?

The main benefit of using large batch sizes is that hardware accelerators such as GPUs can process them efficiently and the algorithm will see more instances per second which will increase its performance. Therefore, many researchers and practitioners recommend using the largest batch size that can fit in GPU RAM.

However, in practice, it was found that 𝐥𝐚𝐫𝐠𝐞 𝐛𝐚𝐭𝐜𝐡 sizes often lead to 𝐭𝐫𝐚𝐢𝐧𝐢𝐧𝐠 𝐢𝐧𝐬𝐭𝐚𝐛𝐢𝐥𝐢𝐭𝐢𝐞𝐬, especially at the beginning of training, and the resulting model may not generalize as well as a model trained with small batch size.

On the other hand, researchers found that using small batches (from 2 to 32) was preferable because small batches led to better models in less training time.

Other researchers showed that it was possible to use very large batch sizes (up to 8,192) using various techniques such as warming up the learning rate (i.e., starting training with a small learning rate, then ramping it up). This will lead to a very short training time without any generalization gap.

So, in summary, a good starting strategy is to try to use a large batch size based on your hardware and to use a learning rate warmup if training is unstable or the final performance is disappointing, then try using a small batch size instead.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 23 ###


𝐇𝐨𝐰 𝐭𝐨 𝐬𝐞𝐭 𝐭𝐡𝐞 𝐧𝐮𝐦𝐛𝐞𝐫 𝐨𝐟 𝐧𝐞𝐮𝐫𝐨𝐧𝐬 𝐢𝐧 𝐞𝐚𝐜𝐡 𝐡𝐢𝐝𝐝𝐞𝐧 𝐥𝐚𝐲𝐞𝐫:

The number of neurons in the 𝐢𝐧𝐩𝐮𝐭 and 𝐨𝐮𝐭𝐩𝐮𝐭 layers is determined by the type of input and output your task requires. For example, the MNIST task requires 28 x 28= 784 input neurons and 10 output neurons since it has ten classes. If the task is regression or binary classification, then the number of output neurons is just one.

As for the 𝐡𝐢𝐝𝐝𝐞𝐧 𝐥𝐚𝐲𝐞𝐫𝐬, it used to be common to size them to form a pyramid, with fewer and fewer neurons at each layer-the rationale being that many low-level features can coalesce into far fewer high-level features. A typical neural network for MNIST might have 3 hidden layers, the first with 300 neurons, the second with 200, and the third with 100.

However, this practice has been largely abandoned because it seems that using the 𝐬𝐚𝐦𝐞 𝐧𝐮𝐦𝐛𝐞𝐫 of neurons in all hidden layers performs just as well in most cases, or even better; plus, there is only one hyperparameter to tune instead of one per layer.

That said, depending on the dataset, it can sometimes help to make the first hidden layer bigger than the others. Just like the number of layers, you can try increasing the number of neurons gradually until the network starts overfitting.

𝐁𝐮𝐭 𝐢𝐧 𝐩𝐫𝐚𝐜𝐭𝐢𝐜𝐞, 𝐢𝐭’𝐬 𝐨𝐟𝐭𝐞𝐧 𝐬𝐢𝐦𝐩𝐥𝐞𝐫 𝐚𝐧𝐝 𝐦𝐨𝐫𝐞 𝐞𝐟𝐟𝐢𝐜𝐢𝐞𝐧𝐭 𝐭𝐨 𝐩𝐢𝐜𝐤 𝐚 𝐦𝐨𝐝𝐞𝐥 𝐰𝐢𝐭𝐡 𝐦𝐨𝐫𝐞 𝐥𝐚𝐲𝐞𝐫𝐬 𝐚𝐧𝐝 𝐧𝐞𝐮𝐫𝐨𝐧𝐬 𝐭𝐡𝐚𝐧 𝐲𝐨𝐮 𝐚𝐜𝐭𝐮𝐚𝐥𝐥𝐲 𝐧𝐞𝐞𝐝, 𝐭𝐡𝐞𝐧 𝐮𝐬𝐞 𝐞𝐚𝐫𝐥𝐲 𝐬𝐭𝐨𝐩𝐩𝐢𝐧𝐠 𝐚𝐧𝐝 𝐨𝐭𝐡𝐞𝐫 𝐫𝐞𝐠𝐮𝐥𝐚𝐫𝐢𝐳𝐚𝐭𝐢𝐨𝐧 𝐭𝐞𝐜𝐡𝐧𝐢𝐪𝐮𝐞𝐬 𝐭𝐨 𝐩𝐫𝐞𝐯𝐞𝐧𝐭 𝐢𝐭 𝐟𝐫𝐨𝐦 𝐨𝐯𝐞𝐫𝐟𝐢𝐭𝐭𝐢𝐧𝐠.
![1_CvOQhsTrRmKgqyfjPcRQyw](https://user-images.githubusercontent.com/72076328/204094791-18dfbcb1-bed9-4abe-baa0-ab90d73641fd.jpeg)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 24 ###

If you would like to build a complex deep learning model a very good example is to use 𝐊𝐞𝐫𝐚𝐬 𝐅𝐮𝐧𝐜𝐭𝐢𝐨𝐧𝐚𝐥 𝐀𝐏𝐈.

For example, if you would like to implement the wide & Deep network neural network shown in the first figure below.
![ezgif com-gif-maker (10)](https://user-images.githubusercontent.com/72076328/205633236-0509bf07-b197-444c-956f-c9bed0850afc.jpg)



In this model, part of the inputs or all of it is connected directly to the output layer as shown. This architecture allows the model to learn both deep information using the deep path and simple information directly from the input through the wide path.

This can build using the code shown in the second figure. Let's explain the code in more detail:

![pika-1670241231875-1x](https://user-images.githubusercontent.com/72076328/205633480-9db6aff0-cf80-4e6b-86e0-b58444c84169.png)

◾ 𝐢𝐧𝐩𝐮𝐭_: In the first line, we created an Input object. This defines the input of the model, which includes its type and shape. The model can have multiple objects, as we will see in the coming models.

◾𝐡𝐢𝐝𝐝𝐞𝐧1: The next step is to create the first hidden layer with 30 neurons and the ReLU activation function, and we pass to it the input we created in the line before. You can see that it is similar to calling a function that’s why it is called Functional API.

◾𝐡𝐢𝐝𝐝𝐞𝐧 2: Next, we create another hidden layer with the same properties as the previously hidden layer, and we passed to it the output of the previously hidden layer.

◾𝐜𝐨𝐧𝐜𝐚𝐭: This is a Concatenate layer and is used to concatenate the input and the output of the second hidden layer.

◾𝐨𝐮𝐭𝐩𝐮𝐭: This is the output layer which consists of one single neuron and no activation function for the sake of simplicity, but you can add what you need for your model.

◾𝐦𝐨𝐝𝐞𝐥: Finally, we create the Keras model and specify which input and outputs to use.


![ezgif com-gif-maker (11)](https://user-images.githubusercontent.com/72076328/205633504-29cf2eb1-71d3-463f-9b5a-88286f0ab38e.jpg)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 25 ###

If you would like to use transform learning and you are not sure how many layers should you unfreeze and retrain. Here is a simple approach to follow:


▶ Try freezing all the reused layers first, then train your model and see how it performs.

▶ Then try unfreezing one or two of the top hidden layers to let backpropagation tweak them and see if performance improves. The more training data you have, the more layers you can unfreeze.

𝐈𝐭 𝐢𝐬 𝐚𝐥𝐬𝐨 𝐮𝐬𝐞𝐟𝐮𝐥 𝐭𝐨 𝐫𝐞𝐝𝐮𝐜𝐞 𝐭𝐡𝐞 𝐥𝐞𝐚𝐫𝐧𝐢𝐧𝐠 𝐫𝐚𝐭𝐞 𝐰𝐡𝐞𝐧 𝐲𝐨𝐮 𝐮𝐧𝐟𝐫𝐞𝐞𝐳𝐞 𝐫𝐞𝐮𝐬𝐞𝐝 𝐥𝐚𝐲𝐞𝐫𝐬: 𝐭𝐡𝐢𝐬 𝐰𝐢𝐥𝐥 𝐚𝐯𝐨𝐢𝐝 𝐰𝐫𝐞𝐜𝐤𝐢𝐧𝐠 𝐭𝐡𝐞𝐢𝐫 𝐟𝐢𝐧𝐞-𝐭𝐮𝐧𝐞𝐝 𝐰𝐞𝐢𝐠𝐡𝐭𝐬.

▶ If you still cannot get good performance, and you have little training data, try dropping the top hidden layer(s) and freezing all the remaining hidden layers again.

▶ You can iterate until you find the right number of layers to reuse. If you have plenty of training data, you may try replacing the top hidden layers instead of dropping them, and even adding more hidden layers.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------

### ML Practical Tip # 26 ###



-----------------------------------------------------------------------------------------------------------------------------------------------------------------
### ML Practical Tip # 27 ###
If you are working on a machine learning project and you are using AUC as an evaluation metric and got it to be less than 0.5 so you probably got your classes inverted so before trying to improve your model make sure that your classes are not inverted.
![1678075618700](https://user-images.githubusercontent.com/72076328/223859887-e3e64ff8-4bfb-4bb3-9fe4-b2aab364a574.jpg)

-----------------------------------------------------------------------------------------------------------------------------------------------------------------
### ML Practical Tip # 28 ###

The goal of data augmentation is to create examples that your learning algorithm can learn from. To be able to do this you need to think of how you can create realistic examples that the algorithm does poorly on, because if the algorithm already does well in those examples, then there's less for it to learn from. 

But you want the examples to still be ones that a human or maybe some other baseline can do well on, because otherwise, one way to generate examples that the algorithm does poorly on, would be to just create examples that are so noisy that no one can hear what anyone said, but that's not helpful.

You want examples that are hard enough to challenge the algorithm, but not so hard that they're impossible for any human or any algorithm to ever do well on. 

Here's a checklist you might go through when you are generating new data to make sure that the new data will be helpful:

* One, does it sound realistic? You want your data to actually sound like realistic data of the sort that you want your algorithm to perform on. 
* Is the X-to-Y mapping clear? In other words, can humans still recognize what was said? This is to verify point two here. 
* Is the algorithm currently doing poorly on this new data? That helps you verify point one. 

If you can generate data that meets all of these criteria, then that would give you a higher chance that when you put this data into your training set and retrain the algorithm, then that will result in improving the model and make it reach the human level performance. 
![Data-augmentation-using-semantic-preserving-transformation-for-SBIR](https://user-images.githubusercontent.com/72076328/223858583-c49e8e48-fce1-4493-be53-b7aa8614a47a.png)

