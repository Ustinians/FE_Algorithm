# 位运算

## 判断一个整数n是否为负数

```js
/**
 * 判断n是负数还是非负数
 * if n是非负数,返回1
 * if n是负数,返回0
 */
const sign = (n) => {
    return 1-((n >> 31) & 1);
}
```

## 比较两个数的大小

```js
// 位运算获取较的大值
const getMax1 = (a,b) => {
    let c = a-b; // a>=b时,c>=0;否则c<0
    let scA = sign(c); // a-b为非负,scA为1,a-b为负,scA为0(当scA为1的时候,a比较大)
    let scB = 1-scA; // 当scB为1的时候,b比较大
    // 当scA为0的时候,scB一定为1,当scA为1的时候,scB一定为0
    // (若scA为1,返回a,若scB为1,返回b)
    return a*scA + b*scB;
}
```

上面代码有一个缺点,即当a,b异号且相差较大的时候,c可能溢出

改善后的代码为

```js
// 优化后
const getMax2 = (a,b) => {
    let c = a-b; // a>=b时,c>=0;否则c<0
    let sa = sign(a); // a>=0时sa=1,否则sa=0
    let sb = sign(b); // b>=0时sb=1,否则sb=0
    let sc = sign(c); // c>=0时sc=1,否则sc=0
    // 异或运算,判断a,b是否同号
    let difSab = sa ^ sb; // a和b的符號不同为1,相同为0
    let sameSab = 1 - difSab; // a和b的符号一样为1,不一样为0
    // 当二者不同号且a>=0时,returnA=1,当二者同号且a>=b(c>=0)时,returnA为1,否则为0
    let returnA = difSab * sa + sameSab * sc;
    let returnB = 1 - returnA;
    // 返回较大的数
    return a*returnA + b*returnB;
}
```

## 判断一个32位正数是不是2的幂,4的幂

```js
/**
 * 判断一个32位正数是不是2的幂
 * 思路:
 * 如果一个正数是2的幂,则它的二进制上只有一个1
 * 00001 -> 2^0
 * 00010 -> 2^1
 * 00100 -> 2^2
 * 01000 -> 2^3
 * 10000 -> 2^4
 * .....
 * 当最后一位为1时,num-1 = 0
 * 其他情况下,1会被打散 
 * 2^1-1 -> 00001
 * 2^2-1 = 000011
 * ......
 * 因此,num & (num-1)=0时.就是2的幂
 * 
 * 判断一个32位正数是不是4的幂
 * 同理,二进制只能有一个1,且必须在0,2,4,6...位上
 * 1. num & (num-1) = 0 (只有一个1)
 * 2. num & 01010101...01 !== 0(1在偶数位上)
 */
// 判断一个数是不是2的幂
const is2Power = (n) => {
    return n & (n-1) === 0;
}

// 判断一个数是不是4的幂
const is4Power = (n) => {
    //                               ...1010101
    return (n & (n-1)) === 0 && (n & 0x55555555) != 0;
}
```

## 位运算实现加减乘除

给定两个有符号32位整数a和b,不能使用算术运算符,分别实现a和b的加,减,乘,除运算

**[要求]**

如果给定a,b执行加减乘除的运算结果就会导致数据溢出,那你实现的函数不必对此负责,除此之外请保证计算过程不发生溢出

### 两个数的二进制进行相加

进行异或运算,得到的结果就是**二者相加后无进位的值**

通过与运算能获取要进位的数

与运算向左移一位,得到的就是**进位结果**

然后将异或运算得到的结果和与运算得到的结果再进行如上运算

直到与运算的结果为0(没有进位了)

![image-20220513203808367](https://github.com/Ustinians/FE_Algorithm/blob/main/images/bit1.jpg)

```js
//@ 加法
// 如果用户传入的参数,a+b就是溢出的,那结果就可能出错
const add = (a,b) => {
    let sum = a;
    while(b ^= 0){
        // 当与运算的值不为0,一直进行循环
        sum = a ^ b; // 无进位相加的结果
        b = (a & b) << 1; // 进位信息
        a = sum;
    }
    return sum;
}
```

### 两个数的二进制进行相减

a-b即为a加上b的相反数(a-(-b))

```js
const negNum = (n) => {
    // 计算n取反后与1相加的结果(即为n的相反数-n)
    return add(~n,1);
}

const minus = (a,b) => {
    // 计算a与-b相加的结果
    return add(a,negNum(b));
}
```

### 两个数的二进制进行相乘

两个二进制相乘的时候,直接使用二进制的每一位互相乘(和十进制乘法相同),最后将结果相加即可

![1652450313(1)](https://github.com/Ustinians/FE_Algorithm/blob/main/images/bit2.jpg)

```js
//@ 二进制计算两个数相乘
// 如果用户传入的参数中,a*b就是溢出的,那结果肯定会出错
const mulit = (a,b) => {
    let res = 0;
    while(b !== 0){
        if((b & 1) != 0){ // b的最后一位不为0
            res = add(res,a);
        } 
        a <<= 1;
        b >>>= 1;
    }
    return res;
}
```

### 两个数的二进制进行相除

```js
const isNeg = (n) => {
    return n < 0;
}

//@ 二进制计算两个数相除
const div = (a,b) => {
    let x = isNeg(a) ? negNum(a) : a;
    let y = isNeg(b) ? negNum(b) : b;
    let res = 0;
    for(let i = 31;i > -1;i = minus(i,1)){
        if((x >> i) >= y){
            res |= (1 << i);
            x = minus(x,y << i);
        }
    }
    return isNeg(a) ^ isNeg(b) ? negNum(res) : res;
}

const divide = (a,b) => {
    if(b === 0) return; // 分母不能为0
    if(a === Number.MIN_VALUE && b === Number.MIN_VALUE){
        return 1;
    }
    else if(b === Number.MIN_VALUE){
        return 0;
    }
    else if(a === Number.MIN_VALUE){
        let res = div(add(a,1),b);
        return add(req,div(minus(a,mulit(res,b)),b));
    }
    else{
        return div(a,b);
    }
}
```





