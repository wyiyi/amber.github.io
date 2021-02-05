---
title: Zero Width Unicode
date: 2021.01.15
tag: Encoding
categories: Technology  
mathjax: true 
---

### 眼见不一定为实
大家都熟悉的 Unicode（万国码）几乎包含[所有符号](https://unicode-table.com/)：
常用的 Emoji： 😂 😸 ✌、颜文字：  (๑•̀ㅂ•́)  ٩(͡๏̯͡๏)۶  $_$  、 表意文字：𠁀 𡮘 𠆳 、国际象棋图案♕ ♛ ♙、扑克牌 🂡 🃁 🂳、以及麻将牌 🀄 🀝 🀇 等等。
比如在朋友圈火热的花式飞机坦克等，组成它们其中几个特殊符号对应的 Unicode 分别为：

![飞机](https://wyiyi.github.io/amber/contents/noseeunicode/huiji.jpg)
![坦克](https://wyiyi.github.io/amber/contents/noseeunicode/tanke.jpg)


| 图形                        |  Unicode编号 |  HTML代码  |
|  :-------                   |  :--------  |:---------- |
|    ◙                        |    U+25D9   |  `&#9689;` |
|    ▬                        |    U+25AC   |  `&#9644;` |
|    ▦                       |    U+25A6   |  `&#9638;` |
|    ▲                        |    U+25B2   |  `&#9650;` |
|    ●                        |    U+25CF   |  `&#9679;` |
|    ◥                       |    U+25E5   |  `&#9701;` |
|  ...                        |    ...      |   ...      |
| zero-width space            |    U+200B   |  `&#8203;` |
| zero-width non-joiner       |    U+200C   |  `&#8204;` |
| zero-width joiner           |    U+200D   |  `&#8205;` |
| Left-to-right mark          |    U+200E   |  `&#8206;` |
| Right-to-left mark          |    U+200F   |  `&#8207;` |


### 什么是零宽字符
顾名思义，就是字节宽度为0的特殊字符。
零宽度字符是一些不可见的，不可打印的字符。它们存在于页面中主要用于调整字符的显示格式，
下面就是一些常见的零宽度字符及它们的 Unicode 码和原本用途：

### 都有哪些零宽字符
零宽字符主要有以下几类：
- 零宽空格（zero-width space, ZWSP）用于较长单词的换行分隔。
   ` Unicode: U+200B`  `HTML: &#8203`;
- 零宽不连字 (zero-width non-joiner，ZWNJ)放在电子文本的两个字符之间，用于阻止特定位置的换行分隔。
   ` Unicode: U+200C`  `HTML: &#8204`;
- 零宽连字（zero-width joiner，ZWJ）是一个控制字符，放在某些需要复杂排版语言（如阿拉伯语、印地语）的两个字符之间，使得这两个本不会发生连字的字符产生了连字效果。
    `Unicode: U+200D`  `HTML: &#8205`;
- 左至右符号（Left-to-right mark，LRM）是一种控制字符，用于在混合文字方向的多种语言文本中（例：混合左至右书写的英语与右至左书写的希伯来语），规定排版文字书写方向为左至右。
    `Unicode: U+200E`  `HTML: &lrm; &#x200E; 或&#8206;`
- 右至左符号（Right-to-left mark，RLM）是一种控制字符，用于在混合文字方向的多种语言文本中，规定排版文字书写方向为右至左。
    `Unicode: U+200F`  `HTML: &rlm; &#x200F; 或&#8207;`

### 零宽字符应用
#### 传递隐秘信息
利用零宽度字符不可见的特性，我们可以用零宽度字符在任何未对零宽度字符做过滤的网页内插入不可见的隐形文本。
安利一个[小工具](http://www.atoolbox.net/Tool.php?Id=829)。

#### 隐形水印
通过零宽度字符我们可以对内部文件添加隐形水印。
在浏览者登录页面对内部文件进行浏览时，我们可以在文件的各处插入使用零宽度字符加密的浏览者信息，如果浏览者又恰好使用复制粘贴的方式在公共媒体上匿名分享了这个文件，我们就能通过嵌入在文件中的隐形水印轻松找到分享者了。

完整源码可见[仓库](https://github.com/wyiyi/bronze)。
~~~~
@SpringBootTest
class ZeroWidthUnicodeTest {
    private WaterMark waterMark = new WaterMark();
    private DeEncode deEncode = new DeEncode();

    @Test
    void testWaterMark() {
        String input = "测试添加水印";
        String string = "原文本：\"" + input + "\"，文本长度：" + input.length();
        String output = "原文本：\"测试添加水印\"，文本长度：6";
        assert string.equals(output);

        String watermarkInput = "抓鸭子，抓几只？";
        String string1 = "水印文本：\"" + watermarkInput + "\"，文本长度：" + watermarkInput.length();
        String watermarkOutput = "水印文本：\"抓鸭子，抓几只？\"，文本长度：8";
        assert string1.equals(watermarkOutput);

        String encode = deEncode.encode(watermarkInput);
        String waterCode = "水印编码：\"" + encode + "\"，编码长度：" + encode.length();
        String waterTextOutput = "水印编码：\"\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\"，编码长度：256";
        assert waterCode.equals(waterTextOutput);


        String result = waterMark.addWatermark(input, encode, CodeUtil.WATERMARK_POS_HEAD);
        String resultOutput = "输出：\"" + result + "\"，文本长度：" + result.length();
        String resultTrue = "输出：\"\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200C\uFEFF\u200C\uFEFF\u200C\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF\u200B\uFEFF测试添加水印\"，文本长度：262";
        assert resultOutput.equals(resultTrue);

        result = waterMark.extractWatermark(result, CodeUtil.WATERMARK_POS_HEAD);
        String watermark = deEncode.decode(result);
        assert watermarkInput.equals(watermark);
    }
}
~~~~

#### 逃脱敏感词过滤
通过零宽度字符我们可以轻松逃脱敏感词过滤。敏感词自动过滤是维持互联网社区秩序的一项重要工具，只需倒入敏感词库和匹配相应敏感词，即可将大量的非法词汇拒之门外。使用谐音与拼音来逃脱敏感词过滤会让语言传递信息的效率降低，而使用零宽度字符可以在逃脱敏感词过滤的同时将词义原封不动地传达给接受者，大大提高信息传播者与接受者之间交流的效率。
~~~~
const sensitive = '敏感词'
// 利用零宽度字符 zero-width joiner U+200D 来分隔敏感词 
sensitive.replace(/敏感词/g, '‍')
// 使用零宽度空格 zero-width space U+200B对字符串进行分隔
Array.from(sensitive).join('​').replace(/敏感词/g, '')
~~~~

### 小心零宽字符带来的困扰，同时也可以很好的利用零宽字符