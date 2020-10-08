---
layout: post
title: "Fine-Grained Visual Categorization State-of-the-Art Pro-and-Con Survey"
date: 2020-10-08 22:25:00 +0800
comments: false
categories: [road2phd]
excerpt: "Emergency post: 细粒度图像分类（FGVC）研究现状的优缺点对比简易综述，整理主流方法和性能指标。拯救 AAAI 2021 废稿。"
math: true
# stickie: true
---

这是一篇 emergency post。

之前没有完全整理过 FGVC state-of-the-art pros and cons，这个子领域百家争鸣，近三年的工作、各种方法之间没有明确的继承关系或者方法分类，对我造成了一定困扰；就好像研究这个方向的一群人坐在了一张桌子上，一个人说：“我有一个方法，嗯（摆出）”，紧接着另一个人又说：“诶，巧了，我这也有一个（伸出）”，然后又有一个人说话了：“啊，我这还有一个呢（拍下）”，以此方式模型和算法不断更迭，某些模型之间 accuracy 仅相差 0.1%。

近期结合某 AAAI2020 的工作，自己设计了构图方法、引入 GCN 做分类，直接导致模型不收敛，至今没有救回来，而之前的代码问题也导致错过 AAAI21 (w/draw)。今天小组和 Ming-Hsuan 开了组会之后，三个做了报告的都有些自闭。我自己感觉还是和顶尖水平的同行实力相差太远，甚至还不需要这种级别的老师指导。Meeting 上我 note 了几点比较有用东西，其中一个就是这个 pros-and-cons survey，虽然时间紧急，还是要做一下，这也是即刻能做起来的一件事情，借此也想针对这些工作的 cons 进行 counter/commence。

## Paper List

