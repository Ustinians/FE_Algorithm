### Sqrt(x)

给你一个非负整数 x ，计算并返回 x 的算术平方根 。

由于返回类型是整数，结果只保留整数部分 ，小数部分将被舍去 。

注意：不允许使用任何内置指数函数和算符，例如 pow(x, 0.5) 或者 x ** 0.5 。

示例 1：

```
输入：x = 4
输出：2
```

示例 2：

```
输入：x = 8
输出：2
```

解释：8 的算术平方根是 2.82842..., 由于返回类型是整数，小数部分将被舍去。


提示：

```
0 <= x <= 231 - 1
```

#### 红蓝区域分法

```js
var MySqrt = function(){
    // 如果x为0或1,直接返回x
    if(x === 0 || x === 1) return x;
    // x>1时,x的平方根不会是它本身
    let l = -1;
    let r = x;
    while(l+1 !== r){
        // 取l和r的平均值
        let mid = Math.floor((l+r)/2);
        if(mid*mid <= x){
            // 小于等于平方根的话,赋值给l
            l= mid;
        }
        else{
            // 大于平方根,赋值给r
            r = mid;
        }
    }
    return l;
}
```

#### 牛顿迭代

```js
var MySqrt = function(){
    let n = x;
    while(n*n > x){
        // #
        n = Math.floor((n + x/n)/2);
    }
    return n;
}
```

#### 二分查找

```js
var mySqrt = function (x){
    let l = 0;
    let r = x
    while( l < r ){
        let mid = l+r+1 >> 1
        if(mid <= x / mid){
            l = mid 
        }else{
            r = mid - 1
        }
    }
    return l
}
```

#### JS移位运算符(<< , >> 和 >>>)

> 移位运算就是对二进制进行有规律低移位。移位运算可以设计很多奇妙的效果，在图形图像编程中应用广泛。

* 二进制的计算

  ![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f8acd796e18a4a5684709ebed08c803a~tplv-k3u1fbpfcp-watermark.awebp?)

* JavaScript位运算符

  ![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ad19fd5ce8ae4ac7bc5c46e166896f9b~tplv-k3u1fbpfcp-watermark.awebp?)

  

##### "<<" 运算符

“<<”运算符执行左移位运算。在移位运算过程中，符号位始终保持不变。如果右侧空出位置，则自动填充为 0；超出 32 位的值，则自动丢弃。

把数字 5 向左移动 2 位，则返回值为 20。

```js
console.log(5 << 2); // 返回值为20
```

用算式进行演示，如图所示。

![img](http://c.biancheng.net/uploads/allimg/190827/6-1ZRGG61JU.gif)

##### ">>" 运算符

“>>”运算符执行有符号右移位运算。与左移运算操作相反，它把 32 位数字中的所有有效位整体右移，再使用符号位的值填充空位。移动过程中超出的值将被丢弃。

如果将数值1000向右移动8位,则返回值为3

```js
console.log(1000 >> 8); // 返回值为3
```

用算式进行演示,则如图所示

![img](http://c.biancheng.net/uploads/allimg/190827/6-1ZRGH0003W.gif)

把数值 -1000 向右移 8 位，则返回值为 -4。

```js
console.log(-1000 >> 8); // 返回值为-4
```

用算式进行演示，如图所示。当符号位值为 1 时，则有效位左侧的空位全部使用 1 进行填充。

![img](http://c.biancheng.net/uploads/allimg/190827/6-1ZRGH214508.gif)

##### ">>>" 运算符

“>>>”运算符执行**无符号右移位运算**。它把无符号的 32 位整数所有数位整体右移。**对于无符号数或正数右移运算，无符号右移与有符号右移运算的结果是相同的。**

例如 : 下面两行表达式的返回值是相同的。

```js
console.log(1000 >> 8); // 返回值为3
console.log(1000 >>> 8); // 返回值为3
```

对于负数来说，无符号右移将使用 0 来填充所有的空位，同时会把负数作为正数来处理，所得结果会非常大所以，使用无符号右移运算符时要特别小心，避免意外错误。

```js
console.log(-1000 >> 8);  //返回值 -4
console.log(-1000 >>> 8);  //返回值 16777212
```

用算式进行演示，如图所示。左侧空位不再用符号位的值来填充，而是用 0 来填充。

![img](http://c.biancheng.net/uploads/allimg/190827/6-1ZRGHZ0200.gif)

















