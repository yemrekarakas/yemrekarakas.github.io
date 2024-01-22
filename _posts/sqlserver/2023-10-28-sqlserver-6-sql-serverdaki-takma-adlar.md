---
title: 6 - SQL Server'daki Takma Adlar
author: yek
date: 2023-10-28 09:17:45 +0300
categories: [Tutorial, SQL Server]
tags: [sqlserver, sql, microsoft, t-sql]
image:
  path: https://yemrekrks.sirv.com/posts/sqlserver/1e22e5841014c2f87617b851389789a6.jpg
  alt: Thatched beach hut at Playa Blanca, Punta Cana, La Altagracia, Dominican Republic
---

<style>
r { color: red }
b { color: blue }
y { color: yellow }
</style>

# Bölüm 6 - SQL Server'daki Takma Adlar (Alias)


## 6.1 - Türetilmiş tablo adından sonra takma ad verilmesi
Bu, çoğu insanın varlığından bile haberdar olmadığı tuhaf bir yaklaşım.

```sql
CREATE TABLE AliasNameDemo(id INT, firstname VARCHAR(20), lastname VARCHAR(20))

INSERT INTO AliasNameDemo
VALUES (1, 'MyFirstName', 'MyLastName')

SELECT *
FROM (SELECT firstname + ' ' + lastname
FROM AliasNameDemo) a (fullname)
```

## 6.2 - AS Kullanımı
Bu ANSI SQL yöntemi tüm RDBMS'lerde çalışır. Yaygın olarak kullanılan yaklaşım.

```sql
CREATE TABLE AliasNameDemo (id INT, firstname VARCHAR(20), lastname VARCHAR(20))

INSERT INTO AliasNameDemo
VALUES (1, 'MyFirstName', 'MyLastName')

SELECT FirstName +' '+ LastName As FullName
FROM AliasNameDemo
```

## 6.3 - = Kullanımı
```sql
CREATE TABLE AliasNameDemo (id INT, firstname VARCHAR(20), lastname VARCHAR(20))

INSERT INTO AliasNameDemo
VALUES (1, 'MyFirstName', 'MyLastName')

SELECT FullName = FirstName +' '+ LastName
FROM AliasNameDemo
```

## 6.4 - AS kullanmadan

```sql
CREATE TABLE AliasNameDemo (id INT, firstname VARCHAR(20), lastname VARCHAR(20))

INSERT INTO AliasNameDemo
VALUES (1, 'MyFirstName', 'MyLastName')

SELECT FirstName +' '+ LastName FullName
FROM AliasNameDemo
```