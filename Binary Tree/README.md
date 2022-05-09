# 二叉树

二叉树节点结构

```js
// 二叉树的结构
function TreeNode(val, left, right) {
    this.val = (val === undefined ? 0 : val)
    this.left = (left === undefined ? null : left)
    this.right = (right === undefined ? null : right)
}
```

二叉树的初始化

```js
// 初始化一个二叉树
const initBinaryTree = (arr) => {
    let len = arr.length;
    let tree = initTreeNode(arr,0,len);
    return tree;
}

const initTreeNode = (arr,i,len) => {
    if(i >= len) return null;
    if(arr[i] === null) return null;
    let node = new TreeNode(arr[i]);
    node.left = initTreeNode(arr,2*i+1,len);
    node.right = initTreeNode(arr,2*i+2,len);
    return node;
}

const arr = [1,2,3,4,5,6,null,8];
var tree = initBinaryTree(arr);
```

## 二叉树的遍历

### 先序遍历

```js
// 先序遍历
const preOrderRecur = (head) => {
    if(!head) return null;
    console.log(head.val);
    preOrderRecur(head.left);
    preOrderRecur(head.right);
}
```

### 中序遍历

```js
// 中序遍历
const inOrderRecur = (head) => {
    if(!head) return null;
    inOrderRecur(head.left);
    console.log(head.val);
    inOrderRecur(head.right);
}
```

### 后序遍历

```js
// 后序遍历
const posOrderRecur = (head) => {
    if(!head) return;
    posOrderRecur(head.left);
    posOrderRecur(head.right);
    console.log(head.val);
}
```

### 层序遍历

```js
// 二叉树的层序遍历
const levelOrder = (root) => {
    const ret = [];
    if(!root) return null; // 如果二叉树为空,直接返回null
    const queue = []; // 创建一个队列用于存取节点
    queue.push(root); // 将根节点存入
    while(queue.length > 0){
        let len = queue.length; // 计算该层子结点的数量
        ret.push([]); // 存入一个新的数组[]
        for(let i = 0;i < len;i++){
            const node = queue.shift();
            ret[ret.length-1].push(node.val); // 依次取出该层节点并将值存入ret
            if(node.left) queue.push(node.left); // 将左右子树存入queue
            if(node.right) queue.push(node.right);
        }
    }
    return ret;
}
```

## 二叉搜索树

又称为二叉排序树,对于一棵二叉排序树,它或者是一棵空树，或者是具有下列性质的二叉树： 若它的左子树不空，则左子树上所有结点的值均小于它的根节点的值； 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值； 它的左、右子树也分别为二叉排序树。

判断代码:

```js
// 判断是不是二叉搜索树
const isSearchTree = (root) => {
    if(!root) return true;
    if(!root.left && !root.right) return true;
    if(root.left.val >= root.val || root.right.val <= root.val) return false;
    return isSearchTree(root.left) && isSearchTree(root.right);
}
```

## 完全二叉树

判断是不是完全二叉树

1. 当左孩子为null时,右孩子如果不为null,一定不是完全二叉树
2. 如果左右孩子有一个为空,下面的节点必须都是叶子节点

```js
// 判断是不是完全二叉树
const isCBT = (root) => {
    if(!root) return true;
    let queue = [];
    let left = null,right = null;
    queue.push(root);
    // 当左右孩子不双全的时候，下面必须都是叶子节点
    let flag = false; // 是否遇到过左右节点不双全的节点
    while(queue.length !== 0){
        let node = queue.shift();
        left = node.left;
        right = node.right;
        if((flag && (left || right)) || (!left && right)) return false;
        if(left) queue.push(left);
        if(right) queue.push(right);
        // 当左右孩子有一个为null,下面必须是叶子节点
        // flag = true
        if(!left || !right) flag = true;
    }
    return true;
}
```
## 满二叉树

获取二叉树的高度

```js
// 获取二叉树的高度
const TreeHeight = (root) => {
    if(!root) return 0;
    return Math.max(TreeHeight(root.left),TreeHeight(root.right)) + 1;
}
```

获取二叉树的节点个数

