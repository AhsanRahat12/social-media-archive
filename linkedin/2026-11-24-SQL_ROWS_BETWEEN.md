I used to get completely lost every time I saw ROWS BETWEEN in a SQL query.

Here's the syntax that finally made it click:

Syntax:
<window_function> OVER (
  [PARTITION BY ...]
  [ORDER BY ...]
  ROWS BETWEEN <lower_boundary> AND <upper_boundary>
)

And here are the use cases I actually reach for day to day.

Trailing 3 week sales per store, for spotting momentum without charting the whole history.

SUM(sales) OVER (
  PARTITION BY store_id
  ORDER BY week_num
  ROWS 2 PRECEDING
)

Running total per category, the classic cumulative total you'd use for YTD spend or production output.

SUM(amount) OVER (
  PARTITION BY category
  ORDER BY exp_date
  ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
)

Centered moving average, for smoothing noisy daily numbers like temperature readings or daily output.

AVG(celsius) OVER (
  ORDER BY day_id
  ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING
)

Once I stopped treating the frame clause as scary syntax and started asking "what's my actual use case," picking the right frame became obvious.

What's the first window function use case that finally made this click for you? I'd genuinely like to know.

#SQL #DataAnalytics #SQLTips #DataEngineering #LearnInPublic
