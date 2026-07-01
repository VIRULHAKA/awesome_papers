# SigRep: Toward Robust Wearable Emotion Recognition With Contrastive Representation Learning

**来源** IEEE Access 2022

**作者** Dissanayake et al.

**arXiv** 

**Code**


---


## Task
基于SimCLR(CV)，设计了一个表示学习框架用于可穿戴设备的数据预训练
SoTA(2022)

## Insight & Novelty
### Inception Network Block
    不同大小的Conv1d层(1、3、5、7)用于学习不同尺度的特征。
    四组Conv1d + Max Pooling --> stack --> output

### Experiment Design: 
    把Sense & Learn framework的八种proxy tasks作为Representative Learning的benchmark。其中相对来说，Task 3, 4, 8训练完的encoder在下游任务中表现得最好。
    Task 3 --- Mask reconstruction: but only the eight statistical values. 
    学会理解局部缺失的信息，提升对局部结构的建模能力
    Task 4 --- Transformation Recognition: classify the respective class of the transformation(total of 8) applied to the signal. 
    理解各种信号变换，提高对不同形态的感知能力。
    Task 8 Triplet Loss: 原始样本-正样本(锚做增强)-负样本(另一种模态)
    学习相似样本靠近、不同样本远离的表示空间。

    Experiment 1
    做2分类/3分类的arousal/valence分类任务 --> Acc & F1 score
    在CASE上没有完全打败CorrNet。其他的数据集上的表现都是最好。

    Experiment 2
    模拟数据缺失(Signal Loss)。随机将p = 10% ~ 90%的数据置零 --> 检验鲁棒性
    几乎在每个p值上都超越了baseline model

    Experiment 3
    减少annotation --> 减少数据依赖 --> 证明representation的价值
    使用LOSO evaluation + 50%的subject级dropping
    相比于监督学习(20%)，准确率下降的更少(10%)
    12个实验中，有9个实验的t-test的p<0.05

    Experiment 4
    有没有inception network --> 对数据的影响
    高于baseline约10个百分点左右
    所有实验的t-test的p都<0.05

    Experiment 5 
    去除模态 --> 看哪个模态更重要
    combined显著更好，但是BVP和ACC是相对准确率更高的
    在50%数据丢失实验中，combined表现出了更强的鲁棒性