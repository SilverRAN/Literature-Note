# A Transformer-based Siamese Network for Change Detection

## Abstract
提出了一个基于Transformer的孪生网络架构以用于变化检测。

## Introduction
略

## Method
模型主要包含三个部分：1）1个层级式的Transformer编码端；2）4个特征差分模块来计算两个时相不同尺度特征之间的差异；3）1个轻量级的MLP解码端来融合多尺度特征并输出结果。
### 2.1 Hierarchical Transformer Encode
Encoder部分类似传统的CNN，逐层下采样，以减小尺寸并增大维度。但众所周知Transformer并不能改变尺寸，因此实际上还是使用了convolutional layer来实现尺寸的缩减。另外为了缩减参数量，模型中借用了Pyramid Vision Transformer中的*sequence reduction*操作。此外文章还提到其使用了一种与原始ViT中固定位置编码不同的方式以实现在不同于训练图片尺寸的样本上进行测试的方法。
在下采样过程中，模型还使用了另一个difference module来对两个时相分别提取到的特征进行融合，但其结构实际上十分简单，只是将两种feature堆叠之后进行常规的Conv+ReLU+BatchNorm操作。

## Experimental setup
略

## Results and discussion
使用了LEVIR-CD和DSIFN-CD两个数据集，其中在LEVIR-CD上的IoU，F1和OA达到了SOTA。
