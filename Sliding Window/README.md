# 滑动窗口

## **绳子覆盖结点问题**

给定一个有序数组`arr`,代表数轴上从左到右有`n`个点`arr[0],arr[1]...arr[n-1]`,给定一个正数`L`,代表一根长度为`L`的绳子,求绳子最多能覆盖其中的几个点?

**思路:**

1. 每次将绳子的末端放到数组中的一个点上面,绳子的初始端往前面的结点延申,计算能够覆盖多少个点(通过二分法计算>=一个数的最左位置)
2. **滑动窗口:** 将left,right都赋值为第一个结点,令right向右移动到绳子能覆盖的最后一个结点的位置,然后将窗口向右滑动继续判断...

```js
/**
 * 
 * @param {number[]} arr 
 * @param {number} rope 
 * @returns number 返回最多能覆盖的点数
 */
const ropeCover = (arr,rope) => {
    // 当绳子长度为0的时候,不能覆盖任何一个点,返回0
    if(rope === 0) return 0;
    // 滑动窗口的左右两端
    let left = 0,right = 0; 
    // 能覆盖的最多的节点个数
    let res = 0;
    // 当righr没有移动到最后一个结点
    while(right < arr.length){
        let count = right-left; // 计算该轮覆盖结点(先计算中间有多少个结点)
        while(arr[right]-arr[left] <= rope){ // 从当前节点往后延伸,计算后面最多能覆盖多少个结点
            count++;
            right++;
        }
        if(count > res) res = count; // 如果当前计算出的覆盖节点个数大于res中记录的,则将count赋值给res
        left++; // left右移一位(right不用动是因为中间的结点肯定在绳子的覆盖范围之内,只需要计算后面有没有新的节点被覆盖)
    }
    return res;
}
```

小虎去附近的商店买苹果，奸诈的商贩使用了捆绑交易，只提供6个每袋和8个每袋的包装包装不可拆分。可是小虎现在只想购买恰好n个苹果，小虎想购买尽量少的袋数方便携带。如果不能购买恰好n个苹果，小虎将不会购买。输入一个整数n，表示小虎想购买的个苹果，返回最小使用多少袋子。如果无论如何都不能正好装下，返回-1。

**思路:**

有题可知,当购买的苹果是奇数的时候,一定不能正好装下,返回-1

## 计算苹果的袋子数量

计算最大能使用的8个苹果的袋子数量,此即为8个苹果袋子的最大数量,如果剩余的余数不能整除6,则将8个苹果的袋子的数量-1,拿剩下的苹果数量看是否能整除6,依次计算直到找到剩余苹果数量恰好能整除6的时候,返回当前用的袋子数量,如果始终无法整除,返回-1

**当剩余的苹果数量大于24(6,8的最小公倍数)但是仍然无法整除6的时候,就不用再尝试了(和前面的情况会有重复的)**

```js
/**
 * 
 * @param {number} n 小明要买的苹果的数量 
 * @returns 返回最少需要使用的袋子的数量(如果无法刚好装下,返回-1)
 */
const getMinBags = (n) => {
    // 由题意,当n为奇数的时候,一定无法刚好装下
    if(n % 2 !== 0) return -1;
    // 如果苹果数量能整除8,也直接返回
    if(n % 8 === 0) return n / 8;
    let numEight = Math.floor(n / 8);
    let numSix = 0;
    // 当装8个苹果的袋子的数量>=0的时候
    while(numEight >= 0){
        // 如果剩余的苹果的数量能整除6
        if((n - 8 * numEight) % 6 === 0){
            numSix = (n - 8 * numEight) / 6;
            return numEight+numSix; // 返回两种苹果数量的总数
        }
        // 如果不能整除,将苹果数量为8的袋子的数量-1,计算剩余的能不能整除6
        numEight--;
    }
    return -1;
}
```

### **优化后**

```js
//@ 优化之后的(当苹果剩余数量大于24的时候,24是6和8的最小公倍数,此时能用8就没必要用6)
const getMinBags = (n) => {
    // 当购买的苹果数量小于0或者为奇数的时候,返回-1
    if(n < 0 || n % 2 !== 0) return -1;
    let numEight = Math.floor(n / 8);
    let numSix = -1;
    let rest = n - 8 * numEight;
    while(n >= 0 && rest < 24){
        let restSix = minBagBaseSix(rest);
        if(restSix !== -1){
            numSix = restSix;
            break;
        }
        rest = n - 8 * (--numEight);
    }
    return numEight === -1 ? -1 : numEight+numSix;
}

// 如果剩余的苹果数量能整除6,返回袋子数量,否则返回-1
const minBagBaseSix = (rest) => {
    return rest % 6 === 0 ? Math.floor(rest / 6) : -1;
}
```

