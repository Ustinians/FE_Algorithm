# 前缀树

> `Trie`树，即字典树，又称单词查找树或键树，是一种树形结构，是一种哈希树的变种。典型应用是用于统计和排序大量的字符串（但不仅限于字符串），所以经常被搜索引擎系统用于文本词频统计。它的优点是：利用字符串的公共前缀来减少查询时间，最大限度地减少无谓的字符串比较。

核心思想: 空间换时间,利用字符串的公共前缀来降低查询时间的开销以达到提高效率的目的。

**基本属性:**

1. 根节点不包含字符，除根节点外每一个节点都只包含一个字符。
2. 从根节点到某一节点，路径上经过的字符连接起来，为该节点对应的字符串。
3. 每个节点的所有子节点包含的字符都不相同。

```js
function TrieNode(pass,end,next){
    this.pass = (pass === undefined ? 0 : pass); // 该节点被通过了多少次
    this.end = (end === undefined ? 0 : end); // 以该节点结束的单词有多少
    this.next = (next === undefined ? new Array(26).fill(0) : next);

    // 创建一棵前缀树
    this.root = new TrieNode();

    this.insert = insert();
    this.search = search();
    this.prefixNumber = prefixNumber();
    this.deleteWord = deleteWord();

    // 创建一棵前缀树
    const insert = (word) => {
        if(!word) return;
        let ans = word.split(''); // 将当前的单词转化成数组
        // 根节点的pass值即是传入前缀树的字符串的数量
        let node = this.root;// 创建一个新的TridNode类型(前缀树)
        node.pass++; // 头结点为空
        let index = 0;
        for(let i = 0;i < ans.length;i++){
            // 计算当前单词应当进入哪一棵子树
            index = ans[i].charCodeAt() - 'a'.charCodeAt(); 
            // 如果当前节点还没经过遍历,先创建一个新的节点,然后将结点被遍历过的次数加一
            if(node.next[index] === 0){
                node.next[index] = new TrieNode();
            }
            // 向下指向子树,节点遍历次数加一
            node = node.next[index];
            node.pass++;
        }
        node.node++;
    }

    // 查询word这个单词之前加入过几次
    const search = (word) => {
        if(!word) return 0;
        let ans = word.split('');
        let node = this.root;
        let index = 0;
        for(let i = 0;i < ans.length;i++){
            index = ans[i].charCodeAt() - 'a'.charCodeAt();
            if(node.next[index] === 0) return 0;
            node = node.next[index];
        }
        return node.end;
    }

    // 所有加入的字符中,有多少单词是以pre为前缀的
    const prefixNumber = (pre) => {
        if(!pre) return 0;
        let ans = pre.split('');
        let node = this.root;
        let index = 0;
        for(let i = 0;i < ans.length;i++){
            index = ans[0].charCodeAt() - 'a'.charCodeAt();
            if(node.next[index] === 0) return 0;
            node = node.next[index];
        }
        return node.pass;
    }

    // 删除某个加入的字符串
    const deleteWord = (word) => {
        // 删除word在前缀树中的遍历
        if(search(word) !== 0){
            // 确定树中曾经加入过word,才能进行删除
            let ans = word.split('');
            let node = this.root;
            let index = 0;
            node.pass--;
            for(let i = 0;i < ans.length;i++){
                index = ans[i].charCodeAt() - 'a'.charCodeAt();
                if(--node.next[index].pass === 0){
                    node.next[index] = 0;
                    return true;
                }
                node = node.nexts[index];
            }
            node.end--;
            return true;
        }
        return false;
    }

}
```























































