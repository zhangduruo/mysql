

**1. Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents.**

```mysql
-- topN
SELECT t1.name, t1.continent
FROM
(SELECT name, continent, population
FROM world AS w1
WHERE (SELECT COUNT(*) FROM world AS w2 WHERE w1.population < w2.population AND w1.continent = w2.continent) < 1) AS t1
JOIN 
(SELECT name, continent, population
FROM world AS w1
WHERE (SELECT COUNT(*) FROM world AS w2 WHERE w1.population < w2.population
AND w1.continent = w2.continent) = 1) AS t2
ON t1.continent = t2.continent
WHERE t1.population > 3*t2.population
```



**2. Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show** **name** **continent** **and** **population**.

```mysql
-- 内联结
SELECT name, w.continent, population 
FROM world AS w INNER JOIN (SELECT continent, max(population) AS mp FROM world GROUP BY continent) AS t
ON w.continent = t.continent
WHERE t.mp <= 25000000
```

**3. Find the largest country (by area) in each continent, show the continent, the name and the area**:

```mysql
-- all的用法，自联结
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND population>0)
-- 另一种
SELECT continent, name, area FROM world x
  WHERE area =
    (SELECT max(area) FROM world y
        WHERE y.continent=x.continent
          AND population>0)
```

