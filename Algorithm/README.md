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





















































