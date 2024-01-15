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
#### Items Sales compared to Quantity in Stock
```SQL
SELECT P.productCode, 
       P.productName, 
       SUM(OD.quantityOrdered) AS Total_order, 
       SUM(P.quantityInStock) AS Total_inventory
FROM orderdetails OD
JOIN products P ON OD.productCode = P.productCode
GROUP BY P.productCode, P.productName
HAVING Total_order < 1000 AND Total_inventory > 150000
ORDER BY Total_inventory DESC;
```
#### Findings
This Analysis offers insights into how individual product items sales (Total_order) compare to its quantity in store (Total_inventory). The significance of this analysis is it can help Mint classics identify Products from each productlines that are not moving by setting the limit for sales as Total_order to be less than 1000 and Total quantity in stock as Total_inventory to be greater than 150000, therefore enabling the organisation drop any product from a productline.

#### Warehouse inventory worth and sales comparison 
```SQL
SELECT warehouseName, 
warehouseCode, 
SUM(quantityInStock * buyPrice) AS Worth, 
SUM(quantityOrdered * priceEach) AS Sales
FROM products P
JOIN warehouses W
    USING(warehouseCode)
JOIN orderdetails OD
     USING (productCode)
GROUP BY warehouseCode;
```
#### Number of quantity in stock for each Warehouse
```SQL
SELECT warehouseName, warehousePctCap, 
warehouseCode, 
SUM(P.quantityInStock) AS Total_Quantity 
FROM products P
JOIN warehouses W
    USING (warehouseCode)
GROUP BY WarehouseName, warehouseCode
ORDER BY Total_Quantity DESC;
```

```SQL
SELECT PR.warehouseCode, COUNT(orderNumber) FROM payments P
JOIN customers C
    USING (customerNumber)
JOIN orders O
    USING (customerNumber)
JOIN orderdetails OD
    USING (OrderNumber)
JOIN products PR
    USING (productCode)
WHERE  requiredDate < shippedDate
GROUP BY warehouseCode;
`` 
