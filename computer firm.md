### Краткая информация о базе данных "Компьютерная фирма"

Схема БД состоит из четырех таблиц:<br/>
Product(maker, model, type)<br/>
PC(code, model, speed, ram, hd, cd, price)<br/>
Laptop(code, model, speed, ram, hd, price, screen)<br/>
Printer(code, model, color, type, price)<br/>
Таблица Product представляет производителя (maker), номер модели (model) и тип ('PC' - ПК, 'Laptop' - ПК-блокнот или 'Printer' - принтер). Предполагается, что номера моделей в таблице Product уникальны для всех производителей и типов продуктов. В таблице PC для каждого ПК, однозначно определяемого уникальным кодом – code, указаны модель – model (внешний ключ к таблице Product), скорость - speed (процессора в мегагерцах), объем памяти - ram (в мегабайтах), размер диска - hd (в гигабайтах), скорость считывающего устройства - cd (например, '4x') и цена - price. Таблица Laptop аналогична таблице РС за исключением того, что вместо скорости CD содержит размер экрана -screen (в дюймах). В таблице Printer для каждой модели принтера указывается, является ли он цветным - color ('y', если цветной), тип принтера - type (лазерный – 'Laser', струйный – 'Jet' или матричный – 'Matrix') и цена - price.<br/>

**Задание 1** <br/>
Найдите номер модели, скорость и размер жесткого диска для всех ПК стоимостью менее 500 дол. Вывести: model, speed и hd <br/>
**Запрос:** <br/>
SELECT model, speed, hd <br/>
FROM PC <br/>
WHERE price < 500 <br/>

**Задание 2** <br/>
Найдите производителей принтеров. Вывести: maker <br/>
**Запрос:**<br/>
SELECT DISTINCT  maker <br/>
FROM Product <br/>
WHERE type = 'printer' <br/>

**Задание 3**  <br/>
Найдите номер модели, объем памяти и размеры экранов ПК-блокнотов, цена которых превышает 1000 дол. <br/>
**Запрос:**<br/>
SELECT model, ram, screen <br/>
FROM Laptop <br/>
WHERE Price > 1000 <br/>

**Задание 4**<br/>
Найдите все записи таблицы Printer для цветных принтеров. <br/>
**Запрос:**<br/>
SELECT * <br/>
FROM Printer <br/>
WHERE color = 'y' <br/>

**Задание 5** <br/>
Найдите номер модели, скорость и размер жесткого диска ПК, имеющих 12x или 24x CD и цену менее 600 дол. <br/>
**Запрос:**<br/>
SELECT model, speed, hd <br/>
FROM PC<br/>
WHERE (cd = '12x' OR cd = '24x') AND price < 600 <br/>

**Задание 6**<br/>
Для каждого производителя, выпускающего ПК-блокноты c объёмом жесткого диска не менее 10 Гбайт, найти скорости таких ПК-блокнотов. Вывод: производитель, скорость. <br/>
**Запрос:**<br/>
SELECT DISTINCT Product.maker, Laptop.speed <br/>
FROM Product, Laptop  <br/>
WHERE Laptop.hd > 10 <br/>

**Задание 7**<br/>
Найдите номера моделей и цены всех имеющихся в продаже продуктов (любого типа) производителя B (латинская буква). <br/>
**Запрос:**<br/>
SELECT model, price <br/>
FROM PC 
WHERE model = (SELECT model <br/>
FROM Product <br/>
WHERE maker = 'B' AND <br/>
type = 'PC') <br/>
UNION<br/>
SELECT model, price <br/>
FROM Laptop <br/>
WHERE model = (SELECT model <br/>
FROM Product <br/>
WHERE maker = 'B' AND <br/>
type = 'Laptop') <br/>
UNION<br/>
SELECT model, price <br/>
FROM PC <br/>
WHERE model = (SELECT model <br/>
FROM Product <br/>
WHERE maker = 'B' AND <br/>
type = 'PC') <br/>

**Задание 8** <br/>
Найдите производителя, выпускающего ПК, но не ПК-блокноты. <br/>
**Запрос:**<br/>
SELECT DISTINCT maker  <br/>
FROM Product  <br/>
WHERE type='PC' AND maker NOT IN (SELECT maker <br/>
FROM product  <br/>
WHERE type = 'Laptop')  <br/>

**Задание 9** <br/>
Найдите производителей ПК с процессором не менее 450 Мгц. Вывести: Maker <br/>
**Запрос:**<br/>
SELECT DISTINCT maker   <br/>
FROM PC INNER JOIN  <br/>
Product ON PC.model = Product.model  <br/>
WHERE speed >= 450  <br/>

**Задание 10**  <br/>
Найдите модели принтеров, имеющих самую высокую цену. Вывести: model, price <br/>
**Запрос:**<br/>
SELECT Printer.model, price <br/>
FROM Printer INNER JOIN   <br/>
Product ON Printer.model = Product.model   <br/>
WHERE price = (SELECT MAX(price) FROM Printer)    <br/>

**Задание 11** <br/>
Найдите среднюю скорость ПК. <br/>
**Запрос:**<br/>
SELECT AVG(speed) <br/>
FROM PC <br/>

**Задание 12** <br/>
Найдите среднюю скорость ПК-блокнотов, цена которых превышает 1000 дол. <br/>
**Запрос:**<br/>
SELECT AVG(speed) <br/>
FROM Laptop <br/>
WHERE price > 1000  <br/>

**Задание 13** <br/>
Найдите среднюю скорость ПК, выпущенных производителем A. <br/>
**Запрос:**<br/>
SELECT AVG(speed) <br/>
FROM PC INNER JOIN    <br/>
Product ON PC.model = Product.model   <br/>
WHERE maker = 'A' <br/>

**Задание 15** <br/>
Найдите размеры жестких дисков, совпадающих у двух и более PC. Вывести: HD <br/>
**Запрос:**<br/>
SELECT hd   <br/>
FROM PC <br/>
GROUP BY hd HAVING COUNT (model) > 1   <br/>

**Задание 19** <br/>
Для каждого производителя, имеющего модели в таблице Laptop, найдите средний размер экрана выпускаемых им ПК-блокнотов. <br/> 
Вывести: maker, средний размер экрана. 
**Запрос:**<br/>
SELECT maker, AVG(screen) <br/>
FROM Laptop INNER JOIN    <br/>
Product ON Laptop.model = Product.model    <br/>
GROUP BY maker <br/>

**Задание 21** <br/>
Найдите максимальную цену ПК, выпускаемых каждым производителем, у которого есть модели в таблице PC.  <br/>
Вывести: maker, максимальная цена. <br/>
**Запрос:**<br/>
SELECT maker, MAX(price) <br/>
FROM PC INNER JOIN   <br/>
Product ON PC.model = Product.model     <br/>
GROUP BY maker <br/>

**Задание 28**
Используя таблицу Product, определить количество производителей, выпускающих по одной модели. <br/>
**Запрос:**<br/>
SELECT count(*)   <br/>
FROM (SELECT maker  <br/>
FROM product  <br/>
GROUP BY maker HAVING COUNT(model)=1) as count <br/>
