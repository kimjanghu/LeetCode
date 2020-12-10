# 201210 357 Count Numbers with Unique Digits

Given a **non-negative** integer n, count all numbers with unique digits, x, where 0 ≤ x < 10n.

**Example:**

```
Input: 2
Output: 91 
Explanation: The answer should be the total numbers in the range of 0 ≤ x < 100, 
             excluding 11,22,33,44,55,66,77,88,99
```

 

**Constraints:**

- `0 <= n <= 8`



## 201210 Code

```python
class Solution:
    def countNumbersWithUniqueDigits(self, n):
        answer = 10
        if n == 0:
            return 1
        elif n > 1:
            for i in range(2, n+1):
                tmp = 1
                for j in range(9, 9-i+1, -1):
                    tmp *= j
                tmp *= 9
                answer += tmp
        return answer
```

