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
## SUM and COUNT [SUM and COUNT](https://sqlzoo.net/wiki/SUM_and_COUNT)
## JOIN [JOIN](https://sqlzoo.net/wiki/The_JOIN_operation)





