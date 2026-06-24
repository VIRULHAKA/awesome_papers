# Provably Contractive and High-Quality Denoisers for Convergent Restoration

**来源** CVPR 2026
**作者** 
**arXiv**
**阅读日期** 2026/5/26


---


## Task
训练一个图像去噪/恢复网络$f$，让它满足：
1. 高质量：在PSNR/SSIM等指标上接近SOTA去噪器
2. 能收缩：Lipschitz constant < 1, 
min_x  (1/2) * ||x - y||^2  +  sum_{i in H} lambda_i * |(Wx)_i|
找一个离y（原始有噪声的图像）相对近的x（理论干净图像） + 惩罚高频项（噪声集中在高频）

y = A * x + n

## Challenge
我们希望去噪网络(denoiser)最终是收缩的(L < 1)，这样再不断地应用denoiser的话，我们就可以找到全局最优解。但是为了实现高质量的图像去噪，有的时候denoiser必须做一些步子比较大的操作(L > 1)。所以这里出现了一个矛盾：高质量的denoiser在面对数据集中小的微扰时，它的鲁棒性有时候会比较差（可能会找不到那个全局最优：prothesized originial image）。

## Insight & Novelty
They built a contractive architecture composed by multiple constrained blocks. 
(梯度 + 小波软阈值步) 高频系数如果绝对值很小（小于阈值 lambda_i），说明可能是噪声，直接置零；如果绝对值大，保留但缩小一点
--> (缩放卷积层) 表达质量！
--> (缩放层)
T(x; alpha, Lambda, K) = (1 / ((1-alpha) + epsilon))  *  Z( U(x; alpha, Lambda); K )


## Why it is useful for us
我们其实在解决类似的问题，其中的本质区别只不过是1D v.s. 2D以及噪声来源的区别。
我在实操中遇到的问题是：
1. 我不知道我做的这些预处理步骤是否合理，以及在什么样的标准之下是合理的。因为我们一直所做的感觉上总是一种主观上的评判：看看基线图像，看看峰有没有被滤掉...我觉得这毫无疑问是有帮助的，但是并不客观和科学。
2. 每次我可能都需要重新设计一个去噪方案，并不体系化。

所以这篇文章给我的一些帮助和idea是：
1. 我们或许可以从Lipschitz收缩的角度对我们的preprocess pipeline做出一定的客观评价。以及，可以从这个角度去优化pipeline。 
2. Import Plug-and-Play. 
我们建立一个框架，每次优化preprocess就是在优化框架里的降噪模块。这其实是一个很经济的方式

## I still don't get it
1. 谱归一化控制卷积层(don't really get it from a mathematical angle) --> 对微扰鲁棒
2. 有一些数学推导真的有点看不懂，我现在能理解insights但硬核推理看不太懂。
3. 缩放卷积层

## Related Key Concepts
1. Lipschitz continuity