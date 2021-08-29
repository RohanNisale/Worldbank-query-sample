# Worldbank-international Education
Q1: What are the top 10 regions in world having highest GDP's in PPP terms

SELECT 
    country_name, indicator_name, SUM(value) AS SUM_VALUE
FROM
    `bigquery-public-data.world_bank_intl_education.international_education`
WHERE
    indicator_name LIKE '%GDP, PPP%'
        AND country_name NOT IN ('World' , 'OECD members',
        'High income',
        'Low & middle income',
        'Middle income',
        'Upper middle income')
GROUP BY country_name , indicator_name
ORDER BY SUM_VALUE DESC
LIMIT 10;

![image](https://user-images.githubusercontent.com/63834420/131241274-ac31a84c-8369-4875-ae91-6be57de8eae9.png)

Q2: Which are the countries with least number of females in labour force

SELECT 
    country_name, indicator_name, AVG(value) AS percent_average
FROM
    `bigquery-public-data.world_bank_intl_education.international_education`
WHERE
    indicator_name = 'Unemployment, female (% of female labor force)'
        AND country_name NOT IN ('World' , 'OECD members',
        'High income',
        'Low & middle income',
        'Middle income',
        'Upper middle income')
GROUP BY country_name , indicator_name
ORDER BY percent_average ASC
LIMIT 10;

![image](https://user-images.githubusercontent.com/63834420/131242761-3d049752-6c14-45fc-a178-f53a19acaa30.png)

Q3: What are the countries who had population growth greater than the average population growth in the year 2016

SELECT 
    country_name, AVG(value) AS gb_avg
FROM
    `bigquery-public-data.world_bank_intl_education.international_education`
WHERE
    indicator_name = 'Population growth (annual %)'
        AND year = 2016
GROUP BY country_name , value
HAVING gb_avg > (SELECT 
        AVG(value)
    FROM
        `bigquery-public-data.world_bank_intl_education.international_education`
    WHERE
        indicator_name = 'Population growth (annual %)'
            AND year = 2016)
ORDER BY gb_avg DESC;

![image](https://user-images.githubusercontent.com/63834420/131242842-85babb2e-cfd2-40aa-93ee-202d72934c17.png)

Q4: What is the list of countries that are present only in the 'country summary' table but not in 'country series definition' table

SELECT 
    *
FROM
    `bigquery-public-data.world_bank_intl_education.country_series_definitions` A
        RIGHT JOIN
    `bigquery-public-data.world_bank_intl_education.country_summary` B ON A.country_code = B.country_code
WHERE
    A.country_code IS NULL;

![image](https://user-images.githubusercontent.com/63834420/131243012-78107ccd-0b02-4e82-b1ae-42388f4a673d.png)

Q5: What are the list of countries that are common in tables 'country series definition' and 'country summary' and what are their corresponding country codes

SELECT 
    A.country_code, B.short_name, description
FROM
    `bigquery-public-data.world_bank_intl_education.country_series_definitions` A
        INNER JOIN
    `bigquery-public-data.world_bank_intl_education.country_summary` B ON A.country_code = B.country_code;

![image](https://user-images.githubusercontent.com/63834420/131243131-501f492d-276f-404e-8f9c-d1e7d30d0ce2.png)
