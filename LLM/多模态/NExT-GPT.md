[NExT-GPT:Any-to-Any Multimodal Large Language Model](https://arxiv.org/pdf/2309.05519.pdf)
[Project page]([link](https://next-gpt.github.io/)https://next-gpt.github.io/)

Demo测试结果：比较差，语义理解能力不够强，且无法做分割、检测等传统任务。

文章提出了一种端到端的通用多模态大语言模型，支持任意模态到任意模态（文字，图像，视频，音频）。主体架构是使用预训练好的高性能编码器和解码器，仅仅微调一些起到连接作用的projection layer（1%的参数量），使其具有良好的可扩展性。此外还提出了一种模态切换的指示微调方法（Modality-switching Instruction Tuning, MosIT）以及相应的数据集。