本节将比较关注工作的论文和代码进行汇总，这些 paper 一部分选自 [Paperw/codes FGVC leaderboard](https://paperswithcode.com/task/fine-grained-image-classification) 的论文，另一部分是从上部分论文中的对比试验、参考文献中挑选拓展出来的。因此这份 paper list 并不能 cover sota 全貌，仅由我主观地挑选出框架、模块、方法设计比较清晰的，专为细粒度任务设计的，代码清晰的工作进行汇总，以便检索。

| Model | Paper | Code | Conference |
| ---- | ---- | ---- | ---- |
| MMAL-Net | [Multi-branch and Multi-scale Attention Learning for Fine-Grained Visual Categorization](https://arxiv.org/pdf/2003.09150v3.pdf) | [w/ code](https://github.com/ZF1044404254/MMAL-Net) | ECCV 2018 |
| PMG | [Fine-Grained Visual Classification via Progressive Multi-Granularity Training of Jigsaw Patches](https://arxiv.org/pdf/2003.03836v3.pdf) | [w/ code](https://github.com/RuoyiDu/PMG-Progressive-Multi-Granularity-Training) | AAAI 2020 |
| CIN | [Channel Interaction Networks for Fine-Grained Image Categorization](https://arxiv.org/pdf/2003.05235v1.pdf) | w/o code | AAAI 2020 |
| DCL | [Destruction and Construction Learning for Fine-Grained Image Recognition](https://openaccess.thecvf.com/content_CVPR_2019/papers/Chen_Destruction_and_Construction_Learning_for_Fine-Grained_Image_Recognition_CVPR_2019_paper.pdf) | [w/ code](https://github.com/JDAI-CV/DCL) | CVPR2019 |
| Cross-X | [Cross-X Learning for Fine-Grained Visual Categorization](https://arxiv.org/pdf/1909.04412v1.pdf) | [w/ code](https://github.com/cswluo/CrossX) | ICCV 2019 |
| S3N | [Selective Sparse Sampling for Fine-Grained Image Recognition](http://openaccess.thecvf.com/content_ICCV_2019/papers/Ding_Selective_Sparse_Sampling_for_Fine-Grained_Image_Recognition_ICCV_2019_paper.pdf) | [w/ code](https://github.com/Yao-DD/S3N) | ICCV 2019 |
| DFL-CNN | [Learning a Discriminative Filter Bank within a CNN for Fine-grained Recognition](https://openaccess.thecvf.com/content_cvpr_2018/papers/Wang_Learning_a_Discriminative_CVPR_2018_paper.pdf) | w/o code | CVPR 2018 |
| MaxEnt | [Maximum-Entropy Fine Grained Classification](http://papers.nips.cc/paper/7344-maximum-entropy-fine-grained-classification.pdf) | w/o code | NerPIS 2018 |
| MC-Loss | [The Devil is in the Channels: Mutual-Channel Loss for Fine-Grained Image Classification](https://arxiv.org/pdf/2002.04264.pdf) | [w/ code](https://github.com/PRIS-CV/Mutual-Channel-Loss) | IEEE TIP20 |
| FCAN | [Fully Convolutional Attention Networks for Fine-Grained Recognition](https://arxiv.org/pdf/1603.06765.pdf)  | w/o code | n/a |
| RA-CNN | [Look Closer to See Better: Recurrent Attention Convolutional Neural Network for Fine-grained Image Recognition](https://openaccess.thecvf.com/content_cvpr_2017/papers/Fu_Look_Closer_to_CVPR_2017_paper.pdf) | w/o [code](https://github.com/Jianlong-Fu/Recurrent-Attention-CNN) | CVPR 2017 |
| ViT | [An Image Is Worth 16x16 Words: Transformers for Image Recognition at Scale](https://openreview.net/pdf?id=YicbFdNTTy) | [w/ code](https://github.com/lucidrains/vit-pytorch) | ICRL 2021 |

## Pro-and-Con Comparison

| Model | Pros / Contributions | Cons | Accuracy |
| ---- | ---- | ---- | ---- | ---- |
| MMAL-Net | √ End-to-end multi-branch network <br/>√ Learn object’s discriminative regions for recognition effectively <br/>√ The accuracy of Attention Object Location Module is achieved by only using category labels <br/>√ Present an Attention Part Proposal Method (APPM) without the need of part annotations <br/>√ Outperform the state-of-the-art methods and baselines on three standard benchmark datasets | × | CUB-200: 89.6%<br/>Stanford Cars 94.7%<br/>FGVC-Aircraft: 95.0% |
| PMG | √ Novel progressive training strategy operates in different training steps <br/>√ Cultivate the inherent complementary properties across different granularities for fine-grained feature learning. <br/>√ A simple yet effective jigsaw puzzle generator to form different levels of granularity <br/>√ PMG obtains state-of-the-art or competitive performances on all three standard FGVC benchmark datasets | × | CUB-200: 89.6%<br/>Stanford Cars 95.1%<br/>FGVC-Aircraft: 93.4% |
| DFL-CNN | √ Enhance the mid-level learning capability of the classical CNN by introducing a bank of discriminative filters <br/>√ Simple and effective <br/>√ High human interpretability <br/>√ Consistent performance across different fine-grained visual domains and various network architectures | × | CUB-200: 87.4%<br/>Stanford Cars 93.8%<br/>FGVC-Aircraft: 92.0% |
| CIN | √ Propose a self-channel interaction (SCI) module able to model the interplay between different channels within an image <br/>√ Propose a novel contrastive channel interaction (CCI) module to learn channel-wise relationships between images <br/>√ Method achieves better performance over current state-of-the-art | × | CUB-200: 88.1%<br/>Stanford Cars 94.5%<br/>FGVC-Aircraft: 92.8% |
| Cross-X | √ Cross-X learning approach for finegrained feature learning <br/>√ Cross-X learning explores relationships between features from different images and different network layers <br/>√ Address the issue of robust multi-scale feature learning through cross-layer regularization | × |  CUB-200: 87.7%<br/>Stanford Cars 94.6%<br/>FGVC-Aircraft: 92.7%<br/>Stanford Dogs: 88.9%<br/>NABirds: 86.4% |
| DCL | √ A novel Destruction and Construction Learning (DCL) framework <br/>√ State-of-the-art performances <br/>√ No need extra part/object annotation <br/>√ No computational overhead at inference time | × | Stanford Cars: 93.0%<br/>FGVC-Aircraft: 94.5% |
| S3N | √ Novel Selective Sparse Sampling framework <br/>√ Substantial improvement over the baselines concerning model accuracy and the ability of mining visual evidence | × | CUB-200: 88.5%<br/>Stanford Cars 94.7%<br/>FGVC-Aircraft: 92.8% |
| ViT | √ Latest trend <br/>√ Experiment with applying a standard Transformer directly to images, with the fewest possible modifications. | × | Oxford Flowers: 99.68% (ViT-H/14) 99.74% (ViT-L/16)<br/>Oxford-IIIT Pets: 97.56% (ViT-H/14) 97.32% (ViT-L/16) |

## Brief Conclusion

明确了 end-to-end 网络的两种方法分类。DFL-CNN 的论文中指出：more recent CNN-based approaches are usually trained end-to-end and can be roughly divided into two categories: **localization-classification subnetworks** and **end-to-end feature encoding**，类似于 detection/segmentation 中的 two-stage 和 one-stage 的分类。根据之前实验的思路，实现细节上，我的方法归类于 part-based end-to-end feature encoding FGVC。实作中往往只有图片的类别标签，所以很大程度上 loc-cls subnet 方法的 localization 处于半监督或弱监督的状态。

其次是各个方法的设计，有出发点上的不同。MC-Loss，MaxEnt 等工作是从 loss function 或 metrics 的角度对 plain network 做提升，对概率分布一类的推理证明要求较高；多层级的网络一般采用多 loss 加和，。MMAL-Net，PMG，DCL，S3N 等工作是以设计了 novel framework 自居，设计了一些新的模块和 feature 使用方式。不同的 part-based 的工作有所不同，但已经成为细粒度分类的必备。例如，MMAL-Net 开发了 AOLM 和 APPM 用于预选区域和目标分片的两级模块；PMG 中的 jigsaws generator 直接将图片分割、打乱成拼图块，但配合 progressive training 就显得独立性不够强，也许存在耦合。对 feature maps 利用方式的改进大多是通过通道的 selection，attention，不同层级、形状的 pooling 等方式进行的。
