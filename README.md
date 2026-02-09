# 100-DAY-SQL-CHALLENGE
-- SOLVING QUESTIONS FROM UDEMY COURSE  “100 Days of SQL: Ace the SQL Interviews Like a PRO!!” by Ankit Bansal on Udemy 

QUESTION#1:

-- how many matches they played 
-- how many matches they won
-- how many they lost. That’s it. Simple cricket points table

-- Script 1 :

CREATE DATABASE MATCH_WORLDCUP;
CREATE TABLE ICC_WORLD_CUP
(
TEAM_1 VARCHAR(20),
TEAM_2 VARCHAR(20),
WINNER VARCHAR(20)
);
INSERT INTO ICC_WORLD_CUP VALUES('INDIA','SL','INDIA');
INSERT INTO ICC_WORLD_CUP VALUES('SL','AUS','AUS');
INSERT INTO ICC_WORLD_CUP VALUES('SA','ENG','ENG');
INSERT INTO ICC_WORLD_CUP VALUES('ENG','NZ','NZ');
INSERT INTO ICC_WORLD_CUP VALUES('AUS','INDIA','INDIA');

SELECT*FROM icc_world_cup;

-- SOLUTION

WITH CTE1 AS (
    SELECT TEAM_1 AS TEAM_NAME, WINNER FROM ICC_WORLD_CUP
    UNION ALL
    SELECT TEAM_2 AS TEAM_NAME, WINNER FROM ICC_WORLD_CUP
),
CTE2 AS (
    SELECT 
        TEAM_NAME,
        COUNT(*) AS NO_MATCHES_PLAYED,
        SUM(CASE WHEN WINNER = TEAM_NAME THEN 1 ELSE 0 END) AS NO_MATCHES_WON
    FROM CTE1
    GROUP BY TEAM_NAME
)
SELECT *,
       (NO_MATCHES_PLAYED - NO_MATCHES_WON) AS NO_OF_LOSSES
FROM CTE2
ORDER BY NO_MATCHES_WON DESC, NO_OF_LOSSES;
SELECT*FROM icc_world_cup;

![image](https://github.com/Anzala189/100-DAY-SQL-CHALLENGE/blob/c7a07cad66fadd64bbc224d9d3afe37598d7421d/image.png)

QUESTION#2:

-- Day 2 : Find the employees with salary more than their manager salary
-- Script 2 :
CREATE DATABASE employeessalary_managersalary; 
CREATE TABLE EMP(EMP_ID INT,EMP_NAME VARCHAR(10),SALARY INT ,MANAGER_ID INT);
INSERT INTO EMP VALUES(1,'ANKIT',10000,4);
INSERT INTO EMP VALUES(2,'MOHIT',15000,5);
INSERT INTO EMP VALUES(3,'VIKAS',10000,4);
INSERT INTO EMP VALUES(4,'ROHIT',5000,2);
INSERT INTO EMP VALUES(5,'MUDIT',12000,6);
INSERT INTO EMP VALUES(6,'AGAM',12000,2);
INSERT INTO EMP VALUES(7,'SANJAY',9000,2);
INSERT INTO EMP VALUES(8,'ASHISH',5000,2);

SELECT * FROM EMP;
--  My Solution: 
SELECT E.EMP_ID, E.EMP_NAME, M.EMP_NAME AS MANAGER_NAME, E.SALARY, M.SALARY AS "MANAGER SALARY"
FROM EMP E INNER JOIN EMP M ON M.EMP_ID = E.MANAGER_ID 
WHERE E.SALARY > M.SALARY
ORDER BY E.EMP_NAME ASC;

![image](https://github.com/Anzala189/100-DAY-SQL-CHALLENGE/blob/c7a07cad66fadd64bbc224d9d3afe37598d7421d/images/Day_2_employeesalary%20more%20than%20managersalary.jpg)


-- Day 3 : Find the new & repeated customers
-- Script 3 :
CREATE DATABASE new_repeated_customers;
USE new_repeated_customers;

CREATE TABLE CUSTOMER_ORDERS (
ORDER_ID INTEGER,
CUSTOMER_ID INTEGER,
ORDER_DATE DATE,
ORDER_AMOUNT INTEGER
);
INSERT INTO CUSTOMER_ORDERS VALUES(1,100,CAST('2022-01-01' AS DATE),2000),(2,200,CAST('2022-01-01' AS DATE),2500),(3,300,CAST('2022-01-01' AS DATE),2100)
,(4,100,CAST('2022-01-02' AS DATE),2000),(5,400,CAST('2022-01-02' AS DATE),2200),(6,500,CAST('2022-01-02' AS DATE),2700)
,(7,100,CAST('2022-01-03' AS DATE),3000),(8,400,CAST('2022-01-03' AS DATE),1000),(9,600,CAST('2022-01-03' AS DATE),3000);
select*FROM Customer_orders;

SOLUTION:

WITH first_visit AS (
    SELECT 
        customer_id, 
        MIN(order_date) AS first_visit_date
    FROM customer_orders
    GROUP BY customer_id
)
SELECT 
    co.order_date,
    SUM(CASE 
            WHEN co.order_date = fv.first_visit_date THEN 1 
            ELSE 0 
        END) AS first_visit_flag,
    SUM(CASE 
            WHEN co.order_date <> fv.first_visit_date THEN 1 
            ELSE 0 
        END) AS repeat_visit_flag
FROM customer_orders co
JOIN first_visit fv 
    ON co.customer_id = fv.customer_id
GROUP BY co.order_date
ORDER BY co.order_date;










