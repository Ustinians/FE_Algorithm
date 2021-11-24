### JS Map对象

> Map是一组键值对结构(key-value),具有极快的查找速度

例 :

存储学生的姓名跟成绩,并使用姓名查询成绩,如果使用数组,需要

```js
const names = ["Ann","Jack","Jerry"];
const scores = [99,97,98];
```

当给定一个名字查询成绩时,需要先从names中查询名字的下标,然后查询对应的成绩,此时,names越长,姓名排的越靠后面,耗时越长

如果使用Mao实现,则只需要创建一个"姓名"

-"成绩"的结构,便可以快速查询

```js
const m = new Map(["Ann",99],["Jack",97],["Jerry",98]);
m.get("Ann"); // 99
```

初始化Map需要一个二维数组,或者直接初始化一个空的Map,然后向其中添加数据

```js
const m1 = new Map(["Ann",99],["Jack",97],["Jerry",98]);
const m2 = new Map();
m2.set(["Elsa",96]); // 向Map实例中添加数据
m2.set(["Tom",94]);
m2.get("Elsa"); // 获取键名为"Elsa"的数据
m2.has("Tom"); // 查询m2中是否有键名为"Tom"的数据,有返回true,没有返回false
```

! 一个key只能对应一个value,如果对一个key进行多次赋值,则后面的value值会替换掉前面的

```js
const m = new Map();
m.set("GY",99);
m.set("GY",100);
m.get("GY"); // 100
```

参考 : https://juejin.cn/post/6985033972531068942



























