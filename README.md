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

# Automatic protocol field inference for deeper protocol understanding

本文作者提出了一些自动提取协议字段、以及推断协议字段类型的方法。同时考虑了textual的协议和二进制协议。
使用n-gram来寻找 消息类型字段、消息长度字段、Host ID字段、Session ID字段、Transaction ID字段以及计数器字段。
文中把 消息长度字段 看作是一个测试线性拟合性的问题。
<a href="https://www.codecogs.com/eqnedit.php?latex=MSG_{len}&space;=&space;a\times&space;FIELD_{value}&space;&plus;&space;b" target="_blank"><img src="https://latex.codecogs.com/gif.latex?MSG_{len}&space;=&space;a\times&space;FIELD_{value}&space;&plus;&space;b" title="MSG_{len} = a\times FIELD_{value} + b" /></a>

把HostID看作是与源IP 互信息特别强的一种变量。

把Session ID 看作是与源IP/目的IP 互信息特别强的变量。

同时在寻找Accumulator的时候，关注同一个会话中，C2S或S2C的相同字段的差分值。

# 基于最大似然概率的协议关键词长度确定方法
本文提出使用隐马尔科夫对报文字段序列进行建模的方法。
在本文的设计中，隐状态对应着某个最长频繁字符串，观察状态对应着报文的具体某个字节的内容。隐状态是带有长度的，模型的主要目的有两个：
1. 寻找概率最大的状态序列，并将状态序列里面的所有状态作为最终关键词。
2. 寻找关键词 概率最大的那个长度

训练的时候，基于数据包，使用极大似然估计参数取值

疑问：
1. 对于有重叠子串的 字符串，如何确定它属于什么状态？？？ 例如 Connection: 和ContentType: 前面的字符一样
2. 论文数学公式上下标结构混乱

创新方向:

状态的定义，是否可以由模型自己生成？ 只不过 用“状态1”,“状态2” ···这种无语义的标签

借鉴随机过程的思想，让模型自己规约出状态序列??
