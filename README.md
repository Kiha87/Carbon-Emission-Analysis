# Carbon-Emission-Analysis
Session 3.5: Project 2 - Carbon Emission Analysis
## What is the project about?
This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.

Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development

## Data Source: Where Our Data Comes From
Our dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

## Data Structure
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.
![image](https://github.com/user-attachments/assets/3144c97c-6d2d-4d32-baae-0e7c7ea58993)

## KEY FINDING
## Top 5 products contribute the most to carbon emissions

| product_name                 | Total_carbon| 
| ---------------------------: | ------: | 
| Wind Turbine G128 5 Megawats | 3718044 | 
| Wind Turbine G132 5 Megawats | 3276187 | 
| Wind Turbine G114 2 Megawats | 1532608 | 
| Wind Turbine G90 2 Megawats  | 1251625 | 
| TCDE                         | 198150  | 

SELECT product_name, SUM(carbon_footprint_pcf) AS Total_carbon
FROM product_emissions
GROUP BY product_name
ORDER BY Total_carbon DESC
LIMIT 5

### The industry groups of top 5 products with highest carbon emissions 
| product_name                 | industry_group                     | Total_carbon    | 
| ---------------------------: | ---------------------------------: | ------: | 
| Wind Turbine G128 5 Megawats | Electrical Equipment and Machinery | 3718044 | 
| Wind Turbine G132 5 Megawats | Electrical Equipment and Machinery | 3276187 | 
| Wind Turbine G114 2 Megawats | Electrical Equipment and Machinery | 1532608 | 
| Wind Turbine G90 2 Megawats  | Electrical Equipment and Machinery | 1251625 | 
| TCDE                         | Materials                          | 198150  | 

SELECT t1.product_name, t4.industry_group, sum(carbon_footprint_pcf) AS Total_carbon
FROM product_emissions t1
LEFT JOIN companies t2 ON t1.company_id=t2.id
LEFT JOIN countries t3 ON t1.country_id=t3.id
LEFT JOIN industry_groups t4 ON t1.industry_group_id=t4.id
GROUP BY t1.product_name, t4.industry_group
ORDER BY Total_carbon DESC
LIMIT 5

## The industries with the highest contribution to carbon emissions
| industry_group                     | Total_carbon     | 
| ---------------------------------: | ------: | 
| Electrical Equipment and Machinery | 9801558 | 

SELECT t4.industry_group, SUM(t1.carbon_footprint_pcf) as Total_carbon
FROM product_emissions t1
LEFT JOIN companies t2 ON t1.company_id=t2.id
LEFT JOIN countries t3 ON t1.country_id=t3.id
LEFT JOIN industry_groups t4 ON t1.industry_group_id=t4.id
GROUP BY t4.industry_group
ORDER BY Total_carbon DESC 
LIMIT 1
## The companies with the highest contribution to carbon emissions
| company_name                           | sum     | 
| -------------------------------------: | ------: | 
| "Gamesa Corporación Tecnológica, S.A." | 9778464 | 

SELECT t2.company_name, SUM(t1.carbon_footprint_pcf) as Total_carbon
FROM product_emissions t1
LEFT JOIN companies t2 ON t1.company_id=t2.id
LEFT JOIN countries t3 ON t1.country_id=t3.id
LEFT JOIN industry_groups t4 ON t1.industry_group_id=t4.id
GROUP BY t2.company_name
ORDER BY Total_carbon DESC 
LIMIT 1

## The country with the highest contribution to carbon emissions
| country_name | Total_carbon     | 
| -----------: | ------: | 
| Indonesia    | 9801558 | 

SELECT t3.country_name, SUM(t1.carbon_footprint_pcf) as Total_carbon
FROM product_emissions t1
LEFT JOIN companies t2 ON t1.company_id=t2.id
LEFT JOIN countries t3 ON t1.country_id=t3.id
LEFT JOIN industry_groups t4 ON t1.industry_group_id=t4.id
GROUP BY t3.country_name
ORDER BY Total_carbon DESC 
LIMIT 1

## The trend of carbon footprints (PCFs) tend to decrease from 2015
| year | Total_carbon               | 
| ---: | ------------------------: | 
| 2013 | 503857                    | 
| 2014 | 624226                    | 
| 2015 | 10840415                  | 
| 2016 | 1640182                   | 
| 2017 | 340271                    | 

SELECT year, sum(carbon_footprint_pcf) AS Total_carbon
FROM product_emissions
GROUP BY year
ORDER BY year

## "Tobacco" and "Technology hardware and Equipment" have demonstrated the most notable decrease in carbon footprints (PCFs) over time

![image](https://github.com/user-attachments/assets/cba48614-9879-4d8d-93b6-b4d578a52dee)

SELECT t1.year, t4.industry_group, sum(carbon_footprint_pcf) AS Total_carbon
FROM product_emissions t1
LEFT JOIN companies t2 ON t1.company_id=t2.id
LEFT JOIN countries t3 ON t1.country_id=t3.id
LEFT JOIN industry_groups t4 ON t1.industry_group_id=t4.id
GROUP BY industry_group, year
ORDER BY industry_group, year

## Average pcf per 1kg weight
| sum_weight      | sum_pcf  | average per kg
| --------------: | -------: | -------:
| 2479662.41      | 13948951 | 5.63

SELECT round(sum(t1.weight_kg),2) AS sum_weight, sum(t1.carbon_footprint_pcf) AS sum_pcf
FROM product_emissions t1
LEFT JOIN companies t2 ON t1.company_id=t2.id
LEFT JOIN countries t3 ON t1.country_id=t3.id
LEFT JOIN industry_groups t4 ON t1.industry_group_id=t4.id



