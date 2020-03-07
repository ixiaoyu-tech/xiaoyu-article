> 用心分享，共同成长
>
> 没有什么比你每天进步一点点更实在了

&emsp;最近疫情慢慢的得到了缓解，各企业开始复工复产，龙叔也不能懈怠，马不停蹄地给大家输出，伙计们，抓紧学起来😊。

&emsp;在公众号刚做起来的时候，龙叔就最先讲了DES算法，DES其实对于每一个刚接触密码学的人来说，都是第一道正儿八经的难题，不过经过之前的哔哩吧啦，不少小伙伴肯定已经明明白白了，如果你还有不明白的问题，请暴击[聊聊密码学中的DES算法](https://mp.weixin.qq.com/s/tpupz8T5Ei-xB2pKdfKQQQ)这里！

&emsp;今天，主要和大家聊聊**DES 算法的安全性**，还有关于**二重、三重DES**算法的精妙之处。

#### DES的安全性分析：

&emsp;对于DES来说，它加密的明文数据仅有64bit，而且在这64bit的数据中，还有8位要用于奇偶校验，这样以来，大大削减了DES的加密安全性，因为其有效密钥只有56位，对于小数据的信息加密还具有一定的保密性，但是我们日常对于一些数据量较大的信息加密时，由于各次迭代中使用的密钥K是递推产生的，这种相关性必然降低了密码体制的安全性。

&emsp;因此，很多密码学的专家就在质疑56位密钥在加密时是否具有足够的安全性，其实我们想想，56bit的密钥必然是不够的。

&emsp;在众多DES算法的破解中，最为行之有效的就是穷举搜索法，那么56位长的密钥总共要测试2<sup>56</sup>次，如果每100毫秒可以测试1次，那么需要7. 2X 10<sup>15</sup>秒,大约是228493000年（两千多万年）。

&emsp;但是，也有人认为，如果时间允许，用穷举搜索法寻找正确密钥已趋于可行，所以也有人建议说如果要保护10年以上的数据，那你就别用DES。

&emsp;最近几年以来，差分法和线性攻击法也被用来破解DES算法。从理论上来说，利用这两种方法破译，无论是从性能还是从效率上来说，都要高于穷举搜索法，但是，对于后者的这两种方法，我们需要有超高的计算机水平来提供技术上的支持，基于以上方法的局限性，截至目前为止，在密码学界，我们还没有任何可以破译DES密码体制的系统分析法。

&emsp;其实我们如果使用穷举搜索法，不仅耗时耗材耗力，也大大拉长了破解的周期，针对于目前的计算技术以及DES加密技术的分析来看，采用16轮迭代的DES加密技术，在一定程度上仍然是可以保证安全的，但是对于16轮以下 的DES加密技术，我们应该谨慎使用。

&emsp;尽管如此，我们仍然需要不断的对DES算法进行进一步的改进，主要的改进方案就是通过增加密钥长度，来确保加密算法的安全性、保密性。

#### 多重DES算法：

&emsp;在密码学界，能够研究出来一种新的加密标准并且可以超过目前使用广泛的加密算法，是实实在在、真真切切不容易，也是密码学界的响当当的头等大事。在DES完成自己十年的任命期后，由于新的算法未被提出，DES不得不临危受命，继续肩负重任，在接下来的十年里，依然活跃在国际保密通信的舞台上。直至多重DES的出现，打破了这个僵局。

### <font face="宋体" color=blue size=3>二重DES算法：</font>

&emsp;为了增加密钥长度，我们可以采用多重的DES算法，最常用的就是二重DES。二重 DES 是多重使用 DES 时最简单的形式，其具体的加密解密操作如下图。

![](https://img-blog.csdnimg.cn/20200223103519611.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzODI4NzM4,size_16,color_FFFFFF,t_70)

&emsp;图中，明文信息为 P，两个加密密钥为 K<sub>1</sub> 和 K<sub>2</sub>。

   加密后，密文为: C = E<sub>K</sub><sub>2</sub> [ E<sub>K</sub><sub>1</sub>(P)]

   解密后，明文为: P = D<sub>K</sub><sub>1</sub> [D<sub>K</sub><sub>2</sub> (C)]             {<font face="宋体" color=blue size=3>注：解密时，以相反顺序使用两个密钥</font>}

&emsp;从上边我们可以看到，在利用二重 DES时，我们就可以巧妙的将原有的56bit的密钥变成**112bit**，极大的增加了加密时的安全性。

&emsp;但是，放过来我们可以想到，假设对任意两个密钥 K<sub>1</sub> 和 K<sub>2</sub>，如果存在另一密钥 K<sub>3</sub>,使得 E<sub>K</sub><sub>2</sub> [ E<sub>K</sub><sub>1</sub>(P)]= E<sub>K</sub><sub>3</sub> [ P] （也就是说，在单重的DES中存在一个密钥，与二重DES合起来加密时等价的），那么，二重 DES 以及多重 DES 都没有意义, 因为它们与 56 比特密钥的单重 DES 等价，但是后来经过证实，我们这种假设对于 DES算法并不能够成立。

**中间相遇攻击：**

​        一种对所有分组密码均有效的攻击方法。

　    首先，以二重DES为例。

​        加密：C = E<sub>K</sub><sub>2</sub> [ E<sub>K</sub><sub>1</sub>(P)]

​        解密：P = D<sub>K</sub><sub>1</sub> [D<sub>K</sub><sub>2</sub> (C)] 

​       首先设定一个中间值X，有  ：X = E<sub>K</sub><sub>1</sub>(P）= D<sub>K</sub><sub>2</sub> (C)

&emsp;在已知给定的消息对（P，C），首先，将明文P按所有可能的密钥 K<sub>1</sub> 加密，得到的256个结果，按X的值将所有结果排序放在一个表内，然后用所有可能的密钥K<sub>2</sub>对密文C解密，每解密一次，将解密结果与表中的数值进行对比，如果相等，就将刚才测试的两个密钥对一个新的明密文对进行验证，若验证成功，则认定这两个密钥对是正确的密钥。

　　结论：中间相遇攻击使用两组已知明密文对就可以猜出正确的密钥。

##### <font face="宋体" color=blue size=3>三重DES算法：</font>

###### <font face="宋体" color=red size=3>两个密钥的三重DES算法：</font>

&emsp;抵抗中途相遇攻击的一种方法是使用 3 个不同的密钥做 3 次加密，从而可使已知明文攻击的代价增加到 2<sup>112</sup>。然而, 这样又会使密钥长度增加到 56×3 = 168 bit，就会使密钥过于复杂，造成加密信息的冗余。

&emsp;所以我们就想出，能不能利用两个不同的密钥进行3次加密，一种实用的方法是仅使用两个密钥做 3 次加密，实现方式为**加密 -解密-加密**，即: C =E<sub>K</sub><sub>1</sub> [D<sub>K</sub><sub>2</sub> [ E<sub>K</sub><sub>1</sub>（P）]] ，第 2 步解密的目的仅在于使得用户可对一重 DES 加密的数据解密。

![](https://img-blog.csdnimg.cn/20200223103854727.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzMzODI4NzM4,size_16,color_FFFFFF,t_70)

&emsp;此方案已在密钥管理标准 ANS X .917 和 ISO 8732 中被采用。

###### <font face="宋体" color=red size=3>三个密钥的三重DES算法：</font>

&emsp;三个密钥的三重 DES 密钥长度为 168 bit，加密方式为C =E<sub>K</sub><sub>3</sub> [D<sub>K</sub><sub>2</sub> [ E<sub>K</sub><sub>1</sub>（P）]]  。

&emsp;关于DES算法呢，以后龙叔就不多bb了，如果你还有不懂，多看看龙叔的文章，当然也可以关注公众号，点击联系作者，第一时间为你解答呦！  

​                                                  <font face="宋体" color=blue size=6>  👉**「点关注，不迷路！」**</font>



### <font face="宋体" color=orange size=5>**更多密码学精彩文章：**</font>

[聊聊密码学中的DES算法](https://mp.weixin.qq.com/s/tpupz8T5Ei-xB2pKdfKQQQ) 

[据说你还不知道的分组密码](https://mp.weixin.qq.com/s/iYCG6sMtRHJBHke1x7XkFA)

[明明白白——流密码](https://mp.weixin.qq.com/s/XCi27yPXNNQkBGtNflUJqg)

[信息安全威胁，威胁到你了吗？](https://mp.weixin.qq.com/s/W0HN44O1YI6UcfeOKj9N-g)

[史上最全密码学思维导图](https://mp.weixin.qq.com/s/kcvm79m1-3SflYUo56idsg)

<h4   style="color:red;text-align:center">求点赞👍  求关注❤️ </h4>
<h4   style="color:blue;text-align:center">「转发」是明目张胆的喜欢，「在看」是偷偷摸摸的爱。</h4>
![](https://img-blog.csdnimg.cn/20200119220000969.gif)

`如果有人想发文章，我这里提供`<font face="宋体" color=blue size=4>**有偿征文**</font>`(具体细则微信联系)，欢迎投稿或推荐你的项目。提供以下几种投稿方式：`

- `去我的github提交 issue:` https://github.com/midou-tech/articles

- `发送到邮箱: 2507367760@qq.com 或者 longyueshier@163.com  或者 longyueshier@gmail.com`

- `微信发送: 扫描下面二维码，公众号里面有作者微信号。`

`精选文章都同步在公众号里面，公众号看起会更方便，随时随地想看就看。微信搜索` **`龙跃十二`**`或者扫码即可订阅。`

<p align="center"><image src="https://tva1.sinaimg.cn/large/006tNbRwly1galsp9a07kj30p00dwae3.jpg" ></image></p>