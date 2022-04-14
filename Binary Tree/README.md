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

