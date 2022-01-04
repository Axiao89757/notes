# Ensemble-Based Cascaded Constrained Energy Minimization for Hyperspectral Target Detection

## Summary

>  写完笔记之后最后填，概述文章的内容，以后查阅笔记的时候先看这一段。**注：写文章summary切记需要通过自己的思考，用自己的语言描述。切忌 ctrl + c 原文**

## Research Objective(s)

> 作者的研究对象是什么？

高光谱图像目标检测

## Background / Problem Statement

> 研究的背景以及问题陈述：作者需要解决的问题是什么？

1. 高光谱图像具有明显的非线性、非高斯性的特征，传统的算法效果不好。辨别能力不行但泛化能力好。
2. 之前的非线性的模型，如：ACE、CEM等，因为目标光谱特征的固有可变性，导致过拟合。辨别能力好但泛化能力差。
3. 权衡辨别能力和泛化能力

## Method(s)

> 作者解决问题的方法/算法是什么？是否基于前人的方法？基于了哪些方法？

提出了E-CEM算法，综合集成学习和约束能量最小化两者优点，同时考虑到了辨别能力和泛化能力。

1. Cascade Detection

   提高非线性辨别能力。多层检测，应对高光谱图像的非线性。

2. Random Average

   提高稳定性。来源于集成学习，多个弱的分类器组成一个强的分类器。具体做法：在每一层随机构建多个CEM，综合它们的输出作为该层输出。

3. Multi-scale Scanning

   提高鲁棒性。具体做法：多尺度扫描光谱矢量，得到光谱特征，然后将原始光谱矢量与光谱特征。

## Evaluation

> 作者如何评估自己的方法？实验的setup是什么样的？感兴趣实验数据和结果有哪些？有没有问题或者可以借鉴的地方？

## Conclusion

> 作者给出了哪些结论？哪些是 strong conclusions，哪些又是 week conclusions (即作者并没有通过实验提供 evidence，只有 discussion 中提到；或实验的数据并没有给出充分的 evidence)？

## Notes(optional)

> 不在以上列表中，但需要特别记录的笔记。

## References(optional)

> 列出相关性高的文献，以便以后可以继续 track 下去。