# 贪心算法

**贪心算法在笔试时的解题套路**

1. 实现一个不依靠贪心策略的解法X,可以使用最暴力的尝试
2. 脑补出贪心策略A,贪心策略B,贪心策略C
3. 用解法X和对数器,去验证每一个贪心策略,用实验的方式得知哪个贪心策略正确
4. 不要去纠结贪心策略的证明

## **例题 1:** 会议宣讲问题

一些项目要占用一个会议室进行宣讲,会议室不能同时容纳两个项目的宣讲,给你每一个项目的开始时间和结束的时间(给你一个数组,里面是一个个具体的项目),你来安排宣讲的日程,要求会议室的宣讲场次最多,返回这个最多的宣讲场次

```js
const compare = (o1,o2) => {
    return o1.end - o2.end;
}
const bestArrange = (programs,timePoint) => {
    let ans = programs; // 按照结束时间进行排序
    ans.sort((a,b) => compare(a,b));
    let result = 0; // 宣讲过的会议为0个
    // 从左到右依次遍历所有的会议
    for(let i = 0;i < programs.length;i++){
        if(timePoint <= programs[i].start){
            // 如果当前会议的开始时间小于上一个会议的结束时间点
            result++; // 宣讲会议的数量
            timePoint = programs[i].end; // 时间点等于当前会议的结束时间
        }
    }
    return result;
}
```

## **例题 2**: 字典排序问题

给你一个数组,里面有若干个字符串,请将这些字符串进行拼接并使得到的字符串在字典中的位置排的最靠前面

```js
// 比较字符串的大小
const compare = (a,b) => {
    /**
     * str.localeCompare(compareStr)
     * 如果a<b,返回-1
     * 如果a=b,返回0
     * 如果a>b,返回1
     */
    return (a+b).localeCompare(b+a) === -1 ? a+b : b+a;
}

// 贪心算法得到拼接后比较小的字符串
const lowestString = (strs) => {
    // 如果传入的数组不存在或者长度为0,直接返回""
    if(!strs || strs.length === 0) return "";
    // 遍历比较并连接strs中的元素
    let res = strs.reduce((item,pre) => {
        pre = compare(item,pre);
        return pre;
    },"");
    // 返回排序后得到的结果
    return res;
}
```

## **例题 3**: 切割金条问题

一根金条切成两半,是需要花费和长度数值一样的铜板,比如长度为20的金条,不管切成长度多大的两半,都需要花费20个铜板。

一群人想整分整块金条,怎么分最省铜板?

例如,给定数组{10,20,30},代表一共3个人,整块金条的长度为10+20+30=60。

金条要分成10,20,30三部分,如果先把长度60的金条分成10和50,花费60;再把长度为50的金条各分成20和30,花费50,一共花费110铜板。

但是如果先把长度为60的金条分成30和30的,花费60,再把长度30的金条分成10和20,花费30;一共花费90铜板。

输入一个数组,返回分割的最小代价

**思路:**

创建一个大根堆,每次将金条切成最大的两份,依次向下

