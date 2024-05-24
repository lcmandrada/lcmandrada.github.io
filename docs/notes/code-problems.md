# Code Problems

## Arrays

### Best Time to Buy and Sell Stock II
You are given an integer array `prices` where `prices[i]` is the price of a given stock on the ith day. Find and return the maximum profit you can achieve.

**Example**
```
Input: prices = [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Total profit is 4 + 3 = 7.
```

[//]: # (more)

**Solution**  
Simple One Pass  
Crawl over the slope and keep on adding the profit obtained from every valid consecutive transaction when the second number is larger than the first one.
```py
def maxProfit(self, prices: List[int]) -> int:
    total_profit = 0

    for i in range(1, len(prices)):
        if prices[i] > prices[i - 1]:
            total_profit += prices[i] - prices[i -1]
            
    return total_profit
```

### Remove Duplicates from Sorted Array
Given an integer array `nums` sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same.

**Example**  
```
Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

**Solution**  
Two Pointers  
Use two pointers tracking the latest index for a unique item and the current item iteration. When the latest unique item is different from the current item (when duplication ends), assign the current item to the next unique index.
```py
def removeDuplicates(self, nums: List[int]) -> int:
    unique_index = 0

    for index in range(1, len(nums)):
        if nums[unique_index] != nums[index]:
            unique_index += 1
            nums[unique_index] = nums[index]
    
    return unique_index + 1
```

### Rotate Array
Given an array, rotate the array to the right by `k` steps, where `k` is non-negative.

**Example**
```
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```

**Solution**
```py
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        # get modulo as any multiples of the array length
        # will produce the same array when rotated
        k %= len(nums)
        
        # reverse the whole array
        self.reverse(nums, 0, len(nums)-1)
        # reverse the left side of k to restore left side order
        self.reverse(nums, 0, k-1)
        # reverse the right side of k to restore right side order
        self.reverse(nums, k, len(nums)-1)
        
    def reverse(self, array: List[int], left: int, right: int) -> None:
        """Reverses an array about the span of the given left and right indices."""
        while left < right:
            # switch items
            array[left], array[right] = array[right], array[left]
            
            # crawl the span
            left += 1
            right -= 1
```

### Contains Duplicate
Given an integer array `nums`, return true if any value appears at least twice in the array, and return false if every element is distinct.

**Example**
```
Input: nums = [1,2,3,1]
Output: true
```

**Solution**
```py
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        # set removes duplicity
        return len(nums) != len(set(nums))
```

### Single Number
Given a non-empty array of integers `nums`, every element appears twice except for one. Find that single one.

**Example**
```
Input: nums = [4,1,2,1,2]
Output: 4
```

**Solution**
```py
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        # apply xor in running adjacents
        # two instances of the same number will cancel each other
        # only the odd instance will be left
        return reduce(lambda x, y: x ^ y, nums, 0)
```

### Intersection of Two Arrays II
Given two integer arrays `nums1` and `nums2`, return an array of their intersection. Each element in the result must appear as many times as it shows in both arrays and you may return the result in any order.

**Example**
```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [4,9]
Explanation: [9,4] is also accepted.
```

**Solution**
```py
from collections import Counter

class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        # optimization
        # iterate smaller array
        if len(nums1) > len(nums2):
            self.intersect(nums2, nums1)
            
        intersection = []
        # count item instances
        counter = Counter(nums1)
        
        for num in nums2:
            # add to intersection only if available in both arrays
            if counter[num] > 0:
                intersection.append(num)
                counter[num] -= 1
                
        return intersection
```

### Plus One
You are given a large integer represented as an integer array `digits`, where each `digits[i]` is the ith digit of the integer. Increment the large integer by one and return the resulting array of digits.

**Example**
```
Input: digits = [9]
Output: [1,0]
Explanation: The array represents the integer 9.
Incrementing by one gives 9 + 1 = 10.
Thus, the result should be [1,0].
```

**Solution**
```py
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        for i in reversed(range(len(digits))):
            if digits[i] < 9:
                # simply add 1 if < 9 and immediately return
                digits[i] += 1
                return digits
            # set 0 if 9 and allow loop to carry over
            digits[i] = 0
            
        # add leading 1 (carry over) if all other digits were 9
        return [1] + digits
```

### Move Zeroes
Given an integer array `nums`, move all 0's to the end of it while maintaining the relative order of the non-zero elements.

**Example**
```
Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]
```

**Solution**
```py
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        nzero_index = 0
        # move all non-zeroes in front
        # swap may also be used here
        for i in range(len(nums)):
            if nums[i] != 0:
                nums[nzero_index] = nums[i]
                nzero_index += 1
                
        # fill the rest with 0
        for j in range(nzero_index, len(nums)):
            nums[j] = 0
