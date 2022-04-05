# 异或运算

相同为1,不同为0

```js
console.log(1^1); // 0
console.log(1^0); // 1
console.log(0^1); // 1
console.log(0^0); // 0
```

## 异或运算相关结论

* `0 ^ N = N; N ^ N = 0`
  
  ```js
  console.log(5^0); // 5
  console.log(5^5); // 0
  ```

* 异或运算满足交换律和结合律
  
  ```js
  console.log(1^0 === 0^1 ? true : false); // true
  ```

* 异或运算可以用于交换两个变量的值(但是尽量不要这么做)
  
  但是前提是<font color="red">两个变量必须在内存中的不同位置</font>,因此数组进行排序的时候无法使用该方法(当交换的两个脚标相等时,一个数值对自己进行异或运算,该值就会变成0)
  
  ```js
  var a = 10,b = 5;
  console.log(a,b); // 10 5
  a = a ^ b;
  b = a ^ b;
  a = a ^ b;
  console.log(a,b); // 5 10
  ```

## 例题

1. 在一个整数数组中,只有一个数出现了奇数次,其他的数都出现了偶数次,请找到出现了奇数次的这个数
   
   **解析:** 因为`N ^ N = 0`,所以出现偶数次的数异或运算后为0,而出现奇数次的那个数,进行异或运算`N ^ N ^ N = 0 ^ N = N`,因此最后`eor`的值就是出现奇数次的那个数的值
   
   ![](C:\Users\admin\AppData\Roaming\marktext\images\2022-04-05-11-36-57-image.png)
   
   ```js
   // 异或运算
   const arr = [1,1,2,2,3,3,3,4,4,5,5];
   let eor = 0;
   for(let i = 0;i < arr.length;i++){
       eor ^= arr[i];
   }
   console.log(eor); // 3
   ```

2. 假如在这个数组中,有两个数都出现了奇数次,他的数都出现了偶数次,请找到出现了奇数次的这两个数
   
   ```js
   
   ```






































































