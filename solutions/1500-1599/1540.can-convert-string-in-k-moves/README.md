---
comments: true
difficulty: Medium
rating: 1631
source: Biweekly Contest 32 Q2
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [1540. Can Convert String in K Moves](https://leetcode.com/problems/can-convert-string-in-k-moves)

## Description

<!-- description:start -->

<p>Given two strings&nbsp;<code>s</code>&nbsp;and&nbsp;<code>t</code>, your goal is to convert&nbsp;<code>s</code>&nbsp;into&nbsp;<code>t</code>&nbsp;in&nbsp;<code>k</code><strong>&nbsp;</strong>moves or less.</p>

<p>During the&nbsp;<code>i<sup>th</sup></code>&nbsp;(<font face="monospace"><code>1 &lt;= i &lt;= k</code>)&nbsp;</font>move you can:</p>

<ul>
	<li>Choose any index&nbsp;<code>j</code>&nbsp;(1-indexed) from&nbsp;<code>s</code>, such that&nbsp;<code>1 &lt;= j &lt;= s.length</code>&nbsp;and <code>j</code>&nbsp;has not been chosen in any previous move,&nbsp;and shift the character at that index&nbsp;<code>i</code>&nbsp;times.</li>
	<li>Do nothing.</li>
</ul>

<p>Shifting a character means replacing it by the next letter in the alphabet&nbsp;(wrapping around so that&nbsp;<code>&#39;z&#39;</code>&nbsp;becomes&nbsp;<code>&#39;a&#39;</code>). Shifting a character by&nbsp;<code>i</code>&nbsp;means applying the shift operations&nbsp;<code>i</code>&nbsp;times.</p>

<p>Remember that any index&nbsp;<code>j</code>&nbsp;can be picked at most once.</p>

<p>Return&nbsp;<code>true</code>&nbsp;if it&#39;s possible to convert&nbsp;<code>s</code>&nbsp;into&nbsp;<code>t</code>&nbsp;in no more than&nbsp;<code>k</code>&nbsp;moves, otherwise return&nbsp;<code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;input&quot;, t = &quot;ouput&quot;, k = 9
<strong>Output:</strong> true
<b>Explanation: </b>In the 6th move, we shift &#39;i&#39; 6 times to get &#39;o&#39;. And in the 7th move we shift &#39;n&#39; to get &#39;u&#39;.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abc&quot;, t = &quot;bcd&quot;, k = 10
<strong>Output:</strong> false
<strong>Explanation: </strong>We need to shift each character in s one time to convert it into t. We can shift &#39;a&#39; to &#39;b&#39; during the 1st move. However, there is no way to shift the other characters in the remaining moves to obtain t from s.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;aab&quot;, t = &quot;bbb&quot;, k = 27
<strong>Output:</strong> true
<b>Explanation: </b>In the 1st move, we shift the first &#39;a&#39; 1 time to get &#39;b&#39;. In the 27th move, we shift the second &#39;a&#39; 27 times to get &#39;b&#39;.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, t.length &lt;= 10^5</code></li>
	<li><code>0 &lt;= k &lt;= 10^9</code></li>
	<li><code>s</code>, <code>t</code> contain&nbsp;only lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canConvertString(self, s: str, t: str, k: int) -> bool:
        if len(s) != len(t):
            return False
        cnt = [0] * 26
        for a, b in zip(s, t):
            x = (ord(b) - ord(a) + 26) % 26
            cnt[x] += 1
        for i in range(1, 26):
            if i + 26 * (cnt[i] - 1) > k:
                return False
        return True
```

#### Java

```java
class Solution {
    public boolean canConvertString(String s, String t, int k) {
        if (s.length() != t.length()) {
            return false;
        }
        int[] cnt = new int[26];
        for (int i = 0; i < s.length(); ++i) {
            int x = (t.charAt(i) - s.charAt(i) + 26) % 26;
            ++cnt[x];
        }
        for (int i = 1; i < 26; ++i) {
            if (i + 26 * (cnt[i] - 1) > k) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canConvertString(string s, string t, int k) {
        if (s.size() != t.size()) {
            return false;
        }
        int cnt[26]{};
        for (int i = 0; i < s.size(); ++i) {
            int x = (t[i] - s[i] + 26) % 26;
            ++cnt[x];
        }
        for (int i = 1; i < 26; ++i) {
            if (i + 26 * (cnt[i] - 1) > k) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func canConvertString(s string, t string, k int) bool {
	if len(s) != len(t) {
		return false
	}
	cnt := [26]int{}
	for i := range s {
		x := (t[i] - s[i] + 26) % 26
		cnt[x]++
	}
	for i := 1; i < 26; i++ {
		if i+26*(cnt[i]-1) > k {
			return false
		}
	}
	return true
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
