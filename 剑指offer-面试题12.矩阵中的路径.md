面试题12. 矩阵中的路径
https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/
请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。

[["a","b","c","e"],
["s","f","c","s"],
["a","d","e","e"]]

但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

 

示例 1：

输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
示例 2：

输入：board = [["a","b"],["c","d"]], word = "abcd"
输出：false
提示：

1 <= board.length <= 200
1 <= board[i].length <= 200
注意：本题与主站 79 题相同：https://leetcode-cn.com/problems/word-search/

```

//别人的代码未看未改
/*
 * @lc app=leetcode id=79 lang=javascript
 *
 * [79] Word Search
 */
function DFS(board, row, col, rows, cols, word, cur) {
  // 边界检查
  if (row >= rows || row < 0) return false;
  if (col >= cols || col < 0) return false;

  const item = board[row][col];

  if (item !== word[cur]) return false;

  if (cur + 1 === word.length) return true;

  // If use HashMap keep track visited letters, then need manual clear HashMap for each backtrack which needs extra space.
  // here we use a little trick
  board[row][col] = null;

  // UP, DOWN, LEFT, RIGHT
  const res =
    DFS(board, row + 1, col, rows, cols, word, cur + 1) ||
    DFS(board, row - 1, col, rows, cols, word, cur + 1) ||
    DFS(board, row, col - 1, rows, cols, word, cur + 1) ||
    DFS(board, row, col + 1, rows, cols, word, cur + 1);

  board[row][col] = item;

  return res;
}
/**
 * @param {character[][]} board
 * @param {string} word
 * @return {boolean}
 */
var exist = function(board, word) {
  if (word.length === 0) return true;
  if (board.length === 0) return false;

  const rows = board.length;
  const cols = board[0].length;

  for (let i = 0; i < rows; i++) {
    for (let j = 0; j < cols; j++) {
      const hit = DFS(board, i, j, rows, cols, word, 0);
      if (hit) return true;
    }
  }
  return false;
};
```



##总结BFS（广度优先）和DFS（深度优先搜索）

DFS是一条链一条链的搜索，而BFS是逐层进行搜索，这是他俩一个很大的区别

DFS一般用树。（栈）后进先出

BFS一般用队列。先进先出。搜索时首先将初始状态添加到队列里，此后从队列的最前端不断取出状态，把从该状态而可以转移到的状态（这些状态还未被访问）加入到队列里，如此重复，直到队列被取空或找到了问题的解。

BFS最经典的是迷宫问题：定义一个二维数组，它表示一个迷宫，其中的1表示墙壁，0表示可以走的路，只能横着走或竖着走，不能斜着走，要求编程序找出从左上角到右下角的最短路线。

广搜常用于找单一的最短路线，或者是规模小的路径搜索，它的特点是”搜到就是最优解”，

深搜用于找多个解或者是”步数已知（好比3步就必需达到前提）”的标题，它的空间效率高，然则找到的不必定是最优解，必需记实并完成全数搜索，故一般情况下，深搜需要很是高效的剪枝（优化）.

优缺点：

BFS:对于解决最短或最少问题特别有效，而且寻找深度小，但缺点是内存耗费量大（需要开大量的数组单元用来存储状态）。

DFS：对于解决遍历和求所有问题有效，对于问题搜索深度小的时候处理速度迅速，然而在深度很大的情况下效率不高