### 打表法

对于这种传入的值是一个`int`型,返回的值也是`int`型的函数,我们可以尝试打印出其多次执行后的结果寻找规律

```
-1
1
-1
-1
-1
1
-1
1
-1
-1
-1
2
-1
2
-1
2
-1
3
-1
3
-1
3
-1
3
-1
4
-1
4
-1
4
-1
4
-1
5
-1
5
-1
5
-1
5
-1
```

由题可找出规律并写出函数

```js
//@ 打表法
const getMinBags = (n) => {
    if((n & 1) !== 0){
        // 如果是奇数
        return -1;
    }
    if(n < 18){
        return n === 0 ? 0 : (n === 6 || n === 8) ? 1
            : (n === 12 || n === 14 || n === 16) ? 2 : -1;
    }
    return Math.floor((n - 18) / 8) + 3;
}
```

## 牛羊吃草问题

假设一头牛和一头羊在同一片草地上吃草,一共有n棵草,牛先吃(先手),羊后吃(后手),二者轮流吃,每次吃草的数量只能是4的次方,

```js
const eatGrass = (n) => {
    // n份青草先放成一堆
    if(n < 5){
        return (n === 0 || n === 2) ? "后手" : "先手";
    }
    // 当n>=5时
    let base = 1; // 先手决定吃的草
    // 有问题
    while(base <= n){
        // 当前一共n份草,先手吃掉的是base份,n-base是留给后手的草
        // 母过程 先手 在子过程中式后手
        if(eatGrass(n-base) === "后手"){
            return "先手";
        }
        // 如果n特别大(接近正数的边界,那么base*4可能超过n,超过整数最大边界)
        if(base > Math.floor(n/4)){
            break; // 防止base*4之后溢出
        }
        base *= 4;
    }
    return "后手";
}
```

### 打表法

在多次执行后找到归规律,可使用打表法如下

```js
const eatGrass = (n) => {
    if(n % 5 === 0 || n % 5 === 2){
        return "后手";
    }
    else{
        return "先手";
    }
}
```

# 预处理技巧

## 涂色方案

牛牛有一些排成一行的正方形。每个正方形已经被染成红色或者绿色。牛牛现在可以选择任意一个正方形然后用这两种颜色的任意一种进行染色，这个正方形的颜色将会被覆盖。牛牛的目标是在完成染色之后，<font color="red">每个红色R都比每个绿色G距离最左侧近。</font>牛牛想知道他最少需要涂染几个正方形。

如样例所示：s＝RGRGR

我们涂染之后变成RRRGG满足要求了，涂染的个数为2，没有比这个更好的涂染方案。

**思路:**

将数组拆分成L,R两个区域,L区域全都涂成红色,R区域全都涂成绿色,计算每个方案要涂染的正方形的数量,返回最小的

green数组用于记录第`i`个结点及其前面的结点中有多少个G(`0~L`区域都染成红色)

red数组用于记录第`i`个结点及其后面的结点中有多少个R(`L~N-1`区域都染成绿色)

```js
/**
 * @ 计算最少需要涂染几个正方形
 * @param {string} tiles 瓷砖排列字符串
 * @returns {number} 返回最小需要染多少个正方形  
 */
const minPrintTest = (tiles) => {
    let ans = tiles.split(''); // 将字符串拆分成数组
    let N = ans.length;
    // 创建两个数组,分别用来统计[0...i]上G的数量和[i...N-1]上R的数量(预处理)
    // 空间换时间?
    let green = new Array(N).fill(0);
    let red = new Array(N).fill(0);
    for(let i = 0;i < N;i++){
        for(let j = 0;j <= i;j++){
            // [0...i]上面的G都需要染成R
            if(ans[j] === 'G') green[i]++;
        }
        for(let j = i;j < N;j++){
            // [i...N-1]上面的R都需要染成G
            if(ans[j] === 'R') red[i]++;
        }
    }
    let minDyeing = Number.MAX_VALUE; // 计算最小的染色正方形数量
    // 左边都是R,右边都是G
    // 枚举左侧部分的大小L,右侧部分的大小为N-L
    for(let L = 0;L <= N;L++){
        let dyeing = 0;
        if(L === 0){
            // 统计ans[0...N-1]一共有多少个R,全都染成G(右边全都是G)
            dyeing = red[0]; // 从0到N-1有多少个R(染成G)
        }
        else if(L === N){
            // 统计arr[0...N-1]一共多少个G,全部染成R(左边全都是L)
            dyeing = green[N-1]; // 从0到N-1上G的数量(染成R)
        }
        else{
            /**
             * 统计arr[...L]一共多少个G,全部染成R(左半侧全都是R)
             * 统计arr[L+1...N-1]一共多少个R,全部染成G(右半侧全都是G)
             */
            // [0...L]上G的数量+[L+1...N-1]上R的数量
            dyeing = green[L] + red[L];
        }
        if(dyeing < minDyeing) minDyeing = dyeing;
    }
    return minDyeing;
}
```

