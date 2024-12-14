There are 3 main methods to find a matching expression for the asymptotic complexity of recursive functions.

1. substitution method
2. recursion tree method
3. master method

## Substitution Method: 

We make a guess for the solution and then we use mathematical induction to prove the guess is correct or incorrect. 

> For example consider the recurrence T(n) = 2T(n/2) + n
> 
> We guess the solution as T(n) = O(nLogn). Now we use induction to prove our guess.
> 
> We need to prove that T(n) <= cnLogn. We can assume that it is true for values smaller than n.
> 
> T(n) = 2T(n/2) + n  
>      <= 2cn/2Log(n/2) + n  
>        =  cnLogn – cnLog2 + n  
>        =  cnLogn – cn + n  
>     <= cnLogn

## **Recurrence Tree Method:**

In this method, we draw a recurrence tree and calculate the time taken by every level of the tree. Finally, we sum the work done at all levels. To draw the recurrence tree, we start from the given recurrence and keep drawing till we find a pattern among levels. The pattern is typically arithmetic or geometric series.   
 

> For example, consider the recurrence relation 
> 
> T(n) = 2T(n/2) + cn
> 
> Breaking down further gives us following
> 
>                        n                                     
>                 /             \\    
>        n/2                     n/2                       cn
>     /          \\                 /        \  \
> n/4         n/4         n/4          n/4          cn/2 + cn/2  ----> cn       
>  /    \\       /    \\      /    \\        /    \\  
>                              cn/4 + cn/4 + cn/4 + cn/4 ----> cn
> 
> we get the following series T(n)  = cn * log(n)  
    which is O(nlog(n))
> 
> 
> T(n) = T(n/4) + T(n/2) + cn^2
> 
> Breaking down further gives us following
> 
>                        n                                     
>                 /             \\    
>        n/4                     n/2                       cn^2
>     /          \\                 /        \  \
> n/16         n/8         n/8          n/4          cn^2/16 + cn^2/4  ----> 5cn^2/16       
>  /    \\       /    \\      /    \\        /    \\  
>                            cn^2 (1/16^2 + 1/8^2 + 1/8^2 + 1/4^2)  
>                                                        25(n^2)/256
> 
> we get the following series T(n)  = c(n^2 + 5(n^2)/16 + 25(n^2)/256) + ….  
> The above series is a geometrical progression with a ratio of 5/16.
> 
> To get an upper bound, we can sum the infinite series. We get the sum as (n2)/(1 – 5/16) which is O(n2)



## **Master Method:** 

Master Method is a direct way to get the solution. The master method works only for the following type of recurrences or for recurrences that can be transformed into the following type.   
 

> $T(n) = aT(n/b) + f(n^d)$ 
                      where a >= 1 and b > 1
> 
> $O(n^d)$    if  d > $logb(a)$
> 
> $O(n^dlogn)$    if  d = $logb(a)$
> 
> $O(n$^$logba$)    if  d < $logb(a)$



> $T(n) = aT(n-b) + f(n)$ 
> 
> $O(nf(n))$               if  a = 1
> 
> $O(f(n))$                 if  a < 1
> 
> $O$(a^(n/b)*$f(n)$)     if  a > 1

$$
1 < log(n) < ✓n < nlog(n) < n^2 < 2^n < n^n
$$


![[Pasted image 20240218170911.png]]

### Multiple Recursion
**Description:** A function makes more than one recursive call, leading to a tree-like branching structure.
```python
def fib(n):
    if n <= 1:
        return n
    else:
        return fib(n-1) + fib(n-2)
```

### Linear Recursion
**Description:** A function makes a single recursive call each time, progressing in a straightforward manner.
```python
def factorial(n):
    if n == 0:
        return 1
    else:
        return n * factorial(n-1)
```

### Mutual Recursion
**Description:** Two or more functions call each other recursively.
```python
def is_even(n):
    if n == 0:
        return True
    else:
        return is_odd(n-1)

def is_odd(n):
    if n == 0:
        return False
    else:
        return is_even(n-1)
```

### Tail Recursion
**Description:** The recursive call is the last operation in the function, allowing for potential optimization.
```python
def factorial(n, accumulator=1):
    if n == 0:
        return accumulator
    else:
        return factorial(n-1, n * accumulator)
```

### Nested Recursion
**Description:** A function makes a recursive call within another recursive call.
```python
def fn(N):
    if N > 100:
        return N - 10
    else:
        return fn(fn(N + 11))
```


[[Basic Data Structures]]