####
[NExT-GPT:Any-to-Any Multimodal Large Language Model](https://arxiv.org/pdf/2309.05519.pdf)

####
[Project page](https://next-gpt.github.io/)

Demo测试结果：比较差，语义理解能力不够强，且无法做分割、检测等传统任务。

文章提出了一种端到端的通用多模态大语言模型，支持任意模态到任意模态（文字，图像，视频，音频）。主体架构是使用预训练好的高性能编码器和解码器，仅仅微调一些起到连接作用的projection layer（1%的参数量），使其具有良好的可扩展性。此外还提出了一种模态切换的指示微调方法（Modality-switching Instruction Tuning, MosIT）以及相应的数据集。

## 1.引言
略

## 2.相关文献
略

## 3.架构
总体上可以分成三个部分：多模态编码端，LLM理解推理部分，以及解码端。
编码端使用的是预训练好的ImageBind，具有处理六种输入模态的能力。然后使用一个线性层将编码后的表征映射为一个LLM可以理解的language-like的表征。
使用Vicuna作为core agent，它在接收不同模态的表征之后会生成两部分：1）文字回答；2）对于每个模态的signal token，以指示每个解码层是否要生成内容，以及生成何种内容。
LLM生成的内容先通过一个基于Transformer的映射层投射为可以被后续的decoders理解的表征形式，然后使用现成的latent conditional diffusion models生成不同模态的内容，包括stable diffusion，Zeroscope以及AudioLDM。

## 4.轻量级多模态对齐学习
该模型采用的是一种松散耦合（loosely-coupled）的系统（*有什么劣势吗？*），因此只需要更新两个映射层就可以了。
编码端的projection layer通过X-Caption任务实现训练和更新；
解码端的主要训练目标是减小LLM生成的带有signal tokens的文本与conditional diffusion models所接收的文本描述之间的差异，因此将LLM生成的结果先进行映射后，再将input对应的captioning输入conditional diffusion model的文字编码器，最小化两者之间的距离，这样学习过程仅仅依赖于captioning文本而不需要图像或视频等，所以是轻量的（*没明白，LLM产生tokens的过程不需要输入图像或视频吗？需要看代码确认*）。

## 5.模态切换指示微调
在实现了编码端和解码端对齐之后，要使得整个系统完全遵从用户指示来运行，这中间仍存在一个gap。因此instruction tuning是需要的。在这个过程中，文章采用LoRA方法，将Adapter的参数和Projection layer的参数一起更新，优化目标和解码端训练时的目标一致，仍然是Output Projection的输出与Golden captioning经过conditional encoder处理后的输出的距离。
具体到数据，文中使用了三种类型的数据集：
1）“Text+X -> Text”: 输入为文本+其他模态，输出为文本；
2）“Text -> Text+X”：输入为文本，输出是文本+其他模态，基于Captioning数据对，利用特定模板以GPT-4来生成；
3）“MosIT”：研究者发现仅使用以上两种数据进行instruction tuning效果并不好，并认为有两点原因：一是上述数据集中缺乏模态变化的数据样本，二是多轮对话内容也不够。因此他们通过GPT-4建立了一个支持多轮对话并带有模态转换内容的IT数据集。由于GPT-4并不能生成其他模态内容，因此其他模态的生成结果是从互联网上利用检索工具（如Youtube）或AIGC工具（如Stable Diffusion和Midjourney）获取并进行人工筛选的。

## 6.实验部分
### 6.1 Text-X（图像、视频、音频）
表现还行，基本处在第一第二的位置
### 6.2 X-Text
