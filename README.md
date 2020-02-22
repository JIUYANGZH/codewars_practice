# codewars_practice

## 1 Sections

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


## 2 Lazy Repeater

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

## 3 Metric Units - 1

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

## 4 flatten() 这题是递归

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
## 5 Diophantine Equation

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

## 6 Regex Password Validation

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
## 7 Extract the domain name from a URL
Write a function that when given a URL as a string, parses out just the domain name and returns it as a string. For example:

domain_name("http://github.com/carbonfive/raygun") == "github" 
domain_name("http://www.zombie-bites.com") == "zombie-bites"
domain_name("https://www.cnet.com") == "cnet"
```
import re

# re.search匹配字符串中的任意部分，不必从头开始匹配, group是返回特定的匹配部位，group(0)返回整体结果，group(1)返回第一个括号的匹配结果
# ?P<value>的意思就是命名一个名字为value的组，匹配规则符合后面的正则表达式，在这里是命名一个叫name的组，然后匹配[\w-]+\.
def domain_name(url):
    return re.search('(https?://)?(www\d?\.)?(?P<name>[\w-]+)\.', url).group('name')
```

## 8 Return substring instance count - 2

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
## 9 Legendre's formula

阶乘数的质因子分解，求一个阶乘数不同质因子的幂指数

对于一个数n的阶乘n!，不同质因子p的幂指数为sum([int(n/p**i) for i in range(0,somevalue)])

## 10 Fixed-length integer partitions 递归题

A generalization of Bézier surfaces, called the S-patch, uses an interesting scheme for indexing its control points.

In the case of an n-sided surface of degree d, each index has n non-negative integers that sum to d, and all possible configurations are used.

For example, for a 3-sided quadratic (degree 2) surface the control points are:

indices 3 2 => [[0,0,2],[0,1,1],[0,2,0],[1,0,1],[1,1,0],[2,0,0]]

```
n为每个list的长度，d为每个list的和
def indices(n, d):
    return [[r] + point for r in range(d + 1) for point in indices(n-1, d-r)] if n > 1 else [[d]]
```

## 11 By the Power Set of Castle Grayskull

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

## 12 Last digit of a large number
returns the last decimal digit of a^b

last_digit(4, 1)                \# returns 4

last_digit(4, 2)                \# returns 6

last_digit(9, 7)                \# returns 9

last_digit(10, 10 ** 10)        \# returns 0

last_digit(2 ** 200, 2 ** 300)  \# returns 6
```
pow(x,y,z) 函数是计算x的y次方，再对结果进行取模，其结果等效于pow(x,y) %z
def last_digit(n1, n2):
    return pow( n1, n2, 10 )
```

## 13 What's a Perfect Power anyway?

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

## 14 Pick peaks 动态规划

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
## 15 Memoized Fibonacci 这个不是很懂

implement the memoization solution to calculate fibonacci quickly, The trick of the memoized version is that we will keep a cache data structure (most likely an associative array) where we will store the Fibonacci numbers as we calculate them. When a Fibonacci number is calculated, we first look it up in the cache, if it's not there, we calculate it and put it in the cache, otherwise we returned the cached number.

这个题是这样的，test给的是一大堆1000以内的数，让你用cache的方法迅速计算这一大堆数的fibonacci，这个算法是每个数计算以后都把计算过程中的
所有数加进cache里，这样后面如果要算比前面的数小的数，就直接从cache里提取就好，所以并不是单个数计算很快，计算单个fibonacci下面的代码快

```
c = [0]

def fib(n):
    if n in [0, 1]:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

def fibonacci(n):
    if n >= len(c):
        c.append(fib(n))
    return c[n]
    
# 快速计算单个数fibonacci
def f(n):
    a,b = 0,1
    for i in range(n-1):
        a,b = b,a+b
    return b
```
## 16 Number of trailing zeros of N!

这道题实际上是让你算N的阶乘中质因子5的幂指数是多少，因为一个10会给N！带来一个0，10又只能由2\*5得到，所以只需计算5的幂指数就可以

The question boils down to how many times n! is divisible by 10, i.e. what's the largest k such that 10^k divides n! Since the prime decomposition of 10 is 5 * 2, this is the same as finding the largest k such that 5^k and 2^k both divide n!.

Write a program that will calculate the number of trailing zeros in a factorial of a given number.

N! = 1 * 2 * 3 * ... * N

Be careful 1000! has 2568 digits...

For more info, see: http://mathworld.wolfram.com/Factorial.html

