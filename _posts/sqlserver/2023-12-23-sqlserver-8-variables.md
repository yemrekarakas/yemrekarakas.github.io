---
title: 8 - Değişkenler (Variables)
author: yek
date: 2023-12-23 10:43:42 +0300
categories: [Tutorial, SQL Server]
tags: [sqlserver, sql, microsoft, t-sql]
image:
  path: https://yemrekrks.sirv.com/posts/sqlserver/1ee39873255ec59ac92ee2429a7941d6.jpg
  alt: Rainbow eucalyptus trees forest at Hana Road, Maui, Hawaii, USA
---

<style>
r { color: red }
b { color: blue }
y { color: yellow }
</style>

# 8: Değişkenler (Variables)

## 8.1: Tablo Değişkeni Tanımlama
```sql
DECLARE @Employees TABLE (
  EmployeeID INT NOT NULL PRIMARY KEY,
  FirstName NVARCHAR(50) NOT NULL,
  LastName NVARCHAR(50) NOT NULL,
  ManagerID INT NULL
)
```

Normal bir tablo oluştururken `CREATE TABLE Name (Columns)` sözdizimini kullanırız.
Bir tablo değişkeni oluştururken, `DECLARE @Name TABLE (Columns)` sözdizimini kullanırız.

Bir SELECT ifadesi içindeki tablo değişkenine başvuruda bulunmak için SQL Server tablo değişkenine bir takma ad (alias) vermenizi ister.
Aksi halde bir hata alırsınız.


```sql
DECLARE @Table1 TABLE (Example INT)
DECLARE @Table2 TABLE (Example INT)

/*
-- Bu iki sorgu hata verir:
SELECT *
FROM @Table1
INNER JOIN @Table2 ON @Table1.Example = @Table2.Example

SELECT *
FROM @Table1
WHERE @Table1.Example = 1
*/

-- Doğrusu:
SELECT *
FROM @Table1 T1
INNER JOIN @Table2 T2 ON T1.Example = T2.Example

SELECT *
FROM @Table1 Table1
WHERE Table1.Example = 1
```

## 8.2: SELECT kullanarak değişkenleri güncelleme
SELECT'i kullanarak birden fazla değişkeni aynı anda güncelleyebilirsiniz.

```sql
DECLARE @Variable1 INT, @Variable2 VARCHAR(10)

SELECT @Variable1 = 1, @Variable2 = 'Hello'

PRINT @Variable1
PRINT @Variable2
```

>1

>Hello

Bir değişkeni güncellemek için SELECT kullanıldığında, birden fazla değer varsa son değeri kullanır.

```sql
CREATE TABLE #Test (Example INT)

INSERT INTO #Test VALUES (1), (2)

DECLARE @Variable INT

SELECT @Variable = Example
FROM #Test
ORDER BY Example ASC

PRINT @Variable
```
> 2

```sql
SELECT TOP 1 @Variable = Example
FROM #Test
ORDER BY Example ASC

PRINT @Variable
```
> 1

Sorgunun döndürdüğü satır yoksa değişkenin değeri değişmez:

```sql
SELECT TOP 0 @Variable = Example
FROM #Test
ORDER BY Example ASC

PRINT @Variable
```
> 1

## 8.3: Birden fazla değişkeni aynı anda tanımlayıp, başlangıç ​​değeri verme
```sql
DECLARE
  @Var1 INT = 5,
  @Var2 NVARCHAR(50) = N'Hello World',
  @Var3 DATETIME = GETDATE()
```

## 8.4: SET kullanarak bir değişkeni güncelleme
```sql
DECLARE @VariableName INT
SET @VariableName = 1

PRINT @VariableName
```
> 1

SET'i kullanarak aynı anda yalnızca bir değişkeni güncelleyebilirsiniz.

## 8.5: Değişkenleri tablodan seçerek güncelleme
Verilerinizin yapısına bağlı olarak dinamik olarak güncellenen değişkenler oluşturabilirsiniz.

```sql
DECLARE @CurrentID int = (SELECT TOP 1 ID FROM Table ORDER BY CreateDate desc)

DECLARE @Year int = 2014

DECLARE @CurrentID int = (SELECT ID FROM Table WHERE Year = @Year)
```

> Çoğu durumda, bu yöntemi kullanırken sorgunuzun yalnızca bir değer döndürdüğünden emin olun.
{: .prompt-warning }

## 8.6: Bileşik atama operatörleri

*<y>Version ≥ SQL Server 2008 R2</y>*

Desteklenen bileşik operatörler:

+= Ekle ve ata
-= Çıkar ve ata
*= Çarp ve ata
/= Böl ve ata

Örnek kullanım:
```sql
DECLARE @test INT = 42;

SET @test += 1;
PRINT @test; --43

SET @test -= 1;
PRINT @test; --42

SET @test *= 2
PRINT @test; --84

SET @test /= 2;
PRINT @test; --42
```

SQL Server'da, `&=`, `^=`, ve `|=` gibi Bitwise AND, Bitwise XOR, ve Bitwise OR işlemlerini ve atamalarını doğrudan destekleyen özel operatörler bulunmamaktadır. Ancak, bu tür işlemleri gerçekleştirmek için Bitwise operatörlerini ve atama operatörlerini ayrı ayrı kullanabilirsiniz.

İşte örnekler:

### Bitwise AND ve Atama (`&=`):
```sql
-- Önceki değeri al, Bitwise AND işlemi yap, sonra atama
DECLARE @value INT = 5;
SET @value = @value & 3; -- @value = @value & 3;
-- Şimdi @value 1 olacaktır (5 & 3 = 1)
```

### Bitwise XOR ve Atama (`^=`):
```sql
-- Önceki değeri al, Bitwise XOR işlemi yap, sonra atama
DECLARE @value INT = 6;
SET @value = @value ^ 3; -- @value = @value ^ 3;
-- Şimdi @value 5 olacaktır (6 ^ 3 = 5)
```

### Bitwise OR ve Atama (`|=`):
```sql
-- Önceki değeri al, Bitwise OR işlemi yap, sonra atama
DECLARE @value INT = 3;
SET @value = @value | 5; -- @value = @value | 5;
-- Şimdi @value 7 olacaktır (3 | 5 = 7)
```

Bu örneklerde, işlem ve atama ayrı adımlarda gerçekleştirilmiştir. SQL Server, `&=`, `^=`, ve `|=` gibi doğrudan Bitwise işlem ve atama operatörlerini desteklemez. Ancak, bu temel operatörlerle aynı sonuçları elde etmek mümkündür.
