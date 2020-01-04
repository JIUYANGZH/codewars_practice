# codewars_practice

## Sections

Consider the following equation of a surface S: z\*z\*z = x\*x * y\*y.
Take a cross section of S by a plane P: z = k where k is a positive integer (k > 0).
Call this cross section C(k).

Task
Find the number of points of C(k) whose coordinates are positive integers.

Examples
If we call c(k) the function which returns this number we have

c(1) -> 1
c(4) -> 4
c(4096576) -> 160
c(2019) -> 0 which means that no point of C(2019) has integer coordinates.

solution
```
import itertools
# Find all prime factors of a number, e.g. 12 --> [2,2,3]
def prime_factors(n):
    for i in itertools.chain([2], itertools.count(3, 2)):
        if n <= 1:
            break
        while n % i == 0:
            n //= i
            yield i

# The solution of this problem is find all integer xy = n, where n = k**(3/2)
so solution will be multiplication of number of unique factor + 1
e.g. for 12, [2,2,3], two 2, one 3. so solution is (2+1) * (1+1)
reason: the question is asking for (x,y) form, x 可以有0到2个2，乘上0到1个3，y take剩下的factor，所以共有3*2=6种情况


def c(k):
    root = int(k**0.5)
    if (root * root != k):
        return 0
        
    result = 1
    a = list(prime_factors1(k**(3/2)))
    for i in set(a):
        result *= a.count(i) + 1
    return result
```


## Lazy Repeater

The makeLooper() function (make_looper in Python) takes a string (of non-zero length) as an argument. It returns a function. The function it returns will return successive characters of the string on successive invocations. It will start back at the beginning of the string once it reaches the end.

For example:

abc = make_looper('abc')
abc() \# should return 'a' on this first call
abc() \# should return 'b' on this second call
abc() \# should return 'c' on this third call
abc() \# should return 'a' again on this fourth call、

solution
```
from itertools import cycle

# cycle: 生成一个<itertools.cycle at 0x21d6d083318>, call next(itertools.cycle) 就会不断循环弹出list的下一个元素

def make_looper(string):
    lst = cycle(list(string))
    return lambda: next(lst)
    
```

## Metric Units - 1

Scientists working internationally use metric units almost exclusively. Unless that is, they wish to crash multimillion dollars worth of equipment on Mars.

Your task is to write a simple function that takes a number of meters, and outputs it using metric prefixes.

In practice, meters are only measured in "mm" (thousandths of a meter), "cm" (hundredths of a meter), "m" (meters) and "km" (kilometers, or clicks for the US military).

For this exercise we just want units bigger than a meter, from meters up to yottameters, excluding decameters and hectometers.

All values passed in will be positive integers. e.g.

meters(5);
// returns "5m"

meters(51500);
// returns "51.5km"

meters(5000000);
// returns "5Mm"

solution:
```
def meters(x):
    arr = ['','k','M','G','T','P','E','Z','Y']
    count=0
    while x>=1000 :
        x /=1000.00 
        count+=1
    if int(x)==x:
        x=int(x) 
    return str(x)+arr[count]+'m'
```

## flatten() 这题是递归

For this exercise you will create a global flatten method. The method takes in any number of arguments and flattens them into a single array. If any of the arguments passed in are an array then the individual objects within the array will be flattened so that they exist at the same level as the other arguments. Any nested arrays, no matter how deep, should be flattened into the single array result.

The following are examples of how this function would be used and what the expected results would be:

flatten(1, [2, 3], 4, 5, [6, [7]]) # returns [1, 2, 3, 4, 5, 6, 7]
flatten('a', ['b', 2], 3, None, [[4], ['c']]) # returns ['a', 'b', 2, 3, None, 4, 'c']

solution:
```
def flatten(*args):
    # 送进来一个list，挨个检查其中的元素，如果元素是list，就再传进flatten，如果不是list，就放进result里
    return [x for a in args for x in (flatten(*a) if isinstance(a, list) else [a])]
    
    相当于
    for a in args:
        if a 是list：
            a1 = flatten(a)
            if a1 是list：
                a2 = flatten(a1)
                ...
        最终把直到不是list为止，然后把所有元素加进result里
        if a 不是list：
            直接把a加进result里
    
```
## Diophantine Equation

In mathematics, a Diophantine equation is a polynomial equation, usually with two or more unknowns, such that only the integer solutions are sought or studied.

In this kata we want to find all integers x, y (x >= 0, y >= 0) solutions of a diophantine equation of the form:

x2 - 4 * y2 = n
(where the unknowns are x and y, and n is a given positive number) in decreasing order of the positive xi.

If there is no solution return [] or "[]" or "". (See "RUN SAMPLE TESTS" for examples of returns).

Examples:
solEquaStr(90005) --> "[[45003, 22501], [9003, 4499], [981, 467], [309, 37]]"
solEquaStr(90002) --> "[]"
Hint:
x2 - 4 * y2 = (x - 2*y) * (x + 2*y)

solution:
```
import math
def sol_equa(n):
    res = []
    for i in range(1, int(math.sqrt(n)) + 1):
        # 从1到根号n，挨个试，如果i是n的因子，就用n除i得到另一个因子j，再判断是不是整数
        if n % i == 0:
            j = n // i
            if (i + j) % 2 == 0 and (j - i) % 4 == 0:
                x = (i + j) // 2
                y = (j - i) // 4
                res.append([x, y])
            
    return res
```
## Return substring instance count - 2

Complete the solution so that it returns the number of times the search_text is found within the full_text.

search_substr( fullText, searchText, allowOverlap = true )
so that overlapping solutions are (not) counted. If the searchText is empty, it should return 0. Usage examples:

search_substr('aa_bb_cc_dd_bb_e', 'bb') \# should return 2 since bb shows up twice

search_substr('aaabbbcccc', 'bbb') \# should return 1

search_substr( 'aaa', 'aa' ) \# should return 2

search_substr( 'aaa', '' ) \# should return 0

search_substr( 'aaa', 'aa', false ) \# should return 1

solution:
```
import re

# ?= is a positive lookahead. It captured match must be followed by whatever is within the parentheses but that part isn't captured.

def search_substr(full_text, search_text, allow_overlap=True):
    if not full_text or not search_text: 
        return 0
    elif allow_overlap == True:
        return len(re.findall(r'(?=({}))'.format(search_text), full_text))
    elif allow_overlap == False:
        return len(re.findall(search_text, full_text))
```
## Legendre's formula

阶乘数的质因子分解，求一个阶乘数不同质因子的幂指数

对于一个数n的阶乘n!，不同质因子p的幂指数为sum([int(n/p**i) for i in range(0,somevalue)])

## Fixed-length integer partitions 递归题

A generalization of Bézier surfaces, called the S-patch, uses an interesting scheme for indexing its control points.

In the case of an n-sided surface of degree d, each index has n non-negative integers that sum to d, and all possible configurations are used.

For example, for a 3-sided quadratic (degree 2) surface the control points are:

indices 3 2 => [[0,0,2],[0,1,1],[0,2,0],[1,0,1],[1,1,0],[2,0,0]]

```
n为每个list的长度，d为每个list的和
def indices(n, d):
    return [[r] + point for r in range(d + 1) for point in indices(n-1, d-r)] if n > 1 else [[d]]
```

## By the Power Set of Castle Grayskull

Write a function that returns all of the sublists of a list or Array.

Example:

power([1,2,3])
# => [[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]]

```
def power(s):
  set = [[]]
  for num in s:
    set += [x+[num] for x in set]
  return set
```
