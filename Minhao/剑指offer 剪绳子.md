[toc]

# 题目

```
给你一根长度为n的绳子，请把绳子剪成整数长的m段（m、n都是整数，n>1并且m>1），每段绳子的长度记为k[0],k[1],...,k[m]。请问k[0]xk[1]x...xk[m]可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。
```

```
输入
8
输出
18
```

# 思路

递归，但注意递归和边界条件不能一起处理，因为题目要求**至少两段绳子**，而递归中设的`f(n)`是`n`长绳子的最优解法，有可能是**1段绳子**，比如`f(7) = f(4) + f(3)`但`f(3) = f(1) + f(2)`且`f(7) !== f(4) + f(1) + f(2)`

用哈希表缓存结果

# 代码

```javascript
function cutRope(number)
{
    switch(number) {
        case 2: return 1;
        case 3: return 2;
        case 4: return 4;
    }
    // write code here
    const cache = new Map();
    cache.set(1, 1)
    cache.set(2, 2)
    cache.set(3, 3)
    cache.set(4, 4)
    const simulateCut = (num) => {
        if(cache.has(num)) return cache.get(num);
        let max = 0;
        for(let i = 1; i < num / 2 + 1; i++) {
            const result = simulateCut(i) * simulateCut(num - i);
            max = result > max ? result : max;
        }
        cache.set(num, max)
        return max;
    }
    
    return simulateCut(number);
}

```







# 复杂度分析

时间复杂度：`O(N^2)`

空间复杂度：`O(N)`

# 运行时间

![12ms](images\image-20200324223341733.png)