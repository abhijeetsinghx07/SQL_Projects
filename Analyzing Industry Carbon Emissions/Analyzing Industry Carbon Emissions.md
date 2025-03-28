
___

### Food, clothing, footwear, and appliances all contribute to more than 75% of the world's carbon emissions.
___

![image](https://user-images.githubusercontent.com/108348003/211213928-ab581c1d-fe7d-453f-8c37-e00ce1f05f28.png)


**Product carbon footprints (PCFs) for various businesses are contained in the data, which is publically accessible online. The PCFs are the CO2 emissions from a certain product that may be attributed to greenhouse gas emissions (carbon dioxide equivalent).**

**PCFs, or product carbon footprints, are becoming more important in helping businesses and consumers make sustainability-related decisions.**

***Read more about carbon footprints here(https://www.nature.com/articles/s41597-022-01178-9#Tab1).***

### 1. Coca-Cola's emissions?

Let's first examine a small portion of the data: the emissions reported by Coca-Cola. Since Coke is truly made up of numerous businesses all around the world, we'll make sure that our query returns information for every business with a name that begins "Coca-Cola." We will only display the top six findings since Coke previously reported on every single one of its products.

```sql
SELECT * 
FROM product_emissions
WHERE company LIKE 'Coca-Cola%'
LIMIT 6;
```
### Providing some background 
- Upstream emissions(upstream_percent_total_pcf): Emissions that happen before to a company's own operations, such as emissions produced by suppliers' bottle-making processes that Coke purchases.
- Operations emissions(operations_percent_total_pcf): Emissions that a business directly produces, such as when bottling Coke.
- Downstream emissions(downstream_percent_total_pcf): Emissions that happen after a product leaves the corporation, like after Coca-Cola sells beverages to McDonald's.


### 2. Nestle emissions?

Let's look at a small subset of the data: emissions reported by Nestle. 
```sql
-- Select all fields where the company name is Nestle, limiting to the first six results
SELECT *
FROM product_emissions	
WHERE company LIKE 'Nestl%'
LIMIT 6;
```

### 3. Most recent data

During this code-along, we'll concentrate on recent emissions data. The most current data collection date was when?
```sql
-- Return the most recent year for which data was collected
SELECT MAX(year)
FROM product_emissions;
```

### 4. Targeting major emitters

Return the industry_group and a rounded total of carbon_footprint_pcf for each industry, aliasing as total_industry_footprint.
Limit to data for 2017 and order by total_industry_footprint.
```sql
SELECT industry_group, ROUND(SUM(carbon_footprint_pcf), 1) AS total_industry_footprint
FROM product_emissions
GROUP BY industry_group, year
HAVING year = 2017
ORDER BY total_industry_footprint DESC;
```
### 5. India emissions in 2017 ?
```sql
SELECT *
FROM product_emissions
WHERE country='India' AND year='2017';
```

### 6. Industry representation

The carbon footprint of the materials sector in 2017 appears to have been rather large. But what if that is simply a result of the dataset's high concentration of businesses in the Materials sector? Let's look at the sectors that year had the highest representation.
```sql
-- Return the industry groups and a count of the number of records for each group
-- Limit the results to only those from 2017 and alias the count as count_industry
-- Order by count_industry, descending
SELECT industry_group, COUNT(*) AS count_industry
FROM product_emissions
GROUP BY industry_group, year
HAVING year = 2017
ORDER BY count_industry DESC;
```


### 7. Capital Goods industry

The Materials industry is the largest emitter, although having a lower representation in our sample than a few other sectors. It is comparable to the capital goods sector. Take a look at the companies and products the capital goods sector reported for 2017.
```sql
-- Return industry_group, company, and product_name for all records reporting in the Capital Goods industry during 2017
SELECT industry_group, company, product_name
FROM product_emissions
WHERE industry_group = 'Capital Goods' AND year= '2017';
```
### 8. Capital Goods lifecycle emissions

Daikin is an air conditioning and refrigeration manufacturer. Let's examine emissions over the course of a Daikin product's life.
```sql
-- Return product_name, company, and all stages of pcf emissions for Daikin in 2017
SELECT company, product_name, SUM(carbon_footprint_pcf) AS total_carbon_footprint,
                              upstream_percent_total_pcf,
                              operations_percent_total_pcf,
                              downstream_percent_total_pcf
FROM product_emissions
GROUP BY product_name, company, upstream_percent_total_pcf,
                              operations_percent_total_pcf,
                              downstream_percent_total_pcf,year
HAVING company= 'Daikin Industries, Ltd.'  AND year='2017'
ORDER BY total_carbon_footprint DESC ;
```

### 9. Country representation

Let's take a look at emissions by country. 
```sql
-- Group by country
-- Select country and the sum of total carbon_footprint_pcf by country, aliasing as total_country_footprint
SELECT country, ROUND(SUM(carbon_footprint_pcf),1) AS total_carbon_footprint
FROM product_emissions
GROUP BY country
ORDER BY total_carbon_footprint DESC ;
```

### 10. Spain's high emissions for this reason

Spain has a lot of emissions!. Let's take a look at emissions by Spain. 
Where do they come from?
```sql
SELECT company, ROUND(SUM(carbon_footprint_pcf),1) AS total_carbon_footprint 
FROM product_emissions
GROUP BY company, country
HAVING country = 'Spain'
ORDER BY total_carbon_footprint DESC ;
```



