# 记忆化搜索

## 机器人到达E位置的方法数

有一个机器人,可以在1~N位置上左右移动,机器人的最终目标位置为E,且机器人当前在cur位置上,已知机器人可以走且必须走够rest步,求机器人从cur位置到达E位置的方法数

暴力递归

```js
/**
 * 一共是1~N这么多位置 固定参数
 * 最终目标是E 固定参数
 * 还剩rest步可以走 当前机器人还需要走rest步
 * 当前在cur位置上 当前机器人在cur位置上
 * 返回方法数
 */
const f = (N, E, rest, cur) => {
    // 当步数rest为0的时候,已经没有步数可用了,判断是否到达E位置,到达返回1,未到达返回0
    if (rest === 0) return cur === E ? 1 : 0;
    // 当test不为0,还有步数可以走
    // 当在1位置的时候,只能往右边(2)位置上走
    if (cur === 1) return f(N, E, rest - 1, 2);
    // 当在N位置上的时候,只能往左边(N-1)位置走
    if (cur === N) return f(N, E, rest - 1, N - 1);
    // 当位于中间某个位置的时候,可以往左边走或者往右边走
    return f(N, E, rest - 1, cur - 1) + f(N, E, rest - 1, cur + 1);
}
```

记忆化搜索

```js
// 缓存
const main = (N,E,S,K) => {
    let dp = new Array(K+1).fill(0).map(item => new Array(N+1));
    for(let i = 0;i <= K;i++){
        for(let j = 0;j <= N;j++){
            dp[i][j] = -1;
        }
    }
    return f2(N,E,K,S,dp);
}

const f2 = (N,E,rest,cur,dp) => {
    // 如果之前计算过该状态,就无需再进行计算
    if(dp[rest][cur] !== -1) return dp[rest][cur];
    // 如果缓存没有命中
    if(rest === 0) {
        // 存入dp格子中
        dp[rest][cur] = cur === E ? 1 : 0;
    }
    // 如果rest > 0,有路可以走
    else if (cur === 1) {
        // 存入dp格子中
        dp[rest][cur] = f(N, E, rest - 1, 2);
    }
    // 当在N位置上的时候,只能往左边(N-1)位置走
    else if (cur === N) {
        // 存入dp格子中
        dp[rest][cur] = f(N, E, rest - 1, N - 1);
    }
    else{
        // 当位于中间某个位置的时候,可以往左边走或者往右边走
        dp[rest][cur] = f(N, E, rest - 1, cur - 1) + f(N, E, rest - 1, cur + 1);
    }
    return dp[rest][cur];   
}
```

## 组成面额需要的最小硬币数

给你一个正数数组,里面有若干个整数,每个整数都代表一枚该面额的硬币,给你一个数`aim`,请计算能组成`aim`的排列方式有多少种,并计算组成`aim`需要的最少硬币数量

```js
/**
 * arr 硬币数组(都是正数,固定参数)
 * aim 最终要达到的目标,固定参数
 * 如果自由选择arr[index.....]这些硬币,但是之前的硬币已经让你拥有了pre这么多钱
 * 最终组成aim的方法数返回
 */
const mainCount = (arr,aim) => {
    return f(arr,0,0,aim);
}

const f = (arr,index,pre,aim) => {
    // 已经没有硬币可以选了
    if(index === arr.length){
        // 判断之前存入pre中硬币总数额是否等于目标数额,等于的话方法数+1,否则为0
        return pre === aim ? 1 : 0;
    }
    // 两种情况
    /**
     * 1. 将当前硬币的面额添加去
     * 2. 不添加当前硬币
     */
    return f(arr,index+1,pre,aim) + f(arr,index+1,pre+arr[index],aim);
}
```

计算组成`aim`的最小硬币书数

**方法一:**

