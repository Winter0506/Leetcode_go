### BFS&DFS

####  DFS

##### [剑指 Offer 13. 机器人的运动范围](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)

跟下面的题思路一样 都按照一个模板来写

// 1. 循环 -->dfs   2. 受限条件  3.进去dfs

```
func movingCount(m int, n int, k int) int {
    // 定义位置  不能被重复使用
    pos := make([][]int, m)
    for i := range pos{
        pos[i] = make([]int, n)
    }
    // 使用dfs
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            return dfs(m, n, i, j, k, pos)
        }
    }
    return 0
}

func dfs(m, n, i, j, k int, pos [][]int) int {
    if i < 0 || j <0 || i == m || j == n || pos[i][j] == 1 || compute(i, j) > k {
        return 0
    }
    // 满足条件
    pos[i][j] = 1
    sum := 1
    sum += dfs(m, n, i+1, j, k, pos)
    sum += dfs(m, n, i-1, j, k, pos)
    sum += dfs(m, n, i, j+1, k, pos)
    sum += dfs(m, n, i, j-1, k, pos)
    return sum
}

func compute(i, j int) int {
    return i % 10 + i / 10 + j % 10 + j / 10 
}
```



##### [剑指 Offer 12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)

```
func exist(board [][]byte, word string) bool {
    m, n := len(board), len(board[0])
    for i := 0; i < m; i++ {
        for j := 0; j < n; j++ {
            // 从第0个数开始找 找到就返回true
            if dfs(board, i, j, 0, word){
                return true
            }
        }
    }
    return false
}

func dfs(board [][]byte, i, j, k int, word string) bool {
    // 找到了最后 都找到了 返回true
    // k值的判断，最后判断是k == len(word)而不是k == len(word) - 1,
    // 因为无论如何，会进入深度搜索，导致k++一次，即使word中只有一个字符word[0],k也会变成1
    // 最后k增长至长度时候 才会判断下面的语句
    if k == len(word) {
        return true
    }
    // 约束条件
    if i < 0 || j < 0 || i == len(board) || j == len(board[0]) {
        return false
    }
    if board[i][j] == word[k] {
        temp := board[i][j]
        board[i][j] = '#'
        if dfs(board, i+1, j, k+1, word) ||
           dfs(board, i-1, j, k+1, word) ||
           dfs(board, i, j-1, k+1, word) ||
           dfs(board, i, j+1, k+1, word) {
               return true
           } else{
               board[i][j] = temp // 还原 从别的路径去找
           }
    }
    return false
}
```



##### [102. 二叉树的层序遍历](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

DFS解法

```go
var res [][]int

func levelOrder(root *TreeNode) [][]int {
    if root == nil{
        return nil
    }
    res = make([][]int, 0)
    dfs(root, 0)
    return res
}

func dfs(root *TreeNode, level int){
    if root == nil{
        return
    }
    // 这句话的意思时，进入下一次循环后，
    // 首先为这一层添加一个空的切片
    if level == len(res){
        res = append(res, []int{})
    }
    res[level] = append(res[level], root.Val)
    dfs(root.Left, level+1)
    dfs(root.Right, level+1)
}

func findLargest(n *TreeNode, level int) {
	if n == nil {
		return
	}

	// !核心在这四行代码
	if len(r) == level {
		r = append(r, math.MinInt64)
	}
	if n.Val > r[level] {
		r[level] = n.Val
	}

	findLargest(n.Left, level+1)
	findLargest(n.Right, level+1)
}

作者：suz1
链接：https://leetcode-cn.com/problems/find-largest-value-in-each-tree-row/solution/he-xin-zai-si-xing-dai-ma-by-suz1-x3jp/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

还有BFS解法，有空再做

##### [515. 在每个树行中找最大值](https://leetcode-cn.com/problems/find-largest-value-in-each-tree-row/)

相似的思路

```go
var res []int 
func largestValues(root *TreeNode) []int {
    if root == nil{
        return nil
    }
    res = make([]int, 0)
    dfs(root, 0)
    return res
}

func dfs(root *TreeNode, level int){
    if root == nil{
        return
    }
    // 先添加一个预设的最小值
    if len(res) == level{
        res = append(res, math.MinInt64)
    }
    if root.Val > res[level]{
        res[level] = root.Val
    }
    dfs(root.Left, level+1)
    dfs(root.Right, level+1)
}
```

##### [200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)

使用dfs，注意一点，把遍历过的岛屿置为0

这个地方为什么传值也能修改呢？

```go
func numIslands(grid [][]byte) int {
    num := 0        
    for i:=0; i<len(grid); i++{
        for j:=0; j<len(grid[0]); j++{
            if grid[i][j] == '1'{
                num++
                dfs(i,j, grid)
            }
        }
    }
    return num
}

func dfs(i, j int, grid [][]byte){
    if(i>=0&&i<len(grid)&&j>=0&&j<len(grid[0])&&grid[i][j]=='1'){
        grid[i][j] = 0
        dfs(i,j+1, grid)
        dfs(i,j-1, grid)
        dfs(i+1,j, grid)
        dfs(i-1,j, grid)
    }
}
```



