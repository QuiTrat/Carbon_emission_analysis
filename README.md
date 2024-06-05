# Carbon_emission_analysis
## A. WHAT IS PROJECT ABOUT
![image](https://github.com/QuiTrat/Carbon_emission_analysis/assets/170105739/0c52349d-c5bc-471c-a53f-4bc9aae0bf88)

This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.

Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

## B. DATA STRUCTURE

#### PRODUCT_EMISSIONS

This table records the information about each product emissions in several industry groups. It show us information about the carbon footprint of the product, measured in CO2 equivalent and percentage of total carbon footprint in each production stage.

id           | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf | operations_percent_total_pcf | downstream_percent_total_pcf | 
| -----------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -------------------------: | ---------------------------: | ---------------------------: | 
| 10056-1-2014 | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10056-1-2015 | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10222-1-2013 | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                      | 17.36                        | 2.01                         | 
| 10261-1-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                      | 5.51                         | 63.84                        | 
| 10261-2-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                      | 4.51                         | 70.41                        | 

#### COUNTRY & INDUSTRY

This research collect data from over 140 companies in 30 industries over 28 countries.

![image](https://github.com/QuiTrat/Carbon_emission_analysis/assets/170105739/80021ec7-3374-4238-bc94-5d5aa8741329)

## C. RESEARCH

#### 1. What are the industries with the highest contribution to carbon emissions?

Hereunder is top 10 of industry with the highest contribution to carbon emissions
```
SELECT ROW_NUMBER() OVER (ORDER BY sub.carbon_footprint_pcf DESC) AS NoId,
		sub.industry_group,
		sub.carbon_footprint_pcf,
		sub.number_company_in_group
from 
(
  SELECT      ind.industry_group,
	        sum(prod.carbon_footprint_pcf) AS carbon_footprint_pcf,
			count(distinct(prod.company_id)) AS number_company_in_group
FROM        product_emissions prod
JOIN        industry_groups ind ON prod.industry_group_id = ind.id
GROUP BY    prod.industry_group_id
ORDER BY    sum(prod.carbon_footprint_pcf) desc LIMIT 10
)   AS sub
ORDER BY sub.carbon_footprint_pcf DESC

```
| NoId | industry_group                                   | carbon_footprint_pcf | number_company_in_group | 
| ---: | -----------------------------------------------: | -------------------: | ----------------------: | 
| 1    | Electrical Equipment and Machinery               | 9801558              | 7                       | 
| 2    | Automobiles & Components                         | 2582264              | 8                       | 
| 3    | Materials                                        | 577595               | 34                      | 
| 4    | Technology Hardware & Equipment                  | 363776               | 32                      | 
| 5    | Capital Goods                                    | 258712               | 15                      | 
| 6    | "Food, Beverage & Tobacco"                       | 111131               | 22                      | 
| 7    | "Pharmaceuticals, Biotechnology & Life Sciences" | 72486                | 1                       | 
| 8    | Chemicals                                        | 62369                | 8                       | 
| 9    | Software & Services                              | 46544                | 3                       | 
| 10   | Media                                            | 23017                | 2                       | 

 
 ![image](https://github.com/QuiTrat/Carbon_emission_analysis/assets/170105739/8d4f5f2d-4b75-4dbf-bb55-ea761ed6f1c7)


#### 2. Which products and their industries contribute the most to carbon emissions?

```
SELECT ROW_NUMBER() OVER (ORDER BY sub.carbon_footprint_pcf desc) AS NoId,
		sub.product_name,
		sub.industry_group,
		sub.carbon_footprint_pcf
from 
(
  SELECT     prod.product_name,
             ind.industry_group,
	     sum(prod.carbon_footprint_pcf) AS carbon_footprint_pcf			 
FROM        product_emissions prod
JOIN        industry_groups ind ON prod.industry_group_id = ind.id
GROUP BY    prod.product_name
ORDER BY    sum(prod.carbon_footprint_pcf) desc LIMIT 10)
  as sub
order by sub.carbon_footprint_pcf desc

```

| NoId | product_name                                                                                                                       | industry_group                     | carbon_footprint_pcf | 
| ---: | ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------: | -------------------: | 
| 1    | Wind Turbine G128 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3718044              | 
| 2    | Wind Turbine G132 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3276187              | 
| 3    | Wind Turbine G114 2 Megawats                                                                                                       | Electrical Equipment and Machinery | 1532608              | 
| 4    | Wind Turbine G90 2 Megawats                                                                                                        | Electrical Equipment and Machinery | 1251625              | 
| 5    | TCDE                                                                                                                               | Materials                          | 198150               | 
| 6    | Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | Automobiles & Components           | 191687               | 
| 7    | Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | Materials                          | 167000               | 
| 8    | Electric Motor                                                                                                                     | Capital Goods                      | 160655               | 
| 9    | Audi A6                                                                                                                            | Automobiles & Components           | 111282               | 
| 10   | Average of all GM vehicles produced and used in the 10 year life-cycle.                                                            | Automobiles & Components           | 100621               | 

![image](https://github.com/QuiTrat/Carbon_emission_analysis/assets/170105739/e7b0da92-3798-4581-9598-d7cfce5a20ee)

#### 4. What are the companies with the highest contribution to carbon emissions?

```
SELECT ROW_NUMBER() OVER (ORDER BY sub.carbon_footprint_pcf desc) AS NoId,
		sub.company_name,
		sub.industry_group,
		sub.carbon_footprint_pcf
from 
(SELECT       com.company_name,
			 ind.industry_group,
			 sum(prod.carbon_footprint_pcf) AS carbon_footprint_pcf
FROM        product_emissions prod
JOIN        industry_groups ind ON prod.industry_group_id = ind.id
JOIN		companies com ON prod.company_id = com.id
GROUP BY    com.company_name
ORDER BY    sum(prod.carbon_footprint_pcf) desc LIMIT 10)
  as sub
order by sub.carbon_footprint_pcf desc

```

| NoId | company_name                            | industry_group                     | carbon_footprint_pcf | 
| ---: | --------------------------------------: | ---------------------------------: | -------------------: | 
| 1    | "Gamesa Corporación Tecnológica, S.A."  | Electrical Equipment and Machinery | 9778464              | 
| 2    | Daimler AG                              | Automobiles & Components           | 1594300              | 
| 3    | Volkswagen AG                           | Automobiles & Components           | 655960               | 
| 4    | "Mitsubishi Gas Chemical Company, Inc." | Materials                          | 212016               | 
| 5    | "Hino Motors, Ltd."                     | Automobiles & Components           | 191687               | 
| 6    | Arcelor Mittal                          | Materials                          | 167007               | 
| 7    | Weg S/A                                 | Capital Goods                      | 160655               | 
| 8    | General Motors Company                  | Automobiles & Components           | 137007               | 
| 9    | "Lexmark International, Inc."           | Technology Hardware & Equipment    | 132012               | 
| 10   | "Daikin Industries, Ltd."               | Capital Goods                      | 105600               | 


#### 5. What are the countries with the highest contribution to carbon emissions?

```
SELECT ROW_NUMBER() OVER (ORDER BY sub.carbon_footprint_pcf desc) AS NoId,
		sub.country_name,
		sub.carbon_footprint_pcf
from 
(SELECT      countries.country_name,
			 sum(prod.carbon_footprint_pcf) AS carbon_footprint_pcf
FROM        product_emissions prod
JOIN		countries ON prod.country_id = countries.id
GROUP BY    countries.id
ORDER BY    sum(prod.carbon_footprint_pcf) desc LIMIT 10)
  as sub
order by sub.carbon_footprint_pcf desc

```

| NoId | country_name | carbon_footprint_pcf | 
| ---: | -----------: | -------------------: | 
| 1    | Spain        | 9786130              | 
| 2    | Germany      | 2251225              | 
| 3    | Japan        | 653237               | 
| 4    | USA          | 518381               | 
| 5    | South Korea  | 186965               | 
| 6    | Brazil       | 169337               | 
| 7    | Luxembourg   | 167007               | 
| 8    | Netherlands  | 70417                | 
| 9    | Taiwan       | 62875                | 
| 10   | India        | 24574                | 


#### 6. What is the trend of carbon footprints (PCFs) over the years?

```
SELECT 	 year,
	 sum(carbon_footprint_pcf) AS carbon_footprint_pcf
FROM	 product_emissions
GROUP BY year
ORDER BY year

```

| year | carbon_footprint_pcf | 
| ---: | -------------------: | 
| 2013 | 503857               | 
| 2014 | 624226               | 
| 2015 | 10840415             | 
| 2016 | 1640182              | 
| 2017 | 340271               | 

![image](https://github.com/QuiTrat/Carbon_emission_analysis/assets/170105739/c1b1523a-7ec7-4857-a22e-6ce891276418)


#### 7. Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?


