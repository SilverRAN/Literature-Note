# Learning Affinity via Spatial Propagation Networks

## Abstract
暂略

## Introduction
暂略

## Related Work
暂略

## Proposed Approch
空间传播网络的功能：可以将一个二维图（例如粗略的分割图）转变成具有所需要性质的新二维图（例如精细的分割图）。本文设计了一个等价于标准各向异性传播过程（standard anisotropic diffusion process）的模块以实现该功能。本文还证明了从旧二维图到新二维图的变换是由一个Laplacian matrix控制的，该矩阵由空间传播模块的参数组成。由于该传播模块是可微的，因此它的参数可以由神经网络学习到。
### 线性传播
暂略
