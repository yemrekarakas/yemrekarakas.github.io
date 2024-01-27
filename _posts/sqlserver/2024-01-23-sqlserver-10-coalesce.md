---
title: 10 - COALESCE
author: yek
date: 2024-01-23 20:46:17 +03:00
categories: [Tutorial, SQL Server]
tags: [sqlserver, sql, microsoft, t-sql]
image:
  path: https://yemrekrks.sirv.com/posts/sqlserver/001764554f92d1da6605245a0c58dcfe.jpg
  alt: Icebergs floating on icy beach at sunrise, Jökulsárlón Lagoon, south Iceland
---

<style>
r { color: red }
b { color: blue }
y { color: yellow }
</style>

# 10: COALESCE

## 10.1: Virgülle Ayrılmış Dize Oluşturmak için COALESCE Kullanma

Aşağıda gösterildiği gibi ``COALESCE`` yi kullanarak birden çok satırdan virgülle ayrılmış bir dize elde edebiliriz.

Tablo değişkeni kullanıldığı için sorgunun tamamını bir kere çalıştırmamız gerekiyor. Anlaşılmasını kolaylaştırmak için BEGIN ve END bloğu ekledim.

```sql
BEGIN

  DECLARE @Table TABLE (FirstName varchar(256), LastName varchar(256))

  INSERT INTO @Table (FirstName, LastName)
  VALUES
  ('John','Smith'),
  ('Jane','Doe'),
  ('Evvy', 'Dury'),
  ('Silvester', 'Fauning'),
  ('Gayel', 'Menci'),
  ('Aurelia', 'Novkovic'),
  ('Indira', 'Lyal'),
  ('Brigham', 'Budgeon'),
  ('Dominik', 'Fermin'),
  ('Melina', 'Barrett'),
  ('Olivia', 'Fussell'),
  ('Belle', 'Sterry')

  DECLARE @Names varchar(4000)

  SELECT @Names = COALESCE(@Names + ',', '') + FirstName
  FROM @Table

  SELECT @Names
END

-- Result : John,Jane,Evvy,Silvester,Gayel,Aurelia,Indira,Brigham,Dominik,Melina,Olivia,Belle
```

## 10.2: Bir Sütundan İlk Null Olmayan Değeri Alma

```sql
SELECT COALESCE(NULL, NULL, 'hepsiburada.com', NULL, 'sahibinden.com');

-- Result: 'hepsiburada.com'
```

```sql
SELECT COALESCE(NULL, 'hepsiburada.com', 'sahibinden.com');

-- Result: 'hepsiburada.com'
```

```sql
SELECT COALESCE(NULL, NULL, 1, 2, 3, NULL, 4);

-- Result: 1
```

## 10.3: Örnek
``COALESCE()``, değişkenler listesindeki ilk NULL OLMAYAN değeri döndürür. 

Diyelim ki telefon numaralarının bulunduğu bir tablomuz var. numaralar ve cep telefonu numaraları ve her kullanıcı için yalnızca bir tane döndürmek isteyelim. 

```sql
DECLARE @Table TABLE (UserID int, PhoneNumber varchar(12), CellNumber varchar(12))
INSERT INTO @Table (UserID, PhoneNumber, CellNumber)
VALUES
(1, '555-869-1123', NULL),
(2, '555-123-7415', '555-846-7786'),
(3, NULL, '555-456-8521')

SELECT
  UserID,
  COALESCE(PhoneNumber, CellNumber)
FROM @Table
```