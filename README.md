# Worldbank-international Education
Q1: What are the top 10 regions in world having highest GDP's in PPP terms

select country_name,indicator_name, sum(value) as SUM_VALUE from `bigquery-public-data.world_bank_intl_education.international_education`
where indicator_name like '%GDP, PPP%' and country_name not in ('World','OECD members','High income','Low & middle income','Middle income','Upper middle income')
group by country_name,indicator_name
order by SUM_VALUE desc 
limit 10;

![image](https://user-images.githubusercontent.com/63834420/131241274-ac31a84c-8369-4875-ae91-6be57de8eae9.png)

Q2: Which are the countries with least number of females in labour force

select country_name,indicator_name,avg(value) as percent_average from `bigquery-public-data.world_bank_intl_education.international_education`
where indicator_name =  'Unemployment, female (% of female labor force)' and country_name not in ('World','OECD members','High income','Low & middle income','Middle income','Upper middle income')
group by country_name,indicator_name
order by percent_average asc 
limit 10;

![image](https://user-images.githubusercontent.com/63834420/131242761-3d049752-6c14-45fc-a178-f53a19acaa30.png)

Q3: What are the countries who had population growth greater than the average population growth in the year 2016

select country_name,avg(value) as gb_avg from `bigquery-public-data.world_bank_intl_education.international_education`
where indicator_name =  'Population growth (annual %)' and year=2016
group by country_name, value
having gb_avg>(select avg(value) from `bigquery-public-data.world_bank_intl_education.international_education`
where indicator_name='Population growth (annual %)' and year=2016)
order by gb_avg desc;

![image](https://user-images.githubusercontent.com/63834420/131242842-85babb2e-cfd2-40aa-93ee-202d72934c17.png)

Q4: What is the list of countries that are present only in the 'country summary' table but not in 'country series definition' table

select * from `bigquery-public-data.world_bank_intl_education.country_series_definitions` A
right join  `bigquery-public-data.world_bank_intl_education.country_summary` B on A.country_code = B.country_code
where A.country_code is Null;

![image](https://user-images.githubusercontent.com/63834420/131243012-78107ccd-0b02-4e82-b1ae-42388f4a673d.png)

Q5: What are the list of countries that are common in tables 'country series definition' and 'country summary' and what are their corresponding country codes

select A.country_code,B.short_name,description from `bigquery-public-data.world_bank_intl_education.country_series_definitions` A
inner join  `bigquery-public-data.world_bank_intl_education.country_summary` B on A.country_code = B.country_code;

![image](https://user-images.githubusercontent.com/63834420/131243131-501f492d-276f-404e-8f9c-d1e7d30d0ce2.png)
