---
title: 12 - CASE ifadesi
author: yek
date: 2024-01-23 22:10:46 +03:00
categories: [Tutorial, SQL Server]
tags: [sqlserver, sql, microsoft, t-sql]
image:
  path: https://yemrekrks.sirv.com/posts/sqlserver/0023a22a3a1070a3970b0a7504f617d8.jpg
  alt: Green hills under cloudy sky, Cap Blanc-Nez, Escalles, Nord-Pas-de-Calais, France
---

<style>
r { color: red }
b { color: blue }
y { color: yellow }
</style>

# 12 - CASE ifadesi

## 12.1 - Basit CASE ifadesi

Basit bir ``CASE`` ifadesinde, bir değer veya değişken, birden çok olası cevapla karşılaştırılır. Aşağıdaki kod, basit bir ``CASE`` ifadesinin bir örneğidir:

```sql
SELECT CASE DATEPART(WEEKDAY, GETDATE())
    WHEN 1 THEN 'Pazar'
    WHEN 2 THEN 'Pazartesi'
    WHEN 3 THEN 'Salı'
    WHEN 4 THEN 'Çarşamba'
    WHEN 5 THEN 'Perşembe'
    WHEN 6 THEN 'Cuma'
    WHEN 7 THEN 'Cumartesi'
END
```

## 12.2 - CASE ifadesi
Başka bir ``CASE`` ifadesinde, her bir seçenek bağımsız olarak bir veya daha fazla değeri test edebilir.

```sql
DECLARE @FirstName varchar(30) = 'John'
DECLARE @LastName varchar(30) = 'Smith'
SELECT CASE
    WHEN LEFT(@FirstName, 1) IN ('a','e','i','o','u') THEN 'Ad ilk harfi ünlü harfle başlar'
    WHEN LEFT(@LastName, 1) IN ('a','e','i','o','u') THEN 'Soyad ilk harfi ünlü harfle başlar'
    ELSE 'Ne ad ünlü harfle başlar, ne de soyad'
END
```

Bu SQL kodu, @FirstName'in ve @LastName'in başındaki harfleri kontrol eder ve her ikisi de ünlü harfle başlıyorsa ilgili mesajı döndürür; aksi takdirde, "Ne ad ünlü harfle başlar, ne de soyad" mesajını döndürür.