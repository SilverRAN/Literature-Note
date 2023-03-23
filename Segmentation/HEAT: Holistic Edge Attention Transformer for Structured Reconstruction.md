# Heat: Holistic Edge Attention Transformer for Structured Reconstruction

## Abstract
本文提出了一个基于注意力的神经网络以实现结构性重建，通过输入一张栅格图，该模型可重建出一张平面图以描绘其几何结构。

## Introduction
略

## Related work
略

## Preliminaries
略

## Holistic Edge Attention Transformer
本文在这里才开始正式介绍其模型架构，主要包含3个过程：1）边缘和结点初始化；2）图片特征与边缘特征融合以及筛选；3）整体边界的自注意力用于结构性推理。
### 4.1 Edge node initialization
将拐角提取器生成的拐角点两两配对以生成边缘候选，并将此作为Transformer的节点，其feature由边缘的两个端点的x，y坐标经过positional encoding得到，从而每个edge会得到一个对应的256x256的feature map。

### 4.2 Image feature fusion and edge filtering
在初始化完成之后，每条边缘上每个像素的特征实际上是均匀采样得到的（因为编码方式都是一样的），因此还需要引入图片特征对其进行引导。文章使用了deformable-DETR中的adaptive attention机制，将每条边缘初始化的feature与图片提取到的feature pyramid融合，最终得到一个fused feature。
此后得到的fused feature还需要经过一个筛选机制，通过两个全连接层和一个sigmoid激活函数得到一个confidence score，并保留其中前K个候选。文章中将K设置为所有拐角点数量的3倍。

### 4.3 Holistic edge decoders
模型的解码端使用两个weight-sharing的Transformer分别作为Image-aware decoder和Geometry-only decoder，后者只是没有使用图片中提取的信息。

### 4.4 Masked training and iterative inference
略

### 4.5 Corner detector
模型中使用的corner detector实际上和上述的edge detector具有十分类似的结构，将原图先进行crop变成多个grid，然后与CNN提取到的features进行融合得到corner候选，再判断每个候选是否是一个真的corner。
