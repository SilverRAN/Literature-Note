Arxiv: [this link](https://arxiv.org/pdf/2210.09276.pdf)

这篇方法比较重要，先写方法
## Method
给定一张原图和一条文字指示，Imagic通过三个阶段实现对原图的语义编辑
### Text embedding optimization
首先使用一个text encoder来编码给定的原始文本描述得到目标文本嵌入e<sub>tgt</sub>，然后使用一个预训练好的生成扩散模型并冻结参数，将e<sub>tgt</sub>输入扩散模型中得到结果并比较与原图的相似度。由于我们在图片编辑的过程中希望图片的大部分内容都不发生变化，因此需要搜索（文中称为优化）得到一个处于e<sub>tgt</sub>附近的文本嵌入e<sub>opt</sub>，它可以生成与原图非常接近的图像结果。

### Model fine-tuning
但是需要注意的是，为了保证后续线性插值操作的有效性，上述第一步的embedding优化操作只能进行很少几次迭代，因此即使是e<sub>opt</sub>也不能保证生成和原图完全一致的结果。为了弥补这一gap，需要在保持e<sub>opt</sub>不变的情况下，再对diffusion模型进行微调。但这里要注意，文中提到这个fine-tuning过程也会对生成模型中的辅助模块，例如超分辨率模块进行微调，而且对这一部分的微调是基于原始的e<sub>tgt</sub>的。

### Text embedding interpolation
微调结束之后，我们得到的生成扩散模型应该能基于e<sub>opt</sub>生成非常接近原图的结果。要得到想要的编辑效果，只需要将e<sub>tgt</sub>和e<sub>opt</sub>做一个简单的线性插值得到<u>e</u>，然后将其输入微调后的生成模型即可得到想要的编辑效果。
