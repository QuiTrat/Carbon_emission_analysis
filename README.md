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
SELECT      ind.industry_group,
	          sum(prod.carbon_footprint_pcf) AS carbon_footprint_pcf
FROM        product_emissions prod
JOIN        industry_groups ind ON prod.industry_group_id = ind.id
GROUP BY    prod.industry_group_id
ORDER BY    sum(prod.carbon_footprint_pcf) desc LIMIT 10
```

| industry_group                                   | carbon_footprint_pcf |  
| -----------------------------------------------: | -------------------: | 
| Electrical Equipment and Machinery               | 9801558              | 
| Automobiles & Components                         | 2582264              |    
| Materials                                        | 577595               | 
| Technology Hardware & Equipment                  | 363776               | 
| Capital Goods                                    | 258712               | 
| "Food, Beverage & Tobacco"                       | 111131               | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 72486                | 
| Chemicals                                        | 62369                | 
| Software & Services                              | 46544                | 
| Media                                            | 23017                | 

 ![image](https://github.com/QuiTrat/Carbon_emission_analysis/assets/170105739/8d4f5f2d-4b75-4dbf-bb55-ea761ed6f1c7)


#### 2. Which products and their industries contribute the most to carbon emissions?

```
SELECT       prod.product_name,
			 ind.industry_group,
	         sum(prod.carbon_footprint_pcf) AS carbon_footprint_pcf			 
FROM        product_emissions prod
JOIN        industry_groups ind ON prod.industry_group_id = ind.id
GROUP BY    prod.industry_group_id
ORDER BY    sum(prod.carbon_footprint_pcf) desc LIMIT 10
```

| product_name                                                                                                                                                                                                                                                                                                                                                                            | industry_group                                   | carbon_footprint_pcf | 
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | -----------------------------------------------: | -------------------: | 
| ACTI9 IID K 2P 40A 30MA AC-TYPE RESIDUAL CURRENT CIRCUIT BREAKER                                                                                                                                                                                                                                                                                                                        | Electrical Equipment and Machinery               | 9801558              | 
| VW Polo V 1.6 TDI BlueMotion Technology                                                                                                                                                                                                                                                                                                                                                 | Automobiles & Components                         | 2582264              | 
| KURALON  fiber                                                                                                                                                                                                                                                                                                                                                                          | Materials                                        | 577595               | 
| Multifunction Printers                                                                                                                                                                                                                                                                                                                                                                  | Technology Hardware & Equipment                  | 363776               | 
| Office Chair                                                                                                                                                                                                                                                                                                                                                                            | Capital Goods                                    | 258712               | 
| Frosted Flakes(R) Cereal                                                                                                                                                                                                                                                                                                                                                                | "Food, Beverage & Tobacco"                       | 111131               | 
| Alliance HPLC (High Peformance Liquid Chromatography)  The Alliance is an HPLC that is unique in that it has a single set of electronic boards that control the functions for both the solvent delivery system and the autosampler in the liquid chromatograph.                                                                                                                         | "Pharmaceuticals, Biotechnology & Life Sciences" | 72486                | 
| Mobile Batteries                                                                                                                                                                                                                                                                                                                                                                        | Chemicals                                        | 62369                | 
| USB software                                                                                                                                                                                                                                                                                                                                                                            | Software & Services                              | 46544                | 
| "Bloomberg's standard-issue flat panel configuration (prior to 2010) was two 19\" panels mounted on a metal stand. In early 2010 Bloomberg engaged in the WRI Product Life Cycle Roadtest for this functional unit (cradle-to-grave). The functional unit has a lifespan of 5 years, so the emissions indicated [in this report] are the full emissions associated with that lifespan." | Media                                            | 23017                | 

![image](https://github.com/QuiTrat/Carbon_emission_analysis/assets/170105739/1da0215a-062a-49b3-8561-9d6d1b1c43a6)


#### 2. What are the industry groups of these products?
#### 3. What are the industries with the highest contribution to carbon emissions?
#### 4. What are the companies with the highest contribution to carbon emissions?
#### 5. What are the countries with the highest contribution to carbon emissions?
#### 6. What is the trend of carbon footprints (PCFs) over the years?
#### 7. Which industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time?