```js
// 获取二叉树的节点个数
const TreeNodeNum = (root) => {
    if(!root) return 0;
    return 1 + TreeNodeNum(root.left) + TreeNodeNum(root.right);
}
```

判断一棵树是不是满二叉树

```js
// 判断一棵树是不是满二叉树
const isFullBinaryTree = (root) => {
    let height = TreeHeight(root);
    let count = TreeNodeNum(root);
    if(count === Math.pow(2,height)-1) return true;
    return false;
}
```
## 平衡二叉树

1. 是二叉排序树
2. 任何一个节点的左子树或者右子树都是平衡二叉树(左右高度差小于等于1)

```js
// 判断一棵树是不是平衡二叉树
const isBalancedBinaryTree = (root) => {
    // 如果节点为空或者无左右子树,返回true
    if(!root) return true;
    if(!root.left && !root.right) return true;
    // 如果左右子树的高度差大于1或者当前树不是二叉搜索树,返回false
    if(Math.abs(TreeHeight(root.left)-TreeHeight(root.right)) > 1 || !isBinarySearchTree(root)) return false;
    // 判断左右子树是不是平衡二叉树
    return isBalancedBinaryTree(root.left) && isBalancedBinaryTree(root.right);
}
```

## 二叉树例题训练

### 题目一: 最低公共祖先节点

1. 给定一个**二叉搜索树**, 找到该树中两个指定节点的最近公共祖先

   ```js
   const lowestCommonAncestor = (root,p,q) => {
       // 二叉搜索树,左边节点的值都小于当前节点,右边节点的值都大于当前节点
       if(p.val < root.val && q.val < root.val){
           // 如果两个值都小于root.val,遍历左子树寻找合适的节点
           return lowestCommonAncestor(root.left);
       }
       else if(p.val > root.val && q.val > root.val){
           // 如果两个值都大于root.val,遍历右子树寻找合适的节点
           return lowestCommonAncestor(root.right);
       }
       // 如果一个大于当前节点一个小于当前节点,返回当前的节点
       return root;
   }
   ```

2. 给定两个二叉树的节点`node1`和`node2`,找出他们的最低公共祖先节点

   方法一:

   ```js
   // 寻找两个节点的最低公共祖先
   const lowestCommonAncestor = (root,p,q) => {
       // 创建一个用于存储父节点的哈希表
       const fatherMap = new Map();
       // 根节点的父节点就是它本身
       fatherMap.set(root,root);
       // 遍历将每个节点及其父节点存储在fatherMap中
       process(root,fatherMap);
       let node = p;
       // 创建一个用于存储p节点祖先节点的哈希表
       const map = new Map();
       while(node != fatherMap.get(node)){
           // 当没有遍历到根节点
           map.set(node);
           // node=node的父节点
           node = fatherMap.get(node);
       }
       // 将根节点也存入到map中(也是p的祖先节点)
       map.set(root);
       // 寻找二者共同的祖先节点
       while(q != fatherMap.get(q)){
           // 向上找q的祖先节点,查看其是否在p的祖先节点中
           // 如果找到了二者的公共祖先节点,返回
           if(map.has(q)) return q;
           q = fatherMap.get(q);
       }
       // 如果上面的遍历都没有找到结果,则最低公共祖先节点为root
       return root;
   }
   
   // 遍历存储每个节点的父节点
   const process = (root,fatherMap) => {
       if(!root) return;
       if(root.left){
           fatherMap.set(root.left,root);
           process(root.left,fatherMap);
       }
       if(root.right){
           fatherMap.set(root.right,root);
           process(root.right,fatherMap);
       }
   }
   ```

   方法二

   ```js
   // 方法二
   /*
   1. p是q的最低公共祖先,或者q是p的最低公共祖先
   2. p,q不是对方的最低公共祖先
   */
   const lowestCommonAncestor = (root,p,q) => {
       if(!root || root == p || root == q) return root;
       const left = lowestCommonAncestor(root.left,p,q);
       const right = lowestCommonAncestor(root.right,p,q);
       // 如果二者都不为空(p,q一个在左子树上,一个在右子树上)
       // 返回当前的根节点
       if(left && right) return root;
       // 如果有一方为空,说明p,q都在另一方上
       return left ? left : right;
   }
   ```

