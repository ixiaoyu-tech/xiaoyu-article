> 我是龙叔，一个分享互联网技术和心路历程的大叔。

##### 写在前面

这里有两个人，龙叔&你们的小编姐姐，

Hello，小伙伴们，《密码学系列》又回来啦，最近呢，我已经把之前的文章全放在了GitHub上，感兴趣的欢迎去star。

https://github.com/fengqing-tech/article

哎，在这一周被迫营业的日子里呢，发生了一些让人匪夷所思、琢磨不透的事情，你说我这么一个女孩子，除了看过李佳琪的直播，还没着迷过别的男主播，这不，梦想照进现实，是什么逼迫我的老师们也成了主播，而我成了在直播间刷礼物的<font face="黑体" color=orange size=3>**精神小伙**</font>。

<img src="https://tva1.sinaimg.cn/large/00831rSTly1gci2h5o0daj307s06v3yl.jpg" style="zoom:67%;" />

虽然学校得要正常营业，但是日常输出也必不可少，今天分享的是一个对于密码学界来说非常重要的算法，这么说你是不是还是体会不到它的重要性，那就比如说，它就像是口红里的Dior，包包里的爱马仕......不说了，钱包疼

#### AES算法：（Advanced Encryption Standard）

##### 🌟介绍：

AES（Advanced Encryption Standard）算法，高级加密标准，在密码学中又称Rijndael加密法，是美国联邦政府采用的一种区块加密标准。

在对 AES 设计之初呢，就有三个基本要求:

- 比三重DES 快

- 至少与三重 DES 一样安全

- 数据分组长度为 128 比特、密钥长度为 128/ 192/ 256 比特。

对DES还有疑问的朋友可以看看我之前的文章 [聊聊密码学中的DES算法](https://mp.weixin.qq.com/s/tpupz8T5Ei-xB2pKdfKQQQ)、 [二重DES、三重DES详解](https://mp.weixin.qq.com/s/Akvn7TR5sJaToK8mDrR5UA)

##### 🌟说明：

其实，从严格意义上讲，AES和Rijndael加密法又不完全一样，但是在实际应用中，我们常常将两者交换使用。

我们来看一组对比数据：

- AES的分组长度为128 比特，密钥长度则可以是128，192或256比特；
- 而Rijndael算法的分组长度和密钥长度可以是32比特的整数倍（下限：128bit，上限：256bit）

相对于AES来说，Rijndael算法使用更加不受约束。Rijndael算法可以支持更大范围的分组长度和密钥长度。

类似于明文分组和密文分组，算法的中间结果也须分组，将这种分组成为“状态”， 所有的操作都在状态上进行。例如：AES加密过程是在一个4×4的字节矩阵上运作，这个矩阵就称为“状态”，其初值就是一个明文分组（矩阵中一个元素大小就是明文分组中的一个Byte）。对于Rijndael算法来说，其加密时明文分组长度不定，其矩阵的大小需要根据具体情况来定。

加密时，AES算法的加密过程都包含4个步骤：

- SubBytes（字节代替）：通过非线性替换函数，用查找表的方式，独立的对每个字节进行替换。代换表 （也就是S-盒) 都是可逆（也就是说替换以后的字节可以通过查表找到原始的字节）。

- ShiftRows（行移位）：目的就是将矩阵中的横和列，进行循环式移位。
- MixColumns（列混淆）：列混淆的方法就是使用线性转换将每列的四个字节进行混合。为了充分混合矩阵中各个行，而进行的一步。
- AddRoundKey（轮密钥加）：矩阵中的各个字节都要与轮秘钥进行异或（XOR）运算，从而生产子密钥。

  最后一个加密循环中略微有所不同。

综上所述，组成 Rijndael 轮函数的计算部件简捷快速，功能互补。轮函数的伪 C 代码如下：

```c
Round (State , RoundKey) 
{ 
ByteSub (State ) ; 
ShiftRow (State ) ; 
MixColumn (Sta te) ; 
AddRoundKey (State , RoundKey) 
}
```

最后一个加密循环的轮函数与前面各轮不同，需要将 MixColumn 这一步去掉，其伪 C 代码如下：

```c
FinalRound ( State , RoundKey) 
{ 
ByteSub (State ) ; 
ShiftRow (State ) ; 
AddRoundKey (State , RoundKey) 
}
```

注：在以上伪 C 代码记法中， State、RoundKey 可用指针类型，函数 Round、FinalRound、 ByteSub、Shift Row、MixColumn、AddRoundKey 都在指针 State、RoundKey 所指向的阵列上进行运算。

#### 加密标准：

AES为分组密码，在加密时需要把明文分成长度相等的小组，每次对一组数据进行加密。在AES标准规范中，分组长度只能是128位，也就是说，每个分组为16个字节（每个字节8位）。AES的密钥长度可以是128bit、192bit或256bit。密钥的长度不同，所需要的加密轮数也会不相同，具体情况如下图所示：

|   AES   | 密钥长度（32位比特字) | 分组长度(32位比特字) | 加密轮数 |
| :-----: | :-------------------: | :------------------: | :------: |
| AES-128 |           4           |          4           |    10    |
| AES-192 |           6           |          4           |    12    |
| AES-256 |           8           |          4           |    14    |

#### 密钥编排 

密钥编排指从种子密钥得到轮密钥的过程, 它由密钥扩展和轮密钥选取两部分组成。 

其基本原则如下: 

- 轮密钥的比特数等于分组长度乘以轮数加 1。

- 种子密钥被扩展成为扩展密钥。 

#### 加密算法： 

加密算法的操作顺序：初始的密钥加，( Nr-1) 轮迭代，一个结尾轮。即： 

```c
Rijndael (State , Ciphe rKey) 
{ 
KeyExpansion (Ciphe rKey , ExpandedKey) ; 
AddRoundKey (State , ExpandedKey) ; 
for ( i=1; i< Nr; i++) Round (State , ExpandedKey + Nb*i) ;
FinalRound (State, ExpandedKey + Nb*Nr)
} 
```

其中 CipherKey 是种子密钥，ExpandedKey 是扩展密钥。密钥扩展可以事先进行 ( 预算) ,且 Rijndael 密码的加密算法可以用这一扩展密钥来描述, 即 ：

```c
Rijndael (State , ExpandedKey) 
{ 
AddRoundKey (State , ExpandedKey) ; 
for ( i=1; i< Nr ; i++) Round (State , ExpandedKey+Nb*i) ;
FinalRound (State, ExpandedKey + Nb*Nr)
}
```

#### 解密算法：

解密算法的操作顺序*:* 初始的密钥加，( Nr-1) 轮迭代， 一个结尾轮。

其中解密算法的轮函数为 ：

```c
InvRound (Sta te, RoundKey) 
{ 
InvByteSub ( State ) ; 
InvShiftRow ( State ) ; 
InvMixColumn (State ) ; 
AddRoundKey (State , RoundKey) 
} 
```

解密算法的结尾轮为 ：

```c
InvFinalRound (State, RoundKey) 
{ 
InvByteSub ( State ) ; 
InvShiftRow ( State ) ; 
AddRoundKey (State , RoundKey) 
}
```

关于AES算法最主要的就是要明白它的加密步骤，明文分组，以及密钥的使用，给大家贴了很多关于轮函数在AES算使用所需的代码块，需要的小伙伴自取哦！

关于文章中有任何问题都可以在公众号后台联系作者！