Examples
zeros(6) = 1
\# 6! = 1 * 2 * 3 * 4 * 5 * 6 = 720 --> 1 trailing zero

zeros(12) = 2
\# 12! = 479001600 --> 2 trailing zeros
```
def zeros(n):
  x = n/5
  return x+zeros(x) if x else 0
```

## 17 Calculating with Functions

这道题学习怎样使一个function输出另一个function，通过lambda, four(plus(five())) = four(plus(five)) = four(lambda x:x+5) = 4+5 = 9

因为 lambda x:x+5 = f(x): return x+5, 运算符里面的赋给x+y的y，运算符外面的赋给x+y的x

This time we want to write calculations using functions and get the results. Let's have a look at some examples:

seven(times(five())) \# must return 35

four(plus(nine())) \# must return 13

eight(minus(three())) \# must return 5

six(divided_by(two())) \# must return 3

```
def zero(f = None): return 0 if not f else f(0)
def one(f = None): return 1 if not f else f(1)
def two(f = None): return 2 if not f else f(2)
def three(f = None): return 3 if not f else f(3)
def four(f = None): return 4 if not f else f(4)
def five(f = None): return 5 if not f else f(5)
def six(f = None): return 6 if not f else f(6)
def seven(f = None): return 7 if not f else f(7)
def eight(f = None): return 8 if not f else f(8)
def nine(f = None): return 9 if not f else f(9)

def plus(y): return lambda x: x+y
def minus(y): return lambda x: x-y
def times(y): return lambda  x: x*y
def divided_by(y): return lambda  x: x/y
```
## 18 Moving Zeros To The End

这道题考察的是python里1=True, 0=False, 但是1 is not True, 0 is not False

Write an algorithm that takes an array and moves all of the zeros to the end, preserving the order of the other elements.

move_zeros([false,1,0,1,2,0,1,3,"a"]) # returns[false,1,1,2,1,3,"a",0,0]

```
def move_zeros(arr):
    l = [i for i in arr if isinstance(i, bool) or i!=0]
    return l+[0]*(len(arr)-len(l))
```

## 19 Minimum number of taxis

 Two customers, overlapping schedule. Two taxis needed.
 First customer wants to be picked up 1 and dropped off 4.
 Second customer wants to be picked up 2 and dropped off 6.
requests = [(1, 4), (2, 6)]
min_num_taxis(requests) # => 2

 Two customers, no overlap in schedule. Only one taxi needed.
 First customer wants to be picked up 1 and dropped off 4.
 Second customer wants to be picked up 5 and dropped off 9.
requests = [(1, 4), (5, 9)]
min_num_taxis(requests) # => 1

我想的第一个办法，是用active_car, free_car, suspend_car分别记录三种状态的车的数量，然后随着时间更新
我的第二个办法，是先制作一张下车时间表，然后循环上车时间表，如果要上车的人时间<=下车表第一个-1，那就去掉下车表第一个，否则要车数量+1

答案办法：
```
def min_num_taxis(a):
    r = np.zeros(max(y for _, y in a) + 1)
    # r是全时刻0
    
    for x, y in a:
        r[x:y+1] += 1
        print(r)
    # 循环所有时段，把所有用车需求加进r
    # [0. 1. 2. 3. 3. 3. 3. 2. 2. 1.]
    # 这个列表表明了在某个时间，所需的车的个数，输出最大即可
    return r.max()
    
```
## 20 关于至少有一人达到某条件的概率求解

解为P(一人达到条件) - P(两人达到条件) + P(三人达到条件) - P(四人达到条件) ...... +/- P(全员达到条件)

这个解的上界可以通过truncate奇数项，下界可以通过truncate偶数项，因为这个式子其实相当于1 - 1/2 + 1/3 - 1/4 + 1/5 - 1/6....
(Bonferroni’s Inequalities)
http://trin-hosts.trin.cam.ac.uk/fellows/dpk10/IA/chap2.pdf theorem 2.2

## 21 Recover a secret string from random triplets

There is a secret string which is unknown to you. Given a collection of random triplets from the string, recover the original string.

A triplet here is defined as a sequence of three letters such that each letter occurs somewhere before the next in the given string. "whi" is a triplet for the string "whatisup".

