Arxiv: [this link](https://arxiv.org/pdf/2210.09276.pdf)

这篇方法比较重要，先写方法
## Method
给定一张原图和一条文字指示，Imagic通过三个阶段实现对原图的语义编辑
### Text embedding optimization
首先使用一个text encoder来编码给定的原始文本描述得到目标文本嵌入e<sub>tgt</sub>，然后使用一个预训练好的生成扩散模型并冻结参数，将该文字
