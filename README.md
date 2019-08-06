# automatic_protocol_inference
自动协议特征推断
# 基于扩展树的协议格式推断方法
https://github.com/jmhIcoding/automatic_protocol_inference/blob/master/reference/%E5%9F%BA%E4%BA%8E%E6%89%A9%E5%B1%95%E5%89%8D%E7%BC%80%E6%A0%91%E7%9A%84%E5%8D%8F%E8%AE%AE%E6%A0%BC%E5%BC%8F%E6%8E%A8%E6%96%AD%E6%96%B9%E6%B3%95.pdf 

方法：
1. 使用N-gram 提取频繁出现的子串单元，本文提出的子串是带位置属性的。
2. 使用PMI(两个词的联合概率除以两个词的概率乘积)将子串单元进行合并,合并的时候只合并连续的、PMI大于一定阈值的串。
3. 根据提取到的关键词序列，构建关键词的前缀树。前缀树里面，节点是关键串，边是报文内容。
4. 根据前缀串，将同一路径的报文聚集起来，对边的内容进行规约，使用Needleman-Wunsch进行序列对齐。并且生成这些边 的取值类型（字符串，数字），取值个数（固定、枚举、 随机）。
5. 合并前缀树的不同路径，将相似的路径合并起来。

缺点：
只能处理明文信息，或者是相同的关键字。

优点：
PMI合并关键字子串是个很好的创新！！

如何评价推断的好坏？ 使用已知协议进行推断。HTTP,FTP,DNS,NetBIOS。重点看 关键词提取的准确率。没有关注召回率或者查全率。
