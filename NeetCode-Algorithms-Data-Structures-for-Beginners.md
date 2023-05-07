#### Dynamic Programming ####
1-Dimension - Fibonacci

F(0) = 0, F(1) = 1

F(n) = F(n - 1) + F(n - 2)

![image](https://user-images.githubusercontent.com/5309726/236654684-dc229eb9-59ea-47ac-a6bf-15316d57674b.png)

```python
# Brute Force
def bruteForce(n):
    if n <= 1:
        return n
    return bruteForce(n - 1) + bruteForce(n - 2)
```

![image](https://user-images.githubusercontent.com/5309726/236654827-3176a9d0-3cb0-4cbd-b81d-dce0972d7d5a.png)

```python
# Memoization (Top-Down approach)
def memoization(n, cache):
    # Base case: If n is 0 or 1, the Fibonacci number is equal to n itself
    if n <= 1:
        return n
    
    # If the result for n is already in the cache, return the cached value
    if n in cache:
        return cache[n]
     
    # If the result for n is not in the cache, calculate it recursively
    # and store it in the cache before returning it
    cache[n] = memoization(n - 1, cache) + memoization(n - 2, cache)
    return cache[n]

```

![image](https://user-images.githubusercontent.com/5309726/236655251-6c04b1df-7fea-42b9-9265-76a9efb99c88.png)

```python
# Tabulation (Botton-Up approach)
# Constant memory space O(1)

def dp(n):
    # Base case: If n is 0 or 1, the Fibonacci number is equal to n itself
    if n < 2:
        return n

    # Initialize a list to store the two most recent Fibonacci numbers
    dp = [0, 1]
    
    # Initialize a counter variable, starting at 2
    i = 2

    # Iterate until i is greater than n
    while i <= n:
        # Store the previous Fibonacci number (dp[1]) in a temporary variable
        tmp = dp[1]
        
        # Calculate the next Fibonacci number by adding the two most recent numbers
        dp[1] = dp[0] + dp[1]
        
        # Update dp[0] with the value stored in the temporary variable
        dp[0] = tmp
        
        # Increment the counter variable
        i += 1
    
    # Return the nth Fibonacci number
    return dp[1]
```

---
