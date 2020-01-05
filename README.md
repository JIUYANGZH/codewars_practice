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

## Regex Password Validation

You need to write regex that will validate a password to make sure it meets the following criteria:

At least six characters long
contains a lowercase letter
contains an uppercase letter
contains a number
Valid passwords will only be alphanumeric characters.

```
from re import compile, VERBOSE
# re模块的re.VERBOSE可以把正则表达式写成多行，并且自动忽略空格。
# *? 重复任意次，但尽可能少重复，非贪婪
regex = compile("""
^              # begin word
(?=.*?[a-z])   # at least one lowercase letter
(?=.*?[A-Z])   # at least one uppercase letter
(?=.*?[0-9])   # at least one number
[A-Za-z\d]     # only alphanumeric
{6,}           # at least 6 characters long
$              # end word
""", VERBOSE)
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
 => [[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]]

```
def power(s):
  set = [[]]
  for num in s:
    set += [x+[num] for x in set]
  return set
```

## Last digit of a large number
returns the last decimal digit of a^b

last_digit(4, 1)                # returns 4

last_digit(4, 2)                # returns 6

last_digit(9, 7)                # returns 9

last_digit(10, 10 ** 10)        # returns 0

last_digit(2 ** 200, 2 ** 300)  # returns 6
```
pow(x,y,z) 函数是计算x的y次方，再对结果进行取模，其结果等效于pow(x,y) %z
def last_digit(n1, n2):
    return pow( n1, n2, 10 )
```

## What's a Perfect Power anyway?

Your task is to check wheter a given integer is a perfect power. If it is a perfect power, return a pair m and k with m^k = n as a proof. 

Note: For a perfect power, there might be several pairs. For example 81 = 3^4 = 9^2, so (3,4) and (9,2) are valid solutions. However, the tests take care of this, so if a number is a perfect power, return any pair that proves it.

```
def isPP(n):
    for i in range(2, int(n**.5) + 1):
        number = n
        times = 0
        # 如果n能被这个数i一直除直到除尽，那么就return这个结果，结果为i, 除的次数
        while number % i == 0:
            number /= i
            times += 1
            if number == 1:
                return [i, times]
    return None
```

## Pick peaks 动态规划

In this kata, you will write a function that returns the positions and the values of the "peaks" (or local maxima) of a numeric array.

For example, the array arr = [0, 1, 2, 5, 1, 0] has a peak at position 3 with a value of 5 (since arr[3] equals 5).

Example: pickPeaks([3, 2, 3, 6, 4, 1, 2, 3, 2, 1, 2, 3]) should return {pos: [3, 7], peaks: [6, 3]} (or equivalent in other languages)

The first and last elements of the array will not be considered as peaks (in the context of a mathematical function, we don't know what is after and before and therefore, we don't know if it is a peak or not).

Also, beware of plateaus !!! [1, 2, 2, 2, 1] has a peak while [1, 2, 2, 2, 3] does not. In case of a plateau-peak, please only return the position and value of the beginning of the plateau. For example: pickPeaks([1, 2, 2, 2, 1]) returns {pos: [1], peaks: [2]} (or equivalent in other languages)

```
def pick_peaks(arr):
    pos = []
    peak = False
    for i in range(1, len(arr)):
        # 从第一个往后数，如果后一个比前一个数大，那么就把后一个数的index暂且列为peak
        if arr[i] > arr[i-1]:
            peak = i
        # 如果后一个数比前一个数小，那么保存当前peak（这里保存的是index，不是那个数）进pos，然后重置peak，设为False (没有peak)
        elif arr[i] < arr[i-1] and peak:
            pos.append(peak)
            peak = False
        # 如果后一个数等于前一个数，不做任何动作，继续比较下一个
    return {'pos':pos, 'peaks':[arr[i] for i in pos]}
```

