---
comments: true
difficulty: Medium
tags:
    - Math
    - String
    - Simulation
---

<!-- problem:start -->

# [43. Multiply Strings](https://leetcode.com/problems/multiply-strings)

## Description

<!-- description:start -->

<p>Given two non-negative integers <code>num1</code> and <code>num2</code> represented as strings, return the product of <code>num1</code> and <code>num2</code>, also represented as a string.</p>

<p><strong>Note:</strong>&nbsp;You must not use any built-in BigInteger library or convert the inputs to integer directly.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<pre><strong>Input:</strong> num1 = "2", num2 = "3"
<strong>Output:</strong> "6"
</pre><p><strong class="example">Example 2:</strong></p>
<pre><strong>Input:</strong> num1 = "123", num2 = "456"
<strong>Output:</strong> "56088"
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= num1.length, num2.length &lt;= 200</code></li>
	<li><code>num1</code> and <code>num2</code> consist of digits only.</li>
	<li>Both <code>num1</code> and <code>num2</code>&nbsp;do not contain any leading zero, except the number <code>0</code> itself.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Simulating Mathematical Multiplication

Assume the lengths of $num1$ and $num2$ are $m$ and $n$ respectively, then the length of their product can be at most $m + n$.

The proof is as follows:

-   If $num1$ and $num2$ both take the minimum value, then their product is ${10}^{m - 1} \times {10}^{n - 1} = {10}^{m + n - 2}$, with a length of $m + n - 1$.
-   If $num1$ and $num2$ both take the maximum value, then their product is $({10}^m - 1) \times ({10}^n - 1) = {10}^{m + n} - {10}^m - {10}^n + 1$, with a length of $m + n$.

Therefore, we can apply for an array of length $m + n$ to store each digit of the product.

From the least significant digit to the most significant digit, we calculate each digit of the product in turn, and finally convert the array into a string.

Note to check whether the most significant digit is $0$, if it is, remove it.

The time complexity is $O(m \times n)$, and the space complexity is $O(m + n)$. Here, $m$ and $n$ are the lengths of $num1$ and $num2$ respectively.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def multiply(self, num1: str, num2: str) -> str:
        if num1 == "0" or num2 == "0":
            return "0"
        m, n = len(num1), len(num2)
        arr = [0] * (m + n)
        for i in range(m - 1, -1, -1):
            a = int(num1[i])
            for j in range(n - 1, -1, -1):
                b = int(num2[j])
                arr[i + j + 1] += a * b
        for i in range(m + n - 1, 0, -1):
            arr[i - 1] += arr[i] // 10
            arr[i] %= 10
        i = 0 if arr[0] else 1
        return "".join(str(x) for x in arr[i:])