```js
const mainCount1 = (arr,aim) => {
    return f(arr,0,0,aim);
}


// arr[index...] 组成rest这么多钱最少硬币数量
const process1 = (arr,index,rest) => {
    // 如果rest<0,无法组成,返回-1
    if(rest < 0) return -1;
    // 当rest为0时,有0中排列方式
    if(rest === 0) return 0;
    // 当rest > 0,已经没有硬币但是面额总额小于rest
    if(index === arr.length) return -1; // 当index走到最后且rest还大于0
    // 两种情况 1.包括当前硬币 2. 不包含当前硬币
    // 包含当前硬币则硬币数量+1
    return Math.min(process1(arr,index+1,rest) ,
        1 + process1(arr,index+1,rest-arr[index])); 
}
```

**方法二：**

```js
const mainCount2 = (arr,aim) => {
    let dp = new Array(arr.length+1).fill(0).map(item => new Array(aim+1).fill(-2))
    return f(arr,0,0,aim,dp);
}


// arr[index...] 组成rest这么多钱最少硬币数量
const process2 = (arr,index,rest) => {
    // 如果rest<0,无法组成,返回-1
    if(rest < 0) return -1;

    // 如果不等于0,已经进行过计算了
    if(dp[index][rest] !== -2) return dp[index][rest];

    // 当rest为0时,有0中排列方式
    if(rest === 0){
        dp[index][rest] = 0;
    }
    // 当rest > 0,已经没有硬币但是面额总额小于rest
    if(index === arr.length) {
        dp[index][rest] = -1; // 当index走到最后且rest还大于0
    }
    else{
        // 两种情况 1.包括当前硬币 2. 不包含当前硬币
        // 包含当前硬币则硬币数量+1
        let p1 = process2(arr,index+1,rest,dp);
        let p2Next = process2(arr,index+1,rest-arr[index],dp);
        if(p1 === -1 && p2Next === -1){
            dp[index][rest] = -1;
        }
        else{
            if(p1 === -1){
                dp[index][rest] = p2Next + 1;
            }
            else if(p2Next === -1){
                dp[index][rest] = p1;
            }
            else{
                dp[index][rest] = Math.min(p1,p2Next+1);
            }
        }
    }
    return dp[index][rest];
}
```

## 抽取卡牌的最好分数

给你若干张写有数字的卡牌,排成一列,A和B两个人轮流从该列卡牌中抽取,每次只能从两边取一个数字,假如A先取,B后取,计算先取和后取能拿到的最好的分数

```js
/**
 * @ 先手函数
 * @ arr 卡牌函数
 * @ i j 卡牌被抽取后的两个边界(每次只能从两边拿牌)
 */
const takeFirst = (arr,i,j) => {
    if(i === j){
        // 如果只有一张卡牌
        return arr[j];
    }
    return Math.max(
        arr[i] + tackLast(arr,i+1,j),
        arr[j] + tackLast(arr,i,j-1)
    )
}

/**
 * @ 后手函数
 */
const tackLast = (arr,i,j) => {
    if(i === j){
        // 如果只有一张卡牌,A先挑,已经拿走了,B的卡牌分数就是0
        return 0;
    }
    /**
     * 如果对手选择了i位置上的数,就轮到挑选i+1~j范围上的数了
     * 如果对手选择了j位置上的数,就轮到挑选i~j-1位置上的数
     * 因为对手一定会挑选较好的牌,因此留给后手的一定是较小的
     */
    return Math.min(
        takeFirst(arr,i+1,j),
        takeFirst(arr,i,j-1)
    )
}

/**
 * @ 判断谁能够赢得比赛的函数,返回赢得比赛的人获得的积分数
 */
const winMatch = (arr) => {
    if(!arr || arr.length === 0) return 0;
    return Math.max(
        takeFirst(arr,0,arr.length-1),
        tackLast(arr,0,arr.length-1)
    )
}
```

## 象棋

三维动态规划

在一个XY坐标轴上,X方向有9个刻度,Y方向上有10个刻度,现在有一个象棋**马**,停在(0,0)位置,现在给你3个参数,a,b表示要到达的坐标位置(必须到达(a,b)坐标),k表示必须经过的步数,问从(0,0)出发到达(a,b)的方法数(**注: 马在象棋盘上只能走"日"字**)

