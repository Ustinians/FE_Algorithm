# 算法问题

## KMP算法解决的问题

### 字符串包含问题

字符串`str1`和`str2`,`str1`是否包含`str2`,如果包含请返回`str2`在`str1`中开始的位置。

如何做到时间复杂度`O(N)`完成 ?

**经典解决办法:**

```js
const isContain = (str1,str2) => {
    if(str1.length < str2.length) return -1;
    // 滑动窗口解决问题
    let arr1 = str1.split('');
    let arr2 = str2.split('');
    let len = arr2.length;
    for(let i = 0;i < arr1.length-len;i++){
        let arr3 = arr1.slice(i,i+len);
        if(isEqual(arr3,arr2)) return i;
    }
    return -1;
}

// 判断两个数组是否相等
const isEqual = (arr1,arr2) => {
    if(arr1.length !== arr2.length) return false;
    let str1 = arr1.toString();
    let str2 = arr2.toString();
    if(str1 == str2) return true;
    return false;
}
```

**KMP方法:**

给str2创建一个next数组,用于存储每个字符前面前后相等的最大字符串长度

![image-20220421151647401](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\image-20220421151647401.png)

```js
/**
 * KMP方法解决问题
 * 创建一个nextarr数组用于存储str2每个字符前面相匹配的字符串的长度(相等的字符串)
 * 
 */

const getindexOf = (s,m) => {
    if(!s || !m || m.length < 1 || s.length < m.length) return -1;
    let str1 = s.split('');
    let str2 = m.split('');
    let i1 = 0,i2 = 0;
    let next = getNextArray(str2); // O(M)
    // O(N)
    while(i1 < str1.length && i2 < str2.length){
        if(str1[i1] === str2[i2]){
            i1++;
            i2++;
        }
        // 当前节点已经是str2的第一个节点且二者不匹配
        // str2中比对的位置已经无法往前跳了
        // 换个开头重新匹配
        else if(next[i2] === -1) i1++;
        else i2 = next[i2];
    }
    // i1越界了或者i2越界了
    return i2 === str2.length ? i1-i2 : -1;
}

const getNextArray = (ms) => {
    if(ms.length == 1) return [-1];
    let next = new Array(ms.length);
    next[0] = -1;
    next[1] = 0;
    let i = 2;
    let cn = 0;
    // 依次向后求出每一个节点的next值
    // cn存储的是前一个节点的next值
    while(i < next.length){
        // 计算前面的节点是否对称
        if(ms[i-1] == ms[cn]){
            next[i++] = ++cn;
        }
        // 当cn位置的字符和i-1位置的字符不匹配
        else if(cn > 0){
            cn = next[cn];
        }
        // 如果前面没有相互对称的字符串,cn=-1,已经到达起点位置
        else{
            next[i++] = 0;
        }
    }
    return next;
}
```
## Manacher算法解决问题

字符串`str`中,最长回文子串的长度如何求解 ?

如何做到时间复杂度`O(N)`完成 ?
```js
// 转换字符串为Manacher形式
const manacherString = (str) => {
    let charArr = str.split('');
    let res = new Array(str.length * 2 + 1);
    let index = 0;
    for(let i = 0;i != res.length;i++){
        res[i] = (i & 1) == 0 ? '#' : charArr[index++];
    }
    return res;
}

const maxLcpLength = (s) => {
    if(!s || s.length == 0) return 0;
    let str = manacherString(s); // 1221 -> #1#2#2#1#
    let pArr = new Array(str.length); // 回文半径数组
    let C = -1; // 对称中心
    let R = -1; // 回文数的右边界再往右一个位置,最右的有效区是R-1位置
    let max = -Number.MAX_VALUE; // 扩出来的最大值
    for(let i = 0;i != str.length;i++){
        // 每个位置都求回文半径
        // i至少回文的区域,先给pArr[i]
        /**
         * 如果R>i说明i在R内(返回与i对称的点的pArr长度与i距离回文数边缘长度中较小的一个)
         * 否则说明i在R外(此时回文长度最小为1)
         */
        pArr[i] = R > i ? Math.min(pArr[2*C-i],R-i) : 1;
        //当回文范围还在数组中
        while(i + pArr[i] < str.length && i - pArr[i] > -1){
            // 当左扩和右扩的数据相等时,回文半径加1
            if(str[i+pArr[i]] == str[i-pArr[i]]) pArr[i]++;
            // 当左扩和右扩不相等的时候,停止循环,寻找下一个节点的回文
            else break;
        }
        // R记录最大的右扩数据
        // C记录右扩最大的对称中心
        if(i + pArr[i] > R){
            R = i + pArr[i];
            C = i;
        }
        // 记录最大值
        max = Math.max(max,pArr[i]);
    }
    return max-1;
}
```

