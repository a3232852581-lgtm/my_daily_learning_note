参考资料1 https://gitee.com/chuan-yi-wang/tinyriscv  
参考资料1.1 从零开始写RISC-V处理器 https://cloud.tencent.cn/developer/article/1813595?from=15425
参考资料2 https://space.bilibili.com/3280670?spm_id_from=333.788.upinfo.head.click  
参考资料3 《手把手教你设计CPU  RISC-V处理器篇》（先放着，不确定之后看不看）
参考资料4 《手把手教你设计RISC-V 处理器》 哔哩哔哩https://www.bilibili.com/video/BV1ZA4y197ng/?spm_id_from=333.337.search-card.all.click&vd_source=33e023647aa5a3b2b8e3d890546be051

2025/9/3 第一天学习
核心概念：RISC-V 是什么？
RISC-V 是一个基于精简指令集（RISC） 原则的开源、免费的指令集架构（ISA），和ARM、MIPS这些是属于同一类东西

三级流水线 取指、译码和执行
取指 从程序存储器（通常是内存或指令缓存）中读取下一条要执行的指令，PC值自动增加，指向下一条指令的地址（例如，每条指令占4字节，则 PC = PC + 4）。  
译码 译码器解析指令中的操作码、寄存器地址  
执行 完成指令所要求的实际计算或操作 
并行： 
时钟周期 3:  
取指单元：取 指令3  
译码单元：解码 指令2  
执行单元：执行 指令1  


TCK	全称Test Clock  	      
|方向 输入	|为JTAG调试逻辑提供同步时钟	时钟信号，频率独立于系统主时钟
TMS	全称Test Mode Select  	
|方向 输入	|控制JTAG状态机的转换路径	在TCK上升沿被采样
TDI	全称Test Data In  	    
|方向 输入	|将调试指令或数据串行移入CPU内部的JTAG调试模块	在TCK上升沿被采样
TDO	全称Test Data Out  	    
|方向 输出	|将CPU内部的调试数据（如寄存器值、内存内容）串行移出至调试器	在TCK下降沿变化

**步骤1：环境搭建**

