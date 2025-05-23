---
comments: true
difficulty: Medium
tags:
    - Recursion
    - Array
    - Math
    - Dynamic Programming
    - Game Theory
---

<!-- problem:start -->

# [486. Predict the Winner](https://leetcode.com/problems/predict-the-winner)

## Description

<!-- description:start -->

<p>You are given an integer array <code>nums</code>. Two players are playing a game with this array: player 1 and player 2.</p>

<p>Player 1 and player 2 take turns, with player 1 starting first. Both players start the game with a score of <code>0</code>. At each turn, the player takes one of the numbers from either end of the array (i.e., <code>nums[0]</code> or <code>nums[nums.length - 1]</code>) which reduces the size of the array by <code>1</code>. The player adds the chosen number to their score. The game ends when there are no more elements in the array.</p>

<p>Return <code>true</code> if Player 1 can win the game. If the scores of both players are equal, then player 1 is still the winner, and you should also return <code>true</code>. You may assume that both players are playing optimally.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,5,2]
<strong>Output:</strong> false
<strong>Explanation:</strong> Initially, player 1 can choose between 1 and 2. 
If he chooses 2 (or 1), then player 2 can choose from 1 (or 2) and 5. If player 2 chooses 5, then player 1 will be left with 1 (or 2). 
So, final score of player 1 is 1 + 2 = 3, and player 2 is 5. 
Hence, player 1 will never be the winner and you need to return false.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,5,233,7]
<strong>Output:</strong> true
<strong>Explanation:</strong> Player 1 first chooses 1. Then player 2 has to choose between 5 and 7. No matter which number player 2 choose, player 1 can choose 233.
Finally, player 1 has more score (234) than player 2 (12), so you need to return True representing player1 can win.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 20</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def PredictTheWinner(self, nums: List[int]) -> bool:
        @cache
        def dfs(i: int, j: int) -> int:
            if i > j:
                return 0
            return max(nums[i] - dfs(i + 1, j), nums[j] - dfs(i, j - 1))

        return dfs(0, len(nums) - 1) >= 0
```

#### Java

```java
class Solution {
    private int[] nums;
    private int[][] f;

    public boolean PredictTheWinner(int[] nums) {
        this.nums = nums;
        int n = nums.length;
        f = new int[n][n];
        return dfs(0, n - 1) >= 0;
    }

    private int dfs(int i, int j) {
        if (i > j) {
            return 0;
        }
        if (f[i][j] != 0) {
            return f[i][j];
        }
        return f[i][j] = Math.max(nums[i] - dfs(i + 1, j), nums[j] - dfs(i, j - 1));
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool PredictTheWinner(vector<int>& nums) {
        int n = nums.size();
        int f[n][n];
        memset(f, 0, sizeof(f));
        function<int(int, int)> dfs = [&](int i, int j) -> int {
            if (i > j) {
                return 0;
            }
            if (f[i][j]) {
                return f[i][j];
            }
            return f[i][j] = max(nums[i] - dfs(i + 1, j), nums[j] - dfs(i, j - 1));
        };
        return dfs(0, n - 1) >= 0;
    }
};
```

#### Go

```go
func PredictTheWinner(nums []int) bool {
	n := len(nums)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, n)
	}
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if i > j {
			return 0
		}
		if f[i][j] == 0 {
			f[i][j] = max(nums[i]-dfs(i+1, j), nums[j]-dfs(i, j-1))
		}
		return f[i][j]
	}
	return dfs(0, n-1) >= 0
}
```

#### TypeScript

```ts
function PredictTheWinner(nums: number[]): boolean {
    const n = nums.length;
    const f: number[][] = new Array(n).fill(0).map(() => new Array(n).fill(0));
    const dfs = (i: number, j: number): number => {
        if (i > j) {
            return 0;
        }
        if (f[i][j] === 0) {
            f[i][j] = Math.max(nums[i] - dfs(i + 1, j), nums[j] - dfs(i, j - 1));
        }
        return f[i][j];
    };
    return dfs(0, n - 1) >= 0;
}
```

#### Rust

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn predict_the_winner(nums: Vec<i32>) -> bool {
        let n = nums.len();
        let mut dp: Vec<Vec<i32>> = vec![vec![0; n]; n];

        // Initialize the dp vector
        for i in 0..n {
            dp[i][i] = nums[i];
        }

        // Begin the dp process
        for i in (0..n - 1).rev() {
            for j in i + 1..n {
                dp[i][j] = std::cmp::max(
                    // Take i-th num
                    nums[i] - dp[i + 1][j],
                    // Take j-th num
                    nums[j] - dp[i][j - 1],
                );
            }
        }

        dp[0][n - 1] >= 0
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def PredictTheWinner(self, nums: List[int]) -> bool:
        n = len(nums)
        f = [[0] * n for _ in range(n)]
        for i, x in enumerate(nums):
            f[i][i] = x
        for i in range(n - 2, -1, -1):
            for j in range(i + 1, n):
                f[i][j] = max(nums[i] - f[i + 1][j], nums[j] - f[i][j - 1])
        return f[0][n - 1] >= 0
```

#### Java

```java
class Solution {
    public boolean PredictTheWinner(int[] nums) {
        int n = nums.length;
        int[][] f = new int[n][n];
        for (int i = 0; i < n; ++i) {
            f[i][i] = nums[i];
        }
        for (int i = n - 2; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                f[i][j] = Math.max(nums[i] - f[i + 1][j], nums[j] - f[i][j - 1]);
            }
        }
        return f[0][n - 1] >= 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool PredictTheWinner(vector<int>& nums) {
        int n = nums.size();
        int f[n][n];
        memset(f, 0, sizeof(f));
        for (int i = 0; i < n; ++i) {
            f[i][i] = nums[i];
        }
        for (int i = n - 2; ~i; --i) {
            for (int j = i + 1; j < n; ++j) {
                f[i][j] = max(nums[i] - f[i + 1][j], nums[j] - f[i][j - 1]);
            }
        }
        return f[0][n - 1] >= 0;
    }
};
```

#### Go

```go
func PredictTheWinner(nums []int) bool {
	n := len(nums)
	f := make([][]int, n)
	for i, x := range nums {
		f[i] = make([]int, n)
		f[i][i] = x
	}
	for i := n - 2; i >= 0; i-- {
		for j := i + 1; j < n; j++ {
			f[i][j] = max(nums[i]-f[i+1][j], nums[j]-f[i][j-1])
		}
	}
	return f[0][n-1] >= 0
}
```

#### TypeScript

```ts
function PredictTheWinner(nums: number[]): boolean {
    const n = nums.length;
    const f: number[][] = new Array(n).fill(0).map(() => new Array(n).fill(0));
    for (let i = 0; i < n; ++i) {
        f[i][i] = nums[i];
    }
    for (let i = n - 2; i >= 0; --i) {
        for (let j = i + 1; j < n; ++j) {
            f[i][j] = Math.max(nums[i] - f[i + 1][j], nums[j] - f[i][j - 1]);
        }
    }
    return f[0][n - 1] >= 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
