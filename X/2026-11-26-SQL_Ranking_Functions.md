I used to mix up ROW_NUMBER, RANK and DENSE_RANK. What cleared it up:

Scores: 95, 90, 90, 85

ROW_NUMBER: 1,2,3,4
RANK: 1,2,2,4
DENSE_RANK: 1,2,2,3

Dedup: ROW_NUMBER
Leaderboard: RANK
Top 3 distinct: DENSE_RANK

Which one burned you first?

#SQL #DataAnalytics
