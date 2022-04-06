# 二分法的详解与拓展

https://latex.codecogs.com/gif.image?\dpi{110}mid&space;=&space;\frac{left&space;&plus;&space;right}{2}\\\\mid&space;=&space;left&space;&plus;&space;\frac{rigjt&space;-&space;left}{2}\\\\mid&space;=&space;left&space;&plus;&space;(right&space;&plus;&space;left)&space;>>&space;1&space;

1. 在一个有序数组中,如何判断某个数是否存在
   
   $$
   时间复杂度: O(log_2N)
   $$
   
   ```js
   const findNumByDichotomy = (arr,num) => {
       let left = 0,right = arr.length-1;
       while(left <= right){
           let middle = (left + right) >> 1;
           if(num === arr[middle]) return true;
           else if(num < arr[middle]) right = middle-1;
           else left = middle+1;
       }
       return false;
   }
   ```

2. 在一个有序数组中,找`>=`某个数最左侧的位置
   
   ```js
   // 找大于等于num的最左边的数
   const findNumByDichotomy = (arr,num) => {
       // 先令flag为arr.length
       let left = 0,right = arr.length-1,flag = arr.length;
       while(left < right){
           let middle = (left + right) >> 1;
           if(arr[middle] >= num){
               // 如果arr[middle]>=num且middle<flag;将flag赋值为middle
               if(middle < flag){
                   flag = middle;
               }
               // right向左移
               right = middle-1;
           }
           else{
               // left向右移
               left = middle+1;
           }
       }
       return flag;
   }
   ```

3. 局部最小值问题
   
   在一个无序的数组中,任何两个相邻的数不相等,求局部最小数(`arr[i] < arr[i-1] && arr[i] < arr[i+1]`) , 要求时间复杂度小于O(n)
   
   ```js
   // 求局部最小
   const findNumByDichotomy = (arr) => {
       // 如果第一个数就是最小值
       if(arr[0] < arr[1]) return 0;
       let left = 0,right = arr.length-1;
       while(left <= right){
          let middle = (left + right) >> 1;
          // 当该节点的值小于左右节点,返回
          if(arr[middle] < arr[middle-1] && arr[middle] < arr[middle+1]){
               return middle;
           }
           else if(arr[middle] > arr[middle-1]){
               right = middle - 1;
           }
           else{
               left = middle + 1;
           }
       }
   }
   ```
   
   
