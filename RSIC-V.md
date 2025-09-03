参考资料1 https://gitee.com/chuan-yi-wang/tinyriscv  
参考资料1.1 从零开始写RISC-V处理器 https://cloud.tencent.cn/developer/article/1813595?from=15425
参考资料2 https://space.bilibili.com/3280670?spm_id_from=333.788.upinfo.head.click  
参考资料3 《手把手教你设计CPU  RISC-V处理器篇》（先放着，不确定之后看不看）
参考资料4 《手把手教你设计RISC-V 处理器》 哔哩哔哩https://www.bilibili.com/video/BV1ZA4y197ng/?spm_id_from=333.337.search-card.all.click&vd_source=33e023647aa5a3b2b8e3d890546be051

2025/9/3 第一天学习
核心概念：RISC-V 是什么？
RISC-V 是一个基于精简指令集（RISC） 原则的开源、免费的指令集架构（ISA），和ARM、MIPS这些是属于同一类东西

让我们拆解这个定义：

指令集架构 (ISA)：

可以把它想象成 CPU 的“母语”或“基本词汇表”。

它定义了软件（如操作系统、应用程序）如何与硬件（CPU）进行通信，包括有哪些基本指令（如加减乘除、内存读写）、寄存器（CPU内部的高速存储单元）等。

你熟悉的 x86 (Intel/AMD 电脑处理器) 和 ARM (大多数手机和苹果 M系列芯片) 就是两种著名的私有指令集架构。

精简指令集 (RISC)：

这是一种设计哲学，其核心思想是：指令集应该尽可能简单、精简、高效。

每条指令只完成一个非常基本的操作，执行速度极快（通常一个时钟周期完成）。

与之相对的是 复杂指令集 (CISC)，如 x86，它的指令功能复杂，一条指令能完成更多工作，但设计和解码也更复杂。

开源和免费：

这是 RISC-V 最革命性的特点！

它不属于任何一家公司（不像 x86 属于 Intel/AMD，ARM 需要从 ARM 公司购买授权）。

任何人都可以免费地使用、修改、设计基于 RISC-V 的处理器，而无需支付高昂的授权费或版税。

所以，简单来说：RISC-V 是一套开放、免费的 CPU 设计标准说明书，任何人都可以按这份说明书来制造自己的芯片。

作者认为，RISC-V就是CPU中的Linux。 
本项目是一个单核32位的小型RISC-V处理器核，语言为verilog，设计目标是对标ARM Cortex-M3系列处理器  
RV32IM 是构建一个实用、高效的 32 位 RISC-V 处理器的标配选择  
ARM Cortex-M3：32位MCU IP核，用于STM32F1

**步骤1：环境搭建**

