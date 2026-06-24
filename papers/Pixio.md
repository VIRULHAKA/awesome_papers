# In Pursuit of Pixel Supervision for Visual Pre-training

**来源** CVPR 2026
**作者** Lihe Yang et al. 
**arXiv** https://arxiv.org/abs/2512.15715
**Code** https://github.com/facebookresearch/pixio
**阅读日期** 2026/6/23


---


## Task
There are two standard paradigms of self-supervised pre-training: masked reconstruction and contrastive learning. The latter relies heavily on human conception of "the world". i.e. "positive pairs" are defined by human: which are alike and which are not. "will ultimately hinder the upper bound of visual intelligence due to inherent human bias"
This is like what I did in this semester: it was I who decided the positive pairs of PPG and GSR signals, even though using the segmentation of the same temporal period seems logical. 
Of course, reducing inducting human bias is important in pursuing "ground truth". However, is it reasonable to leave out the distribution of humans?
## Insight & Novelty
They improved MAE by introducing four key designs. 
1. Deeper encoder
2. Larger mask block 
    Single-patch masking could be reconstructed plausibly by simply copying from nearby visible patches. 
3. More [CLS] tokens
    What does it mean? 
4. Web-Scale Data and Curation
    They pre-train the model on a large-scale data, and they have used two novel curation strategies. 
    4.1 Loss-based curation
        Pre-compute loss for every image and choose those with higher loss to leave out the images that are more easy-to-reconstruct. 
    4.2 Color hitogram entropy enlightened curation
        Text-heavy images are usually with low histogram entropy but with high loss, so they filter these images to preserve rich and diverse visual content. 
## Why it is useful for us
EDA(GSR)领域的重建的预训练做的人比较少。以对比学习为主。UME
而且需要花很多时间在去除运动伪影上。
“EDA 现在几乎全靠对比学习，而对比需要精心设计的归纳偏置（正样本怎么定义、增强怎么做）——这恰恰是 Pixio 批评的"人为偏置"。Pixio 证明了纯重建（无隐空间目标）也能学出强表征。EDA 领域几乎没人认真试过"大规模掩码重建 + 难度软采样"这条路，这是个明显的空白点。不过要注意 EDA 慢变非周期，单纯时间掩码很可能太容易（线性插值就能猜），得用 Pixio 说的"更大掩码块"思路，甚至专门掩掉 phasic 反应段（SCR 峰）这种高信息事件。”
自回归预测式与训练。
## I still don't get it
## Related Key Concepts