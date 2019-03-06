# Recursive CTEs

Common Table Expressions (CTEs) can be useful for creating complex SQL queries and functions without having to use subqueries, thus making for more readable and modular code.  

A special type of CTE is the recursive CTE, which allow the use of recursion in SQL.  They can be especially useful when working with hierarchical data. 

## Integers between 1 and 100

A simple example of a recursive CTE, taken from [https://malisper.me/understanding-postgres-recursive-ctes/](https://malisper.me/understanding-postgres-recursive-ctes/), shows the use of recursive CTEs to generate all integers between 1 and 100 as separate rows:

```
WITH RECURSIVE ints (n) AS (
    SELECT 1
  UNION ALL
    SELECT n+1 FROM ints WHERE n+1<= 100
)
SELECT n FROM ints;
```

For an in-depth description of what is happening on each line, visit the URL above.

## Triangle numbers

Triangle numbers are the sum of natural numbers.  Whilst there are simpler methods to calculate triangle numbers, we can manually calculate the first 100 triangle numbers using the following recursive CTE.

```
WITH RECURSIVE ints (n, m) AS (
    SELECT 1, 0
  UNION ALL
    SELECT n+1, m + n FROM ints WHERE n+1<= 100
)
SELECT m as triangle_number FROM ints;
```
To break it down, this is what happens:

1. A recursive CTE is here somewhat akin to a loop, with the initial values of n and m set to 0 and 1, which are added to the working table.

2. It then appends (using `UNION ALL`) subsequent results to the table.  To calculate these subsequent results, the query checks that the `WHERE` clause is TRUE and then takes the previous values (so in the case of row 2, these would be 1 and 0, our starting values), and then appends `n + 1` and `m + n`, so 2 and 1.  

3. As long as the `WHERE` clause remains TRUE, step 2 is repeated until `n+1<=100` is FALSE.

## Usage

Recursive CTEs can be used in hierarchical data where we wish to return, for example, of children of a certain entity.