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
    SELECT mountain,continent, height as meters, height*3.2084 as feet
        FROM (
            SELECT mountain, continent, height, RANK() OVER (PARTITION BY sub.continent ORDER BY height DESC) as ranking
            FROM (
            SELECT DISTINCT mountain.name as mountain, mountain.height, continent.name AS continent
                FROM encompasses
                  INNER JOIN continent
                    ON encompasses.continent = continent.name
                  INNER JOIN geo_mountain
                    ON encompasses.country = geo_mountain.country
                  INNER JOIN mountain
                    ON geo_mountain.mountain = mountain.name
                ) as sub
            ) as ranked
        WHERE ranked.ranking <=2
    ```

4.  Show the country name and ratio of border length to number of neighboring countries in descending order.

    ```sql
    SELECT country.name, ROUND(SUM(c.len)/SUM(c.count)) as ratio
    FROM (
    (
      SELECT country1 as country,SUM(length) as len, COUNT(country2) as count
      FROM borders
      GROUP BY country1
    )
    UNION ALL (
    (
      SELECT country2 as country,SUM(length) as len, COUNT(country1) as count
      FROM borders
      GROUP BY country2
    )
    )) as c
    INNER JOIN country ON c.country = country.code
    GROUP BY country.code
    ORDER BY ratio DESC
    ```

5.  Show a list of country name and projected populations in 10, 25,50 and 100 years if current demographic trends continue unabated.

    ```sql
    SELECT name,ROUND(population),year
    FROM (
        SELECT
        -- now
        country.name,
        country.population,
        EXTRACT(ISOYEAR FROM timestamp 'now') AS year
        FROM population
        INNER JOIN country ON population.country = country.code
    UNION ALL
        -- 10 years later
        SELECT
        country.name,
        country.population*POWER(1+COALESCE(population.population_growth,0)/100,10) as population,
        EXTRACT(ISOYEAR FROM timestamp 'now' + INTERVAL '10 years') AS year
        FROM population
        INNER JOIN country ON population.country = country.code
    UNION ALL
        -- 25 years later
        SELECT
        country.name,
        country.population*POWER(1+COALESCE(population.population_growth,0)/100,25) as population,
        EXTRACT(ISOYEAR FROM timestamp 'now' + INTERVAL '25 years') AS year
        FROM population
        INNER JOIN country ON population.country = country.code
    UNION ALL
        -- 50 years later
        SELECT
        country.name,
        country.population*POWER(1+COALESCE(population.population_growth,0)/100,50) as population,
        EXTRACT(ISOYEAR FROM timestamp 'now' + INTERVAL '50 years') AS year
        FROM population
        INNER JOIN country ON population.country = country.code
    UNION ALL
        -- 100 years later
        SELECT
        country.name,
        country.population*POWER(1+COALESCE(population.population_growth,0)/100,100) as population,
        EXTRACT(ISOYEAR FROM timestamp 'now' + INTERVAL '100 years') AS year
        FROM population
        INNER JOIN country ON population.country = country.code
        ) as sub
        ORDER BY name,year
    ```

6.  Names of all non-NATO countries that border a NATO country?

    ```sql
    SELECT country.name
    FROM
        (
        -- left country => right country
        SELECT country1 as country
        FROM borders
        WHERE country2 IN (SELECT country from ismember where organization='NATO')
        AND country1 NOT IN (SELECT country from ismember where organization='NATO')
    UNION
        -- right country => left country
        SELECT country2 as country
        FROM borders
        WHERE country1 IN (SELECT country from ismember where organization='NATO')
        AND country2 NOT IN (SELECT country from ismember where organization='NATO')
        ) as c
        -- get country name
        INNER JOIN country ON c.country = country.code
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
