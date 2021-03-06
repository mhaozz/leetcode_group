[toc]

# 题目

```
如果一个数列至少有三个元素，并且任意两个相邻元素之差相同，则称该数列为等差数列。

例如，以下数列为等差数列:

1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9
以下数列不是等差数列。

1, 1, 2, 5, 7
 

数组 A 包含 N 个数，且索引从0开始。数组 A 的一个子数组划分为数组 (P, Q)，P 与 Q 是整数且满足 0<=P<Q<N 。

如果满足以下条件，则称子数组(P, Q)为等差数组：

元素 A[P], A[p + 1], ..., A[Q - 1], A[Q] 是等差的。并且 P + 1 < Q 。

函数要返回数组 A 中所有为等差数组的子数组个数。
```

```
示例:

A = [1, 2, 3, 4]

返回: 3, A 中有三个子等差数组: [1, 2, 3], [2, 3, 4] 以及自身 [1, 2, 3, 4]。
```

# 思路

纪念第一个做出来的dp

由于做的是dp分类，所以直接考虑dp解法

设`f(n)`是以第`n`个元素为末位元素的数列中等差数列的个数

则`f(n) = f(n - 1) + [由于加上第n个元素导致增加的等差数列数量]k`

假设等差数列一直没断开，则有

`k = [等差数列长度] - 2`

假设中间断开了，则有

`k = [以第n个元素为末位元素的等差数列长度] - 2`

举个例子

```
[1, 2, 3]  // 个数为1
[1, 2, 3, 4] //个数为pre + 4 - 2 = 3
[1, 2, 3, 4, 5] // 个数为pre  + 5 - 2 = 3 + 3 = 6
[1, 2, 3, 4, 5, 7] // 个数为pre + 2 - 2 = 6 + 0 = 6
[1, 2, 3, 4, 5, 7, 9] // 个数为pre + 3 - 2 = 6 + 1 = 7
```

所以重点就是维护一个`以第n个元素为末位元素的等差数列长度`，连上长度加1，连不上长度重置为2.

# 代码

```javascript
var numberOfArithmeticSlices = function(A) {
    const {length} = A;
    if(length < 3) return 0;
  	// 缓存
    const cache = new Map();
    cache.set(0, 0);
    cache.set(1, 0);

		// 目前以当前元素为末位元素的等差数列的长度
    let j = 2;
    for(let i = 2; i < length; i++) {
        const pre = cache.get(i - 1);
      	// 如果当前元素能和之前的数列连成等差数列
        if(A[i] - A[i - 1] === A[i - 1] - A[i - 2]) {
            j += 1;
        }else {
        // 如果连不上，重置等差数列（为什么重置成2？因为i - 1与i - 2是基准，所以这个
        // 子等差数列长度从一开始就是2）
            j = 2;
        }
        cache.set(i, pre + j - 2);
    }
    return cache.get(A.length - 1)
};
```

空间优化：利用原数组优化空间，因为观察发现数组中只用到i - 2到i，所以用i-2之前的元素储存pre

```javascript
var numberOfArithmeticSlices = function(A) {
    const {length} = A;
    if(length < 3) return 0;

    let j = 2, i = 2;
    while(i < length) {
        const pre = A[i - 3] === undefined ? 0 : A[i - 3];
        const linked = A[i] - A[i - 1] === A[i - 1] - A[i -2];
        j = linked ? (j + 1) : 2;
        A[i - 2] = pre + j - 2;
        i += 1
    }
    return A[i - 3]
};
```

# 复杂度分析

时间复杂度：`O(N)`

空间复杂度：`O(N)`优化后`O(1)`

# 运行时间

![image-20200325115454547](images\image-20200325115454547.png)

优化后，真是玄学

![image-20200325121025540](images\image-20200325121025540.png)