# Label-Efficient Semantic Segmentation with Diffusion Models

## Abstract
探究了diffusion model作为语义分割用具的可行性，特别是在带标注数据匮乏的情况下（即小样本学习）。通过全面而充分的研究，分析了diffusion model不同time step和不同decoder block生成的特征所分别具有的特点，并且验证了其捕捉到的特征确实含有语义信息。在此探究基础上提出了一个基于diffusion model的小样本训练语义分割方法。

## Introduction
Diffusion model作为当下SOTA的生成模型，已经在相当多的任务上超过了基于GAN的模型，例如colorization，inpainting，super-resolution以及semantic editing。但是diffusion model作为通用的特征学习工具的潜力还没有得到充分验证。因此，文中通过大量实验，对diffusion model产生的中间特征进行了探究，证实了其捕捉到了语义信息，并且可以作为一个image representation应用于下游任务。其贡献可概括为三点：
1）充分探究了diffusion model学习到的representation的特点，证实了其蕴含有语义信息；
2）提出了一个基于DDPM的语义分割方法，并且在小样本学习中表现优秀；
3）对比了diffusion model和GAN在相同数据集上学习到的representation的不同，说明了前者的优越性。

## Related works
略

## Representations from diffusion model
文中使用的DDPM网络为 https://github.com/openai/guided-diffusion 中的结构。预训练参数为在LSUN-Horse和FFHQ-256上预训练得到的结果。文中设计了一个用于pixel分类的MLP分类头以获取每个pixel位置的语义信息。该MLP使用20张图片进行微调，然后在20张图片上进行测试。
### Representation analysis
实验发现，反向扩散过程中相对靠后的步骤（即t比较小的时候）学习到的表征带有更多的语义信息，而相对靠前的步骤所产生的特征几乎不含有语义信息。而不同的block中，处于decoder中间位置（6，8，10附近）的block含有更多的语义信息。
之后实验又对比了不同尺寸的语义类别随时间和block的变化趋势。研究发现反向扩散过程刚开始的步骤产生的特征更多地包含大尺寸物体的信息，而后期的步骤才会满满开始包含小尺寸物体的信息。实验还发现相对较浅位置的block（即接近输出端）包含更多关于小尺寸物体的信息，而相对较深位置的block（即接近encoder）则含有更多大尺寸物体的信息。而且两种情况下仍然都是中间位置的block学习到的特征包含更多的信息。
此后，文章通过对不同block在不同time step产生的representation做聚类验证了相似的representation对应了含有相同语义信息的像素，并且也验证了较浅位置的block蕴含较多的细节信息而较深位置的block含有较多的全局信息。不同的time step产生的影响也是显而易见的，特别是对于位置较浅的block，在time step较大时生成的representation非常混乱，几乎是噪声。
### DDPM-based representations for few-shot semantic segmentation
介绍了文章所提出的语义分割架构，即通过大量无标注训练样本进行自监督预训练，然后使用少量带有ground truth的样本对segmentation head进行微调。特征从{5，6，7，8，12}这五个block和{50，150，250}三个time step的组合得到。其模型架构中并没有进行逐层上采样，而是统一将所有的features全都直接插值到原图尺寸然后concatenate，得到一个(1024+512+512+512+256)=8448维的tensor，然后使用一个MLP层对每个pixel的通道进行降维和预测，得到每个pixel的类别。

## Experiments
实验从LSUN选取了"bedroom","cat"和"horse"三个categories以及FFHQ-256作为数据集。除了真实数据之外，还分别用GAN和DDPM生成了一定数量的样本。此外还加入了ADE-Bedroom-30和CelebA-19两个数据集。实验选取的baseline如下：
* DatasetGAN：一种用于生成大量带有语义分割标签的图片样本的生成器，仅需要少量带标注的样本对其额外分支进行训练（专门用于和DatasetDDPM做对比）
* ALAE/GAN Inversion/GAN Encoder（基于GAN的方法）
* VDVAE（目前最好的自编码器方法）
* SwAV/MAE（自监督方法）
