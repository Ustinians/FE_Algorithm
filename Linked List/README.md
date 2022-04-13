# 链表

## 链表的增删查改

### 定义数组结构

```js
function ListNode(val, next) {
    this.val = (val===undefined ? 0 : val)
    this.next = (next===undefined ? null : next)
}
```

### 初始化一个链表

```js
// 初始化一个链表
const initLink = (arr) => {
    let head = new ListNode(arr[0]);
    let list = head;
    for(let i = 1;i < arr.length;i++){
        let node = new ListNode(arr[i]);
        list.next = node;
        list = list.next;
    }
    return head;
}
```

### 查找链表中是否存在某节点

```js
// 在链表中查找某个节点
const selectLink = (list,val) => {
    let l = list;
    let i = 1;
    while(l){
        if(l.val === val) return true;
        l = l.next;
        i++;
    }
    return false;
}
```

### 修改链表中某个节点的值

```js
// 更改链表中某个节点的值
const updateLink = (list,val,index) => {
    let l = list;
    for(let i = 1;i < index;i++){
        l = l.next;
    }
    l.val = val;
    return list;
}
```

### 向链表中插入节点

```js
// 向链表中插入节点
const insertLink = (list,val,index) => {
    let l = list;
    if(index === 1){
        let node = new ListNode(val);
        node.next = l;
        return node;
    }
    for(let i = 1;i < index-1;i++){
        if(!l){
            console.log("插入位置无效");
            return;
        }
        l = l.next;
    }
    let node = new ListNode(val);
    node.next = l.next;
    l.next = node;
    return list;
}
```

### 删除链表中的节点

```js
// 从链表中删除节点
const deleteLink = (list,index) => {
    if(index === 1) return list.next;
    let l = list;
    for(let i = 1;i < index-1;i++){
        if(!l){
            console.log("删除位置无效");
            return;
        }
        l = l.next;
    }
    if(l.next){
        l.next = l.next.next;
    }
    else{
        l.next = null;
    }
    return list;
}
```

### 展示链表中的值

```js
// 展示链表
const showLink = (list) => {
    let l = list;
    let ans = [];
    while(l){
        ans.push(l.val);
        l = l.next;
    }
    return ans;
}
```

### 完整的代码

```js
/**
 * 单链表
 */

function ListNode(val, next) {
    this.val = (val===undefined ? 0 : val)
    this.next = (next===undefined ? null : next)
}

// 初始化一个链表
const initLink = (arr) => {
    let head = new ListNode(arr[0]);
    let list = head;
    for(let i = 1;i < arr.length;i++){
        let node = new ListNode(arr[i]);
        list.next = node;
        list = list.next;
    }
    return head;
}

// 在链表中查找某个节点
const selectLink = (list,val) => {
    let l = list;
    let i = 1;
    while(l){
        if(l.val === val) return true;
        l = l.next;
        i++;
    }
    return false;
}

// 更改链表中某个节点的值
const updateLink = (list,val,index) => {
    let l = list;
    for(let i = 1;i < index;i++){
        l = l.next;
    }
    l.val = val;
    return list;
}

// 向链表中插入节点
const insertLink = (list,val,index) => {
    let l = list;
    if(index === 1){
        let node = new ListNode(val);
        node.next = l;
        return node;
    }
    for(let i = 1;i < index-1;i++){
        if(!l){
            console.log("插入位置无效");
            return;
        }
        l = l.next;
    }
    let node = new ListNode(val);
    node.next = l.next;
    l.next = node;
    return list;
}

// 从链表中删除节点
const deleteLink = (list,index) => {
    if(index === 1) return list.next;
    let l = list;
    for(let i = 1;i < index-1;i++){
        if(!l){
            console.log("删除位置无效");
            return;
        }
        l = l.next;
    }
    if(l.next){
        l.next = l.next.next;
    }
    else{
        l.next = null;
    }
    return list;
}

// 展示链表
const showLink = (list) => {
    let l = list;
    let ans = [];
    while(l){
        ans.push(l.val);
        l = l.next;
    }
    return ans;
}

const arr =  [ 1,2,3,4,5,6,7,8];
var list = initLink(arr);
console.log(showLink(list)); // [ 1, 2, 3, 4, 5 ]

// 向链表中插入节点
insertLink(list,9,3);
console.log(showLink(list)); // [ 1, 2, 9, 3, 4, 5 ]

// 查找链表中是否有某个节点
console.log(selectLink(list,5)); // true
console.log(selectLink(list,10)); // false

// 删除链表中的某个节点
deleteLink(list,5);
console.log(showLink(list)); // [ 1, 2, 9, 3, 5 ]

// 修改链表中的某个节点
updateLink(list,-9,3);
console.log(showLink(list)); // [ 1, 2, -9, 3, 5 ]
```

