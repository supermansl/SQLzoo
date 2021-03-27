# More JOIN operations

## Harder Questions

### 11.Busy years for Rock Hudson

Which were the busiest years for 'Rock Hudson', show the year and the number of movies he made each year for any year in which he made more than 2 movies.

```sql
SELECT yr,COUNT(title) FROM
  movie JOIN casting ON movie.id=movieid
        JOIN actor   ON actorid=actor.id
WHERE name='Doris Day'
GROUP BY yr
HAVING COUNT(title) > 1
```



### 12.Lead actor in Julie Andrews movies

List the film title and the leading actor for all of the films 'Julie Andrews' played in.

*you get "Little Miss Marker twice"?*

Julie Andrews starred in the 1980 remake of Little Miss Marker and not the original(1934).

Title is not a unique field, create a table of IDs in your subquery

```sql
SELECT title , a.name FROM
movie m JOIN casting c ON m.id=c.movieid JOIN actor a ON c.actorid=a.id
WHERE m.id IN
(SELECT movieid FROM casting
WHERE actorid IN (
  SELECT id FROM actor
  WHERE name='Julie Andrews')) AND ord=1
```



### 13.Actors with 15 leading roles.

Obtain a list, in alphabetical order, of actors who've had at least 15 **starring** roles.

```sql
SELECT name FROM casting JOIN actor ON actorid=id 
WHERE ord=1
GROUP BY name
HAVING COUNT(movieid)>=15
ORDER BY name
```



### 14.List the films released in the year 1978 ordered by the number of actors in the cast, then by title.

```sql
SELECT title, COUNT(movie.id) FROM 
movie JOIN casting ON movie.id = casting.movieid JOIN actor ON casting.actorid=actor.id
WHERE yr=1978
GROUP BY title
ORDER BY COUNT(movie.id) DESC,title
```



### 15. List all the people who have worked with 'Art Garfunkel'.

```sql
SELECT name FROM casting JOIN actor ON id=actorid 
WHERE movieid IN 
(SELECT movieid FROM casting JOIN actor ON id=actorid WHERE name = 'Art Garfunkel') AND name!='Art Garfunkel'
```