```

#### Java

```java
class Solution {
    public String multiply(String num1, String num2) {
        if ("0".equals(num1) || "0".equals(num2)) {
            return "0";
        }
        int m = num1.length(), n = num2.length();
        int[] arr = new int[m + n];
        for (int i = m - 1; i >= 0; --i) {
            int a = num1.charAt(i) - '0';
            for (int j = n - 1; j >= 0; --j) {
                int b = num2.charAt(j) - '0';
                arr[i + j + 1] += a * b;
            }
        }
        for (int i = arr.length - 1; i > 0; --i) {
            arr[i - 1] += arr[i] / 10;
            arr[i] %= 10;
        }
        int i = arr[0] == 0 ? 1 : 0;
        StringBuilder ans = new StringBuilder();
        for (; i < arr.length; ++i) {
            ans.append(arr[i]);
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string multiply(string num1, string num2) {
        if (num1 == "0" || num2 == "0") {
            return "0";
        }
        int m = num1.size(), n = num2.size();
        vector<int> arr(m + n);
        for (int i = m - 1; i >= 0; --i) {
            int a = num1[i] - '0';
            for (int j = n - 1; j >= 0; --j) {
                int b = num2[j] - '0';
                arr[i + j + 1] += a * b;
            }
        }
        for (int i = arr.size() - 1; i; --i) {
            arr[i - 1] += arr[i] / 10;
            arr[i] %= 10;
        }
        int i = arr[0] ? 0 : 1;
        string ans;
        for (; i < arr.size(); ++i) {
            ans += '0' + arr[i];
        }
        return ans;
    }
};
```

#### Go

```go
func multiply(num1 string, num2 string) string {
	if num1 == "0" || num2 == "0" {
		return "0"
	}
	m, n := len(num1), len(num2)
	arr := make([]int, m+n)
	for i := m - 1; i >= 0; i-- {
		a := int(num1[i] - '0')
		for j := n - 1; j >= 0; j-- {
			b := int(num2[j] - '0')
			arr[i+j+1] += a * b
		}
	}
	for i := len(arr) - 1; i > 0; i-- {
		arr[i-1] += arr[i] / 10
		arr[i] %= 10
	}
	i := 0
	if arr[0] == 0 {
		i = 1
	}
	ans := []byte{}
	for ; i < len(arr); i++ {
		ans = append(ans, byte('0'+arr[i]))
	}
	return string(ans)
}
```

#### TypeScript

```ts
function multiply(num1: string, num2: string): string {
    if (num1 === '0' || num2 === '0') {
        return '0';
    }
    const m: number = num1.length;
    const n: number = num2.length;
    const arr: number[] = Array(m + n).fill(0);
    for (let i: number = m - 1; i >= 0; i--) {
        const a: number = +num1[i];
        for (let j: number = n - 1; j >= 0; j--) {
            const b: number = +num2[j];
            arr[i + j + 1] += a * b;
        }
    }
    for (let i: number = arr.length - 1; i > 0; i--) {
        arr[i - 1] += Math.floor(arr[i] / 10);
        arr[i] %= 10;
    }
    let i: number = 0;
    while (i < arr.length && arr[i] === 0) {
        i++;
    }
    return arr.slice(i).join('');
}
```

#### Rust

```rust
impl Solution {
    pub fn multiply(num1: String, num2: String) -> String {
        if num1 == "0" || num2 == "0" {
            return String::from("0");
        }
        let (num1, num2) = (num1.as_bytes(), num2.as_bytes());
        let (n, m) = (num1.len(), num2.len());
        let mut res = vec![];
        for i in 0..n {
            let a = num1[n - i - 1] - b'0';
            let mut sum = 0;
            let mut j = 0;
            while j < m || sum != 0 {
                if i + j == res.len() {
                    res.push(0);
                }
                let b = num2.get(m - j - 1).unwrap_or(&b'0') - b'0';
                sum += a * b + res[i + j];
                res[i + j] = sum % 10;
                sum /= 10;
                j += 1;
            }
        }
        res.into_iter()
            .rev()
            .map(|v| char::from(v + b'0'))
            .collect()
    }
}
```

#### C#

```cs
public class Solution {
    public string Multiply(string num1, string num2) {
        if (num1 == "0" || num2 == "0") {
            return "0";
        }

        int m = num1.Length;
        int n = num2.Length;
        int[] arr = new int[m + n];

        for (int i = m - 1; i >= 0; i--) {
            int a = num1[i] - '0';
            for (int j = n - 1; j >= 0; j--) {
                int b = num2[j] - '0';
                arr[i + j + 1] += a * b;
            }
        }

        for (int i = arr.Length - 1; i > 0; i--) {
            arr[i - 1] += arr[i] / 10;
            arr[i] %= 10;
        }

        int index = 0;
        while (index < arr.Length && arr[index] == 0) {
            index++;
        }

        StringBuilder ans = new StringBuilder();
        for (; index < arr.Length; index++) {
            ans.Append(arr[index]);
        }

        return ans.ToString();
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param string $num1
     * @param string $num2
     * @return string
     */

    function multiply($num1, $num2) {
        $length1 = strlen($num1);
        $length2 = strlen($num2);
        $product = array_fill(0, $length1 + $length2, 0);

        for ($i = $length1 - 1; $i >= 0; $i--) {
            for ($j = $length2 - 1; $j >= 0; $j--) {
                $digit1 = intval($num1[$i]);
                $digit2 = intval($num2[$j]);

                $temp = $digit1 * $digit2 + $product[$i + $j + 1];
                $product[$i + $j + 1] = $temp % 10;

                $carry = intval($temp / 10);
                $product[$i + $j] += $carry;
            }
        }
        $result = implode('', $product);
        $result = ltrim($result, '0');
        return $result === '' ? '0' : $result;
    }
}
```

#### Kotlin

```kotlin
class Solution {
    fun multiply(num1: String, num2: String): String {
        if (num1 == "0" || num2 == "0") return "0"

        val chars_1 = num1.toCharArray().reversedArray()
        val chars_2 = num2.toCharArray().reversedArray()

        val result = mutableListOf<Int>()

        chars_1.forEachIndexed { i, c1 ->
            val multiplier_1 = c1 - '0'
            var over = 0
            var index = 0

            fun sum(product: Int = 0): Unit {
                while (index >= result.size) {
                    result.add(0)
                }
                val value = product + over + result[index]
                result[index] = value % 10
                over = value / 10
                return
            }

            chars_2.forEachIndexed { j, c2 ->
                index = i + j
                val multiplier_2 = c2 - '0'
                sum(multiplier_1 * multiplier_2)
            }

            while (over > 0) {
                index++
                sum()
            }
        }

        return result.reversed().joinToString("")
    }
}
```

#### JavaScript

```js
/**
 * @param {string} num1
 * @param {string} num2
 * @return {string}
 */
var multiply = function (num1, num2) {
    if (num1 === '0' || num2 === '0') return '0';

    const result = Array(num1.length + num2.length).fill(0);
    const code_0 = '0'.charCodeAt(0);

    const num1_len = num1.length;
    const num2_len = num2.length;

    for (let i = 0; i < num1_len; ++i) {
        const multiplier_1 = num1.charCodeAt(num1_len - i - 1) - code_0;
        for (let j = 0; j < num2_len; ++j) {
            const multiplier_2 = num2.charCodeAt(num2_len - j - 1) - code_0;
            result[i + j] += multiplier_1 * multiplier_2;
        }
    }

    result.reduce((carry, value, index) => {
        const sum = carry + value;
        result[index] = sum % 10;
        return (sum / 10) | 0;
    }, 0);

    return result
        .slice(0, result.findLastIndex(d => d !== 0) + 1)
        .reverse()
        .join('');
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
