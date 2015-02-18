#Lab 1

####Name: Christopher Lillthors

1.  Show the names, populations and capitals of         the provinces (län) of Sweden order by      population with largest first.
    ```sql
    SELECT name,population,capital
    FROM province
    WHERE country='S' --S = Sweden
    ORDER BY population DESC
    ```
2.  Names of the organizations in Europe containing the word 'Nuclear', with an unknown date of establishment.
    ```sql
    SELECT name
    FROM organization
    WHERE name LIKE '%Nuclear%' AND
    established IS NULL
    ```
3.  In each continent, the two highest mountains and their heights in meters and in feet. (Note I have changed this problem to an easier variant. I will show the solution for the harder one later, but I don't want people to get stuck on this problem now. Getting time for Lab \#2 is more important.)
    ```sql
    SELECT DISTINCT mountain.name, mountain.height, continent.name AS Continent
FROM encompasses
  INNER JOIN continent
    ON encompasses.continent = continent.name
  INNER JOIN geo_mountain
    ON encompasses.country = geo_mountain.country
  INNER JOIN mountain
    ON geo_mountain.mountain = mountain.name
GROUP BY continent.name,mountain.name --Group by doesn't work.
    ```
4.  Show the country name and ratio of border length to number of neighboring countries in descending order.
    ```sql

    ```
5.  Show a list of country name and projected populations in 10, 25,50 and 100 years if current demographic trends continue unabated.
    ```sql

    ```
6.  Names of all non-NATO countries that border a NATO country?
    ```sql

    ```
7.  Give all countries that can be reached overland from Mexico. Thus Canada is in answer, but Sweden is not. Hint, use recursion.
    ```sql

    ```
8.  Create a view EightThousanders(name,mountains,height,coordinates) which
includes the mountains over or equal to the height of 8000 meters. Query for the countries including EightThousanders. Try to avoid materializing the whole Mountain relation. Verify via EXPLAIN ANALYSE.
    ```sql

    ```
9.  Write rules to enable updates to EightThousanders to be reflected back to Mountain. For example be able to rectify the injustice of Everest being taller than K2!
    ```sql

    ```
