# efficient-bayesian-estimation

This repo includes the following scripts:
- How to generate target map.ipynb
- Training the NN.ipynb
- Hybrid estimation.ipynb
- Method of moments estimation.ipynb
- Uniform sampling estimation.ipynb

### How to generate target map.ipynb
This script shows how to generate training data for the neural network used in the hybrid estimation scheme.
the code provided here is written to be descriptive and give some insight into the calculations going on. 
For the actual script used to generate whole data sets, the code here is wrapped inside a parallelised loop over parameter values and run on a cluster 
(also, the interval of measurement times that we search over is constricted to gain better resolution for this variable, here the interval is kept quite large to give a nice plot of the risk).
Datasets generated on a cluster for some different dephasing times can be found in the folder *maps_to_learn/*.

### Training the NN.ipynb
Using the  already generated datasets in the *maps_to_learn/* folder, the network is trained using the Tensorflow library. The NN output is shown together with the target maps from the dataset. 
NB: The number of training-epochs is high, so the script takes a long time to run. Therefore, the network is saved and is already available (pre-trained) in the repo.

### Hybrid estimation.ipynb
Loads the pre-trained network in order to run a hybrid NN/method of moments Bayesian estimation.

### Method of moments estimation.ipynb
Bayesian estimation with the method of moments fit to Gaussian priors/posteriors.

### Uniform sampling estimation.ipynb
Bayesian estimation with a predetermined uniform sampling of measurement times.

## Estimation runtime
We use the Numba library for Python in order to speed up computations. Unfortunately, Numba is not compatible with Tensorflow, so for the hybrid scheme we cannot use it (although the low-level Tensorflow functions are probably optimised to give a similar speed-up anyway). 
Here we present an overview of runtimes (to the nearest second) for each of the schemes with and without Numba's Just In Time compiling enabled, for the estimation of 10000 values using 50 measurements. 
Note that the numbers presented here are just for a single test of each program and that comparing a case with Tensorflow to cases without Numba is probably unfair.

|    | Without Numba | With Numba |
| -------- | ------- | ------- |
| Hybrid  |   232 sec  | NA    |
| Method of Moments | 13 sec     | 1 sec    |
| Uniform sampling    | 1094 sec    | 15 sec    |
