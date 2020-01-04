# codewars_practice

## Consider the following equation of a surface S: z\*z\*z = x\*x * y\*y.

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
def prime_factors1(n):
    for i in itertools.chain([2], itertools.count(3, 2)):
        if n <= 1:
            break
        while n % i == 0:
            n //= i
            yield i


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