```

### Two Sum
Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to target.

**Example**
```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```

**Solution**
```py
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # store iterated numbers with their indices
        num_index = {}
        for i, num in enumerate(nums):
            # if the current number's addend was already iterated,
            # return both their indices
            if target - num in num_index:
                return num_index[target - num], i
            num_index[num] = i
```

### Valid Sudoku
Determine if a 9 x 9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:
- Each row must contain the digits 1-9 without repetition.
- Each column must contain the digits 1-9 without repetition.
- Each of the nine 3 x 3 sub-boxes of the grid must contain the digits 1-9 without repetition.

**Example**
```
Input: board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being modified to 8.
Since there are two 8's in the top left 3x3 sub-box, it is invalid.
```

**Solution**
```py
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        seen = set()
        
        # traverse the 9x9 board
        for row in range(9):
            for col in range(9):
                # skip .
                if board[row][col] == ".":
                    continue
                    
                # record number as seen in row, col, box
                row_entry = board[row][col] + "@row" + str(row)
                col_entry = board[row][col] + "@col" + str(col)
                # box is 3x3
                box_entry = board[row][col] + "@box" + str(row // 3) + str(col // 3)
                    
                # if the number is already recorded,
                # the board is invalid
                if any((
                    row_entry in seen,
                    col_entry in seen,
                    box_entry in seen,
                )):
                    return False
                
                # record number
                seen.add(row_entry)
                seen.add(col_entry)
                seen.add(box_entry)
                
        return True
```

### Rotate Image
You are given an `n x n` 2D matrix representing an image, rotate the image by 90 degrees (clockwise).

**Example**
```
Input: matrix = [[1,2,3],[4,5,6],[7,8,9]]
Output: [[7,4,1],[8,5,2],[9,6,3]]
```

**Solution**
```py
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        # reverse rows for easier swapping
        matrix.reverse()
        
        for i in range(len(matrix)):
            # i+1 is necessary as starting from 0 will reverse the swap
            # from the previous iterations
            for j in range(i+1, len(matrix)):
                # distribute row items to other rows
                # i.e. first row items go to other rows' first indices
                # this can be achieved by swapping counterparts
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
```



## Strings

### Reverse String
Write a function that reverses a string. The input string is given as an array of characters `s`.

**Example**
```
Input: s = ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
```

**Solution**
```py
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        # shortcut
        # s[:] = s[::-1]
        
        # uses swap
        l = 0
        r = len(s) - 1
        
        while l < r:
            s[l], s[r] = s[r], s[l]
            l += 1
            r -= 1
```

### Reverse Integer
Given a signed 32-bit integer `x`, return `x` with its digits reversed. If reversing `x` causes the value to go outside the signed 32-bit integer range [-231, 231 - 1], then return 0.

**Example**
```
Input: x = -123
Output: -321
```

**Solution**
```py
class Solution:
    def reverse(self, x: int) -> int:
        ans = 0
        sign = -1 if x < 1 else 1
        # get absolute value
        x *= sign
        
        while x:
            # multiply ans by 10 to move the current ans to the next tens place
            # add the modulo by 10 to get the number in ones place
            # this will build a number from most to least significant
            # by adding the numbers in ones place in each iteration
            ans = ans * 10 + x % 10
            # truncate the current ones place to process
            # the next tens place in the next iteration
            x //= 10
            
        # restore sign
        ans *= sign

        # apply constraints
        return ans if -2**31 <= ans <= 2**31 - 1 else 0
```

### First Unique Character in a String
Given a string `s`, find the first non-repeating character in it and return its index. If it does not exist, return -1.

**Example**
```
Input: s = "loveleetcode"
Output: 2
```

**Solution**
```py
from collections import Counter

class Solution:
    def firstUniqChar(self, s: str) -> int:
        counter = Counter(s)
        
        for i, char in enumerate(s):
            # return index of current character
            # if instance count is exactly 1
            if counter[char] == 1:
                return i
            
        return -1
```

### Valid Anagram
Given two strings `s` and `t`, return true if `t` is an anagram of `s`, and false otherwise.

**Example**
```
Input: s = "anagram", t = "nagaram"
Output: true
```

**Solution**
```py
from collections import Counter

class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        # automatically not anagram if different lengths
        if len(s) != len(t):
            return False
        
        counter = Counter(s)
        
        for c in t:
            # if a character in t doesn't exist in s, not anagram
            if counter[c] == 0:
                return False
            # decrement to consider number of character instances
            counter[c] -= 1
            
        return True
```

### Valid Palindrome
A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return true if it is a palindrome, or false otherwise.

**Example**
```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

**Solution**
```py
class Solution:
    def isPalindrome(self, s: str) -> bool:
        l = 0
        r = len(s) - 1
        
        # traverse from each ends of the string
        while l < r:
            # crawl until a valid left side character
            while l < r and not s[l].isalnum():
                l += 1
            # crawl until a valid right side character
            while l < r and not s[r].isalnum():
                r -= 1
            # if characters are not similar, not a palindrome
            if s[l].lower() != s[r].lower():
                return False
            l += 1
            r -= 1
            
        return True
```

### String to Integer (atoi)
Implement the `myAtoi(string s)` function, which converts a string to a 32-bit signed integer (similar to C/C++'s atoi function).

**Example**
```
Input: s = "   -42"
Output: -42
Explanation:
Step 1: "   -42" (leading whitespace is read and ignored)
            ^
Step 2: "   -42" ('-' is read, so the result should be negative)
             ^
Step 3: "   -42" ("42" is read in)
               ^
The parsed integer is -42.
Since -42 is in the range [-231, 231 - 1], the final result is -42.
```

**Solution**
```py
class Solution:
    def myAtoi(self, s: str) -> int:
        s = s.strip()

        if not s:
            return 0

        num = 0
        
        # save sign and remove if available
        sign = -1 if s[0] == "-" else 1
        if s[0] in ("-", "+"):
            s = s[1:]
        
        for char in s:
            if char.isdigit():
                # shift current num to higher decimal place
                # and add current character to ones place
                # this builds the number by decimal place
                num = num * 10 + ord(char) - ord("0")
            else:
                break
                
        num *= sign
        lower_bound = -2**31
        upper_bound = 2**31 - 1
        
        if num < lower_bound:
            return lower_bound
        elif num > upper_bound:
            return upper_bound
        
        return num
```

### Implement strStr()
Return the index of the first occurrence of `needle` in `haystack`, or -1 if `needle` is not part of `haystack`.

**Example**
```
Input: haystack = "hello", needle = "ll"
Output: 2
```

**Solution**
```py
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        if not needle:
            return 0

        h_len = len(haystack)
        n_len = len(needle)
        
        for i in range(h_len - n_len + 1):
            # check if the slice with the same length as needle
            # is the same as needle
            if haystack[i:i+n_len] == needle:
                return i
            
        return -1
```

### Longest Common Prefix
Write a function to find the longest common prefix string amongst an array of strings. If there is no common prefix, return an empty string `""`.

**Example**
```
Input: strs = ["flower","flow","flight"]
Output: "fl"
```

**Solution**
```py
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if not strs:
            return ""
        
        # check each character of the first word
        # against other words
        for char in range(len(strs[0])):
            for arr in range(1, len(strs)):
                # if current character index is the same as the length
                # of any of the other words, return
                # if first word character is different from any
                # of the other words' of the same index, return
                if char == len(strs[arr]) or strs[0][char] != strs[arr][char]:
                    return strs[0][:char]
                
        # return the whole first word if not interrupted above
        return strs[0]
```
