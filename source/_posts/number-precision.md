---
title:  Number-Precision
date: 2021.03.25 
tags: Others
categories: 
  - Technology
mathjax: true 
---

## 精度失真的解决办法

在前一篇文章中提到小数相加会存在精度丢失的问题，在涉及运算的时候我们应该怎么解决呢？

### `toFixed()` 的值错误？

```
// Firefox 和 Chrome 中存在四舍五入值不对的问题？
1.35.toFixed(1) // 1.4 正确
1.335.toFixed(2) // 1.33  错误
1.3335.toFixed(3) // 1.333 错误
1.33335.toFixed(4) // 1.3334 正确
1.333335.toFixed(5)  // 1.33333 错误
1.3333335.toFixed(6) // 1.333333 错误
```
可以看到，小数点位数为2，5时四舍五入是正确的，其它是错误的。
Firefox 和 Chrome的实现没有问题，根本原因还是计算机里浮点数精度丢失的问题。

方案一：
```
// 修复浏览器中 toFixed 对小数最后一位为5时进位不正确的情况，即最后一位为5，改成6，在toFixed()，其中：number 为原始数字，precision 为位数
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
方案二：
```
// 转换为整数再处理，避免浮点数精度的损失
function toFixed(num, s) {
    var times = Math.pow(10, s)
    var des = num * times
    des = parseInt(des, 10) / times
    return des + ''
}
console.log(toFixed(1.333332, 5))
```

方案三：
- 误差检查函数，出自[《ES6标准入门》- 阮一峰](https：//es6.ruanyifeng.com/)
- ES6 在 Number 对象上新增了一个极小的常量：`Number.EPSILON`，引入一个这么小的量，
目的在于为浮点数计算设置一个误差范围，如果误差能够小于 `Number.EPSILON` ，我们就可以认为结果是可靠的。
- Number.EPSILON.toFixed(20) 值为：“2.220446049250313e-16” 也可以说是 “0.00000000000000022204”

```
function withinErrorMargin (left, right) {
    return Math.abs(left - right) < Number.EPSILON
}
withinErrorMargin(0.1+0.2, 0.3)
```

### `BigDecimal` 获得预期结果 
在大多数的商业计算中，一般采用 `java.math.BigDecimal` 类来进行精确计算。但在使用 Java 的浮点数进行运算时，经常出现精度丢失的问题。
见以下示例：
```
@Test
void calculation(){
    double d1 = 0.06;
    double d2 = 0.01;
    BigDecimal b1 = new BigDecimal(d1);
    BigDecimal b2 = new BigDecimal(d2);
    assert b1.add(b2).doubleValue() != 0.07;

    BigDecimal b3 = new BigDecimal(Double.toString(d1));
    BigDecimal b4 = new BigDecimal(Double.toString(d2));
    assert b3.add(b4).doubleValue() == 0.07;        
    assert b3.subtract(b4).doubleValue() == 0.05; 
    assert b3.multiply(b4).doubleValue() == 0.0006;
    assert b3.divide(b4).doubleValue() == 6;
    
}
```
 从执行测试用例中可看出，`b1.add(b2)` 获得到的值 `!= 0.07` 且调用的构造方法为 BigDecimal(double var) 仍存在精度丢失的问题。
 - 建议使用 BigDecimal(String val)。
 - 需要精确的表示两位小数时我们需要把他们转换为 BigDecimal 对象，然后再进行加减乘除运算。


### 总结
**对于整数，前端出现问题的几率可能比较低，有少数业务需要用到超大整数，只要运算结果不超过 `Math.pow(2, 53)` 也就是 `9007199254740992` 就不会丢失精度。**

**对于小数，前后端出现问题的几率还是很多的，尤其在一些电商网站涉及到金额等数据，还需妥善处理。**