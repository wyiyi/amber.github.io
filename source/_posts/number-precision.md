---
title: 精度丢失的解决办法
date: 2021.03.25 
tags: Others
categories: 
  - Technology
mathjax: true 
---

## 精度丢失的解决办法
0.1 + 0.2 != 0.3，在JS内部所有的计算都是以二进制方式计算的。所以运算 0.1+ 0.2 时要先把 0.1和 0.2 从十进制转成二进制。

由于在文章[二进制算术运算](https://mp.weixin.qq.com/s/RvN33qA4ouS29ETeKY-MUw)已介绍过为何会不等于0.3的原理，那么在涉及运算的时候我们应该如何解决呢？

### 在 JS 中：
#### 方法一：扩大小数点位数最多的倍数

```
function add(num1, num2){ 
　  var r1, r2, m;
    try {
        r1 = num1.toString().split('.')[1].length;
    } catch (e) {
        r1 = 0;
    }
    try {
        r2 = num2.toString().split(".")[1].length;
    } catch (e) {
        r2 = 0;
    }
    m = Math.pow(10, Math.max(r1, r2));
    return Math.round(num1 * m + num2 * m) / m
}
```

#### 方法二：误差检查函数，出自[《ES6标准入门》- 阮一峰](https：//es6.ruanyifeng.com/)
- ES6 在 Number 对象上新增了一个极小的常量：`Number.EPSILON`，引入一个这么小的量，
目的在于为浮点数计算设置一个误差范围，如果误差能够小于 `Number.EPSILON` ，我们就可以认为结果是可靠的。
- Number.EPSILON.toFixed(20) 值为：“2.220446049250313e-16” 也可以说是 “0.00000000000000022204”

```
# 0.1+0.2 的值为多少，如果误差在Number.EPSILON范围内，说明0.1+0.2=0.3
var value1 = 0.1 + 0.2
var value2 = 0.3
var value = Math.abs(value1 - value2)
if (value < Number.EPSILON) {
   value1 = value2
}
```

####  在涉及到金额时，需要最终保留几位小数，我们需要对最后的结果值进行四舍五入处理，但是在浏览器中会有如下的现象：四舍五入 `toFixed()` 的值错误？
```
// Firefox 和 Chrome 中存在四舍五入值不对的问题？
1.5.toFixed(0) // 2 正确
1.35.toFixed(1) // 1.4 正确
1.335.toFixed(2) // 1.33  错误
1.3335.toFixed(3) // 1.333 错误
1.33335.toFixed(4) // 1.3334 正确
1.333335.toFixed(5)  // 1.33333 错误
1.3333335.toFixed(6) // 1.333333 错误
```
可以看到，小数点位数为1，2，5时四舍五入是正确的，其它是错误的。
Firefox 和 Chrome的实现没有问题，根本原因还是计算机里浮点数精度丢失的问题。

方案三：转换为整数再处理，避免浮点数精度的损失
```
// 转换为整数再处理，避免浮点数精度的损失
function toFixed(num, m) {
    var des =  Math.round(Math.pow(10, m) * num) / Math.pow(10, m);
    return des;
}
console.log(toFixed(1.333335, 5))
```

方案四：修复浏览器中 toFixed 对小数最后一位为5时进位不正确的情况，即最后一位为5，改成6，在toFixed()，其中：number 为原始数字，precision 为位数
```
function toFixed(number, precision) {
    var str = number + ''
    var len = str.length
    var last = str.substring(len - 1, len)
    if (last == '5') {
        last = '6'
        str = str.substring(0, len - 1) + last
        return (str - 0).toFixed(precision)
    } else {
        return number.toFixed(precision)
    }
}
console.log(toFixed(1.333335, 5))
```

### 在 JAVA 中：`BigDecimal` 获得预期结果 
在大多数的商业计算中，一般采用 `java.math.BigDecimal` 类来进行精确计算。但在使用 Java 的浮点数进行运算时，经常出现精度丢失的问题。
见以下示例：
```
@Test
void calculation() {
    double d1 = 0.06;
    double d2 = 0.01;
    BigDecimal b1 = new BigDecimal(d1);
    BigDecimal b2 = new BigDecimal(d2);
    assert b1.add(b2).doubleValue() == 0.06999999999999999;
    assert b1.add(b2).doubleValue() != 0.07d;

    BigDecimal b3 = new BigDecimal(Double.toString(d1));
    BigDecimal b4 = new BigDecimal(Double.toString(d2));
    assert b3.add(b4).doubleValue() == 0.07;
    assert b3.subtract(b4).doubleValue() == 0.05;
    assert b3.multiply(b4).doubleValue() == 0.0006;
    assert b3.divide(b4).doubleValue() == 6;
}
```
 从执行测试用例中可看出，`b1.add(b2).doubleValue` 获得到的值 `!= 0.07` 且调用的构造方法为 BigDecimal(double var) 仍存在精度丢失的问题。
 - 建议使用 BigDecimal(String val)。
 - 需要精确的表示两位小数时我们需要把他们转换为 BigDecimal 对象，然后再进行加减乘除运算。


### 总结
**对于整数，前端出现问题的几率可能比较低，有少数业务需要用到超大整数，只要运算结果不超过 `Math.pow(2, 53)` 也就是 `9007199254740992` 就不会丢失精度。**

**对于小数，前后端出现问题的几率还是很多的，尤其在一些电商网站涉及到金额等数据，还需妥善处理。**
