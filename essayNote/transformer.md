## Transformer

原网站在 [illustrated-transformer](https://jalammar.github.io/illustrated-transformer/)

中文版在 [中文版](https://blog.csdn.net/longxinchen_ml/article/details/86533005)

解读源码 [blog](https://blog.csdn.net/zhaojc1995/article/details/109276945)

### 深度神经网络

1.	其复杂程度收到算力的限制。
2.	局部连接，权重共享和池化操作都让神经网络变得更简单。
3.	缓解复杂度的方法还有attention，选择关键信息（与任务相关的）进行处理。
三步：信息输入；注意力分布计算（通过计算与目标的相关性得到打分）；信息加权平均。

### RNN
1.	为什么需要RNN（循环神经网络）
神经网络只能单独的取处理一个个的输入，前一个输入和后一个输入是完全没有关系的。但是，某些任务需要能够更好的处理序列的信息，即前面的输入和后面的输入是有关系的。
用户行为，或许平均流量不能反映流量使用情况，一天每个小时内使用流量的序列才能反应用户行为。

参考一：例如正常转账交易情况下主动方登录后直接进行转账，而被骗转账交易下被骗者登录后先查看自己的芝麻分或借呗后再转账；一般正常人收到转账前无任何操作，收到钱后也不会直接转出，而欺诈者在收到转账之前，往往会查看自己账户信息或更改头像，而且会在收到钱后直接提现转出。所以在输入的行为中，只输入登录这个行为可能不能发现问题，登录后+查看信用积分+转账这一系类行为可能才是有问题。

![img](https://github.com/SnowZhao-wazi/SnowZhao-wazi.github.io/blob/main/essayNote/img/图片8.jpg)

2.	输入：一个向量，输出一个向量。

![img](https://github.com/SnowZhao-wazi/SnowZhao-wazi.github.io/blob/main/essayNote/img/图片9.jpg)
![img](https://github.com/SnowZhao-wazi/SnowZhao-wazi.github.io/blob/main/essayNote/img/图片10.png)

前一个隐藏层会继续作为下一时刻的输入。

3.	Lstm（1997）
主要是为了解决长序列训练过程中的梯度消失和梯度爆炸问题。简单来说，就是相比普通的RNN，LSTM能够在更长的序列中有更好的表现。

Forget、information、output

![img](https://github.com/SnowZhao-wazi/SnowZhao-wazi.github.io/blob/main/essayNote/img/图片11.jpg)

https://blog.csdn.net/zhaojc1995/article/details/80572098

### transformer
原论文Attention Is All You Need 2017 nips）
目前最强的特征抽取器当属transformer，bert、gpt等预训练模型也是基于transformer。从序列长度问题上，transformer依靠其独特的self-attention解决了RNN长序列梯度消失的问题；从训练方式上，transformer结构上的非序列性也解决了RNN难以并行的问题。

放弃RNN，attention is all u need。

第一步是从每个编码器的输入向量（每个单词的词向量）中生成三个向量。也就是说对于每个单词，我们创造一个查询向量、一个键向量和一个值向量。这三个向量是通过词嵌入与三个权重矩阵后相乘创建的。

![img](https://github.com/SnowZhao-wazi/SnowZhao-wazi.github.io/blob/main/essayNote/img/图片12.jpg)

计算自注意力的第二步是计算得分。假设我们在为这个例子中的第一个词“Thinking”计算自注意力向量，我们需要拿输入句子中的每个单词对“Thinking”打分。这些分数决定了在编码单词“Thinking”的过程中有多重视句子的其它部分。

查询thinking得分时，分数通过打分单词（所有输入句子的单词）的键向量与“Thinking”的查询向量求点积获得。

第三步和第四步是将分数除以8(8是论文中使用的键向量的维数64的平方根，这会让梯度更稳定。这里也可以使用其它值，8只是默认值)，然后通过softmax传递结果。softmax的作用是使所有单词的分数归一化，得到的分数都是正值且和为1。

第五步是将每个值向量乘以softmax分数。这里是希望关注语义上相关的单词，并弱化不相关的单词。

第六步是对加权值向量求和。（译注：自注意力的另一种解释就是在编码某个单词时，就是将所有单词的表示（值向量）进行加权求和，而权重是通过该词的表示（键向量）与被编码词表示（查询向量）的点积并通过softmax得到。）

![img](https://github.com/SnowZhao-wazi/SnowZhao-wazi.github.io/blob/main/essayNote/img/图片13.jpg)

“多头”注意力（“multi-headed” attention）的机制
它扩展了模型专注于不同位置的能力。
它给出了注意力层的多个“表示子空间”。

![img](https://github.com/SnowZhao-wazi/SnowZhao-wazi.github.io/blob/main/essayNote/img/图片14.jpg)

![img](https://github.com/SnowZhao-wazi/SnowZhao-wazi.github.io/blob/main/essayNote/img/图片15.jpg)



