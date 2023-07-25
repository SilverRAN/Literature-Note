https://arxiv.org/abs/2305.17100

Motivation：目前主流的大模型和多模态训练方法还是在一个大尺度的一般数据集上预训练，然后放到特定领域数据或针对特定任务来fine-tune。但是这在实际应用中是不可能的。以疾病分类为例，目前的最新版国际疾病分类标准编码（International Classification of Diseases, ICD-10）收录的疾病编码有接近7万种，我们不可能针对每种疾病都去专门fine-tune一个分类的模型。此外，疾病诊断是一个需要综合所有信息全面考虑的过程，因为很多疾病之间存在内在联系，只关注单一症状有可能陷入“治标不治本”的误区。但是，要进行这样的多任务训练，则要求训练数据集也需要具有类似的同质结构或服从同分布。

摘要：提出了一种专门针对医学数据的GPT模型，可以接收多模态输入并用于多种下游任务，在包括15种模态的20个数据集上进行的5项任务都达到了SOTA。

模型架构：模仿OFA，使用BART（Bidirectional and Auto-Regressive Transformers）作为骨架。图片输入统一使用ResNet的前三个Block来编码，语言输入使用BPE（byte-pair encoding）来编码。输出方面使用VQ-VAE类似的codebook的思路对图像输出和文本输出进行统一。
改动：1）为了加快收敛速度和稳定性，增加了三个normalization层；2）对位置编码做了改动，设计了一种解耦方法以分离位置相关性，添加了位置bias。

总结：无监督预训练过程还是先采用单模态的方法，通过Masked language modelling和Masked image infilling实现，然后通过Visual question answering和Image captioning训练多模态，同时通过训练Object detection提升位置感知能力。最后通过fine-tune可以做classification和natural language inference。
**训练过程中使用自然语言实现任务管理，通过这样的方法把视觉和语言任务都统一为语言输入**

问题：好像最后并没有圆回如何解决训练数据同分布的问题？虽然效果不错，但和前面的motivation有点脱节了。
