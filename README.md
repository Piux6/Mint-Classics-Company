# Mint-Classics-Company
## Table of Contents
- [BUSINESS UNDERSTANDING](business-understanding)
- [BUSINESS OBJECTIVE](business-objective)
- [PROJECT OBJECTIVE](projective-objective)
- [DATA ANALYSIS](data-analysis)
- [RECOMMENDATIONS](recommendations)

  
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
ORDER BY Total_inventory DESC
LIMIT 3;
```
![sql1](https://github.com/Piux6/Mint-Classics-Company/assets/128375363/7abcc8d8-401b-4440-938b-a121e6f53e8f)


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
![sql3](https://github.com/Piux6/Mint-Classics-Company/assets/128375363/5a917392-4bc0-4575-8101-421ee916c83f)


#### Findings
The Analysis of warehouse inventory worth and sales comparison provides insights into the value of a each warehouse by calculating the worth of products in stock and returns on quantity ordered from each warehouse. This analysis will help Mint Classics make decisions on rearranging inventory and possibly eliminating a warehoue if needed.

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
![sql2](https://github.com/Piux6/Mint-Classics-Company/assets/128375363/b1321250-afb9-40cc-83b7-d42bc4cc7a1b)

#### Findings 
The Analysis of Number of quantity in stock for each warehouse by calculating the sum of products in stock and returns on quantity ordered from each warehouse. This analysis will help Mint Classics make decisions on rearranging inventory and possibly eliminating a warehoue if needed.

#### Warehouses with delayed shippments
```SQL
SELECT PR.warehouseCode, COUNT(orderNumber)
FROM payments P
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
```
![sql3](https://github.com/Piux6/Mint-Classics-Company/assets/128375363/bb15df18-fd48-47fb-8c6c-d918bdac9200)


#### Findings 
This analysis presents the warehouse with shipment dates later than the supposed delivery date (requiredDate). This will enable Mint classics easily identify and solve problems associated with delayed shippment as it plans to improve order efficiency and deliver within 24 Hours.  

#### Office with delayed shippments
```SQL
SELECT employeeNumber, 
       firstName, 
       lastName, 
       OFC.country, 
       OFC.officeCode, 
       COUNT(orderNumber) 
FROM payments P
JOIN customers C
    USING (customerNumber)
JOIN employees E
     ON salesRepEmployeeNumber = employeeNumber
JOIN offices OFC
    USING (officeCode)
JOIN orders O
    USING (customerNumber)
JOIN orderdetails OD
    USING (OrderNumber)
JOIN products PR
    USING (productCode)
WHERE  requiredDate < shippedDate
GROUP BY employeeNumber;
```
![sql5](https://github.com/Piux6/Mint-Classics-Company/assets/128375363/dd25f6c0-e972-4b12-8bca-db8a4e40dae3)

#### Findings 
This analysis presents the office and employee information that handled orders with shipment dates later than the supposed delivery date (requiredDate). This will enable Mint classics easily identify and solve problems associated with delayed shippment as it plans to improve order efficiency and deliver within 24 Hours.

### Recommendations
#### Based on the analysis conducted on Mint Classics' data models, the following recommendations have been identified:
- It is advisable for the organization to enhance its sales strategy in order to boost sales. Additionally, it is recommended to postpone any new purchases, given the observation that a considerable number of products remain unsold in the warehouse. The value of these unsold items exceeds the revenue generated so far.
- While all warehouses exhibit satisfactory performance in terms of quantity, value of stored items, and sales, Warehouse D emerges as a candidate for potential elimination. This recommendation is based on its lower storage capacity, lower inventory value, and reduced sales volume compared to other warehouses. Furthermore, Warehouse D also holds half the total number of items categorized as delayed orders.
- Investigation and inquiry into the work activities of Employee ID 1621, located in Office No. 5 in Japan, are recommended. This employee is associated with the handling of all 72 delayed orders, warranting a closer examination of their role and responsibilities.
- Products S700_2466, S18_1984, and S18_2325 have been identified as top-performing products in terms of inventory quantities, with total numbers of 270,284, 263,844, and 261,912 respectively. Considering this, Mint Classics may want to evaluate the feasibility of discontinuing these products from their product lines.

