---
comments: true
difficulty: Medium
rating: 1257
source: Biweekly Contest 72 Q2
tags:
    - Math
    - Simulation
---

<!-- problem:start -->

# [2177. Find Three Consecutive Integers That Sum to a Given Number](https://leetcode.com/problems/find-three-consecutive-integers-that-sum-to-a-given-number)

## Description

<!-- description:start -->

<p>Given an integer <code>num</code>, return <em>three consecutive integers (as a sorted array)</em><em> that <strong>sum</strong> to </em><code>num</code>. If <code>num</code> cannot be expressed as the sum of three consecutive integers, return<em> an <strong>empty</strong> array.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> num = 33
<strong>Output:</strong> [10,11,12]
<strong>Explanation:</strong> 33 can be expressed as 10 + 11 + 12 = 33.
10, 11, 12 are 3 consecutive integers, so we return [10, 11, 12].
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> num = 4
<strong>Output:</strong> []
<strong>Explanation:</strong> There is no way to express 4 as the sum of 3 consecutive integers.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= num &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Mathematics

Assume the three consecutive integers are $x-1$, $x$, and $x+1$. Their sum is $3x$, so $\textit{num}$ must be a multiple of $3$. If $\textit{num}$ is not a multiple of $3$, it cannot be represented as the sum of three consecutive integers, and we return an empty array. Otherwise, let $x = \frac{\textit{num}}{3}$, then $x-1$, $x$, and $x+1$ are the three consecutive integers whose sum is $\textit{num}$.

The time complexity is $O(1)$, and the space complexity is $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfThree(self, num: int) -> List[int]:
        x, mod = divmod(num, 3)
        return [] if mod else [x - 1, x, x + 1]
```

#### Java

```java
class Solution {
    public long[] sumOfThree(long num) {
        if (num % 3 != 0) {
            return new long[] {};
        }
        long x = num / 3;
        return new long[] {x - 1, x, x + 1};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> sumOfThree(long long num) {
        if (num % 3) {
            return {};
        }
        long long x = num / 3;
        return {x - 1, x, x + 1};
    }
};
```

#### Go

```go
func sumOfThree(num int64) []int64 {
	if num%3 != 0 {
		return []int64{}
	}
	x := num / 3
	return []int64{x - 1, x, x + 1}
}
```

#### TypeScript

```ts
function sumOfThree(num: number): number[] {
    if (num % 3) {
        return [];
    }
    const x = Math.floor(num / 3);
    return [x - 1, x, x + 1];
}
```

#### Rust

```rust
impl Solution {
    pub fn sum_of_three(num: i64) -> Vec<i64> {
        if num % 3 != 0 {
            return Vec::new();
        }
        let x = num / 3;
        vec![x - 1, x, x + 1]
    }
}
```

#### JavaScript

```js
/**
 * @param {number} num
 * @return {number[]}
 */
var sumOfThree = function (num) {
    if (num % 3) {
        return [];
    }
    const x = Math.floor(num / 3);
    return [x - 1, x, x + 1];
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
