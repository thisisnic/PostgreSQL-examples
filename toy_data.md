# Toy Data

If there are no appropriate datasets available in the environment we're working in, we might want to create a toy dataset.  We can do this using Common Table Expressions (CTEs).

In the example below I create columns in the first CTEs as arrays, explicitly declaring their type, and then unnest them in the second CTE.

The `weather` CTE can now be used like a table and we can query it as if it was one.

```
WITH values (temp, day) AS (
	SELECT '{10, 9, 8}'::INT[], '{"Wed", "Thurs", "Fri"}'::VARCHAR[]
),
weather AS (
	SELECT unnest(temp) AS temperature, unnest(day) AS day FROM values
)
SELECT * FROM weather;
```