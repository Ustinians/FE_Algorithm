#### 二进制求和

给你两个二进制字符串，返回它们的和（用二进制表示）。

输入为 **非空** 字符串且只包含数字 `1` 和 `0`。

示例 1:

```
输入: a = "11", b = "1"
输出: "100"
```

示例 2:

```
输入: a = "1010", b = "1011"
输出: "10101"
```


提示：

> 每个字符串仅由字符 '0' 或 '1' 组成。
> 1 <= a.length, b.length <= 10^4
> 字符串如果不是 "0" ，就都不含前导零。

**解答:**

```js
/**
 * @param {string} a
 * @param {string} b
 * @return {string}
 */
var addBinary = function(a, b) {
    // BigInt ES10引入的
    // parseInt在数字过大的时候容易造成精度损失
    return (BigInt('0b'+a)+BigInt('0b'+b)).toString(2);
};
```

**知识点总结:** 

a,b前面加上`'0b'`的原因

JS / TS里的**最大安全整数**是`2^53 - 1`，超出这个范围的整数运算就不再准确(比如著名的`2**53 === 2**53 + 1`) , 故ES2020中提出了`BigInt`

`BigInt`的构造函数只有一个参数，所以需要加上这个前缀来表明这是一个代表二进制数的字符串（`0B`也行）

如果是使用`parseInt()`,则可以在后面再传一个参数

#### BigInt

`BigInt`是ES2020中新添加是属性

`BigInt`是一种特殊的数字类型,它提供了对任意长度整数的

创建 bigint 的方式有两种：在一个整数字面量后面加 `n` 或者调用 `BigInt` 函数，该函数从字符串、数字等中生成 `bigint`

```js
const bigint1 = 1234567894578978464564n;
const bigint2 = BigInt("1234567894578978464564");
const bigint3 = BigInt(10); // 和10n相同
```

##### 数学运算符

`BigInt` 大多数情况下可以像常规数字类型一样使用，例如：

```js
console.log(1n + 2n);
console.log(5n / 2n);
```

请注意：除法 `5/2` 的结果向零进行舍入，舍入后得到的结果没有了小数部分。对 `bigint `的所有操作，返回的结果也是 `bigint`。

**! 不可以将`bigint`类型和常规数字类型混合使用 :**

```js
alert(1n + 2); // Error: Cannot mix BigInt and other types
```

如果需要进行运算,我们应该显式的转换它们 : 使用`BigInt()` 或者 `Number()` 进行转换

```js
let bigint = 1n;
let number = 2;

// 将number转换成bigint
console.log(bigint + BigInt(number));

// 将bigint转换成number
console,log(number + Number(bigint));
```

转换操作始终是静默的，绝不会报错，但是如果 bigint 太大而数字类型无法容纳，则会截断多余的位，因此我们应该谨慎进行此类转换。

##### BigInt不支持一元加法

一元加法运算符 `+value`，是大家熟知的将 `value` 转换成数字类型的方法。

为了避免混淆，在 bigint 中不支持一元加法：

```js
let bigint = 1n;

alert( +bigint ); // error
```

所以我们应该用 `Number()` 来将一个 bigint 转换成一个数字类型。

##### 参考文章

https://juejin.cn/post/6854573218125234190