```js
/**
 * @ step表示必须经过的步数
 * @ x,y表示当前所在的位置
 * @ 从(0,0)处开始跳,跳了step步之后,来到(r,c)的方法数有多少种
 * @ 注: 马在象棋盘上只能走"日"字
 * 一个重现的过程,让象棋马从(x,y)处往(0,0)点跳
 * 每次能往周围的八个点位置跳
 * 如果跳出界,说明该方法不能从(0,0)跳到(x,y)
 * 如果最后跳到(0,0)点,说明该方法可以从(0,0)点跳到(x,y),方法数+1
 */
const process = (x,y,step) => {
    // 如果跳出界了
    if(x < 0 || x > 8 || y < 0 || y > 9){
        return 0; // 如果(x,y)坐标在棋盘外面
    }
    if(step === 0){
        // 如果最后剩余的步数为0步并且跳到了(0,0)位置
        // 说明该种方法可行
        return (x === 0 && y === 0) ? 1 : 0;
    }
    // 要想到达的位置不越界,也有位置可以跳,每向周围跳一步,step-1
    return process(x-1,y+2,step-1)
        + process(x-1,y-2,step-1)
        + process(x+1,y+2,step-1)
        + process(x+1,y-2,step-1)
        + process(x+2,y+1,step-1)
        + process(x+2,y-1,step-1)
        + process(x-2,y+1,step-1)
        + process(x-2,y-1,step-1);
}

/**
 * @ 主函数
 * @param {*} x 表示目标坐标的x轴位置[0-8]
 * @param {*} y 表示目标坐标的y轴位置[0-9]
 * @param {*} k 表示能跳的步数step
 * @returns 返回从(0,0)位置跳到(x,y)位置且步数为k次的方法数
 */
const main = (x,y,k) => {
    return process(x,y,k);
}
```

使用动态规划解决该问题

```js
//@ 动态规划解决该问题
const dpWays = (x,y,step) => {
    if(x < 0 || x > 8 || y < 0 || y > 9 || step < 0) return 0;
    // 创建一个9*10*(step+1)的立方体
    let dp = new Array(9).fill(0).map(
        item => new Array(10).fill(0).map(
            subItem => new Array(step+1).fill(0)
        )
    )
    dp[0][0][0] = 1;
    for(let h = 1;h <= step;h++){ // 层
        for(let r = 0;r < 9;r++){
            for(let c = 0;c < 10;c++){
                dp[r][c][h] += getValue(dp,r-1,c+2,h-1);
                dp[r][c][h] += getValue(dp,r-1,c-2,h-1);
                dp[r][c][h] += getValue(dp,r+1,c+2,h-1);
                dp[r][c][h] += getValue(dp,r+1,c-2,h-1);
                dp[r][c][h] += getValue(dp,r-2,c+1,h-1);
                dp[r][c][h] += getValue(dp,r-2,c-1,h-1);
                dp[r][c][h] += getValue(dp,r+2,c+1,h-1);
                dp[r][c][h] += getValue(dp,r+2,c-1,h-1);
            }
        }
    }
    return dp[x][y][step];
}

//@ getValue 获取dp的值
const getValue = (dp,row,col,step) => {
    if(row < 0 || row > 8 || col < 0 || col > 9){
        return 0;
    }
    return dp[row][col][step];
}

const mainDP = (x,y,step) => {
    return dpWays(x,y,step);
}
```

## 活下来的概率

给你一个N行M列的布局,(a,b)是Bob当前所在的坐标,Bob每次等概率随机往上下左右四个方向走(不能斜着走),需要走k步,如果越界,Bob会死掉,问最后Bob活下来的概率是多少?

