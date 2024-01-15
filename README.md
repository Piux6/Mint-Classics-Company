# Mint-Classics-Company
A Data Analytics project that helps to analyze data in a relational database with the goal of supporting inventory-related business decisions that lead to the closure of a storage facility.
### BUSINESS UNDERSTANDING: 
Mint Classics Company, a retailer of classic model cars and other vehicles, is looking at closing one of their storage facilities. To support a data-based business decision, they are looking for suggestions and recommendations for reorganizing or reducing inventory, while still maintaining timely service to their customers. 
### BUSINESS OBJECTIVE:
As a data analyst, I have been asked to use MySQL Workbench to familiarize myself with the general business by examining the current data. I have been provided with a data model and sample data tables to review. I will then need to isolate and identify those parts of the data that could be useful in deciding how to reduce inventory by writting queries to answer questions like these:
-  If items stored were rearranged, could a warehouse be eliminated?
-  How are inventory numbers related to sales figures? Do the inventory counts seem appropriate for each item?
-  Are items that are not moving stored? Are there any items candidates for being dropped from the product line?
The answers to questions like those should help you to formulate suggestions and recommendations for reducing inventory with the goal of closing one of the storage facilities. 
### PROJECT OBJECTIVE:
1. Explore products currently in inventory.
2. Determine important factors that may influence inventory reorganization/reduction.
3. Provide analytic insights and data-driven recommendations.

### DATA ANALYSIS:
```SQL
SELECT P.productCode, 
       P.productName, 
       SUM(OD.quantityOrdered) AS Total_order, 
       SUM(P.quantityInStock) AS Total_inventory
FROM orderdetails OD
JOIN products P ON OD.productCode = P.productCode
GROUP BY P.productCode, P.productName
HAVING Total_order < 1000 AND Total_inventory > 150000
ORDER BY Total_inventory DESC 

