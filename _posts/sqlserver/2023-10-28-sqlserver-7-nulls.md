---
title: 7 - NULL
author: yek
date: 2023-10-28 11:41:43+0300
categories: [Tutorial, SQL Server]
tags: [sqlserver, sql, microsoft, t-sql]
image:
  path: https://yemrekrks.sirv.com/posts/sqlserver/1e24ecdbe9018d19c7581ad7dec16175.jpg
  alt: Distant Wonderland, Landmannalaugar, Iceland
---


<style>
r { color: red }
b { color: blue }
y { color: yellow }
</style>


# Bölüm 7 - NULL
SQL Server'da NULL, eksik veya bilinmeyen verileri temsil eder. Bu, NULL'un gerçekte bir değer olmadığı anlamına gelir. Onun bir değer için yer tutucu olarak daha iyi tanımlanır. NULL'u herhangi bir değerle karşılaştıramamanızın nedeni de budur ve başka bir NULL ile bile.

## 7.1 - COALESCE()
Bağımsız değişkenleri sırayla değerlendirir ve başlangıçta NULL olarak değerlendirilmeyen ilk ifadenin geçerli değerini döndürür.

```sql
DECLARE @MyInt1 int -- Değişken; Değer verilene kadar değeri null dur.
DECLARE @MyInt2 int -- Değişken; Değer verilene kadar değeri null dur.
DECLARE @MyInt3 int -- Değişken; Değer verilene kadar değeri null dur.

SET @MyInt3 = 3

SELECT COALESCE (@MyInt1, @MyInt2, @MyInt3, 5) 

-- Returns 3 : @MyInt3 ün değeri.
```
@MyInt1 in değerine bakar değeri null ise bir sonraki argumana gider (@MyInt2) değerine bakar null değilse yazdırır, null ise sonraki argümana bakar (@MyInt3) null değilse değerini yazdırır. tüm argumanlar null ise en son değer yazılır (5)

## 7.2 - ISNULL()
IsNull() işlevi iki parametre kabul eder, ilki null ise ikinci parametreyi döndürür.

```sql
DECLARE @MyInt int -- Tüm değişkenler değer atanana kadar null dur.

SELECT ISNULL(@MyInt, 3) -- Returns 3
```

## 7.3 - Is null / Is not null
Null bir değer olmadığından, karşılaştırma işleçlerini null değerlerle kullanamazsınız.

Bir sütunun veya değişkenin null olup olmadığını kontrol etmek için is null kullanmanız gerekir:

Boş değerlere sahip tüm karşılaştırmalar yanlış veya bilinmiyor olarak değerlendirildiğinden aşağıdaki ifade 6 değerini seçecektir.
```sql
DECLARE @Date date = '2016-08-03'

SELECT CASE 
    WHEN @Date = NULL THEN 1
    WHEN @Date <> NULL THEN 2
    WHEN @Date > NULL THEN 3
    WHEN @Date < NULL THEN 4
    WHEN @Date IS NULL THEN 5
    WHEN @Date IS NOT NULL THEN 6
END
```

@Date değişkeninin içeriğini null olarak ayarlayıp tekrar deneyin, aşağıdaki ifade 5 değerini döndürecektir:
```sql
SET @Date = NULL -- Not Buradaki '=' eşittir bir atama operatorudur.

SELECT CASE 
    WHEN @Date = NULL THEN 1
    WHEN @Date <> NULL THEN 2
    WHEN @Date > NULL THEN 3
    WHEN @Date < NULL THEN 4
    WHEN @Date IS NULL THEN 5
    WHEN @Date IS NOT NULL THEN 6
END
```

## 7.5 - NULL Karşılaştırma
NULL karşılaştırması özel bir durumdur.

Varsayalım

| id | column1 |
| -- | ------- |
|0   | NULL    |
|1   | 1       |
|2   | 2       |

Sorgular:

```sql
SELECT id
FROM table
WHERE column1 = 1
```
id si 1 olan dönecektir.

```sql
SELECT id
FROM table
WHERE column1 <> 1
```
id si 2 olan dönecektir.


```sql
SELECT id
FROM table
WHERE column1 IS NULL
```
id si 0 olan dönecektir.


```sql
SELECT id
FROM table
WHERE column1 IS NOT NULL
```
id si 1 ve 2 olan dönecektir.

NULL'ların bir =, <> karşılaştırmasında değer olarak "sayılmasını" istiyorsanız, öncelikle sayılabilir bir veriye dönüştürülmesi gerekir.

```sql
SELECT id
FROM table
WHERE ISNULL(column1, -1) <> 1
```
veya
```sql
SELECT id
FROM table
WHERE column1 IS NULL OR column1 <> 1
```
id si 0 ve 2 donecektir.

## 7.6 - NOT IN Alt Sorgusu ile NULL
`NOT IN` alt sorgusu içinde `NULL` değerlerle başa çıkarken, beklenen sonuçları elde etmek için NULL'leri uygun şekilde ele almak önemlidir. SQL'de, `NULL`'ı `=` veya `<>` gibi eşitlik operatörleriyle karşılaştırmak beklenen sonuçları üretmez çünkü `NULL`, bilinmeyen veya tanımsız bir değeri temsil eder.

Bu sorunu ele almak için, `IS NULL` veya `IS NOT NULL` koşullarını açıkça kullanabilirsiniz. İşte bir örnek:

Varsayalım ki şu şekilde bir ana sorgunuz var:

```sql
SELECT column_name
FROM your_table
WHERE some_column NOT IN (SELECT another_column FROM another_table WHERE condition);
```

Eğer alt sorgudaki `another_column` değerleri `NULL` içerebiliyorsa, sorguyu bu null'leri ele alacak şekilde değiştirmeniz gerekebilir. `IS NULL` koşulunu kullanabilirsiniz:

```sql
SELECT column_name
FROM your_table
WHERE some_column NOT IN (SELECT another_column FROM another_table WHERE another_column IS NOT NULL AND condition);
```

Bu değişiklik, `NULL` değerlerinin alt sorgudan hariç tutulmasını sağlar ve `NOT IN` karşılaştırmasında beklenmeyen davranışları önler.

SQL'de `NULL` değerleriyle uğraşırken her zaman dikkatli olunmalıdır, çünkü bunlar karşılaştırmaların ve koşulların sonuçlarını etkileyebilir. Belirli gereksinimlere bağlı olarak sorgularınızı uygun şekilde ayarlamak gerekebilir.

```sql
create table #outertable (i int)
create table #innertable (i int)


insert into #outertable (i) values (1), (2), (3), (4), (5)
insert into #innertable (i) values (2), (3), (null)


select * from #outertable where i in (select i from #innertable)
--2
--3

select * from #outertable where i not in (select i from #innertable)
-- Burada beklenti 1,4,5 ama değil
-- NULL nedeniyle boş sonuç dönecektir.
-- Sanki {select * from #outertable where i not in (null)} çalıştırır gibi

--Bunu düzeltmek için
select * from #outertable where i not in (select i from #innertable where i is not null)
--Beklenen sonuçlar
--1
--4
--5
```