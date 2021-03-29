## Window functions

### 1.Warming up

Show the **lastName**, **party** and **votes** for the **constituency** 'S14000024' in 2017.

```sql
SELECT lastName, party, votes
  FROM ge
 WHERE constituency = 'S14000024' AND yr = 2017
ORDER BY votes DESC
```

### 2.Who won?

You can use the RANK function to see the order of the candidates. If you RANK using (ORDER BY votes DESC) then the candidate with the most votes has rank 1.

Show the party and RANK for constituency S14000024 in 2017. List the output by party
SELECT party, votes,
```sql
RANK() OVER (ORDER BY votes DESC) as posn
  FROM ge
 WHERE constituency = 'S14000024' AND yr = 2017
ORDER BY party
```

### 3.PARTITION BY

The 2015 election is a different PARTITION to the 2017 election. We only care about the order of votes for each year.

Use PARTITION to show the ranking of each party in S14000021 in each year. Include yr, party, votes and ranking (the party with the most votes is 1).
```sql
SELECT yr,party, votes,
      RANK() OVER (PARTITION BY yr ORDER BY votes DESC) as posn
  FROM ge
 WHERE constituency = 'S14000021'
ORDER BY party,yr
```

### 4.Edinburgh Constituency

Edinburgh constituencies are numbered S14000021 to S14000026.
Use PARTITION BY constituency to show the ranking of each party in Edinburgh in 2017. Order your results so the winners are shown first, then ordered by constituency.

```sql
SELECT constituency,party, votes,
RANK() OVER (PARTITION BY constituency ORDER BY votes DESC) as posn
  FROM ge
 WHERE constituency BETWEEN 'S14000021' AND 'S14000026'
   AND yr  = 2017
ORDER BY posn,constituency 
```

### 5.Winners Only

You can use SELECT within SELECT to pick out only the winners in Edinburgh.

Show the parties that won for each Edinburgh constituency in 2017.
```sql
SELECT a.constituency,a.party FROM
(SELECT constituency,party, 
RANK() OVER  (PARTITION BY constituency ORDER BY votes DESC) as posn
  FROM ge
 WHERE constituency BETWEEN 'S14000021' AND 'S14000026'
   AND yr  = 2017) as a
WHERE posn = 1
```

### 6.Scottish seats

You can use COUNT and GROUP BY to see how each party did in Scotland. Scottish constituencies start with 'S'
Show how many seats for each party in Scotland in 2017.

```sql
SELECT party, COUNT(*)
  FROM ge g1
 WHERE constituency LIKE 'S%'
   AND yr  = 2017
AND votes >=ALL(
SELECT votes FROM ge g2
WHERE constituency LIKE 'S%'
   AND yr  = 2017
   AND g1.constituency= g2.constituency

)
GROUP BY party
```

<!--a little complex-->