### 题目二: 寻找后继节点

现在有一种新的二叉树节点类型如下:

```js
// 二叉树的结构
function TreeNode(val, left, right, parent) {
    this.val = (val === undefined ? 0 : val)
    this.left = (left === undefined ? null : left)
    this.right = (right === undefined ? null : right)
    this.parent = (parent === undefined ? null : parent)
}
```

该结构比普通二叉树结构多了一个指向父节点的`parent`指针

假设有一棵`TreeNode`类型的节点组成的二叉树,书中的每个节点的`parent`指针都指向自己的父节点,头结点的`parent`指向`null`

只给一个在二叉树中的某个节点`node`,请实现返回`node`的后继节点的函数

在二叉树的中序遍历的序列中,`node`的下一个节点叫做`node`的后继节点

```js
// 获取一个节点的后继节点
const getSuccessorNode = (root) => {
    if(!root) return null;
    // 如果有右子树,后继节点就是右子树
    if(node.right){
        return getLeftNost(node);
    }
    // 如果没有右子树,就遍历找当前节点是其父节点的右子树的节点
    else{
        let parent = node.parent;
        while(parent && parent.right == node){
            // 当前节点是其父节点的右孩子
            node = parent;
            parent = node.parent;
        }
        return parent;
    }
}
// 获取当前子树的最左边节点
const getLeftMost = (node) => {
    if(!node) return node;
    // 如果子树上还有左子树,一直向下
    while(node.left) node = node.left;
    return node;
}
```

### 二叉树的序列化和反序列化

在内存中的一棵树如何变成字符串形式,有如何从字符串形式变成内存里的树?

```js
// 将二叉树转化成字符串返回
const serialByPre = (root) => {
    if(!root) return "#_";
    let res = root.val + '_';
    res += serialByPre(root.left);
    res += serialByPre(root.right);
    return res;
}

// 通过字符串创建二叉树
const reconPreOrder = (queue) => {
    let value = queue.pop();
    if(value === '#') return null;
    let node = new TreeNode(value);
    node.left = reconPreOrder(queue);
    node.right = reconPreOrder(queue);
    return node;
}

const arr = [8,5,11,2,7];
var root = initBinaryTree(arr);
console.log(serialByPre(root));
var res = serialByPre(root);
var queue = res.split('_').reverse();
console.log(reconPreOrder(queue));
  
```

### 纸条折痕问题

给你一张纸条,每次从中间向上对折,计算对折n次后凹折痕和凸折痕的数量