```

# 核心思想，不断循环triplets，只找当前的第一个字母，找到了就把这个字母删去，当前的第一个字母一定只出现在triplets的第一位而且不会出现在后面位
# 如果这个字母不是首字母，那么它必定会出现在后面位，试想，如果有两个字母都只出现在第一位，那么就无法分辨哪个在前面，也就无法推出整个string
所以必定只有一个字母只出现在第一位

def recoverSecret(triplets):
    res = ''
    while triplets != []:
        non_firsts = [num for t in triplets for num in t[1:]]
        firsts = [t[0] for t in triplets]
        
        #print(non_firsts)
        #print(firsts)
        #print('\n')
        
        
        for f in firsts:
            if f not in non_firsts:
                #print(f)
                res += f
                for t in triplets:
                    if t[0] == f:
                        t.pop(0)
                #print(triplets)
                break
        triplets = [t for t in triplets if t != []]
    return res
```

## 22 Smallest possible sum

Description
Given an array X of positive integers, its elements are to be transformed by running the following operation on them as many times as required:

if X[i] > X[j] then X[i] = X[i] - X[j]

When no more transformations are possible, return its sum ("smallest possible sum").

For instance, the successive transformation of the elements of input X = [6, 9, 21] is detailed below:

X_1 = [6, 9, 12] # -> X_1[2] = X[2] - X[1] = 21 - 9
X_2 = [6, 9, 6]  # -> X_2[2] = X_1[2] - X_1[0] = 12 - 6
X_3 = [6, 3, 6]  # -> X_3[1] = X_2[1] - X_2[0] = 9 - 6
X_4 = [6, 3, 3]  # -> X_4[2] = X_3[2] - X_3[1] = 6 - 3
X_5 = [3, 3, 3]  # -> X_5[1] = X_4[0] - X_4[1] = 6 - 3
so result will be 3 + 3 + 3 = 9
这道题用题目给出的方法计算必定超时
仔细观察后发现，其实每一步的subtraction，都是从比较大的那个数中取走一个比较小的数，在每一步相减的过程中，**差值不可能小于他们的最大公约数**，也就是，list会收敛到每一位都是他们的最大公约数
```
#举例 [6,9,12] = [3*2,3*3,3*4] --> [3*2,3*3,3*4-3*3=3*1] --> [3*2,3*1,3*1] --> [3*1,3*1,3*1]

解法1：
from fractions import gcd
from functools import reduce


def find_gcd(list):
    x = reduce(gcd, list)
    return x
    
def solution(a):
    return find_gcd(a) * len(a)
    
解法2：使用set，我们知道收敛后list的每一位都相同，所以只需找到这一个数字就可以，使用set，每次递归把 max(a) 替换成 max(a) - secondmax(a)，如果 max(a) - secondmax(a)已经在a里，因为a是set，相当于不加，这个解法好处是非常快，如果把set换成list，同样的方法会超时

def solution(a):
    a_len = len(a)
    a = set(a)
    while len(a) != 1:
        b = max(a)
        a.remove(b)
        a.add(b-max(a))
    return(max(a) * a_len)

```
## 22 Coin problem

**注意，与coin problem相似的还有一个coin change problem，是个动态规划题，以后整理一下**
Briefing: There is a list of positive natural numbers. Find the largest number that cannot be represented as the sum of this numbers, given that each number can be added unlimited times. Return this number, either 0 if there are no such numbers, or -1 if there are an infinite number of them.

Example:

Let's say [3,4] are given numbers. Lets check each number one by one:
1 - (no solution) - good
2 - (no solution) - good
3 = 3 won't go
4 = 4 won't go
5 - (no solution) - good
6 = 3+3 won't go
7 = 3+4 won't go
8 = 4+4 won't go
9 = 3+3+3 won't go
10 = 3+3+4 won't go
11 = 3+4+4 won't go
13 = 3+3+3+4 won't go
...and so on. So 5 is the biggest 'good'. return 5

这题完全不会，贴两个解法在下面
```
# 解法1
from functools import reduce
from math import gcd

def survivor(a):
    """Round Robin by Bocker & Liptak"""
    def __residue_table(a):
        n = [0] + [None] * (a[0] - 1)
        for i in range(1, len(a)):
            d = gcd(a[0], a[i])
            for r in range(d):
                try:
                    nn = min(n[q] for q in range(r, a[0], d) if n[q] is not None)
                except ValueError:
                    continue
                for _ in range(a[0] // d):
                    nn += a[i]
                    p = nn % a[0]
                    if n[p] is not None: nn = min(nn, n[p])
                    n[p] = nn
        return n

    a.sort()
    if len(a) < 1 or reduce(gcd, a) > 1: return -1
    if a[0] == 1: return 0
    return max(__residue_table(a)) - a[0]
    
# 解法2
from fractions import gcd
from functools import reduce
from itertools import count

def survivor(l):
    if 1 in l: return 0
    if len(l) < 2 or reduce(gcd,l) > 1: return -1
    if len(l) == 2: return (l[0]-1)*(l[1]-1)-1
    m,t,r,w=[True],0,0,max(l)
    for i in count(1):
        m = m[-w:] + [any(m[-n] for n in l if len(m)>=n)]
        if not m[-1]: t,r = i,0
        else: r += 1
        if r == w: break
    return t
```

