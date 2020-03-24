[toc]

# 题目

```
给定一个包含非负整数的 m x n 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

说明：每次只能向下或者向右移动一步。
```

```
输入:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 7
解释: 因为路径 1→3→1→1→1 的总和最小。
```

# 思路

一开始状态转移方程是这样写的，用递归实现

`f(y,x) = min(f(y - 1, x), f(y, x - 1)) + grid[y][x]`

不知道为啥总有两个case过不了，而且不是边界条件的case。

看了题解之后学会了一种新的思路，从**已知的一个元素**出发，**迭代**计算所有的最短路径

要注意必须蛇形遍历，要不会出现没有缓存过的情况

其实和递归是一样的原理



注：用原本的数组优化储存空间，因为蛇形遍历时访问的上左元素肯定是计算过之后的结果



注：比较最小值，应该用`Number.MAX_SAFE_INTEGER`

# 代码

```javascript
var minPathSum = function(grid) {
    const height = grid.length;
    const width = grid[0].length;

    for(let y = 0; y < height; y++) {
        for(let x = 0; x < width; x++) {
            if(y === 0 && x === 0) {
							// do nothing
            }else {
                const left = x === 0 ? Number.MAX_SAFE_INTEGER : grid[y][x - 1];
                const up = y === 0 ? Number.MAX_SAFE_INTEGER : grid[y - 1][x];
                grid[y][x] = (left < up ? left : up) + grid[y][x]
            }
        }
    }
    return grid[height - 1][width - 1];
};
```

# 复杂度分析

时间复杂度：`o(mn)`

空间复杂度：1

# 运行时间

这个时间复杂度是玄学，不过大概都是65ms左右，内存讲道理不应该用这么多的

![80/85](images\image-20200324235004979.png)