![大根堆](https://pic.imgdb.cn/item/625d2b82239250f7c50cc67a.jpg)

先将金条分成9和14两份,花费23铜板

然后将长度为14的金条分成7和7两份,花费14铜板

.....

```js
// 通过构建最大堆的方式解决问题
const lessMoney = (arr) => {
    // 创建一个数组用于存储排序后的数组arr
    let ans = [];
    // 将数值依次放入ans中
    for(let i = 0;i < arr.length;i++){
        ans.push(arr[i]);
    }
    // 对ans中的数值进行排序(升序)
    ans.sort((a,b) => a-b);
    // 记录消耗的铜板数
    let sum = 0;
    // 存储当前消耗的金币数
    let cur = 0;
    while(ans.length > 1){
        // 取出最前面最小的两个数据
        let num1 = ans.shift();
        let num2 = ans.shift();
        cur = num1 + num2;
        sum += cur;
        ans.push(cur);
        // 将cur放入队列然后重新排序
        ans.sort((a,b) => a-b);
    }
    return sum;
}
```

## 例题 4: 计算最终资金

假如你是一名创业者,你拥有m的启动资金,由于你的团队规模较小,在做项目的时候只能串行进行,现在给你一个长度为`n`的数组:每个项目包括两个属性: `cost`(花费)和`profit`(纯利润),在你最多能做的项目个数是k个的前提下,你能得到的最多的资金是多少?

**输入:**

正数数组`items`

正数`k`

正数`m`

**含义:**

`items[i].cost`表示`i`号项目的花费

`items[i].profit`表示`i`号项目在扣除花费后还能挣到的钱(纯利润)

`k`表示你只能串行的最多做`k`个项目

`m`表示你的初始资金

**说明:**

你每做完一个项目,马上获得的收益,可以支持你去做下一个项目

**输出:**

你最后获得的最大的钱数

**例子:**

```js
items = [
    {cost: 3,profit: 1},
    {cost: 1,profit: 2},
    {cost: 4,profit: 3},
    {cost: 9,profit: 7},
    {cost: 9,profit: 4},
]
M = 1
K = 4
```

假设最开始先做第二个项目,花费为1,利润为2,做完之后的M = 3, K = 3

然后做第一个项目,花费为3,利润为1,做完之后的金额M = 4, K = 2

然后进行第三个实验,花费为4,利润为3,做完之后的金额M = 7, K = 1

由于最后两个实验的成本都高于M,项目停止

```js
// 计算最大的钱数
const maxMoney = (items,m,k) => {
    let ans = [];
    for(let i = 0;i < items.length;i++){
        ans.push(items[i]);
    }
    // ans用于存储花费的最小堆
    ans.sort((a,b) => a.cost-b.cost);
    while(k--){
        // ant用于存储花费<=m的利润最大堆
        let ant = [];
        for(let i = 0;i < ans.length;i++){
            if(ans[i].cost > m) break;
            ant.push(ans[i]);
        }
        // 如果没有花费小于等于m的项目,直接停止
        if(ant.length === 0) break;
        // 按照利润进行排序
        ant.sort((a,b) => b.profit-a.profit);
        // 取出利润最大的节点
        let item = ant.shift();
        // 将利润添加入资金
        m += item.profit;
        // 在ans中寻找该项目并将其移除
        let index = ans.indexOf(item);
        ans.splice(index,1);
        console.log(index);
        console.log(ans);
        // 对ans进行重新排序
        // ans.sort((a,b) => a.cost-b.cost);
    }
    return m;
}
```

# 堆的应用

一个数据流中,可以随时取得中位数

```js
// 计算一组数字的中位数
const median = (nums) => {
    let bigPile = []; // 设置大根堆
    let smallPile = []; // 设置小根堆
    for(let i = 0;i < nums.length;i++){
        // 如果两个堆都为空,直接进入大根堆
        if(bigPile.length === 0 && smallPile.length === 0) bigPile.push(nums[i]);
        // 如果当前节点的值<= 大根堆,进入大根堆
        else if(nums[i] <= bigPile[0]){
            bigPile.push(nums[i]);
        }
        // 否则进入小根堆
        else{
            smallPile.push(nums[i]);
        }
        // 将大小根堆重新进行排序
        bigPile.sort((a,b) => b-a);
        smallPile.sort((a,b) => a-b);
        // 判断大根堆和小根堆的长度差值,大于2则互相移动
        // 每次都是取出大根堆中最大的入小根堆
        // 或者取出小根堆里最大的入大根堆
        // 这样最后的中位数一定在二者的堆顶中产生
        if(bigPile.length - smallPile.length >= 2){
            smallPile.unshift(bigPile.shift());
        }
        else if(smallPile.length - bigPile.length >= 2){
            bigPile.unshift(smallPile.shift());
        }
    }
    if(bigPile.length > smallPile.length) return bigPile.shift();
    else if(bigPile < smallPile.length) return smallPile.shift();
    else return (bigPile.shift() + smallPile.shift()) / 2;
}
/**
 * 大根堆 [ 3, 2, 1 ]
 * 小根堆 [ 4, 5, 8 ]
 * 中位数 3.5
 */

let nums = [2,8,3,1,4,5];

console.log(median(nums));
```

# N皇后问题

N皇后问题是指在N*N的棋盘上要摆N个皇后,要求任何两个皇后不同行,不同列,也不在同一条斜线上

给定一个整数n,返回b皇后的摆法有多少种

n=1,返回1

n=2或3,2皇后和3皇后问题无论怎么摆都不行,返回0

n=8,返回92

**分析:**

由题知,两个皇后不能同行,因此每行必定有一个皇后 

**暴力递归:** 

```js
// N皇后问题
/*
N皇后问题是指在N*N的棋盘上要摆N个皇后,要求任何两个皇后不同行,不同列,也不在同一条斜线上
给定一个整数n,返回b皇后的摆法有多少种
n=1,返回1
n=2或3,2皇后和3皇后问题无论怎么摆都不行,返回0
n=8,返回92
*/

const num = (n) => {
    if(n < 1) return 0;
    let record = new Array(n); // record[i]代表第i行的皇后放在了第几列
    return process(0,record,n);
}


/**
 * record[0,...,i-1]行的皇后，一定不共行、不共列、不共斜线
 * 目前来到了第i行
 * record[0,...,i-1]表示之前的行,放了皇后的位置
 * n表示整体一共有多少行
 * 返回值是,摆完所有的皇后,合法的摆法有多少种
 */
// 暴力递归
const process = (i,record,n) => {
    if(i === n) return 1; // 终止行
    let res = 0;
    for(let j = 0;j < n;j++){
        // 当前第i行的皇后,放在第j列,会不会和之前(0,...,i-1)的皇后,共行共列或者公斜线
        // 如果不是,认为有效
        if(isValid(record,i,j)){
            record[i] = j;
            res += process(i+1,record,n);
        }
    }
    return res;
}

/** 
 * record[0,...,i-1]需要看,record[i,...]不需要看
 * 返回i行皇后放在第j列是否有效
 */
const isValid = (record,i,j) => {
    for(let k = 0;k < i;k++){
        // 如果在同一列或者在同一条斜线上(列-列 = 行-行)
        if(j == record[k] || Math.abs(record[k]-j) === Math.abs(i-k)){
            return false;
        }
    }
    return true;
}
```













































