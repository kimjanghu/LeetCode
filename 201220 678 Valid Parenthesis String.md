# 201220 678 Valid Parenthesis String

Given a string containing only three types of characters: '(', ')' and '*', write a function to check whether this string is valid. We define the validity of a string by these rules:

1. Any left parenthesis `'('` must have a corresponding right parenthesis `')'`.
2. Any right parenthesis `')'` must have a corresponding left parenthesis `'('`.
3. Left parenthesis `'('` must go before the corresponding right parenthesis `')'`.
4. `'*'` could be treated as a single right parenthesis `')'` or a single left parenthesis `'('` or an empty string.
5. An empty string is also valid.



**Example 1:**

```
Input: "()"
Output: True
```



**Example 2:**

```
Input: "(*)"
Output: True
```



**Example 3:**

```
Input: "(*))"
Output: True
```



**Note:**

1. The string size will be in the range [1, 100].



## Code 201220

---

* Solution1 - Memoization

```python
class Solution:
    def dfs(self, idx, n, s, l, r, memo):
        if idx == n:
            return l == r

        if (idx, l, r) not in memo:
            if s[idx] == ')':
                if r + 1 > l:
                    return False
                res = self.dfs(idx+1, n, s, l, r+1, memo)
            elif s[idx] == '(':
                res = self.dfs(idx+1, n, s, l+1, r, memo)
            else:
                res = self.dfs(idx+1, n, s, l+1, r, memo) or \
                    self.dfs(idx+1, n, s, l, r, memo) or \
                    self.dfs(idx+1, n, s, l, r+1, memo)
            memo[(idx, l, r)] = res
        return memo[(idx, l, r)]

    def checkValidString(self, s: str) -> bool:
        n = len(s)
        return self.dfs(0, n, s, 0, 0, {})
```

* Solution2

```python
class Solution:
    def checkValidString(self, s: str) -> bool:
        n = len(s)
        l, r = 0, 0
        for i in range(n):
            if s[i] in '(*':
                l += 1
            else:
                l -= 1

            if s[n-i-1] in '*)':
                r += 1
            else:
                r -= 1

            if l < 0 or r < 0:
                return False
        return True
```

