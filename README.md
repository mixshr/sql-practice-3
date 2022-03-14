# SQL PRACTICE

### SQL Practice №3 | The warehouse

#### Relational shema
![Relation shema](https://upload.wikimedia.org/wikipedia/commons/4/47/Sql_warehouse.png)

#### The creation code
``` sql
 CREATE TABLE Warehouses (
   Code INTEGER PRIMARY KEY NOT NULL,
   Location TEXT NOT NULL ,
   Capacity INTEGER NOT NULL 
 );
 
 CREATE TABLE Boxes (
   Code TEXT PRIMARY KEY NOT NULL,
   Contents TEXT NOT NULL ,
   Value REAL NOT NULL ,
   Warehouse INTEGER NOT NULL, 
   CONSTRAINT fk_Warehouses_Code FOREIGN KEY (Warehouse) REFERENCES Warehouses(Code)
 );
```
#### Sample datase
``` sql
 INSERT INTO Warehouses(Code,Location,Capacity) VALUES(1,'Chicago',3);
 INSERT INTO Warehouses(Code,Location,Capacity) VALUES(2,'Chicago',4);
 INSERT INTO Warehouses(Code,Location,Capacity) VALUES(3,'New York',7);
 INSERT INTO Warehouses(Code,Location,Capacity) VALUES(4,'Los Angeles',2);
 INSERT INTO Warehouses(Code,Location,Capacity) VALUES(5,'San Francisco',8);
 
 INSERT INTO Boxes(Code,Contents,Value,Warehouse) VALUES('0MN7','Rocks',180,3);
 INSERT INTO Boxes(Code,Contents,Value,Warehouse) VALUES('4H8P','Rocks',250,1);
 INSERT INTO Boxes(Code,Contents,Value,Warehouse) VALUES('4RT3','Scissors',190,4);
 INSERT INTO Boxes(Code,Contents,Value,Warehouse) VALUES('7G3H','Rocks',200,1);
 INSERT INTO Boxes(Code,Contents,Value,Warehouse) VALUES('8JN6','Papers',75,1);
 INSERT INTO Boxes(Code,Contents,Value,Warehouse) VALUES('8Y6U','Papers',50,3);
 INSERT INTO Boxes(Code,Contents,Value,Warehouse) VALUES('9J6F','Papers',175,2);
 INSERT INTO Boxes(Code,Contents,Value,Warehouse) VALUES('LL08','Rocks',140,4);
 INSERT INTO Boxes(Code,Contents,Value,Warehouse) VALUES('P0H6','Scissors',125,1);
 INSERT INTO Boxes(Code,Contents,Value,Warehouse) VALUES('P2T6','Scissors',150,2);
 INSERT INTO Boxes(Code,Contents,Value,Warehouse) VALUES('TU55','Papers',90,5);
```
#### Exercises
1. Select all warehouses.
2. Select all boxes with a value larger than $150.
3. Select all distinct contents in all the boxes.
4. Select the average value of all the boxes.
5. Select the warehouse code and the average value of the boxes in each warehouse.
6. Same as previous exercise, but select only those warehouses where the average value of the boxes is greater than 150.
7. Select the code of each box, along with the name of the city the box is located in.
8. Select the warehouse codes, along with the number of boxes in each warehouse. Optionally, take into account that some warehouses are empty (i.e., the box count should show up as zero, instead of omitting the warehouse from the result).
9. Select the codes of all warehouses that are saturated (a warehouse is saturated if the number of boxes in it is larger than the warehouse's capacity).
10. Select the codes of all the boxes located in Chicago.
11. Create a new warehouse in New York with a capacity for 3 boxes.
12. Create a new box, with code "H5RT", containing "Papers" with a value of $200, and located in warehouse 2.
13. Reduce the value of all boxes by 15%.
14. Apply a 20% value reduction to boxes with a value larger than the average value of all the boxes.
15. Remove all boxes with a value lower than $100.
16. Remove all boxes from saturated warehouses.

``` sql
1. SELECT * FROM Warehouses;

2. SELECT * FROM Boxes WHERE value > 150;

3. SELECT DISTINCT contents FROM Boxes;

4. SELECT avg(value) FROM Boxes;

5. SELECT warehouse, avg(value) FROM  Boxes GROUP BY warehouse;

6. SELECT warehouse FROM Boxes GROUP BY warehouse HAVING AVG(Value) > 150;

7. SELECT Boxes.Code, Location  FROM Warehouses INNER JOIN Boxes ON Warehouses.Code = Boxes.Warehouse;
    
8. SELECT Warehouses.Code, COUNT(Boxes.Code) FROM Warehouses LEFT JOIN Boxes ON Warehouses.Code = Boxes.Warehouse GROUP BY Warehouses.Code;
 
9. SELECT Warehouses.Code FROM Warehouses JOIN Boxes ON Warehouses.Code = Boxes.Warehouse GROUP BY Warehouses.code, Warehouses.Capacity HAVING Count(Boxes.code) > Warehouses.Capacity;
  
10. INSERT INTO Warehouses ('code', 'Location', 'Capacity') VALUES(6, 'New York', 3);

11. INSERT INTO Boxes VALUES('H5RT','Papers',200,2);
   
12. UPDATE Boxes SET Value = Value * 0.85;

13. UPDATE Boxes SET Value = Value * 0.80
   WHERE Value > (SELECT AVG(Value) FROM (SELECT * FROM Boxes) AS X);
  
14. DELETE FROM Boxes WHERE value < 100;

15. DELETE FROM Boxes WHERE Warehouse IN (SELECT Code FROM Warehouses WHERE Capacity < (SELECT COUNT(*) FROM Boxes WHERE Warehouse = Warehouses.Code));
```
