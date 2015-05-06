---
layout: post
title: The difference among TrainSet, ValidationSet and TestSet
date: 2015-05-06
tags: machine-learning 
categories: Research
published: true

---

之前一直没搞明白这三者的区别，尤其是验证集和测试集。后来，google了一下，看了一些资料，下面这个最靠谱吧。

>Training set: A set of examples used for learning, which is to fit the parameters [i.e., weights] of the classifier. 
Validation set: A set of examples used to tune the parameters [i.e., architecture, not weights] of a classifier, for example to choose the number of hidden units in a neural network. 
Test set: A set of examples used only to assess the performance [generalization] of a fully specified classifier.

这是Ripley, B.D（1996）在他的经典专著Pattern Recognition and Neural Networks中给出了这三个词的定义。

简单来说就是：

* 训练集：用于决定模型参数（如神经网络中，各层之间的权重系数）
* 验证集：用于选择模型（如神经网络的结构，隐藏节点的个数）
* 测试集：用于测试模型的泛化性能 



#### References:
* http://blog.sina.com.cn/s/blog_4d2f6cf201000cjx.html
* http://www.cppblog.com/guijie/archive/2008/07/29/57407.html
* http://stats.stackexchange.com/questions/19048/what-is-the-difference-between-test-set-and-validation-set
* http://blog.sciencenet.cn/blog-397960-666113.html
* http://stackoverflow.com/questions/2976452/whats-is-the-difference-between-train-validation-and-test-set-in-neural-networ