## 计算最大正方形边长

给定一个N*N的数组`matrix`,只有0和1两种值,返回边框全都是1的最大正方形的边长长度

例如

```
0 1 1 1 1
0 1 0 0 1
0 1 0 0 1
0 1 1 1 1
0 1 0 1 1
```

其中边框全都是1的最大正方形的大小为4*4,所以返回4

**思路:**

创建两个矩阵,分别记录当前节点向右和向下延申能得到的最长的1的数量(包括当前结点)

在每一个结点处依次延长border的值,判断当前子矩阵是否符合要求,如果符合要求将border的值赋值给side(记录每个结点处最大的矩阵变长)

maxSide用来记录最长的边长长度

```js
/**
 * 
 * @param {number[][]} matrix 由0和1组成的矩阵
 * @returns {number} 返回边框全都是1的正方形的最大边长
 */
const maxAllOneBorder = (matrix) => {
    // 拿到矩阵的行数和列数
    let N = matrix.length;
    let M = matrix[0].length;
    // 创建right和down两个数组,分别表示当前节点包括自身在内向右/向下延申一共有多少个连续的1(预处理)
    let right = new Array(N).fill(0).map(item => new Array(M).fill(0));
    let down = new Array(N).fill(0).map(item => new Array(M).fill(0));
    for(let i = 0;i < N;i++){
        for(let j = 0;j < M;j++){
            // 计算向右延申的结点有多少个连续的1
            for(let k = j;k < M;k++){
                if(matrix[i][k] === 0) break;
                right[i][j]++;
            }
            // 计算向下延伸的结点中有多少个连续的1
            for(let k = i;k < N;k++){
                if(matrix[k][j] === 0) break;
                down[i][j]++;
            }
        }
    }
    let maxSide = 0;
    // 左上角点的所有可能性
    for(let row = 0;row < N;row++){
        for(let col = 0;col < M;col++){
            let side = 0;
            // 枚举边长
            for(let border = 1;border <= Math.min(N-row,M-col);border++){
                // 左上角坐标是row,边长为border
                // 当border是值超过能延伸出来的最大边长
                if(right[row][col] < border || down[row][col] < border){
                    break;
                }
                // 如果向下和向右延伸出来的左下点和右上点也符合要求,将border赋值给side
                if(right[row+border-1][col] >= border && down[row][col+border-1] >= border){
                    side = border;
                }
            }
            if(side > maxSide) maxSide = side;
        }
    }
    return maxSide;
}
```

## 加工函数

给定一个函数f，可以1～5的数字等概率返回一个。请加工出1～7的数字等概率返回一个的函数g。

给定一个函数f，可以a～b的数字等概率返回一个。请加工出c～d的数字等概率返回一个的函数g。

```js
//@ 等概率返回1-5的函数f
const f = () => {
    return Math.floor(Math.random()*5) + 1;
}

//@ 等概率返回0和1的函数
const returnZeroOrOne = () => {
    let res = 0;
    // 循环直到res不为3
    do{
        res = f();
    }while(res === 3);
    // 当res=1/2的时候返回0,当res=4/5的时候返回1
    return res < 3 ? 0 : 1;
}

//@ 等概率得到1~7
const g = () => {
    let res = 0;
    // 等概率得到0~6的循环(当得到7的时候重新循环)
    do{
        // 得到0~6需要二进制三位数 _ _ _ ,可以通过returnZeroOrOne()的返回值依次向左移动拼成最后的结果
        res = (returnZeroOrOne() << 2) + (returnZeroOrOne() << 1) + returnZeroOrOne();
    }while(res === 7);
    return res+1;
}
```

给定一个函数f，以p概率返回0，以1—p概率返回1。请加工出等概率返回0和1的函数g

```js
//@ p概率返回0,1-p概率返回1
const randByP = (p) => {
    // 0 <= p <= 1
    return Math.random() < p ? 0 : 1;
}

//@ 等概率返回0,1
const randEqual = () => {
    let res1 = 0,res2 = 0;
    /**
     * 获得 0 0的概率是p^2
     * 获得 1 1的概率是(1-p)^2
     * 获得 0 1的概率是p(1-p)
     * 获得 1 0的概率是p(1-p)
     * 当获得0 1时返回0,获得1 0时返回1
     * 否则重新循环一次
     */
    do{
        res1 = randByP(0.83);
        res2 = randByP(0.83);
    }while(res1 === res2);
    return res1 === 0 && res2 === 1 ? 0 : 1;
}
```



























