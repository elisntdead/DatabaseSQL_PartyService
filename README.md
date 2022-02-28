# DatabaseSQL_PartyService
Для проекта PartyService. В этом репозитории собраны схема базы данных, а также примерные SQL-запросы к примерной базе данных.  Предметная область  - предоставление услуг и товаров в рамках праздничных мероприятий.

Навигация
--------------------
[Схема базы данных](https://github.com/elisntdead/DatabaseSQL_PartyService#схема-базы-данных)

[Запросы SQL](https://github.com/elisntdead/DatabaseSQL_PartyService#запросы-sql-к-базе-данных)

Схема базы данных
--------------------
![Схема базы данных](https://github.com/elisntdead/DatabaseSQL_PartyService/blob/main/Images/PartyService_DB_Scheme.png)
  
[Наверх](https://github.com/elisntdead/DatabaseSQL_PartyService#навигация) 
  
Запросы SQL к базе данных
--------------------
Представленные ниже запросы являются примерами запросов к [этой БД](https://github.com/elisntdead/DatabaseSQL_PartyService/blob/main/Queries/PartyService_DB_SQL.db) и их файлы находятся в [этой папке](https://github.com/elisntdead/DatabaseSQL_PartyService/blob/main/Queries/)

```
SELECT DISTINCT Orders.id
FROM Clients INNER JOIN Orders ON Clients.id = Orders.id_client
WHERE Clients.phone Like '7(999)%'
```
Заказы, номера клиентов которых начинаются с "7(999)".
```
SELECT Orders.id AS OrderID, Employees.id AS ENPLOYEEID
FROM Employees INNER JOIN Orders ON Employees.id = Orders.id_employee
WHERE Employees.id <> 6;
```
Заказы, которые не выполнял определённый сотрудник. В данном случае под 6 id.
```
SELECT DISTINCT Orders.id
FROM Orders INNER JOIN (Offers INNER JOIN OffersInOrder ON Offers.id = OffersInOrder.id_offer) ON Orders.id = OffersInOrder.id_order
WHERE Offers.name="Шарики";
```
Заказы, в которых отсутствуют товары с определенным названием. В данном случае "Шарики".
```
SELECT DISTINCT Orders.id
FROM Orders INNER JOIN (Offers INNER JOIN OffersInOrder ON Offers.id = OffersInOrder.id_offer) ON Orders.id = OffersInOrder.id_order
WHERE Offers.id<>3;  
```
Все заказы, кроме одного с определенным id. В данном случае 3 id.
```
SELECT Orders.id,  SUM(OffersInOrder.amount * OffersInOrder.price) AS orderPrice
FROM Orders INNER JOIN OffersInOrder ON Orders.id = OffersInOrder.id_order INNER JOIN Offers ON Offers.id = OffersInOrder.id_offer
GROUP BY Orders.id
ORDER BY orderPrice DESC
LIMIT 3;
```
Три самых дорогих заказа.
  
```
SELECT Offers.id, Offers.Name, SUM(OffersInOrder.amount) AS Total 
FROM Offers INNER JOIN OffersInOrder ON OffersInOrder.id_offer = Offers.id
INNER JOIN Orders ON Orders.id = OffersInOrder.id_order
WHERE Orders.date between '2021/09/01' and '2021/10/01'
GROUP BY Offers.id, Offers.Name
ORDER BY Total DESC
LIMIT 1
```
  Самый популярный заказ сентября
  
```
SELECT Offers.id, Offers.name
FROM Offers
EXCEPT SELECT Offers.id, Offers.name
FROM Offers INNER JOIN OffersInOrder ON OffersInOrder.id_offer = Offers.id
INNER JOIN Orders ON Orders.id = OffersInOrder.id_order AND Orders.date BETWEEN '2021/09/01' AND '2021/10/01'
```
Заказы, не покупавшиеся в сентябре

[Наверх](https://github.com/elisntdead/DatabaseSQL_PartyService#навигация)  
