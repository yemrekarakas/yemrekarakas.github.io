---
title: 4 - Kullanıcı Tanımlı Tablo Tipleri (User Defined Table Types)
author: yek
date: 2023-10-26 16:00:05 +0300
categories: [Tutorial, SQL Server]
tags: [sqlserver, sql, microsoft, t-sql]
image:
  path: https://yemrekrks.sirv.com/posts/sqlserver/1e7e3cee96bcd57a9274ce3f16d528da.jpg
  alt: Country House near San Quirico d'Orcia, Val d'Orcia, Siena District, Tuscany, Italy
---

<style>
r { color: red }
b { color: blue }
y { color: yellow }
</style>


# Bölüm 4 - Kullanıcı Tanımlı Tablo Tipleri (User Defined Table Types)

Kullanıcı tanımlı tablo tipleri (kısa adıyla UDT), kullanıcının bir tablo yapısını tanımlamasına izin veren veri tipleridir. Kullanıcı tanımlı tablo tipleri, birincil anahtarlar, benzersiz kısıtlamalar ve varsayılan değerleri destekler.

## 4.1 - Tek bir tamsayı sütunu içeren ve aynı zamanda birincil anahtar olan bir UDT oluşturma

```sql
CREATE TYPE dbo.Ids as TABLE (
    Id int PRIMARY KEY
)
```

## 4.2 - Birden fazla sütun içeren bir UDT oluşturma

```sql
CREATE TYPE MyComplexType as TABLE (
    Id int,
    Name varchar(10)
)
```

## 4.3 - Benzersiz bir kısıtlamayı içeren bir UDT oluşturma

```sql
CREATE TYPE MyUniqueNamesType as TABLE (
    FirstName varchar(10),
    LastName varchar(10),
    UNIQUE (FirstName, LastName)
)
```
Not: Kullanıcı tanımlı tablo tiplerinde kısıtlamalar adlandırılamaz.

## 4.4 - Birincil anahtar ve varsayılan bir değere sahip bir sütunu olan bir UDT oluşturma

```sql
CREATE TYPE MyUniqueNamesType as TABLE (
    FirstName varchar(10),
    LastName varchar(10),
    CreateDate datetime DEFAULT GETDATE(),
    PRIMARY KEY (FirstName, LastName)
)
```