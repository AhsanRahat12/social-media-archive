I used to mix up ROW_NUMBER, RANK and DENSE_RANK all the time. This is the mental model that finally cleared it up.
Same ORDER BY, but each one handles a tie differently. Say four reps closed 95, 90, 90 and 85 deals:

1. ROW_NUMBER gives 1, 2, 3, 4. Every row is unique, even the tie. Add a tiebreaker to your ORDER BY or the two 90s can swap places between runs.
2. RANK gives 1, 2, 2, 4. Ties share a rank, then it skips. Nobody is 3rd because two reps are already ahead of the next one.
3. DENSE_RANK gives 1, 2, 2, 3. Ties share a rank, and there are no gaps.

Here's how I choose:
One row per group, like the latest order per customer? ROW_NUMBER.
A leaderboard where ties push everyone else down? RANK.
The top 3 distinct values, like salaries? DENSE_RANK.
That last one is a classic interview trap. Use RANK there and you can end up with only two salary levels.
Which of these three confused you the longest, and what finally made it click? 👇
Follow me for more SQL that shows up in real work and real interviews.
#SQL #DataAnalytics #DataEngineering #WindowFunctions #AnalyticsEngineering
