设计一种结构,在该结构中有如下三个功能

```js
insert(key) : 将某个key加入到该结构中,做到不重复加入
delete(key) : 将某个原本在结构中的key移除
getRandom() : 等概率随机返回结构中的任何一个key
```

**要求:**

`insert`,`delete`,`getRandom`方法的时间复杂度都是`O(1)`

```js
/**
 * keyIndexMap key为字符串,value为索引的哈希表
 * indexKeyMap key为索引,value为字符串的哈希表
 */
function RandonPoll(){
    this.keyIndexMap = new Map(); // key为字符串,value为索引的哈希表
    this.indexKeyMap = new Map(); // key为索引,value为字符串的哈希表
    this.size = 0;

    // 添加进哈希表
    const insertKey = (key) => {
        if(!this.keyIndexMap.has(key)){
            // 如果哈希表中没有此值
            this.keyIndexMap.set(key,size);
            this.indexKeyMap.set(size,key); 
            this.key++;
        }
    }

    // 从哈希表中删除
    const deleteKey = (key) => {
        if(this.keyIndexMap.has(key)){
            // 如果哈希表中存储有此值
            let deleteIndex = this.indexKeyMap.get(key); // 获取要删除字符串的索引
            let lastIndex = --this.size; // 获得最后一个索引
            let lastKey = this.indexKeyMap.get(lastIndex); // 获取最后一个节点
            
            this.keyIndexMap.set(lastKey,deleteIndex); // 将最后一个节点的索引修改为当前要删除节点的索引
            this.indexKeyMap.set(deleteIndex,lastKey); // 将索引哈希表中索引值指向的节点修改为最后一个

            this.keyIndexMap.delete(key); // 删除当前节点在两个哈希表中的值
            this.indexKeyMap.delete(lastIndex); // 删除最后一个索引 
        }
    }
}
```



































































