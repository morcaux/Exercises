# SQLZOO Solutions
This is currently an ongoing project and I found that SQLZOO is a good platform for people who want to learn coding SQL. 
I intend to make an instructional manual in Taglish (Tagalog-English) or Tagalog (Philippine's vernacular)   

## SELECT basics

Click here to get started [SELECT basics ](https://sqlzoo.net/wiki/SELECT_basics) 
### Introducing the world table of countries
**1.** The example uses a WHERE clause to show the population of 'France'. Note that strings (pieces of text that are data) should be in 'single quotes';
**Modify it to show the population of Germany**
```sql
  SELECT population FROM world
  WHERE name = 'Germany'
```
**2.**
Checking a list The word IN allows us to check if an item is in a list. The example shows the name and population for the countries 'Brazil', 'Russia', 'India' and 'China'.
**Show the name and the population for 'Sweden', 'Norway' and 'Denmark'.**
```sql
SELECT name, population FROM world
  WHERE name IN ('Sweden', 'Norway', 'Denmark');
```
**3.** 
Which countries are not too small and not too big? BETWEEN allows range checking (range specified is inclusive of boundary values). The example below shows countries with an area of 250,000-300,000 sq. km. Modify it to show the country and the area for countries with an area between 200,000 and 250,000.
```sql
SELECT name, area FROM world
  WHERE area BETWEEN 200000 AND 250000 
```
## SELECT within SELECT  [SELECT within SELECT](https://sqlzoo.net/wiki/SELECT_within_SELECT_Tutorial)
**1.** List each country name where the population is larger than that of 'Russia'.
```sql
 SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
```
**2.** Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
```sql
 SELECT name FROM world
  WHERE continent = 'Europe' AND gdp / population >
     (SELECT gdp / population FROM world
      WHERE name='United Kingdom')
```
**3.** List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
```sql 
SELECT name, continent FROM world
  WHERE continent LIKE
     (SELECT continent FROM world
      WHERE name= 'Argentina')
OR continent LIKE
   (SELECT continent FROM world
      WHERE name= 'Australia')
ORDER BY name
```
**4.** Which country has a population that is more than Canada but less than Poland? Show the name and the population.
```sql
SELECT name, population FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name= 'Canada')
AND 
population <
   (SELECT population FROM world
      WHERE name= 'Poland')
```
**5.** Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.
```sql
SELECT name, CONCAT(ROUND(100*population / 
             (SELECT population FROM world
              WHERE name = 'Germany'),0),'%') as percentage
FROM world 
WHERE continent = 'Europe'
```
**6.** Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)
```sql
SELECT name FROM world 
WHERE gdp > 
(SELECT gdp FROM world WHERE continent = 'Europe' ORDER BY gdp DESC LIMIT 1)
```
**7.** Find the largest country (by area) in each continent, show the continent, the name and the area:
```sql
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area>0)
```
**8.** List each continent and the name of the country that comes first alphabetically.
```sql
SELECT continent, name FROM world x
  WHERE name <= ALL
    (SELECT name FROM world y
        WHERE y.continent=x.continent 
      )
```
**9.** Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.
```sql
SELECT name, continent, population FROM world x
WHERE 25000000 >= ALL(SELECT population FROM world y WHERE x.continent=y.continent) 
```
## [SUM and COUNT](https://sqlzoo.net/wiki/SUM_and_COUNT)
**1.** Show the total population of the world.
```sql
SELECT SUM(population)
FROM world
```
**2.** List all the continents - just once each
```sql
SELECT DISTINCT continent FROM world
```
**3.** Give the total GDP of Africa
```sql
SELECT SUM(gdp) as TotalGDPAfrica
FROM world 
WHERE continent = 'Africa'
```
**4.** How many countries have an area of at least 1000000
```sql
SELECT COUNT(name) FROM world
WHERE area > 1000000
```
**5.**  What is the total population of ('Estonia', 'Latvia', 'Lithuania')
```sql
SELECT SUM(population) FROM world
WHERE name IN ('Estonia', 'Latvia', 'Lithuania')
```
**6.** For each continent show the continent and number of countries.
```sql
SELECT continent, COUNT(name) 
FROM world
GROUP BY continent
```
**7.** For each continent show the continent and number of countries with populations of at least 10 million
```sql
SELECT continent, COUNT(name) 
FROM world
WHERE population > 10000000
GROUP BY continent
```
**8.** List the continents that have a total population of at least 100 million.
```sql
SELECT continent
  FROM world
 GROUP BY continent
HAVING SUM(population)>100000000
```

## [JOIN](https://sqlzoo.net/wiki/The_JOIN_operation)
**1.** Modify it to show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'
```sql
SELECT matchid, player
FROM goal
WHERE teamid = 'GER'
```
**2.** Show id, stadium, team1, team2 for just game 1012
```sql
SELECT id,stadium,team1,team2
  FROM game
WHERE id = 1012
```
**3.** Modify it to show the player, teamid, stadium and mdate for every German goal.
```sql 
SELECT player, teamid, stadium, mdate
FROM game
JOIN goal ON (game.id = goal.matchid)
WHERE teamid = 'GER'
```
**4.** Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'
```sql
SELECT team1, team2, player
FROM game
JOIN goal ON (game.id = goal.matchid)
WHERE player LIKE 'Mario%'
```
**5.** Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10
```sql
SELECT player, teamid, coach, gtime
  FROM goal 
JOIN eteam ON(eteam.id = goal.teamid)
 WHERE gtime<=10
```
**6.** List the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.
```sql
SELECT mdate, teamname
  FROM eteam
JOIN game ON(eteam.id = game.team1)
WHERE coach = 'Fernando Santos'
```
**7.** List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'
```sql
SELECT player
  FROM goal
JOIN game ON(game.id = goal.matchid)
WHERE stadium = 'National Stadium, Warsaw'
```
**8.** The example query shows all goals scored in the Germany-Greece quarterfinal.
Instead show the name of all players who scored a goal against Germany.
```sql
SELECT DISTINCT player
  FROM game JOIN goal ON matchid = id 
    WHERE  (teamid!='GER' AND (team1='GER' OR team2='GER'))
```
**9.** Show teamname and the total number of goals scored.
```sql
SELECT teamname, COUNT(*) 
  FROM eteam JOIN goal ON id=teamid
 GROUP BY teamname
```
**10.** Show the stadium and the number of goals scored in each stadium.
```sql
SELECT stadium, COUNT(*) 
  FROM game JOIN goal ON id=matchid
 GROUP BY stadium
```
**11.** For every match involving 'POL', show the matchid, date and the number of goals scored.
```sql
SELECT matchid,mdate, teamid
  FROM game JOIN goal ON matchid = id 
 WHERE (team1 = 'POL' OR team2 = 'POL')
```
**12.** For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'
```sql
SELECT matchid, mdate, COUNT(PLAYER) as GERGOALS
  FROM game JOIN goal ON matchid = id 
 WHERE (team1 = 'GER' OR team2 = 'GER') AND teamid = 'GER'
 GROUP BY matchid, mdate
```
**13.** wala di ko pa alam sagot
```sql
```
