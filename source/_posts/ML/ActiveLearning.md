---
title: 'Active One-shot Learning'
date: 2022-12-08 20:43:22
categories: 机器学习 #
tags: [FSL]
mathjax: true
---

> 主动学习，半监督学习的一种，让模型决定哪个数据点被打标签，减少执行任务所需要的监督量。
>
> 用在 犯错误成本很高，而请求标签成本较小的领域中。
>
> 方法：某些example提供最多的信息，某些数据打什么标签具有极大的不确定性。传统的办法是
>
> **启发式的来近似风险**

```
For: Input
模型可以选择自己预测对应的标记（如果预测对了就会给一个奖励，预测错了会给一个惩罚），
也可以选择索要正确的标记（直接给一个小的惩罚）
```


