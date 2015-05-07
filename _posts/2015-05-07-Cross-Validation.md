---
layout: post
title: Cross Validation
date: 2015-05-07
tags: Machine-learning
categories: Research
published: true

---
#### Purpose of CV

>The goal of cross validation is to define a dataset to "test" the model in the training phase (i.e., the validation dataset), in order to limit problems like overfitting, give an insight on how the model will generalize to an independent dataset (i.e., an unknown dataset, for instance from a real problem), etc.

Cross-Validation的主要目的是为了准确估计模型的泛化能力。简单来说，交叉验证就是把数据集分为训练集和验证集，前者用来训练获得模型参数，后者用来测试模型的泛化能力，可预防模型过拟合现象。


#### K-fold cross-validation

K折交叉验证：把原始数据随机分成K等份，每次取其中一份作为验证集，其余K－1份作为训练集，如此重复K次，使得所有数据都有机会作为训练或者验证。一般情况下，K＝10已经足够。

#### Reference:
* [Cross-validation (statistics)](http://en.wikipedia.org/wiki/Cross-validation_(statistics))