```js
/**
 * 
 * @param {*} N 表示Bob所在区域有多少行 [0,N)
 * @param {*} M 表示Bob所在区域有多少列 [0,M)
 * @param {*} row 表示Bob当前所在行坐标
 * @param {*} col 表示Bob当前所在列坐标
 * @param {*} rest 表示剩余的步数
 * @returns 返回走rest步之后获得的生存下来的方法数
 * @ Bob往上下左右四个方向随机走rest步,方法数一共是4^rest
 */
const process = (N,M,row,col,rest) => {
    if(row < 0 || row >= N || col < 0 || col >= M){
        return 0; // 超出范围后不会生存下来
    }
    // row,col没有越界,步数已经走完
    if(rest === 0){
        return 1;
    }
    // row,col没有越界并且还没走完
    // Bob往上下左右四个方向走的概率
    return process(N,M,row-1,col,rest-1)
        + process(N,M,row+1,col,rest-1)
        + process(N,M,row,col-1,rest-1)
        + process(N,M,row,col+1,rest-1);
}

/**
 * 计算生存率的函数
 * @param {*} all Bob能走的总方法数
 * @param {*} live Bob活下来的方法数
 * @returns 返回Bob活下来的概率
 */
const gcd = (all,live) => {
    let num1 = all;
    let num2 = live;
    while(num1 % num2 !== 0){
        let temp = num1;
        num1 = num1 - num2 > num2 ? num1 - num2 : num2;
        num2 = temp - num2 > num2 ? num2 : temp - num2;
    } 
    return num2;
}

const BobLive = (N,M,i,j,k) => {
    let all = Math.pow(4,k); // 总方法数
    let live = process(N,M,i,j,k); // 活下来的方法数
    let gcdNum = gcd(all,live); // 计算二者最大公约数
    return (live / gcdNum) + '/' + (all / gcdNum);
}
```

## 零钱兑换

给你一个整数数组`coins` ，表示不同面额的硬币；以及一个整数`amount` ，表示总金额。

计算并返回可以凑成**总金额的方法数**以及**可以凑成总金额的所需的最少的硬币个数** 。如果没有任何一种硬币组合能组成总金额，返回 `-1` 

你可以认为每种硬币的数量是无限的。

计算总方法数:

```js
/**
 * @ 主函数
 * @param {number[]} coins 零钱面额数组(正整数)
 * @param {number} amount 要找零的面额
 * @returns number 返回能组成amount面额的方法数
 */
const main = (coins,amount) => {
    return exchangeCoins(coins,0,amount);
}

/**
 * @ 计算找零的方法数的函数
 * @param {number[]} coins 零钱面额数组
 * @param {number} index 可自由使用coins[index...]所有的面值
 * @param {number} amount 要找零的面额
 * @returns number 能组成amount的方法数
 */
const exchangeCoins = (coins,index,amount) => {
    // 如果已经没有面值可以选了
    if(index === coins.length){
        return amount === 0 ? 1 : 0;
    }
    // 如果还有面值可以选
    // coins[index]选择0张,1张,... 不要超过rest的值
    let ways = 0;
    for(let sheet = 0;coins[index] * sheet <= amount;sheet++){
        ways += exchangeCoins(coins,index+1,amount - (coins[index] * sheet));
    }
    return ways;
}
```

动态规划

```js
/**
 * @动态规划计算零钱兑换的方法数
 * @param {number[]} coins 
 * @param {number} amount 
 * @returns 
 */
const exchangeCoinsDP = (coins,amount) => {
    if(coins === null || coins.length === 0){
        return 0;
    }
    let N = coins.length;
    // 创建动态规划的数组dp
    let dp = new Array(N+1).fill(0).map(item => new Array(amount+1).fill(0));
    dp[N][0] = 1;
    for(let index = N-1;index >= 0;index--){
        for(let rest = 0;rest <= amount;rest++){
            let ways = 0;
            for(let sheet = 0;coins[index] * sheet <= rest;sheet++){
                ways += dp[index+1][rest - coins[index] * sheet];
            }
            dp[index][rest] = ways;
        }
    }
    return dp[0][amount]; // 返回当零钱兑换总额为amount时的方法数
}
```

























