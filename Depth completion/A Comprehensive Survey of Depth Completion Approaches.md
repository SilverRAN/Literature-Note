# A Comprehensive Survey of Depth Completion Approaches

## Abstract
激光雷达生成的深度图通常是稀疏的，因此深度补全任务的目的就是从稀疏的深度图生成密集的深度图。传统方法单纯使用深度图来补全，而现代方法通常使用RGB图像作为参考,同时也有一些方法依赖于affinity matrix（关联矩阵/亲和矩阵/相似度矩阵）。所以本文主要分成两大类来介绍：传统方法和基于主干的方法，而后者由可以分成两小类：双分支方法和空间传播方法。

## Introduction
介绍了depth completion的应用场景和意义，之后概括了每个分类下几个具有代表性的工作.

## Methodologies
### 传统方法
这里作者介绍了一些没有使用神经网络的传统图像处理方法（我们可以看到最早的时候depth completion也被叫做depth inpainting或者depth upsampling），以及一些深度学习技术诞生之后使用了CNN的方法。但是仅仅使用深度图的情况下模型很难精确分割物体的边缘，因而限制了结果的精确度。

### 多模态融合的方法
1. Early fusion
多个模态的原始数据直接融合（拼合或相加），然后输入模型
2. Sequential fusion
也就是先对某一模态的数据（如RBG图像）编码-解码得到特征，然后再跟另一模态的数据early fusion。这样的好处是从RBG中提取到的depth map虽然更粗糙但会一定程度上包含物体的边界信息。
3. Later fusion
也即对多个模态的数据都先编码再解码，最后融合多个模态的结果得到最终结果。
4. Deep fusion
深度融合，也即在一个模态的数据编码的过程中将另一模态的数据的解码结果添加进去。

### 图片指导的方法
暂略，之后填坑
