## 807. Max Increase To Keep City Skyline

In a 2 dimensional array grid, each value `grid[i][j]` represents the height of a building located there. We are allowed to increase the height of any number of buildings, by any amount (the amounts can be different for different buildings). Height 0 is considered to be a building as well. 

At the end, the "skyline" when viewed from all four directions of the grid, i.e. top, bottom, left, and right, must be the same as the skyline of the original grid. A city's skyline is the outer contour of the rectangles formed by all the buildings when viewed from a distance. See the following example.

What is the maximum total sum that the height of the buildings can be increased?

```
Example:
Input: grid = [[3,0,8,4],[2,4,5,7],[9,2,6,3],[0,3,1,0]]
Output: 35
Explanation: 
The grid is:
[ [3, 0, 8, 4], 
  [2, 4, 5, 7],
  [9, 2, 6, 3],
  [0, 3, 1, 0] ]

The skyline viewed from top or bottom is: [9, 4, 8, 7]
The skyline viewed from left or right is: [8, 7, 9, 3]

The grid after increasing the height of buildings without affecting skylines is:

gridNew = [ [8, 4, 8, 7],
            [7, 4, 7, 7],
            [9, 4, 8, 7],
            [3, 3, 3, 3] ]

```

**Note:**

* `1 < grid.length = grid[0].length <= 50`
* All heights `grid[i][j]` are in the range `[0, 100]`.
* All buildings in `grid[i][j]` occupy the entire grid cell: that is, they are a `1 x 1 x grid[i][j]` rectangular prism

## Java Solution

The skyline looking from the top is `col_maxes = [max(column_0), max(column_1), ...]`. Similarly, the skyline from the left is row_maxes `[max(row_0), max(row_1), ...]`

In particular, each building `grid[r][c]` could become height `min(max(row_r), max(col_c))`, and this is the largest such height. If it were larger, say `grid[r][c] > max(row_r)`, then the part of the skyline row_maxes = `[..., max(row_r), ...]` would change.

These increases are also independent (none of them change the skyline), so we can perform them independently.

```
public int maxIncreaseKeepingSkyline(int[][] grid) {
    int N = grid.length;
    int[] rowMaxes = new int[N];
    int[] colMaxes = new int[N];

    for (int r = 0; r < N; ++r)
        for (int c = 0; c < N; ++c) {
            rowMaxes[r] = Math.max(rowMaxes[r], grid[r][c]);
            colMaxes[c] = Math.max(colMaxes[c], grid[r][c]);
    }

    int result = 0;
    for (int r = 0; r < N; ++r)
        for (int c = 0; c < N; ++c)
            result += Math.min(rowMaxes[r], colMaxes[c]) - grid[r][c];

    return result;
}
```

