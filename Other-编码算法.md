# Other-编码算法

> Unicode、GBK和UTF8的关系是什么？

Unicode是**字符表**，GBK、UTF-8是**编码方式**

+ 字符表：将字符转化为数字序列
+ 字符编码：指示如何间断上述序列
+ 编码方式是字符表的具体**实现形式**

## 字符集

Unicode中文名为**万国码**，和ASCII一样是**字符集(character set)**；

其作用是**用一系列数字来表示字符(character)**，这些数字有时也称为码点(code points)

目前Unicode字符集表示完所有字符后还有剩余，这些暂时用不到的部分通常用占位符`FFFD`表示

## 字符编码

字符集将任何字符化为一串数字

数字存入计算机必须经过字符编码**，**使用时译码：

+ 指示数字用几位字节表示
+ 指示字节的范围

```python
# utf-8将1个汉字分为3个字节，gbk将1个汉字分为2个字节
# utf-8编码为6个字节，采用gbk译码会产生3个汉字
print("乱码".encode('utf8'))
# outcome:b'\xe4\xb9\xb1\xe7\xa0\x81'
print("乱码".encode('gbk'))
# outcome:b'\xc2\xd2\xc2\xeb'
print("乱码".encode('utf8').decode('gbk'))
# outcome:'涔辩爜'
```

+ utf-8：是Unicode的一种实现方式，并兼容ASCII
  + 对英文使用1个字节，中文使用3个字节
+ GBK：中国自己采用的编码方式，GK指代“国标”
  + 无论中英文都是2个字节
+ URL编码：浏览器发送数据给服务器时的编码；**文本字符**转为`%XX`的形式
  + 不编码字符：字母、数字，以及`-`、`_`、`.`、`*`
  + 其他字符：转化为utf-8编码，然后每个字节前加`%`，并始终为大写；
    + 例：`中` > `0xe4b8ad` > `%E4%B8%AD`
+ Base64：将**二进制数据**编码为文本格式
  + 所谓Base64，指的是共有64种字符参与编码（字母、数字、`+`、`/`），并使用`=`作为占位符
  + 原理：将3字节（3*8=24bit）的二进制数据，以6bit为单位分成4个int整数，再将整数映射到字符上；如果不能被3整除，则补`0x00`再进行编码，编码后在结尾增加相同数量的`=`；
    + 字符`A`~`Z`对应索引`0`~`25`，字符`a`~`z`对应索引`26`~`51`
    + 字符`0`~`9`对应索引`52`~`61`
    + 最后两个索引`62`、`63`分别用字符`+`和`/`表示。
  + 适用于`url`的Base64：将`+`、`/`分别换为`-`、`_`

---

### utf-8编码与解码（JavaScript中）

> 参见：[TextDecoder 和 TextEncoder](https://zh.javascript.info/text-decoder)

解码：

```js
let decoder = new TextDecoder();	// 不指定格式，默认为utf-8
decoder.decode(input);	// 被解码的缓冲区(buffer)，例如Uint8Array格式
```

编码：

```js
let encoder = new TextEncoder();
encoder.encode("中");	// Uint8Array(5) [228, 184, 173]
encodeURI("中") 	// '%E4%B8%AD'
```

将上述数组中的值转为16进制，就能得到平常形式的utf-8编码：[228, 184, 173] > [0xe4, 0xb8, 0xad] > 0xe4b8ad

## 乱码的出现

### “烫烫烫”和“屯屯屯”

在Windows下：

+ 未初始化的栈会初始化为0xcc，【烫】的GBK编码是```0xcc0xcc```

+ 未初始化的堆内存会初始化为0xcd，【屯】的GBK编码是```0xcd0xcd```

```python
print(b"\xcc\xcc\xcc\xcc\xcc\xcc".decode('utf8'))
# outcome:错误，无法识别
print(b"\xcc\xcc\xcc\xcc\xcc\xcc".decode('gbk'))
# outcome:烫烫烫
print(b"\xcd\xcd\xcd\xcd\xcd\xcd".decode('utf8'))
# outcome:错误，无法识别
print(b"\xcd\xcd\xcd\xcd\xcd\xcd".decode('gbk'))
# outcome:屯屯屯
```

### “锟斤拷”

之前提到：Unicode字符集表用不到的部分用占位符`FFFD`表示

当两个UTF8编码的未知字串连在一起时，又被GBK译码后，就会出现这三个字

```python
print(('\uFFFD'.encode('utf8')*2).decode('utf8'))
# outcome:��
print(('\uFFFD'.encode('utf8')*2).decode('gbk'))
# outcome:锟斤拷
print(('\uFFFD'.encode('gbk')*3).decode('utf8'))
# outcome:错误，GBK无法编码未知字符
```

## 其他

位（bit）：1或0

字节（byte/B）：1byte = 8bit；1byte存1个英文字母，2byte存一个汉字 

字：若干字节一个字；由一些字符组成的，是计算机处理数据时一次存取，加工和传送的数据长度 

字长：字的总位数

**计算机的字长决定了其CPU一次操作处理实际位数的多少，即：计算机的字长越大，其性能越好。** 

1KB=1024B	1MB=1024KB	1GB=1024MB	1TB=1024GB