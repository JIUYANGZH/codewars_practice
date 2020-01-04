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