```js
/**
 * manacherString函数
 * 将字符串转化成Manacher形式
 * 1221 -> #1#2#2#3#
 */
const manacherString = (str) => {
    let charArr = str.split(''); // 转化成数组
    let res = new Array(str.length * 2 + 1); // 用于存储转化后的数组
    let index = 0;
    for(let i = 0;i < res.length;i++){
        /**
         * 计算,当i与1进行与运算后等于0,插入'#'(偶数位)
         * 0 & 1 = 0 (00000000 & 00000001 = 0)
         * 1 & 1 = 1 (00000001 & 00000001 = 1)
         * 2 & 1 = 0 (00000010 & 00000001 = 0)
         * 3 & 1 = 1 (00000011 & 00000001 = 1)
         * ......
         */
        res[i] = [i & 1] == 0 ? '#' : charArr[index++];
    }
    // 返回转化后的字符数组
    return res;
}

// 计算最长回文子串的长度
const maxLcpLength = (s) => {
    // 如果字符串不存在或者为空串,最长回文子串的长度为0
    if(!s || s.length == 0) return 0;
    // 1221 -> ['#','1','#','2','#','2','#','1','#']
    let str = manacherString(s); // 将字符串s转化成字符数组str
    let pArr = new Array(str.length); // 记录每个节点处的回文半径的数组
    let C = -1; // 记录对称中心
    let R = -1; // 记录回文半径
    let max = -Number.MAX_VALUE; // 记录扩出来的最大值'
    for(let i = 0;i < str.length;i++){
        // 求每个位置的回文半径
        // 先计算出i最少回文的半径,赋值给pArr[i]
        if(R > i){
            // 如果i在R范围内(最右回文长度)
            /**
             * 在回文半径内时
             * 比较与i对称的i'的pArr长度和R-i长度
             * 1. 如果i'的pArr长度在R内,pArr[i] = pArr[i']
             * 2. 如果i'的pArr长度已经超过i'到R边的距离,先将pArr[i]赋值为R-i,在这段距离内,i两边是回文的,超出R后的数据需要另行判断
             * 3. 如果二者相等,赋值给pArr[i],再比较外面是否也符合回文条件
             */
            pArr[i] = Math.min(pArr[2*C-i],R-i);
        }
        else pArr[i] = 1; // 自己回文对称(前后再进行判断,先将回文长度赋值最小值1)
        while(i+pArr[i] < str.length && i-pArr[i] > -1){
            // 当还在数组内部的时候
            // 当左扩和右扩一位的值相同的时候,回文数半径+1
            if(str[i+pArr[i]] === str[i-pArr[i]]) pArr[i]++;
            else break; // 左扩右扩 不相等,停止循环,寻找下一个节点的回文半径
        }
        // R记录最大的右扩长度
        // C记录右扩最大的对称中心
        if(i + pArr[i] > R) {
            R = i + pArr[i];
            C = i;
        }
        // 记录最长的回文数长度
        max = Math.max(max,pArr[i]);
    }
    // 半径-1就是原串最大的回文长度
    return max-1;
}
```
## 窗口最大值

由一个代表题目,引出一种结构

**题目:**

有一个整型数组`arr`和一个大小为`w`的窗口从数组最左边滑动到最右边,,窗口每次向右滑动一个位置

例如,数组为`[4,3,5,4,3,3,6,7]`,窗口大小为`3`的时候

```
[ 4 3 5 ] 4 3 3 6 7         // 窗口中的最大值为5
4 [ 3 5 4 ] 3 3 6 7         // 窗口中的最大值为5
4 3 [ 5 4 3 ] 3 6 7         // 窗口中的最大值为5
4 3 5 [ 4 3 3 ] 6 7         // 窗口中的最大值为4
4 3 5 4 [ 3 3 6 ] 7         // 窗口中的最大值为6
4 3 5 4 3 [ 3 6 7 ]         // 窗口中的最大值为7
```

如果数组长度为`m`,窗口大小为`w`,则一共产生`n-w+1`个窗口的最大值

请实现一个函数,

输入: 整型数组`arr`,窗口大小`w`

输出: 一个长度为`m-w+1`的数组`res`,`res[i]`表示每一种窗口状态下的最大值

以本题为例,结果应该返回`[5,5,5,4,6,7]` 

![image-20220425220939191](https://pic.imgdb.cn/item/6266ab78239250f7c501fb04.jpg)

```js
const getMaxWindow = (arr,w) => {
    if(!arr || w < 1 || arr.length < w) return null;
    // 创建一个双向进出的队列
    let qmax = new Array(); 
    let res = new Array(arr.length - w + 1); // 存储每个窗口的最大值
    let index = 0;
    for(let i = 0;i < arr.length;i++){ // 窗口的R
        // i -> arr[i]
        while(qmax.length > 0 && arr[qmax[qmax.length-1]] <= arr[i]){
            // 如果双向队列中的最后一个节点小于将要存入的节点,将最后一个节点取出
            qmax.pop();
        }
        qmax.push(i);
        if(qmax[0] === i-w){
            // i-w为过期的下标
            // (不属于滑动窗口内下标)
            qmax.shift(); // 取出第一个节点
        }
        if(i >= w-1){
            // 窗口形成了
            // 取出队列中当前最大的值存入res
            res[index++] = arr[qmax[0]];
        }
    }
    return res;
}
```

## 返回指标A的最大值

有一个全都是正数的数组

定义: 数组中累积和与最小值的乘积,假设叫做指标A

给定一个数组,请返回子数组中,指标A最大的值

**思路:**

往右依次扩大子串,且每次都保证当前结点是子串中值最小的结点


















