## 链表相关题目

### 题目一: 判断链表是否为回文结构

给定一个单链表的头结点`head`,请判断该链表是否为回文结构

**例子**: 1->2->1,返回true,1->2->2->1,返回true,15->16->15,返回true,1->2->3,返回false

#### 方法一: 栈

思路:

创建一个栈,将链表中的元素依次放入栈中,然后将链表从头遍历,与出栈的元素进行比较,如果有不同的元素就返回false,否则返回true

代码:

```js
// 定义链表结构
function ListNode(val, next) {
    this.val = (val===undefined ? 0 : val)
    this.next = (next===undefined ? null : next)
}

// 初始化一个数组
const initLink = (arr) => {
    let head = new ListNode(arr[0]);
    let l = head;
    for(let i = 1;i < arr.length;i++){
        let node = new ListNode(arr[i]);
        l.next = node;
        l = l.next;
    }
    return head;
}

const isPalindrome = (list) => {
    const ans = [];
    let l = list;
    while(l){
        ans.push(l.val);
        l = l.next;
    }
    l = list;
    while(l){
        if(l.val !== ans.pop()) return false;
        l = l.next;
    }
    return true;
}

const arr = [1,2,3,2,4];
var list = initLink(arr);
console.log(isPalindrome(list));
```

#### 方法二: 用n/2的方法找快慢指针

```js
// 定义链表结构
function ListNode(val, next) {
    this.val = (val===undefined ? 0 : val)
    this.next = (next===undefined ? null : next)
}

// 初始化一个数组
const initLink = (arr) => {
    let head = new ListNode(arr[0]);
    let l = head;
    for(let i = 1;i < arr.length;i++){
        let node = new ListNode(arr[i]);
        l.next = node;
        l = l.next;
    }
    return head;
}

const isPalindrome = (list) => {
    if(!list || !list.next) return true;
    let slow = list;
    let fast = list;
    while(fast && fast.next){
        slow = slow.next;
        fast = fast.next.next;
    }
    let ans = [];
    while(slow){
        ans.push(slow.val);
        slow = slow.next;
    }
    slow = list;
    while(ans.length !== 0){
        if(slow.val !== ans.pop()) return false;
        slow = slow.next;
    }
    return true;
}

const arr = [1,2,3,2,1];
var list = initLink(arr);
console.log(isPalindrome(list));
```

#### 方法三: 链表自翻转

**要求:** 如果链表长度为N,时间复杂度为O(N),额外空间复杂度达到O(1)

找到中间点,将右边链表进行翻转,两个指针一个指向最左边,一个指向最右边,进行遍历比较

```js
// 定义链表结构
function ListNode(val, next) {
    this.val = (val===undefined ? 0 : val)
    this.next = (next===undefined ? null : next)
}

// 初始化一个数组
const initLink = (arr) => {
    let head = new ListNode(arr[0]);
    let l = head;
    for(let i = 1;i < arr.length;i++){
        let node = new ListNode(arr[i]);
        l.next = node;
        l = l.next;
    }
    return head;
}

// 链表自翻转
const isPalindrome = (list) => {
    if(!list || !list.next) return true; // 如果只有一个节点或者没有节点,直接返回true
    let slow = list;
    let fast = list;
    let prev; // 令prev指向right的前一个
    while(fast && fast.next){
        prev = slow;
        slow = slow.next;
        fast = fast.next.next;
    }
    // slow = prev.next
    prev.next = null; // 让中间的节点的next指针指向null
    let p = slow; // 令p指向中间节点的后一个
    let q = null;
    while(p){
        // 依次修改后半部分的next指向
        q = p.next;// q用以存储p后面部分链表
        p.next = prev; // 让p指向前一个节点
        prev = p;
        p = q;
    }
    q = prev; // prev指向的是最后一个节点
    p = list; // p指向头一个节点
    while(p && q){
        if(q.val !== p.val) return false;
        q = q.next;
        p = p.next;
    }
    // 恢复链表原来的结构
    // 令q指向倒数第二个节点
    q = prev.next;
    prev.next = null; // 最后一个节点指向null
    while(q){
        p = q.next; // p指向q的前一个节点
        q.next = prev; // 改变q的指向到后一个节点
        prev = q;
        q = p;
    }
    return true;
}

const arr = [1,2,3,2,1];
var list = initLink(arr);
console.log(isPalindrome(list));
```

### 题目二: 将单链表按照某值划分成左边小,中间相等,右边大的形式

将单链表按照某值划分成左边小,中间相等,右边大的形式

