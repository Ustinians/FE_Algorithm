# Sort

## 选择排序

> 选择排序（Selection sort）是一种简单直观的[排序算法](https://baike.baidu.com/item/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95/5399605)。它的工作原理是：第一次从待排序的[数据元素](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E5%85%83%E7%B4%A0/715313)中选出最小（或最大）的一个元素，存放在序列的起始位置，然后再从剩余的未排序元素中寻找到最小（大）元素，然后放到已排序的序列的末尾。以此类推，直到全部待排序的数据元素的个数为零。<font color="red">选择排序是不稳定的排序方法</font>。

**代码:**

```js
// 选择排序
const selectSort = (arr) => {
  const len = arr.length;
  for(let i = 0;i < len;i++){
    for(let j = i+1;j < len;j++){
      if(arr[i] > arr[j]){
        [arr[i],arr[j]] = [arr[j],arr[i]]; // 进行数值交换
      }
    }
  }
  console.log(arr);
}
```

## 冒泡排序

> 冒泡排序（Bubble Sort），是一种[计算机科学](https://baike.baidu.com/item/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6/9132)领域的较简单的[排序算法](https://baike.baidu.com/item/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95/5399605)。
> 
> 它重复地走访过要排序的元素列，依次比较两个相邻的[元素](https://baike.baidu.com/item/%E5%85%83%E7%B4%A0/9563223)，如果顺序（如从大到小、首字母从Z到A）错误就把他们交换过来。走访元素的工作是重复地进行直到没有相邻元素需要交换，也就是说该元素列已经排序完成。
> 
> 这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端（升序或降序排列），就如同碳酸饮料中[二氧化碳](https://baike.baidu.com/item/%E4%BA%8C%E6%B0%A7%E5%8C%96%E7%A2%B3/349143)的气泡最终会上浮到顶端一样，故名“冒泡排序”。

冒泡排序算法的原理如下：

1. 比较相邻的元素。如果第一个比第二个大，就交换他们两个。

2. 对每一对相邻元素做同样的工作，从开始第一对到结尾的最后一对。在这一点，最后的元素应该会是最大的数。 

3. 针对所有的元素重复以上的步骤，除了最后一个。

4. 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

$$
时间复杂度:O(n^2)
$$

**代码:**

```js
// 冒泡排序
const bubbleSort = (arr) => {
  const len = arr.length;
  for(let i = 0;i < len;i++){
    for(let j = 1;j < len;j++){
      if(arr[j-1] > arr[j]){
        [arr[j-1],arr[j]] = [arr[j],arr[j-1]];
      }
    }
  }
  console.log(arr);
}
```

## 插入排序

> 插入排序，一般也被称为直接插入排序。对于少量元素的排序，它是一个有效的算法 。插入排序是一种最简单的排序方法，它的基本思想是将一个记录插入到已经排好序的有序表中，从而一个新的、记录数增1的有序表。在其实现过程使用双层循环，外层循环对除了第一个元素之外的所有元素，内层循环对当前元素前面有序表进行待插入位置查找，并进行移动 。

对于一个长度为n的数组,先将0~1进行排序,然后0~2进行排序,……,0~n进行排序

**代码:**

```js
// 插入排序
const insertSort = (arr) => {
    for(let i = 1; i < arr.length; i++){
        // 当j <= 0 或者arr[j]不大于arr[j+1]时,就需要循环了
        // 前面都是有序的
        for(let j = i-1;j >= 0 && arr[j] > arr[j+1];j--){
            [arr[j],arr[j+1]] = [arr[j+1],arr[j]];
        }
    }
    console.log(arr);
}og(arr);
}
```

$$
最差时的时间复杂度: O(n^2)
$$

归并排序

> **归并排序**是建立在[归并](https://baike.baidu.com/item/%E5%BD%92%E5%B9%B6/253741)操作上的一种有效，稳定的[排序算法](https://baike.baidu.com/item/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95/5399605)，该算法是采用[分治法](https://baike.baidu.com/item/%E5%88%86%E6%B2%BB%E6%B3%95/2407337)（Divide and Conquer）的一个非常典型的应用。将已有序的子序列[合并](https://baike.baidu.com/item/%E5%90%88%E5%B9%B6/5615281)，得到完全有序的[序列](https://baike.baidu.com/item/%E5%BA%8F%E5%88%97/1302588)；即先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为[二路归并](https://baike.baidu.com/item/%E4%BA%8C%E8%B7%AF%E5%BD%92%E5%B9%B6/53201558)。

将一个数组分成两份,分别进行排序,最后创建 一个备用数组,依次比较两份数组变量大小并插入数组

1. 整体就是一个简单递归,左边排好序,右边排好序,让其整体有序
  
2. 让其整体有序的过程里用了<font color="red">排外序方法</font>(两个指针进行排序,拷贝到一个外部的数据中,再拷贝回来)
  
3. 利用`master`公式来求解时间复杂度
  
  $$
  时间复杂度: O(N*logN), 额外空间复杂度O(N)
  $$

```js
const process = (arr,left,right) => {
    if(left === right){
        // arr只有一个元素,直接返回
        return arr;
    }
    let middle = left +  ((right - left) >> 1);
    process(arr,left,middle);
    process(arr,middle+1,right);
    merge(arr,left,right,middle);
}

const merge = (arr,left,right,middle) => {
    let ans = [];
    let l = left;
    let r = middle+1;
    while(l <= middle && r <= right){
        ans.push(arr[l] <= arr[r] ? arr[l++] : arr[r++]);
    }
    while(l <= middle){
        ans.push(arr[l++]);
    }
    while(r <= right){
        ans.push(arr[r++]);
    }
    for(let i = 0;i < ans.length;i++){
        arr[left+i] = ans[i];
    }
}
```

### 归并排序的拓展

#### 小和问题

在一个数组中,每一个数左边比当前数小的数累加起来,叫做这个数组的小和,求一个数组的小和。

对于每一个元素,计算其右侧比其大的元素的个数(逆向思维)

```js
const process = (arr,left,right) => {
    if(left === right) return 0; // 当只有一个元素
    let middle = left + ((right - left) >> 1);
    return  process(arr,middle+1,right)
        + process(arr,left,middle)
        + merge(arr,left,right,middle);
}

const merge = (arr,left,right,middle) => {
    let ans = [];
    let l = left,r = middle+1;
    let res = 0;
    while(l <= middle && r <= right){
        // 因为此时已经排好序了,当arr[r]大于arr[l]的时候,arr[r]后面的数据也一定大于arr[l]
        res += arr[l] < arr[r] ? (right-r+1)*arr[l] : 0;
        ans.push(arr[l] < arr[r] ? arr[l++] : arr[r++]);
    }
    while(l <= middle){ 
        ans.push(arr[l++]);
    }
    while(r <= right){
        ans.push(arr[r++]);
    }
    for(let i = 0;i < ans.length;i++){
        arr[left+i] = ans[i];
    }
    return res;
}

const arr = [1,3,4,2,5];
console.log(process(arr,0,arr.length-1));
```

例子: `[1,3,4,2,5]`中,1左边比1小的数,没有,3左边比3小的数,1,4左边比4小的数:1,3;2左边比2小的数,1;5左边比5小的数,1,3,4,2;所以小和为`1 + 1 + 3 + 1 + 1 + 3 + 4 + 2 = 16`

#### 逆序对问题

在一个数组中,左边的数如果比右边的数大,则称两个数构成一个逆序对,请打印所有逆序对。

> 例: 在数组[1,3,4,2,5]中,逆序对有[3,2],[4,2]
> 
>       在数组[3,2,1]中,逆序对有[3,2],[3,1].[2,1]

## 快速排序

### 问题一

给定一个数组arr,和一个数num,请把小于等于num的数放在数组的左边,大于num的数放在数组的右边,要求额外空间复杂度O(1),时间复杂度O(N)

思路: 设定一个左边界,将小于等于num的都放在左边界前

![](file://C:\Users\admin\AppData\Roaming\marktext\images\2022-04-08-15-19-31-image.png?msec=1649571040676)

```js
const exchangeNum = (arr,num) => {
    let pos = 0; // 设定一个边界
    for(let i = 0;i < arr.length;i++){
        if(arr[i] <= num){
            if(pos !== i){
                // 进行交换
                let temp = arr[i];
                arr[i] = arr[pos];
                arr[pos] = temp;
            }
            // 边界向右移
            pos++;
        }
    }
    return arr;
}

const arr = [3,5,6,7,4,3,5,8];
console.log(exchangeNum(arr,5));
```

### 问题二 荷兰国旗问题

给定一个数组arr,和一个数num,请把小于num的数放在数组的左边,等于num的数放在数组中间,大于num的数放在数组的右边,要求额外空间复杂度O(1),时间复杂度O(N)

与上面思路类似,设定两个边界,小于num的放在左边界,大于num的放在右边界

```js
const exchangeNum = (arr,num) => {
    let left = 0,right = arr.length-1;
    for(let i = 0;i < arr.length;i++){
        if(left === right-1) break;
        if(arr[i] < num){
            let temp = arr[i];
            arr[i] = arr[left];
            arr[left] = temp;
            left++;
        }
        else if(arr[i] > num){
            let temp = arr[i];
            arr[i] = arr[right];
            arr[right] = temp;
            right--;
            i--;
        }
    }
    return arr;
}

const arr = [8,5,4,1,9,3];
console.log(exchangeNum(arr,5));
```

### 快速排序

$$
额外空间复杂度: O(logN)
$$

```js
// 快速排序
var quickSort = function (arr) {
    if (arr.length <= 1) {//如果数组长度小于等于1无需判断直接返回即可 
        return arr;
    }
    var pivotIndex = Math.floor(arr.length / 2);//取基准点 
    var pivot = arr.splice(pivotIndex, 1)[0];//取基准点的值,splice(index,1)函数可以返回数组中被删除的那个数
    var left = [];//存放比基准点小的数组
    var right = [];//存放比基准点大的数组 
    for (var i = 0; i < arr.length; i++) { //遍历数组，进行判断分配 
        if (arr[i] < pivot) {
            left.push(arr[i]);//比基准点小的放在左边数组 
        } else {
            right.push(arr[i]);//比基准点大的放在右边数组 
        }
    }
    //递归执行以上操作,对左右两个数组进行操作，直到数组长度为<=1； 
    return quickSort(left).concat([pivot], quickSort(right));
};

const arr = [4, 3, 5, 8, 1];
console.log(quickSort(arr));
```

## 堆排序

代码:

```js
/**
 * 堆排序
 * 传入一个数组
 * 将其从最下面的非叶子节点开始排序
 * 依次向上将堆调整成最大堆
 * 每次将最顶上最大的数取出放在新的数组ans中
 * 然后重新调整堆为最大堆,再取出最大的放入ans中
 * 重复以上过程知道arr为空
 * 返回ans
 */
// 堆排序
const heapSort = (arr) => {
    if(arr.length <= 1) return arr;
    buildHeap(arr);
    var len = arr.length;
    const ans = [];
    for(let i = len-1;i >= 0;i--){
        swap(arr,0,i); // 进行递归调整,每次都将最大的数放在最后
        ans.unshift(arr.pop());
        heapify(arr,0); // 然后调整堆
    }
    // ans.unshift(arr.pop());
    return ans;
}

// 创建一个堆
const buildHeap = (arr) => {
    // if(arr.length <= 1) return arr;
    var len = arr.length;
    for(let i = Math.floor(len/2)-1;i >= 0;i--){
        /**
         * 从len/2开始的原因是因为len/2后面的就都是叶子节点了
         */
        // 调整位置使其变成最大堆
        heapify(arr,i);
    }
}

// 调整成最大堆
const heapify = (arr,index) => {
    // 调整数组使其成为最大堆
    var left = 2 * index + 1;
    var len = arr.length;
    while(left < len){
        let largest = index; // 先将最大值赋值给index
        largest = (left+1 < len && arr[left+1] > arr[left]) ? left+1 : left; // 获取到左右节点中较大的一个
        largest = arr[largest] > arr[index] ? largest : index;
        if(largest == index) break; // 此时当前节点比左右节点大
        swap(arr,largest,index); // 如果largest节点的值大于index
        index = largest;
        left = index * 2 + 1; // 循环调整左右子堆直到叶子节点
    }
}
// 交换数组中两个节点的函数
const swap = (arr,i,j) => {
    let temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}

var arr = [5,8,4,9,1,5,4];
console.log(heapSort(arr));
```

### 堆排序扩展

已知有一个几乎有序的数字,几乎有序是指,如果把数组排好顺序的话,每个元素移动的距离不可以超过k,并且k对于数组来说比较小,请选择一个合适的排序算法针对这个数据进行排序

```js
// 准备一个小根堆
// 堆排序
const heapSort = (arr) => {
    if(arr.length <= 1) return arr;
    buildHeap(arr);
    const ans = [];
    for(let i = arr.length-1;i >= 0;i--){
        swap(arr,0,i);
        ans.push(arr.pop());
        heapify(arr,0);
    }
    return ans;
}

const buildHeap = (arr) => {
    var len = arr.length;
    for(let i = Math.floor(len/2)-1;i >= 0;i--){
        heapify(arr,i);
    }
}

const heapify = (arr,index) => {
    let len = arr.length;
    let left = index*2+1;
    while(left < len){
        let min = index;
        min = (left+1 < len && arr[left+1] < arr[left]) ? left+1 : left;
        // if(arr[index] <= arr[min]) break;
        min = arr[min] < arr[index] ? min : index;
        if(min == index) break;
        swap(arr,index,min);
        index = min;
        left = index * 2 + 1;
    }
}

const swap = (arr,i,j) => {
    let temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}

var arr = [1,3,6,4,5,2];
console.log(heapSort(arr));
```
