# 1. Character count in a text file
```yml
from collections import Counter

with open("file.txt", "r") as f:
    text = f.read()
print(Counter(text))
```
# 2. Check if a number is prime
```yml
def is_prime(n):
    return n > 1 and all(n % i for i in range(2, int(n ** 0.5) + 1))
```
# 3. Check if a number is Armstrong
    
 ```yml
     def is_armstrong(n):
        return n == sum(int(d) ** len(str(n)) for d in str(n))
```
# 4. Factorial without recursion
```yml
def factorial(n):
    result = 1
    for i in range(2, n + 1):
        result *= i
    return result
```
# 5. Fibonacci series using generators
```yml
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b
```
# 6. N-th Fibonacci number
```yml
def nth_fibonacci(n):
    a, b = 0, 1
    for _ in range(n):
        a, b = b, a + b
    return a
```

# 7. Fibonacci series without recursion
```yml
def fibonacci_series(n):
    a, b = 0, 1
    for _ in range(n):
        print(a, end=" ")
        a, b = b, a + b
```

# 8. Fibonacci series using recursion
```yml
def fibonacci_recursive(n):
    if n <= 1:
        return n
    return fibonacci_recursive(n - 1) + fibonacci_recursive(n - 2)
```
# 9. Prime number check (repeat of 2)
# 10. Check if a number is palindrome
```yml
def is_palindrome(n):
    return str(n) == str(n)[::-1]
```
# 11. Factorial without recursion (repeat of 4)
# 12. Armstrong check (repeat of 3)

# 13. Reverse a number
```yml
def reverse_number(n):
    return int(str(n)[::-1])
```
# 14. Find even numbers in list using lambda
```yml
even_numbers = list(filter(lambda x: x % 2 == 0, [1, 2, 3, 4, 5, 6]))
```
# 15. Remove duplicates from an array

```yml
unique_elements = list(set([1, 2, 2, 3, 4]))
```
# 16. High and low nibble without shift operator

```yml
def nibbles(byte):
    return byte & 0xF, byte >> 4
```
# 17. Palindrome check using recursion
```yml
def is_palindrome_recursive(n):
    s = str(n)
    if len(s) < 2:
        return True
    return s[0] == s[-1] and is_palindrome_recursive(int(s[1:-1]))
```
# 18. Check if a number is binary
```yml
def is_binary(n):
    return all(c in '01' for c in str(n))
```
# 19. Sum of digits using recursion
```yml
def sum_of_digits(n):
    if n == 0:
        return 0
    return n % 10 + sum_of_digits(n // 10)
```
# 20. Add two numbers without +
```yml
def add(a, b):
    while b:
        a, b = a ^ b, (a & b) << 1
    return a
```
# 21. Subtract without -
```yml
def subtract(a, b):
    return add(a, add(~b, 1))
```
# 22. Multiply by 2 without *

```yml
def multiply_by_2(n):
    return n << 1
```
# 23. Check even or odd without arithmetic
```yml
def is_even(n):
    return not n & 1
```
# 24. Reverse a linked list
```yml
class Node:
    def __init__(self, val):
        self.val = val
        self.next = None
def reverse_linked_list(head):
    prev = None
    while head:
        nxt = head.next
        head.next = prev
        prev = head
        head = nxt
    return prev
```
# 25. Balanced parentheses using stack
```yml
def is_balanced(s):
    stack = []
    for char in s:
        if char in "({[":
            stack.append(char)
        elif char in ")}]":
            if not stack or {')': '(', '}': '{', ']': '['}[char] != stack.pop():
                return False
    return not stack
```
# 26. Intersection of two linked lists
# Implementing with simple algorithm for brevity

# 27. Merge two sorted linked lists
```yml
def merge_two_lists(l1, l2):
    dummy = Node(0)
    tail = dummy
    while l1 and l2:
        if l1.val < l2.val:
            tail.next, l1 = l1, l1.next
        else:
            tail.next, l2 = l2, l2.next
        tail = tail.next
    tail.next = l1 or l2
    return dummy.next
```
# 28. Swap two numbers without third variable
```yml
a, b = 3, 4
a, b = b, a
```

# 29. Variable number of arguments
```yml
def variable_args(*args):
    return args
```
# 30. Check if numbers are unique

```yml
def all_unique(nums):
    return len(nums) == len(set(nums))
```
# 31. Character count in a text file (repeat of 1)
# 32. Find pairs with target sum
```yml
def find_pairs(arr, target):
    seen = set()
    pairs = []
    for num in arr:
        if target - num in seen:
            pairs.append((num, target - num))
        seen.add(num)
    return pairs
```
# 33. Add without +
# 34. Solve equations (using linear equations solutions)
# 35. Match pattern with regex

```yml
import re
pattern = r'a(b{4,8})'
match = re.match(pattern, "abbbbb")
```
# 36. Date format conversion
```yml
def convert_date(date):
    return "-".join(date.split("-")[::-1])
```
# 37. Combine dictionaries
```yml
def combine_dicts(d1, d2):
    result = d1.copy()
    for k, v in d2.items():
        result[k] = result.get(k, 0) + v
    return result
```
# 38. Access Google Drive CSV
# Requires Google Drive API setup

# 39. Reverse words in string
```yml
def reverse_words(s):
    return " ".join(reversed(s.split()))
```
# 40. Reverse each word
```yml
def reverse_each_word(s):

    return " ".join(word[::-1] for word in s.split())
```
# 41. Count characters in string
```yml
def count_characters(s):
    return Counter(s)
```
# 42. Find all substrings
```yml
def substrings(s):
    return [s[i:j] for i in range(len(s)) for j in range(i + 1, len(s) + 1)]
```
# 43. Circular rotation
```yml
 left_rotate(arr, d):
    return arr[d:] + arr[:d]

def right_rotate(arr, d):
    return arr[-d:] + arr[:-d]
```
# 44. Prime check (repeat)
# 45. Sum of digits
```yml
def sum_digits(n):
    return sum(int(d) for d in str(n))
```
# 46. Second largest in array
```yml
def second_largest(arr):
    first, second = float('-inf'), float('-inf')
    for num in arr:
        if num > first:
            second, first = first, num
        elif first > num > second:
            second = num
    return second
```
# 47. Clock angle
```yml
def clock_angle(hour, minute):
    hour_angle = (hour % 12) * 30 + minute * 0.5
    minute_angle = minute * 6
    angle = abs(hour_angle - minute_angle)
    return min(angle, 360 - angle)
```
