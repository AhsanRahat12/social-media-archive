1/
I used to get completely lost every time I saw ROWS BETWEEN in SQL.

Here's the syntax, and 3 window frame use cases I actually use day to day. 🧵

2/
Syntax:

<window_fn> OVER (
 PARTITION BY ...
 ORDER BY ...
 ROWS BETWEEN <lower> AND <upper>
)

3/
Trailing 3 week sales per store, for spotting momentum:

SUM(sales) OVER (
 PARTITION BY store_id
 ORDER BY week_num
 ROWS 2 PRECEDING
)

4/
Running total per category, your classic YTD / cumulative total:

SUM(amount) OVER (
 PARTITION BY category
 ORDER BY exp_date
 ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
)

5/
Centered moving average, for smoothing noisy daily numbers:

AVG(celsius) OVER (
 ORDER BY day_id
 ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING
)

6/
Once I stopped treating the frame clause as scary syntax and asked "what's my actual use case," picking the right frame got easy.

What's the first window function use case that made it click for you?

#SQL #DataEngineering #SQLTips