给定一个单链表的头结点head,节点的值类型是整型,再给定一个整数pivot,实现一个调节链表的函数,将链表调整为左部分都是小于pivot的,中间部分都是等于pivot的,右边部分都是大于pivot的.

**要求:**

1. 调整后所有小于pivot的节点的相对顺序和调整前一样

2. 调整后所有等于pivot的节点的相对顺序和调整前一样

3. 调整后所有大于pivot的节点的相对顺序和调整前一样

4. 时间复杂度请达到O(N),额外空间复杂度请达到O(1)

**思路:**

创建六个变量

- SH: 小于pivot部分的开始 ST: 小于povit部分的结束

- EH: 等于pivot部分的开始 ET: 等于pivot部分的结束

- BH: 大于pivot部分的开始 BT: 大于pivot部分的结束

依次遍历链表,获得小于target的值就存入小于target的部分,并且让ST后移指向小于target部分的最后一个值,SH指向第一个值

剩下的同理

最后讨论情况并将三部分连在一起

```js
const listPartition = (list,pivot) => {
    let SH = null;
    let ST = null;
    let EH = null;
    let ET = null;
    let BH = null;
    let BT = null;
    let next = null;
    // 将每个节点放入三个列表中
    while(list){
        next = list.next;
        list.next = null;
        if(list.val < pivot){
            if(SH == null){
                // 小于pivot的部分为空
                SH = list;
                ST = SH;
            }
            else{
                ST.next = list;
                ST = ST.next;
            }
        }
        else if(list.val == pivot){
            if(EH == null){
                // 小于pivot的部分为空
                EH = list;
                ET = EH;
            }
            else{
                ET.next = list;
                ET = ET.next;
            }
        }
        else{
            if(BH == null){
                // 小于pivot的部分为空
                BH = list;
                BT = BH;
            }
            else{
                BT.next = list;
                BT = BT.next;
            }
        }
        list = next;
    }
    if(ST != null){
        ST.next = EH;
        ET = ET == null ? ST : ET; // 决定谁去连下一部分,如果中间部分为空,直接将ST与BH连在一起
    }
    if(ET != null){
        ET.next = BH;
    }
    return SH != null ? SH : (EH != null ? EH : BH);
}
```

### 题目三: 复制含有随机指针节点的链表

复制含有随机指针节点的链表

一种特殊的单链表节点类描述如下// 定义链表结构

```js
function ListNode(val, next) {
    this.val = (val === undefined ? 0 : val)
    this.next = (next === undefined ? null : next)
    this.rand = (rand === undefined ? null : rand)
}
```

rand指针是单链表节点结构重新增的指针,rand可能指向链表重任意一个节点,也可能指向null,给定一个由ListNode节点类型组成的无环单链表的头结点head,请事先一个函数完成这个链表的复制,并返回复制的新链表的节点

**要求:** 时间复杂度O(N),额外空间复杂度O(1)

1. 方法一: 利用哈希表的方法做,创建一个哈希表,将链表中的节点作为key,将创建好的新节点作为value,让value的指针指向key指向的节点的value值即可,最后返回开头结点的value值
   
   ```js
   // 方法一
   const copyListWithRand = (list) => {
     const map = new Map();
     let l = list;
     // 创建一个哈希表,将节点和复制后的节点都存储到map中
     while(l){
         let node = new ListNode(l.val);
         map.set(l,node);
         l = l.next;
     }
     for(let m of map){
         m[1].next = map.get(m[0].next);
         m[1].rand = map.get(m[0].rand);
     }
     return map.get(list);
   }
   ```

2. 方法二: 在每一个节点后面复制一个节点,但是其rand指针没有指向,例如,1的rand指针指向3,则1'的rand指针指向3',最后将新老列表分离
   
   ```js
   // 方法二:链表内部复制
   const copyListWithRand = (list) => {
     if(!list) return null;
     let l = list;
     let next = null;
     // 1->2->3
     // 1->1'->2->2'->3->3'
     while(l){
         next = l.next;
         l.next = new ListNode(l.val);
         l.next.next = next;
         l = next;
     }
     l = list;
     let copyNode = null;
     // 将每个复制后的节点的rand指针指向正确的位置
     while(l){
         next = l.next.next;
         copyNode = l.next;
         copyNode.rand = l.rand ? l.rand.next : null;
         l = next;
     }
     l = list;
     let res = l.next;
     // 将原来的链表与复制后的链表分离
     while(l){
         next = l.next.next;
         copyNode = l.next;
         l.next = next;// 原来的节点连在一起
         copyNode.next = next ? next.next : null; // 将复制后的节点相连接
         l = next;
     }
     return res;
   }
   ```
