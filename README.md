# Missing Data Imputation using Variational Autoencoders and Autoencoders
## Abstract
Missing data is a common appearance that can be seen in almost all statistical analyses. Sometimes missing data can introduce bias and affect the results significantly. Though many data imputation methods are proposed previously, it’s hard to determine which one is better to use in practice. In this project, we compare four imputation methods including mean-replacement, K-nearest neighbour, Autoencoder, and Variational Autoencoder. We test these imputation methods on three datasets and found that for dataset with small-size features, both KNN and Variational Autoencoder(VAE) perform better than the other two. Furthermore, we find that VAE achieves the best performance for all kinds of cases in terms of RMSE reconstruction error and data visualization.


## Approaches
Four approaches were applied in our project.

### 1)  basic method: Mean replacement
First Ignore all missing data. Then Calculate the mean of the remaining data with respect to each feature and replace the missing data with the calculated mean 

### 2) Mean replacement plus VAE  
Train:
We train a VAE model using fully observed data. To mimic the test data during imputation stage, we partially corrupted the training data and replace each missing position with mean replacement, which is a noise adding step.
Imputation:
For test data, we first fill the missing position with mean and feed into the trained VAE.
Sample from the latent variable distribution (the output of the encoder) to generate z, given 𝑥.
Sample from the reconstructed data distribution (the output of the decoder) to generate 𝑥̃ given z.
Replace the missing values with the reconstructed values, leaving the observed values unchanged.
The above steps are repeated for certain times (here we use 25), as multiple multiple imputation

### 3) Mean replacement plus Autoencoder
Train:
Preprocessing the training data the same as mentioned in VAE and train a multilayer autoencoder 
Imputation:
Replace missing values in test data with mean first, 
feed in the test data to the trained AE and do multiple imputation as mentioned in VAE

### 4) KNN
Replace all missing data with nan. 
A masked Euclidean distance is computed between the current target and all other instances.
Fill each missing position with the average of their K neighbours.
If an instances has too many missing values (more than 80% missing), we will directly apply mean replacement algorithm described above.


## Experiments
### Dataset
In this study we explored 3 different dataset. 
One is a synthetic data set[1], with 4 features and all real values; 
The second is milling circuit dataset[2], The data has been subsampled in order to introduce some independence between samples. This resulted in a set of 9985 data points with 8 features. 
The third is MNIST dataset which is a large database of handwritten digits, 
here we use the Keras to load the data [2].

### Evaluation metrics
Evaluation using RMSE and Visualiztion 

### Results
We test four methods to impute missing data, among them, VAE performs the best on both small and large dataset with heavy or light corruption. KNN is also a good choice for dataset with small feature size and light corruption. Moreover, KNN works well for sparse data.

## References
[1] McCoy, John T., Steve Kroon, and Lidia Auret. "Variational Autoencoders for Missing Data Imputation with
Application to a Simulated Milling Circuit." IFAC-PapersOnLine 51.21 (2018): 141-146.
[2] Wakefield, B.J., Lindner, B.S., McCoy, J.T., Auret, L., 2018. Monitoring of a simulated milling circuit: Fault
diagnosis and economic impact. Miner. Eng. 120, 132–151. doi:10.1016/j.mineng.2018.02.007
[3] LeCun, Y., Bottou, L., Bengio, Y., and Haffner, P. (1998) . Gradient-based learning applied to document
recognition. Proceedings of the IEEE, 86, 2278–2324. http://yann.lecun.com/exdb/mnist/
