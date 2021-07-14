---
layout: post
title: "HackerRank SQL"
subtitle: "SQL challenges, sorted"
date: 2021-04-14 11:19:13 -1300
background: '/img/posts/hacker-rank/bg-hacker-rank.png'
---

# Hacker Rank #

HackerRank is a technology hiring platform that is the standard for assessing developer skills for over 2,000+ companies around the world. By enabling tech recruiters and hiring managers to objectively evaluate talent at every stage of the recruiting process, HackerRank helps companies hire skilled developers and innovate faster.

<br>
<hr>
<br>

# Challenges #

<br>
<h2>Occupations</h2>

Pivot the Occupation column in OCCUPATIONS so that each Name is sorted alphabetically and displayed underneath its corresponding Occupation. The output column headers should be Doctor, Professor, Singer, and Actor, respectively.

Note: Print NULL when there are no more names corresponding to an occupation.

The OCCUPATIONS table is described as follows:

<table>
    <tr>
        <td>COLUMN</td><td>TYPE</td>
    </tr>
    <tr>
        <td>Name</td><td>String</td>
    </tr>
    <tr>
        <td>Occupation</td><td>String</td>
    </tr>
</table>

Occupation will only contain one of the following values: Doctor, Professor, Singer or Actor.


**Sample Output**

<table>
    <tr>
        <td>Jenny</td><td>Ashley</td><td>Meera</td><td>Jane</td>
    </tr>
    <tr>
        <td>Samantha</td><td>Christeen</td><td>Priya</td><td>Julia</td>
    </tr>
    <tr>
        <td>NULL</td><td>Ketty</td><td>NULL</td><td>Maria</td>
    </tr>
</table>

**Explanation**

The first column is an alphabetically ordered list of Doctor names.
The second column is an alphabetically ordered list of Professor names.
The third column is an alphabetically ordered list of Singer names.
The fourth column is an alphabetically ordered list of Actor names.
The empty cell data for columns with less than the maximum number of names per occupation (in this case, the Professor and Actor columns) are filled with NULL values.

<h2>Solution:</h2>

For this challenge I divide the problem into two parts. In order to transpose the table we first need to create another table with a subquery, checking professions row by row. This will return a table with one value (name) per row; other colums will be NULL, plus the count of that profession at the very end, named ROW_NUM.

```sql
SET @d = 0, @p = 0, @s = 0, @a = 0;
SELECT MIN(DOCTOR_NAMES), MIN(PROFESSOR_NAMES), MIN(SINGER_NAMES), MIN(ACTOR_NAMES)
FROM
    (
    SELECT
        CASE WHEN OCCUPATION = 'Doctor' THEN NAME END AS DOCTOR_NAMES,
        CASE WHEN OCCUPATION = 'Professor' THEN NAME END AS PROFESSOR_NAMES,
        CASE WHEN OCCUPATION = 'Singer' THEN NAME END AS SINGER_NAMES,
        CASE WHEN OCCUPATION = 'Actor' THEN NAME END AS ACTOR_NAMES,
        CASE
            WHEN OCCUPATION = 'Doctor' THEN (@d := @d + 1)
            WHEN OCCUPATION = 'Professor' THEN (@p := @p + 1)
            WHEN OCCUPATION = 'Singer' THEN (@s := @s + 1)
            WHEN OCCUPATION = 'Actor' THEN (@a := @a + 1)
            END AS ROW_NUM
    FROM OCCUPATIONS
    ORDER BY NAME
    ) AS TEMP
GROUP BY ROW_NUM;
```
<br>
Once executed this subquery and stored as TEMP, we use GROUP BY ROW_NUM. This, combined with the aggregation functions we used at the beggining, will return the expected table. I used MIN but could have been any aggregation, since only action needed here is to take the names out of the NULLs.
<br><br>
<!-- ---------------------------- -->
<hr>

<br>
<h2>Weather Observation Station 19</h2>

Consider P₁(a, c) and P₂(b, d) to be two points on a 2D plane where (a, b) are the respective minimum and maximum values of Northern Latitude (LAT_N) and (c, d) are the respective minimum and maximum values of Western Longitude (LONG_W) in STATION.

Query the Euclidean Distance between points P₁ and P₂ and format your answer to display 4 decimal digits.

**Input Format**

The STATION table is described as follows:

<table>
    <tr>
        <td>FIELD</td><td>TYPE</td>
    </tr>
    <tr>
        <td>ID</td><td>Number</td>
    </tr>
    <tr>
        <td>CITY</td><td>VARCHAR(21)</td>
    </tr>
    <tr>
        <td>STATE</td><td>VARCHAR(2)</td>
    </tr>
    <tr>
        <td>LAT_N</td><td>NUMBER</td>
    </tr>
    <tr>
        <td>LONG_W</td><td>NUMBER</td>
    </tr>
</table>

where LAT_N is the northern latitude and LONG_W is the western longitude.

<h2>Soution:</h2>

```sql
SELECT TRUNCATE(SQRT(POW(MAX(lat_n) - MIN(lat_n), 2) + POW(MAX(long_w) - MIN(long_w), 2)), 4)
FROM Station
```

For this approach, calculated the distance between the two points on a real line as the absolute value of the numerical difference of their coordinates P₁ and P₂. This is the square root of the sum of their squared differences.

<br><br>

<p class="cen">More challenges coming soon!</p>

<br><br>


<!-- <br>
<h2>Challenge X:</h2>

<h2>Soution:</h2>

```sql

```



<br>
<h2>Challenge X:</h2>

<h2>Soution:</h2>

```sql

```


<br>
<h2>Challenge X:</h2>

<h2>Soution:</h2>

```sql

```


<br>
<h2>Challenge X:</h2>

<h2>Soution:</h2>

```sql

```
 -->
