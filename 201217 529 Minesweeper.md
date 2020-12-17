# 201217 529 Minesweeper

Let's play the minesweeper game ([Wikipedia](https://en.wikipedia.org/wiki/Minesweeper_(video_game)), [online game](http://minesweeperonline.com/))!

You are given a 2D char matrix representing the game board. **'M'** represents an **unrevealed** mine, **'E'** represents an **unrevealed** empty square, **'B'** represents a **revealed** blank square that has no adjacent (above, below, left, right, and all 4 diagonals) mines, **digit** ('1' to '8') represents how many mines are adjacent to this **revealed** square, and finally **'X'** represents a **revealed** mine.

Now given the next click position (row and column indices) among all the **unrevealed** squares ('M' or 'E'), return the board after revealing this position according to the following rules:

1. If a mine ('M') is revealed, then the game is over - change it to **'X'**.
2. If an empty square ('E') with **no adjacent mines** is revealed, then change it to revealed blank ('B') and all of its adjacent **unrevealed** squares should be revealed recursively.
3. If an empty square ('E') with **at least one adjacent mine** is revealed, then change it to a digit ('1' to '8') representing the number of adjacent mines.
4. Return the board when no more squares will be revealed.

 

**Example 1:**

```
Input: 

[['E', 'E', 'E', 'E', 'E'],
 ['E', 'E', 'M', 'E', 'E'],
 ['E', 'E', 'E', 'E', 'E'],
 ['E', 'E', 'E', 'E', 'E']]

Click : [3,0]

Output: 

[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'M', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]

Explanation:
```

**Example 2:**

```
Input: 

[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'M', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]

Click : [1,2]

Output: 

[['B', '1', 'E', '1', 'B'],
 ['B', '1', 'X', '1', 'B'],
 ['B', '1', '1', '1', 'B'],
 ['B', 'B', 'B', 'B', 'B']]

Explanation:
```

 

**Note:**

1. The range of the input matrix's height and width is [1,50].
2. The click position will only be an unrevealed square ('M' or 'E'), which also means the input board contains at least one clickable square.
3. The input board won't be a stage when game is over (some mines have been revealed).
4. For simplicity, not mentioned rules should be ignored in this problem. For example, you **don't** need to reveal all the unrevealed mines when the game is over, consider any cases that you will win the game or flag any squares.



## 201217 Code

```python
from collections import deque

class Solution:
    def check(selfs, x, y, n, m, di, dj, visited):
        for k in range(8):
            nx, ny = x + di[k], y + dj[k]
            if 0 <= nx < n and 0 <= ny < m:
                visited[nx][ny] += 1

    def dfs(self, board, x, y, n, m, di, dj, visited):
        q = deque()
        q.append((x, y))
        while q:
            x_, y_ = q.pop()
            for k in range(8):
                nx, ny = x_ + di[k], y_ + dj[k]
                if 0 <= nx < n and 0 <= ny < m and board[nx][ny] == 'E':
                    if visited[nx][ny]:
                        board[nx][ny] = str(visited[nx][ny])
                    else:
                        q.append((nx, ny))
            board[x_][y_] = 'B'
        return

    def updateBoard(self, board, click):
        x, y = click
        if board[x][y] == 'M':
            board[x][y] = 'X'
            return board

        n, m = len(board), len(board[0])
        di = [-1, -1, 0, 1, 1, 1, 0, -1]
        dj = [0, 1, 1, 1, 0, -1, -1, -1]
        visited = [[0] * m for _ in range(n)]
        for i in range(n):
            for j in range(m):
                if board[i][j] == 'M':
                    self.check(i, j, n, m, di, dj, visited)
        if board[x][y] == 'E' and not visited[x][y]:
            self.dfs(board, x, y, n, m, di, dj, visited)
        else:
            board[x][y] = str(visited[x][y])
        return board
```

