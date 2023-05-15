#### Graphs ####

Matrix

![image](https://github.com/loonloon/Notes/assets/5309726/41d2c520-9df9-4c97-8b5f-982751d9d494)

Adjacency Matrix

![image](https://github.com/loonloon/Notes/assets/5309726/06f454ce-57b4-4a40-9e70-be7037920e6a)

Adjacency List

![image](https://github.com/loonloon/Notes/assets/5309726/1a04727d-eff5-452b-b93c-3f4158c3e530)

---

#### Dynamic Programming ####
1-Dimension - Fibonacci

F(0) = 0, F(1) = 1

F(n) = F(n - 1) + F(n - 2)

![image](https://user-images.githubusercontent.com/5309726/236654684-dc229eb9-59ea-47ac-a6bf-15316d57674b.png)

```python
# Brute Force (Top-Down approach)

def bruteForce(n):
    if n <= 1:
        return n
    return bruteForce(n - 1) + bruteForce(n - 2)
```

![image](https://user-images.githubusercontent.com/5309726/236654827-3176a9d0-3cb0-4cbd-b81d-dce0972d7d5a.png)

```python
# Memoization (Top-Down approach)
# Top-down approach because it starts with the original problem and breaks it down into subproblems

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
# Constant memory O(1)
# Bottom-up approach would start with the smallest subproblems and combine them to solve the larger ones, 
# typically done in an iterative fashion rather than recursively

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

2-Dimension - Count paths

![image](https://github.com/loonloon/Notes/assets/5309726/e3d77b90-9cdf-4220-8527-b8f0f13d6359)
![image](https://github.com/loonloon/Notes/assets/5309726/c78ed465-603a-44a7-8c3c-a958eae200c6)

```python
# Brute Force (Top-Down approach)
# Time and space O(2 ^ (n + m))

def bruteForce(r, c, total_rows, total_cols):
    # If the current row or column is out of grid bounds, return 0 (indicating no valid paths)
    if r == total_rows or c == total_cols:
        return 0

    # If the current position is at the bottom right of the grid (goal), return 1 (indicating a valid path)
    if r == total_rows - 1 and c == total_cols - 1:
        return 1

    # Recursively explore two possibilities: moving down (r+1) and moving right (c+1)
    # The sum of these two recursive calls represents the total number of valid paths from the current position
    return bruteForce(r + 1, c, total_rows, total_cols) + bruteForce(r, c + 1, total_rows, total_cols)
```

```python
# Memoization (Top-Down approach)
# Time and space O(n * m)
def memoization(r, c, total_rows, total_cols, cache):
    # If the current row or column is out of grid bounds, return 0 (indicating no valid paths).
    if r == total_rows or c == total_cols:
        return 0

    # If we have already computed the value for this cell (r, c), return it from the cache.
    if cache[r][c] > 0:
        return cache[r][c]

    # If the current position is at the bottom right of the grid (goal), return 1 (indicating a valid path).
    if r == total_rows - 1 and c == total_cols - 1:
        return 1

    # If we have not computed the value for this cell, compute it by exploring two possibilities: 
    # moving down (r+1) and moving right (c+1). Store this computed value in the cache for future reference.
    cache[r][c] = memoization(r + 1, c, total_rows, total_cols, cache) + memoization(r, c + 1, total_rows, total_cols, cache)
    
    # Return the cached value for this cell.
    return cache[r][c]
```

![image](https://github.com/loonloon/Notes/assets/5309726/11640ddc-7b5e-4344-ba60-e4f2f14903c0)

```python
def dp(total_rows, total_cols):
    # Initialize the previous row with zeros
    prev_row = [0] * total_cols

    # Loop from the last row up to the first row (inclusive)
    for r in range(total_rows - 1, -1, -1):
        # Create a new current row with all zeros
        cur_row = [0] * total_cols
        
        # Set the last element in the current row to 1
        cur_row[- 1] = 1

        # Loop from the second last column up to the first column (inclusive)
        for c in range(total_cols - 2, -1, -1):
            # For each cell, sum the cell to the right and the cell in the previous row (same column)
            cur_row[c] = cur_row[c + 1] + prev_row[c]
            
        # Set the previous row to be the current row for the next iteration
        prev_row = cur_row

    # Return the first element in the last computed row
    return prev_row[0]
```

---
