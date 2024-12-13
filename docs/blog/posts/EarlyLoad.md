---
draft: yes
date: 2024-12-13
categories:
  - paper
tags:
    - Apple Inc.
    - US Patent
    - Microarchitecture
---

# Early Load Execution via Constant Address and Stride Prediction

## Abstract

一种有效减少加载操作延迟的系统和方法。
在各种实施例中，处理器的逻辑在获取指令之后访问预测表。
对于预测表命中，逻辑使用从预测表中检索到的预测地址执行加载指令。
对于预测表未命中，当逻辑确定加载指令的地址并命中学习表时，且当学习表中存储的地址与确定的地址匹配时，逻辑更新置信度指示以指示更高的置信度。
当逻辑确定存储在学习表的给定表条目中的置信度指示达到阈值时，逻辑在预测表中分配存储在给定条目中的信息。
因此，在下一次查找预测表时，预测地址可用。

