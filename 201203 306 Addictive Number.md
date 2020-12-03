Additive number is a string whose digits can form additive sequence.

A valid additive sequence should contain **at least** three numbers. Except for the first two numbers, each subsequent number in the sequence must be the sum of the preceding two.

Given a string containing only digits `'0'-'9'`, write a function to determine if it's an additive number.

**Note:** Numbers in the additive sequence **cannot** have leading zeros, so sequence `1, 2, 03` or `1, 02, 3` is invalid.

 

**Example 1:**

```
Input: "112358"
Output: true
Explanation: The digits can form an additive sequence: 1, 1, 2, 3, 5, 8. 
             1 + 1 = 2, 1 + 2 = 3, 2 + 3 = 5, 3 + 5 = 8
```

**Example 2:**

```
Input: "199100199"
Output: true
Explanation: The additive sequence is: 1, 99, 100, 199. 
             1 + 99 = 100, 99 + 100 = 199
```

 

**Constraints:**

- `num` consists only of digits `'0'-'9'`.
- `1 <= num.length <= 35`



## 201203 Code

```python
class Solution:
    def checkAdditiveNumber(self, one, two, other):
        if len(one) > 1 and one[0] == '0':
            return False
        if len(two) > 1 and two[0] == '0':
            return False

        pivot = str(int(one) + int(two))
        len_pivot = len(pivot)
        if len_pivot > len(other):
            return False

        if pivot == other:
            return True
        elif other.startswith(pivot):
            return self.checkAdditiveNumber(two, pivot, other[len_pivot:])

    def isAdditiveNumber(self, num):
        n = len(num)
        if n < 3: return False

        for i in range(1, n//2+1):
            for j in range(1, n//2+1):
                if self.checkAdditiveNumber(num[:i], num[i:i+j], num[i+j:]):
                    return True
        return False
```

