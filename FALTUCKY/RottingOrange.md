#### 题目
在给定的网格中，每个单元格可以有以下三个值之一：

值 0 代表空单元格；
值 1 代表新鲜橘子；
值 2 代表腐烂的橘子。
每分钟，任何与腐烂的橘子（在 4 个正方向上）相邻的新鲜橘子都会腐烂。

返回直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 -1。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/rotting-oranges

#### 思路
多源BFS  
开始将所有烂橘子入队，之后循环遍历访问， 同时用一个二维数组来标记是否访问并记录下腐烂时间, 最后返回腐烂时间最大值。

#### 代码
```java
class Solution {
  public int orangesRotting(int[][] grid) {
        int rowMax = grid.length;
        int colMax = grid[0].length;
        int[][] visit = new int[rowMax][colMax];
        int[] X = new int[]{0, 1, 0, -1};
        int[] Y = new int[]{1, 0, -1, 0};

        LinkedList<Pair<Integer, Integer>> queue = new LinkedList<>();

        for (int i = 0; i < rowMax; i++) {
            for (int j = 0; j < colMax; j++) {
                visit[i][j] = -1;
                if (grid[i][j] == 2) {
                    queue.addLast(new Pair<>(i, j));
                    visit[i][j] = 0;
                }
            }
        }

        while (!queue.isEmpty()) {
            Pair<Integer, Integer> pair = queue.pop();
            for (int i = 0; i < 4; i++) {
                int row = pair.getKey() + X[i];
                int col = pair.getValue() + Y[i];
                if (row >= 0 && row < rowMax && col >= 0 && col < colMax && visit[row][col] == -1 && grid[row][col] == 1) {
                    visit[row][col] = visit[pair.getKey()][pair.getValue()] + 1;
                    grid[row][col] = 2;
                    queue.addLast(new Pair<>(row, col));
                }
            }
        }

        int result = 0;
        for (int i = 0; i < rowMax; i++) {
            for (int j = 0; j < colMax; j++) {
                result = result < visit[i][j] ? visit[i][j] : result;
                if (visit[i][j] == -1 && grid[i][j] == 1) {
                    return -1;
                }
            }
        }
        return result;
    }
}
```
#### 复杂度
- 时间复杂度：O(nm)
- 空间复杂度：O(nm)