## 23 走迷宫
You are at position [0, 0] in maze NxN and you can only move in one of the four cardinal directions (i.e. North, East, South, West). Return true if you can reach position [N-1, N-1] or false otherwise.

Empty positions are marked as . Walls are marked as W. Start and exit positions are empty in all test cases.

解法：一步一步走，matrix为迷宫记录，stack为计划路径，到达新的一个格子以后依次尝试各个可能的move，如果move是墙，就continue，如果move是走过的格子，也continue，如果move是没走过的格子，那么就标记此格为x，意思是走过了，然后把这个格子的下一个所有可能的moves加进stack计划里
```
def path_finder(maze):
    print(maze)
    matrix = list(map(list, maze.splitlines()))
    stack, length = [[0, 0]], len(matrix)
    while len(stack):
      for i in matrix:
          print(i)
      print('\n')
      x, y = stack.pop()
      
      if matrix[x][y] == '.':
        matrix[x][y] = 'x'
        for x, y in (x, y-1), (x, y+1), (x-1, y), (x+1, y):
          if 0 <= x < length and 0 <= y < length:
            stack.append((x, y))
    return matrix[length-1][length-1] == 'x'
   
  ```

## 24 大数质因子分解 large number factorial factors

```
def primeFactors(n): 
    r = []
    # 先把因子2除尽
    while n % 2 == 0: 
        r.append(2)
        n = n / 2
    # 再遍历剩下的，依次除尽
    for i in range(3,int(math.sqrt(n))+1,2): 
        while n % i== 0: 
            r.append(i)
            n = n / i 
    if n > 1:
        r.append(n)
    return list(set(r))
    
  ```
## 25 Euler's totient function, find the number of coprime number below n
欧拉函数phi(n)，比n小的所有与n互质的数的个数，举例
phi(8) = 4, 因为1,3,5,7与8互质，互质：最大公约数为1
```
def proper_fractions(n):
    # 先让phi=n，然后逐步减
    phi = n > 1 and n
    for p in range(2, int(n ** .5) + 1):
        # 如果n可以整除p，也就是说p是n的一个因子
        if not n % p:
            # 就把phi减去phi//p
            phi -= phi // p
            # 把n缩小为除掉所有这个质因子p的数
            while not n % p:
                n //= p
                
    if n > 1: phi -= phi // n
    return phi
```
若
<img src="https://latex.codecogs.com/gif.latex?%7B%5Cdisplaystyle%20n%3Dp_%7B1%7D%5E%7Bk_%7B1%7D%7Dp_%7B2%7D%5E%7Bk_%7B2%7D%7D%5Ccdots%20p_%7Br%7D%5E%7Bk_%7Br%7D%7D%7D" />

则
<img src="https://latex.codecogs.com/gif.latex?%7B%5Cdisplaystyle%20%5Cvarphi%20%28n%29%3D%5Cprod%20_%7Bi%3D1%7D%5E%7Br%7Dp_%7Bi%7D%5E%7Bk_%7Bi%7D-1%7D%28p_%7Bi%7D-1%29%3D%5Cprod%20_%7Bp%5Cmid%20n%7Dp%5E%7B%5Calpha%20_%7Bp%7D-1%7D%28p-1%29%3Dn%5Cprod%20_%7Bp%7Cn%7D%5Cleft%281-%7B%5Cfrac%20%7B1%7D%7Bp%7D%7D%5Cright%29%7D" />

例如
<img src="https://latex.codecogs.com/gif.latex?%5Cvarphi%20%2872%29%3D%5Cvarphi%20%282%5E%7B3%7D%5Ctimes%203%5E%7B2%7D%29%3D2%5E%7B%7B3-1%7D%7D%282-1%29%5Ctimes%203%5E%7B%7B2-1%7D%7D%283-1%29%3D2%5E%7B2%7D%5Ctimes%201%5Ctimes%203%5Ctimes%202%3D24" />

