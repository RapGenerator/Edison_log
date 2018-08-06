# ChaoqunWang
8.1:  
关于《Attention Is All You Need》提出的Transformer模型，有一篇非常详细的[解释(包含代码)](http://nlp.seas.harvard.edu/2018/04/03/attention.html)

8.2:  
找到一份比较清晰的seq2seq的TensorFlow代码实现（用于生成歌词，未加入attention）

8.3:  
对dynamic_rnn的解释：  
基础的RNNCell有一个很明显的问题：对于单个的RNNCell，我们使用它的call函数进行运算时，只是在序列时间上前进了一步。比如使用x1、h0得到h1，通过x2、h1得到h2等。这样的h话，如果我们的序列长度为10，就要调用10次call函数，比较麻烦。对此，TensorFlow提供了一个tf.nn.dynamic\_rnn函数，使用该函数就相当于调用了n次call函数。即通过{h0,x1, x2, …., xn}直接得{h1,h2…,hn}。
具体来说，设我们输入数据的格式为(batch\_size, time\_steps, input\_size)，其中time\_steps表示序列本身的长度，如在Char RNN中，长度为10的句子对应的time\_steps就等于10。最后的input\_size就表示输入数据单个序列单个时间维度上固有的长度。另外我们已经定义好了一个RNNCell，调用该RNNCell的call函数time\_steps次
  
学习如何堆叠RNNCell：MultiRNNCell  
很多时候，单层RNN的能力有限，我们需要多层的RNN。将x输入第一层RNN的后得到隐层状态h，这个隐层状态就相当于第二层RNN的输入，第二层RNN的隐层状态又相当于第三层RNN的输入

  
 Output说明:   
找到源码中BasicRNNCell的call函数实现：这句“return output, output”说明在BasicRNNCell中，output其实和隐状态的值是一样的。因此，我们还需要额外对输出定义新的变换，才能得到图中真正的输出y。由于output和隐状态是一回事，所以在BasicRNNCell中，state\_size永远等于output\_size。


8.4:  
修改Attention的种类和参数，以及查loss下不去的原因。

8.6:  
根据老师的经验：  
1. 开始时可以用adam（lr不需要调），快收敛时改成sgd。  
2. sparse cross entrophy loss效果可以  
3. sigmoid loss适用于多分类多标签  
4. 取中间最好的一次结果的checkpoint，不用考虑epoch的值。  
5. 过拟合的时候可以加dropout  
6. 后期需要爬更多数据（至少几十万条）  
7. 控制生成句子长度的方法（实习生做的）  
8. 最后input的时候大多数用模板（固定的句子长度、格式等）  
9. seqGAN效果不错（训练方法在论文里有）  

