---
layout: post
title: How to determine the number of neurons in hidden layer
date: 2015-05-09
tags: Machine_Learning Neural_Network
categories: Research
published: true

---

毕设进行到实验阶段了，整个模型关键两个参数：
1. 用于集成基学习机的个数
2. SLFN隐藏层神经元节点个数


#### 关于隐藏节点个数的选取

** use a Cross-Validation technique **

>One the most common approaches to determine the hidden units is to start with a very small network (one hidden unit) and apply the K-fold cross validation ( k over 30 will give very good accuracy) and estimate the average prediction risk. Then you will have to repeat the procedure for increasing growing networks, for example for 1 to 10 hidden units or more if needed.

>when you plot the prediction risk vs the number of hidden units (if the training of the network is correct, avoid of local minimums) you will have the following shape: the prediction risk will start to decrease (almost monotonically) and at some point will start to increase (almost) monotonically. The optimal number of neurons is the number that minimizes the prediction risk. 

Available [Here](https://www.researchgate.net/post/How_should_I_choose_the_optimum_number_for_the_neurons_in_the_input_hidden_layer_for_a_recurrent_neural_network).

Basically, it works like this:

1) You split your training data into k equal-sized parts (called folds). Typical numbers of k are between 3 and 10.

2) You choose a suitable number of candidate dimensionalities for your hidden layer, e.g. 40 neurons, 50 neurons, etc.

3) For each of these candidate dimensionalities, you train the network k times, using k-1 folds as training data and the k-th one as testing data.

4) You choose the number of neurons whose average testing error over the k trials of point (3) is lowest. 


** Rules-of-thumb: **
>'The optimal size of the hidden layer is usually between the size of the input and size of the output layers'. Jeff Heaton, author of Introduction to Neural Networks in Java offers a few more.
 
One additional rule of thumb for supervised learning networks, the upperbound on the number of hidden neurons that won't result in over-fitting is:

$$ \mathbf{N}\_h = {\mathbf{N}\_s \over alpha * (\mathbf{N}\_i + \mathbf{N}\_o)} $$ 

$$$ \mathbf{N}\_i $$$ = number of input neurons. 

$$$ \mathbf{N}\_o $$$ = number of output neurons.

$$$ \mathbf{N}\_s $$$ = number of samples in training data set.

$$$ alpha $$$ = an arbitrary scaling factor usually 2-10.



#### Pruning

* Look weights very close to zero--it's the nodes on either end of those weights that are often removed during pruning.) 
* 也可以通过网络输出值对各个权重系数的敏感度进行分析，然后利用largest gap method来进行pruning



#### Reference:
* [How to choose the number of hidden layers and nodes in a feedforward neural network?](http://stats.stackexchange.com/questions/181/how-to-choose-the-number-of-hidden-layers-and-nodes-in-a-feedforward-neural-netw)
* [Estimating the number of neurons and number of layers of an artificial neural network](http://stackoverflow.com/questions/3345079/estimating-the-number-of-neurons-and-number-of-layers-of-an-artificial-neural-ne)