![image-20220416173916006](https://github.com/Ustinians/FE_Algorithm/blob/main/images/test3.jpg)

第一次对折后,得到一条凹折痕

第二次对折后,上面为凹折痕,下面为凸折痕

再对折一次,发现规律为当前折痕上面为凹折痕,下面为凸折痕

设凹折痕为`true`,凸折痕为`false`

## 树形DP套路

树形DP套路使用前提

如果题目求解目标是S规则,则求解流程可以定成以每一个结点为头结点的子树在S规则下的每一个答案,并且最终答案一定在其中

* 树形`dp`套路第一步：

  以某个节点X为头节点的子树中，分析答案有哪些可能性，并且这种分析是以X的左子树、X的右子树和X整棵树的角度来考虑可能性的

* 树形`dp`套路第二步：

  根据第一步的可能性分析，列出所有需要的信息

* 树形`dp`套路第三步：

  合并第二步的信息，对左树和右树提出同样的要求，并写出信息结构

* 树形`dp`套路第四步：

  设计递归函数，递归函数是处理以X为头节点的情况下的答案。

  包括设计递归的`basecase`，默认直接得到左树和右树的所有信息，以及把可能性做整合，并且要返回第三步的信息结构这四个小步骤
  
### 二叉树节点间的最大距离问题

从二叉树的节点`a`出发，可以向上或者向下走，但沿途的节点只能经过一次，到达节点`b`时路径上的节点个数叫作`a`到`b`的距离，那么二叉树任何两个节点之间都有距离，求整棵树上的最大距离。

例如,下面图片的最大距离是7(8->4->2->1->3->6->9)
![test1](https://github.com/Ustinians/FE_Algorithm/blob/main/images/test1.jpg)

```js
// 二叉树的结构
function TreeNode(val, left, right) {
    this.val = (val === undefined ? 0 : val)
    this.left = (left === undefined ? null : left)
    this.right = (right === undefined ? null : right)
}

// 初始化一个二叉树
const initBinaryTree = (arr) => {
    let len = arr.length;
    let tree = initTreeNode(arr,0,len);
    return tree;
}

const initTreeNode = (arr,i,len) => {
    if(i >= len) return null;
    if(arr[i] === null) return null;
    let node = new TreeNode(arr[i]);
    node.left = initTreeNode(arr,2*i+1,len);
    node.right = initTreeNode(arr,2*i+2,len);
    return node;
}

// 创建信息结构
function Info(maxDistance,height){
    this.maxDistance = (maxDistance === undefined ? 0 : maxDistance); // 当前树的最大距离
    this.height = (height === undefined ? 0 : height); // 当前树的高度
}

// 返回以x为头的整棵树的两个信息(树的高度和最大距离)
const process = (x) => {
    if(!x) return new Info(0,0); // 当当前树为空的时候,最大距离和高度都为0的
    let leftInfo = process(x.left);
    let rightInfo = process(x.right);
    // 计算左树和右树的Info(maxDistance,height)
    /*
    	如果遍历最大距离事经过当前节点,则当前树的最大距离就是左右子树的高度之和+1
    	如果不经过当前结点,则当前树的最大距离就是左树右树中较大的最大距离
     */
    let p1 = leftInfo.maxDistance; // 令p1等于左树的最大距离
    let p2 = rightInfo.maxDistance; // 令p2等于右树的最大距离
    let p3 = leftInfo.height + rightInfo.height + 1; // 令p3等于左右树的高度之和加一(如果经过当前节点)
    // 当经过当前节点的时候,当前节点的最大距离等于左右子树高度之和,否则为左树最大距离或者右树最大距离
    let maxDistance = Math.max(p3,Math.max(p1,p2)); // 当前树的最大距离等于左右子树高度之和与左右子树的最大距离比较得出的最大的值
    let height = Math.max(leftInfo.height,rightInfo.height) + 1; // 当前树节点的高度等于左右子树中较高的一棵树的高度+1
    return new Info(maxDistance,height); // 返回根据当前树构建的Info
}

// 返回最大距离
const maxDistance = (head) => {
    return process(head).maxDistance;
}
```

### 派对的最大快乐值

员工信息的定义如下:
```js
function Employee(happy,subordinates){
	this.happy = (happy === undefined ? 0 : happy) // 这名员工可以带来的快乐值
    this.subordinates = (subordinates === undefined ? null : subordinates) // 这名员工有哪些直接下级
}
```
or
```java
class Employee {
    public int happy; // 这名员工可以带来的快乐值
    List<Emplotee> subordinates; // 这名员工有哪些直接下级
}
```

公司的每个员工都符合`Employee`类的描述。整个公司的人员结构可以看作是一棵标准的、没有环的多叉树。树的头节点是公司唯一的老板。除老板之外的每个员工都有唯一的直接上级。叶节点是没有任何下属的基层员工（`subordinates`列表为空），除基层员工外，每个员工都有一个或多个直接下级。

这个公司现在要办`party`，你可以决定哪些员工来，哪些员工不来。但是要遵循如下规则。

1．如果某个员工来了，那么这个员工的所有直接下级都不能来

2．派对的整体快乐值是所有到场员工快乐值的累加

3．你的目标是让派对的整体快乐值尽量大

给定一棵多叉树的头节点`boss`，请返回派对的最大快乐值。

例如,在下列结构中
![test2](https://github.com/Ustinians/FE_Algorithm/blob/main/images/test2.jpg)

x树的快乐值分为两种情况

1. `x`来的时候,总快乐值等于`x`的快乐值 + `a`不来时`a`子树的快乐值总值 + `b`不来时`b`子树的快乐值总值 + `c`不来时`c`子树的快乐值总值
2. `x`不来的时候,总快乐值等于`a`树的最大快乐之 + `b`树的最大快乐值 + `c`树的最大快乐值

