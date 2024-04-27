---
title: 9 - Tarihler (Dates)
author: yek
date: 2023-12-24 20:23:55 +0300
categories: [Tutorial, SQL Server]
tags: [sqlserver, sql, microsoft, t-sql]
image:
  path: https://yemrekrks.sirv.com/posts/sqlserver/1eaa2d89a74198628ec0af85feb66356.jpg
  alt: Sea stacks of Reynisdrangar at sunrise from the black volcanic sand beach at Vík í Mýrdal, South Iceland
---

<style>
r { color: red }
b { color: blue }
y { color: yellow }
</style>

# 9 - Tarihler (Dates)

## 9.1: CONVERT kullanarak Tarih ve Saat Biçimlendirmesi
Tarih saat veri türünü biçimlendirilmiş bir dizeye dönüştürmek için CONVERT işlevini kullanabilirsiniz.

`SELECT GETDATE() AS [Result] -- 2023-12-24 20:23:55.927`

Belirli bir formata dönüştürmek için bazı yerleşik kodları da kullanabilirsiniz:

```sql
DECLARE @convert_code INT = 100
SELECT CONVERT(VARCHAR(30), GETDATE(), @convert_code) AS [Result]
```

```sql
SELECT GETDATE() AS [Result]

SELECT CONVERT(VARCHAR(30), GETDATE(), 100) AS [Result]       --12/25/2023
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 101) AS [Result] --12:21:12
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 102) AS [Result] --12:21:12:890
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 103) AS [Result] --12-25-2023
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 104) AS [Result] --13 ????? ??????? 1445 12:21:12
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 105) AS [Result] --13/06/1445 12:21:12:890PM
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 106) AS [Result] --2023.12.25
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 107) AS [Result] --2023/12/25
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 108) AS [Result] --20231225
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 109) AS [Result] --2023-12-25 12:21:12
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 110) AS [Result] --2023-12-25 12:21:12.890
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 111) AS [Result] --2023-12-25T12:21:12.890
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 112) AS [Result] --25 Dec 2023
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 113) AS [Result] --25 Dec 2023 12:21:12:890
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 114) AS [Result] --25.12.2023
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 120) AS [Result] --25/12/2023
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 121) AS [Result] --25-12-2023
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 126) AS [Result] --Dec 25 2023 12:21:12:890PM
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 127) AS [Result] --Dec 25 2023 12:21:12:890PM
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 130) AS [Result] --Dec 25 2023 12:21PM
UNION SELECT CONVERT(VARCHAR(30), GETDATE(), 131) AS [Result] --Dec 25, 2023
```

## 9.2: FORMAT kullanarak Tarih ve Saat Biçimlendirmesi

*<y>Version ≥ SQL Server 2012</y>*
Fonksiyon FORMAT()

Bunu kullanarak DATETIME alanlarınızı kendi özel VARCHAR formatınıza dönüştürebilirsiniz.

Örnek:
```sql
DECLARE @Date DATETIME = '2016-09-05 00:01:02.333'

SELECT FORMAT(@Date, N'dddd, MMMM dd, yyyy hh:mm:ss tt')
```
> Monday, September 05, 2016 12:01:02 AM

**Argümanlar**

Biçimlendirilen DATETIME'ın aşağıdaki çıktıları almak için sağlanan argümanlar.

'2016-09-05 00:01:02.333' için;

| Argüman |   Çıktı   |
|:-------:|:---------:|
|  yyyy   |   2016    |
|   yy    |    16     |
|  MMMM   | September |
|   MM    |    09     |
|    M    |     9     |
|  dddd   |  Monday   |
|   ddd   |    Mon    |
|   dd    |    05     |
|    d    |     5     |
|   HH    |    00     |
|    H    |     0     |
|   hh    |    12     |
|    h    |    12     |
|   mm    |    01     |
|    m    |     1     |
|   ss    |    02     |
|    s    |     2     |
|   tt    |    AM     |
|    t    |     A     |
|   fff   |    333    |
|   ff    |    33     |
|    f    |     3     |

Ayrıca önceden biçimlendirilmiş bir çıktı oluşturmak için FORMAT() işlevine tek bir argüman da verebilirsiniz:

```sql
DECLARE @Date DATETIME = '2016-09-05 00:01:02.333'

SELECT FORMAT(@Date, N'U')
```
> Monday, September 05, 2016 4:01:02 AM

| Tek Argüman | Çıktı                                  |
|:-----------:|:-------------------------------------- |
|   D         | Monday, September 05, 2016             |
|   d         | 9/5/2016                               |
|   F         | Monday, September 05, 2016 12:01:02 AM |
|   f         | Monday, September 05, 2016 12:01 AM    |
|   G         | 9/5/2016 12:01:02 AM                   |
|   g         | 9/5/2016 12:01 AM                      |
|   M         | September 05                           |
|   O         | 2016-09-05T00:01:02.3330000            |
|   R         | Mon, 05 Sep 2016 00:01:02 GMT          |
|   s         | 2016-09-05T00:01:02                    |
|   T         | 12:01:02 AM                            |
|   t         | 12:01 AM                               |
|   U         | Monday, September 05, 2016 4:01:02 AM  |
|   u         | 2016-09-05 00:01:02Z                   |
|   Y         | September, 2016                        |

> Yukarıdaki liste varsayılan en-US kültürünü kullanmaktadır. Üçüncü bir argüman girerek farklı bir kültür belirtilebilir.
{: .prompt-info }

```sql
DECLARE @Date DATETIME = '2016-09-05 00:01:02.333'

SELECT FORMAT(@Date, N'U', 'zh-cn')
```
> 2016年9月5日 4:01:02

## 9.3: Zaman eklemek ve çıkarmak için DATEADD()
Sözdizimi: `DATEADD (datepart, number, datetime_expr)`

Bir zaman ölçüsü eklemek için sayının pozitif olması gerekir. Bir zaman ölçüsünü çıkarmak için sayının negatif olması gerekir.

```sql
DECLARE @now DATETIME2 = GETDATE();

SELECT @now; -- 2023-12-25 13:32:13.5470000
SELECT DATEADD(YEAR, 1, @now) -- 2024-12-25 13:32:13.5470000
SELECT DATEADD(QUARTER, 1, @now) -- 2024-03-25 13:32:13.5470000
SELECT DATEADD(WEEK, 1, @now) -- 2024-01-01 13:32:13.5470000
SELECT DATEADD(DAY, 1, @now) -- 2023-12-26 13:32:13.5470000
SELECT DATEADD(HOUR, 1, @now) -- 2023-12-25 14:32:13.5470000
SELECT DATEADD(MINUTE, 1, @now) -- 2023-12-25 13:33:13.5470000
SELECT DATEADD(SECOND, 1, @now) -- 2023-12-25 13:32:14.5470000
SELECT DATEADD(MILLISECOND, 1, @now) -- 2023-12-25 13:32:13.5480000
```

>  DATEADD ayrıca datepart parametresindeki kısaltmaları da kabul eder.  Bu kısaltmaların kullanımı genel olarak kafa karıştırıcı olabileceğinden tavsiye edilmez (m vs mi, ww vs w, vb.).
{: .prompt-info }

## 9.4: Bir kişinin yaşını hesaplamak için fonksiyon oluşturma
Bu fonksiyon 2 tarihsaat parametresi alacaktır. Doğum tarihi (DateOfBirth) ve kontrol edilecek diğer tarih

```sql
CREATE FUNCTION [dbo].[Calc_Age] (
  @DOB datetime, 
  @calcDate datetime
)
RETURNS int
AS
BEGIN
  DECLARE @age INT

  -- Eğer doğum tarihi kontrol tarihinden büyükse -1 döner
  IF (@calcDate < @DOB )
    RETURN -1
    
  SELECT @age = YEAR(@calcDate) - YEAR(@DOB) +
    CASE 
      WHEN DATEADD(year, YEAR(@calcDate) - YEAR(@DOB), @DOB) > @calcDate THEN -1 
    ELSE 0 
  END
  
  RETURN @age
END
```

örneğin 1/1/2000 tarihinde doğan birinin bugünkü yaşını kontrol etmek için,
```sql
SELECT dbo.Calc_Age('2000-01-01', Getdate())
```

## 9.5: Geçerli(şuanki) Tarih Saat
Yerleşik GETDATE ve GETUTCDATE işlevlerinin her biri, saat dilimi farkı olmadan geçerli tarih ve saati döndürür. Her iki işlevin dönüş değeri, SQL örneğinin bulunduğu bilgisayarın işletim sistemine bağlıdır. GETDATE'in dönüş değeri, işletim sistemiyle aynı saat dilimindeki geçerli saati temsil eder. GETUTCDATE değeri geçerli UTC saatini temsil eder. Her iki işlev de bir sorgunun SELECT yan tümcesine veya WHERE yan tümcesindeki boolean ifadesinin bir parçası olarak dahil edilebilir.

Örnekler:
```sql
SELECT GETDATE() as SystemDateTime, GETUTCDATE() as UTCDateTime

SELECT * FROM MyEvents WHERE EventDate < GETDATE()
```
Geçerli tarih-saatin farklı varyasyonlarını döndüren birkaç yerleşik işlev daha vardır:

```sql
SELECT
  GETDATE(),           -- 2023-12-25 13:59:19.547
  GETUTCDATE(),        -- 2023-12-25 10:59:19.547
  CURRENT_TIMESTAMP,   -- 2023-12-25 13:59:19.547
  SYSDATETIME(),       -- 2023-12-25 13:59:19.5507985
  SYSDATETIMEOFFSET(), -- 2023-12-25 13:59:19.5507985 +03:00
  SYSUTCDATETIME()     -- 2023-12-25 10:59:19.5507985
```

## 9.6: Bir ayın son gününü almak

`DATEADD` ve `DATEDIFF` işlevlerini kullanarak ayın son tarihini döndürmek mümkündür.

```sql
SELECT DATEADD(d, -1, DATEADD(m, DATEDIFF(m, 0, '2016-09-23') + 1, 0))
-- 2016-09-30 00:00:00.000
```

*<y>Version ≥ SQL Server 2012</y>*

`EOMONTH` işlevi, bir ayın son tarihini döndürmek için daha kısa bir yol sağlar ve isteğe bağlı bir işleve sahiptir.

```sql
SELECT EOMONTH('2016-07-21')     --2016-07-31
SELECT EOMONTH('2016-07-21', 4)  --2016-11-30
SELECT EOMONTH('2016-07-21', -5) --2016-02-29
```

## 9.7: DateTime'dan yalnızca Tarihi döndür
Birkaç yol:

1. ``SELECT CONVERT(Date, GETDATE())``
2. ``SELECT DATEADD(dd, 0, DATEDIFF(dd, 0, GETDATE()))``
3. ``SELECT CAST(GETDATE() AS DATE)``
4. ``SELECT CONVERT(CHAR(10), GETDATE(), 111)``
5. ``SELECT FORMAT(GETDATE(), 'yyyy-MM-dd')``

> 4 ve 5 in dönus tipi metin (dize) dir.


## 9.8: Tarih farklarını hesaplamak DATEDIFF
Sözdizimi:

`DATEDIFF (datepart, datetime_expr1, datetime_expr2)`

Datetime_expr, datetime_expr2'ye göre geçmişteyse pozitif bir sayı, aksi halde negatif bir sayı döndürür.

 ```sql
DECLARE @now DATETIME2 = GETDATE();
DECLARE @oneYearAgo DATETIME2 = DATEADD(YEAR, -1, @now);

SELECT @now
SELECT @oneYearAgo
SELECT DATEDIFF(YEAR, @oneYearAgo, @now)
SELECT DATEDIFF(QUARTER, @oneYearAgo, @now)
SELECT DATEDIFF(WEEK, @oneYearAgo, @now) 
SELECT DATEDIFF(DAY, @oneYearAgo, @now)
SELECT DATEDIFF(HOUR, @oneYearAgo, @now)
SELECT DATEDIFF(MINUTE, @oneYearAgo, @now)
SELECT DATEDIFF(SECOND, @oneYearAgo, @now)
 ```


> DATEDIFF, datepart parametresindeki kısaltmaları da kabul eder. Bu kısaltmaların kullanımı genel olarak kafa karıştırıcı olabileceğinden tavsiye edilmez (m vs mi, ww vs w, vb.).

``DATEDIFF`` ayrıca UTC ile SQL Server'ın yerel saati arasındaki farkı belirlemek için de kullanılabilir.

```sql
SELECT DATEDIFF(hh, getutcdate(), getdate()) as 'CentralTimeOffset'
```

## 9.9: DATEPART ve DATENAME
``DATEPART``, belirtilen tarihsaat ifadesinin belirtilen tarih bölümünü sayısal bir değer olarak döndürür.

``DATENAME``, belirtilen tarihin belirtilen tarih bölümünü temsil eden bir karakter dizesi döndürür.

``DATENAME`` çoğunlukla ayın adını veya haftanın gününü almak için kullanışlıdır.

Ayrıca tarihsaat ifadesinin yılını, ayını veya gününü elde etmek için şu şekilde davranan bazı kısayol işlevleri de vardır:

Sözdizimi:
```sql
DATEPART ( datepart , datetime_expr )
DATENAME ( datepart , datetime_expr )
DAY ( datetime_expr )
MONTH ( datetime_expr )
YEAR ( datetime_expr )
```

Örnekler:
```sql
DECLARE @now DATETIME2 = GETDATE();

SELECT @now -- 2024-01-18 10:01:26.790
SELECT DATEPART(YEAR, @now) -- 2024
SELECT DATEPART(QUARTER, @now) -- 1
SELECT DATEPART(WEEK, @now) --3
SELECT DATEPART(HOUR, @now) --10
SELECT DATEPART(MINUTE, @now) --1
SELECT DATEPART(SECOND, @now) --26

-- DATEPART ve DATENAME arasındaki fark
SELECT DATEPART(MONTH, @now) --1
SELECT DATENAME(MONTH, @now) --January
SELECT DATEPART(WEEKDAY, @now) --5
SELECT DATENAME(WEEKDAY, @now) --Thursday

-- Daha kısa kullanım
SELECT DAY(@now) --18
SELECT MONTH(@now) --1
SELECT YEAR(@now) --2024
```

>NOT: DATEPART ve DATENAME aynı zamanda datepart parametresindeki kısaltmaları da kabul eder. Bu kısaltmaların kullanımı kafa karıştırıcı olabileceğinden genellikle önerilmez (m vs mi, ww vs w, vb.).
{: .prompt-info }

## 9.10: DATEPART referansları

Bunlar tarih ve saat işlevlerinde kullanılabilen ``DATEPART`` değerleridir:

| datepart    | kısaltması |
| :---------: | :--------: |
| year        | yy, yyyy   |
| quarter     | qq, q      |
| month       | mm, m      |
| dayofyear   | dy, y      |
| day         | dd, d      |
| week        | wk, ww     |
| weekday     | dw, w      |
| hour        | hh         |
| minute      | mi, n      |
| second      | ss, s      |
| millisecond | ms         |
| microsecond | mcs        |
| nanosecond  | ns         |


## 9.11: Date Format

**YY-MM-DD**
```sql
SELECT RIGHT(CONVERT(VARCHAR(10), SYSDATETIME(), 20), 8) AS [YY-MM-DD]
SELECT REPLACE(CONVERT(VARCHAR(8), SYSDATETIME(), 11), '/', '-') AS [YY-MM-DD]

-- Result : 24-01-08
```

**YYYY-MM-DD**
```sql
SELECT CONVERT(VARCHAR(10), SYSDATETIME(), 120) AS [YYYY-MM-DD]
SELECT REPLACE(CONVERT(VARCHAR(10), SYSDATETIME(), 111), '/', '-') AS [YYYY-MM-DD]

-- Result : 2024-01-08
```

**YYYY-M-D**
```sql
SELECT 
  CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)) + '-' +
  CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '-' + 
  CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) AS [YYYY-M-D]

-- Result : 2024-1-8
```

**YY-M-D**
```sql
SELECT 
  RIGHT(CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)), 2) + '-' +
  CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '-' + 
  CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) AS [YY-M-D]

-- Result : 24-1-8
```

**M-D-YYYY**
```sql
SELECT 
  CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '-' +
  CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) + '-' + 
  CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)) AS [M-D-YYYY]

-- Result : 1-8-2024
```

**M-D-YY**
```sql
SELECT 
  CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '-' +
  CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) + '-' +
  RIGHT(CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)), 2) AS [M-D-YY]

-- Result : 1-8-24
```

**D-M-YYYY**
```sql
SELECT 
  CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) + '-' +
  CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '-' + 
  CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)) AS [D-M-YYYY]

-- Result : 8-1-2024
```

**D-M-YY**
```sql
SELECT 
  CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) + '-' +
  CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '-' +
  RIGHT(CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)), 2) AS [D-M-YY]

-- Result : 8-1-24
```

**YY-MM**
```sql
SELECT RIGHT(CONVERT(VARCHAR(7), SYSDATETIME(), 20), 5) AS [YY-MM]
SELECT SUBSTRING(CONVERT(VARCHAR(10), SYSDATETIME(), 120), 3, 5) AS [YY-MM]

-- Result : 24-01
```

**YYYY-MM**
```sql
SELECT CONVERT(VARCHAR(7), SYSDATETIME(), 120) AS [YYYY-MM] 

-- Result : 2024-01
```

**YY-M**
```sql
SELECT 
  RIGHT(CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)), 2) + '-' +
  CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) AS [YY-M] 

-- Result : 24-1
```

**YYYY-M**
```sql
SELECT  
  CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)) + '-' +
  CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) AS [YYYY-M] 

-- Result : 2024-1
```

**MM-YY**
```sql
SELECT RIGHT(CONVERT(VARCHAR(8), SYSDATETIME(), 5), 5) AS [MM-YY]
SELECT SUBSTRING(CONVERT(VARCHAR(8), SYSDATETIME(), 5), 4, 5) AS [MM-YY] 

-- Result : 01-24
```

**MM-YYYY**
```sql
SELECT RIGHT(CONVERT(VARCHAR(10), SYSDATETIME(), 105), 7) AS [MM-YYYY] 

-- Result : 01-2024
```

**M-YY**
```sql
SELECT 
  CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '-' +
  RIGHT(CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)), 2) AS [M-YY] 

-- Result : 1-24
```

**M-YYYY**
```sql
SELECT 
  CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '-' +
  CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)) AS [M-YYYY] 

-- Result : 1-2024
```

**MM-DD**
```sql
SELECT CONVERT(VARCHAR(5), SYSDATETIME(), 10) AS [MM-DD] 

-- Result : 01-08
```

**DD-MM**
```sql
SELECT CONVERT(VARCHAR(5), SYSDATETIME(), 5) AS [DD-MM] 

-- Result : 08-01
```

**M-D**
```sql
SELECT 
  CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '-' +
  CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) AS [M-D] 

-- Result : 1-8
```

**D-M**
```sql
SELECT 
  CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) + '-' +
  CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) AS [D-M] 

-- Result : 8-1
```

**M/D/YYYY**
```sql
SELECT 
  CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '/' +
  CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) + '/' + 
  CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)) AS [M/D/YYYY]

-- Result : 1/8/2024
```

**M/D/YY**
```sql
SELECT 
  CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '/' +
  CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) + '/' +
  RIGHT(CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)), 2) AS [M/D/YY]

-- Result : 1/8/24
```

**D/M/YYYY**
```sql
SELECT 
  CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) + '/' +
  CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '/' + 
  CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)) AS [D/M/YYYY]

-- Result : 8/1/2024
```

**D/M/YY**
```sql
SELECT 
  CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) + '/' +
  CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '/' +
  RIGHT(CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)), 2) AS [D/M/YY]

-- Result : 8/1/24
```

**YYYY/M/D**
```sql
SELECT 
  CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)) + '/' +
  CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '/' + 
  CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) AS [YYYY/M/D]

-- Result : 2024/1/8
```

**YY/M/D**
```sql
SELECT RIGHT(CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)), 2) + '/' +
CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '/' + 
CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) AS [YY/M/D]

-- Result : 
```

**MM/YY**
```sql
SELECT RIGHT(CONVERT(VARCHAR(8), SYSDATETIME(), 3), 5) AS [MM/YY]

-- Result : 
```

**MM/YYYY**
```sql
SELECT RIGHT(CONVERT(VARCHAR(10), SYSDATETIME(), 103), 7) AS [MM/YYYY]

-- Result : 
```

**M/YY**
```sql
SELECT CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '/' +
RIGHT(CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)), 2) AS [M/YY]

-- Result : 
```

**M/YYYY**
```sql
SELECT CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '/' +
CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)) AS [M/YYYY]

-- Result : 
```

**YY/MM**
```sql
SELECT CONVERT(VARCHAR(5), SYSDATETIME(), 11) AS [YY/MM]

-- Result : 
```

**YYYY/MM**
```sql
SELECT CONVERT(VARCHAR(7), SYSDATETIME(), 111) AS [YYYY/MM]

-- Result : 
```

**YY/M**
```sql
SELECT RIGHT(CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)), 2) + '/' +
CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) AS [YY/M]

-- Result : 
```

**YYYY/M**
```sql
SELECT CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)) + '/' +
CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) AS [YYYY/M]

-- Result : 
```

**MM/DD**
```sql
SELECT CONVERT(VARCHAR(5), SYSDATETIME(), 1) AS [MM/DD]

-- Result : 
```

**DD/MM**
```sql
SELECT CONVERT(VARCHAR(5), SYSDATETIME(), 3) AS [DD/MM]

-- Result : 
```

**M/D**
```sql
SELECT CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '/' +
CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) AS [M/D]

-- Result : 
```

**D/M**
```sql
SELECT CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) + '/' +
CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) AS [D/M]

-- Result : 
```

**MM.DD.YYYY**
```sql
SELECT REPLACE(CONVERT(VARCHAR(10), SYSDATETIME(), 101), '/', '.') AS [MM.DD.YYYY]

-- Result : 
```

**MM.DD.YY**
```sql
SELECT REPLACE(CONVERT(VARCHAR(8), SYSDATETIME(), 1), '/', '.') AS [MM.DD.YY]

-- Result : 
```

**M.D.YYYY**
```sql
SELECT 
CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '.' +
CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) + '.' + 
CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)) AS [M.D.YYYY]

-- Result : 
```

**M.D.YY**
```sql
SELECT CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '.' +
CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) + '.' +
RIGHT(CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)), 2) AS [M.D.YY]

-- Result : 
```

**DD.MM.YYYY**
```sql
SELECT CONVERT(VARCHAR(10), SYSDATETIME(), 104) AS [DD.MM.YYYY]

-- Result : 
```

**DD.MM.YY**
```sql
SELECT CONVERT(VARCHAR(10), SYSDATETIME(), 4) AS [DD.MM.YY]

-- Result : 
```

**D.M.YYYY**
```sql
SELECT CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) + '.' +
CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '.' + CAST(YEAR(SYSDATETIME())
AS VARCHAR(4)) AS [D.M.YYYY]

-- Result : 
```

**D.M.YY**
```sql
SELECT CAST(DAY(SYSDATETIME()) AS VARCHAR(2)) + '.' +
CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '.' +
RIGHT(CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)), 2) AS [D.M.YY]

-- Result : 
```

**YYYY.M.D**
```sql
SELECT CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)) + '.' +
CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '.' + CAST(DAY(SYSDATETIME()) AS
VARCHAR(2)) AS [YYYY.M.D]

-- Result : 
```

**YY.M.D**
```sql
SELECT RIGHT(CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)), 2) + '.' +
CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '.' + CAST(DAY(SYSDATETIME()) AS
VARCHAR(2)) AS [YY.M.D]

-- Result : 
```

**MM.YYYY**
```sql
SELECT RIGHT(CONVERT(VARCHAR(10), SYSDATETIME(), 104), 7) AS [MM.YYYY]

-- Result : 
```

**MM.YY**
```sql
SELECT RIGHT(CONVERT(VARCHAR(8), SYSDATETIME(), 4), 5) AS [MM.YY]

-- Result : 
```

**M.YYYY**
```sql
SELECT CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '.' +
CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)) AS [M.YYYY]

-- Result : 
```

**M.YY**
```sql
SELECT CAST(MONTH(SYSDATETIME()) AS VARCHAR(2)) + '.' +
RIGHT(CAST(YEAR(SYSDATETIME()) AS VARCHAR(4)), 2) AS [M.YY]

-- Result : 
```

**YYYY.MM**
```sql
SELECT CONVERT(VARCHAR(7), SYSDATETIME(), 102) AS [YYYY.MM]

-- Result : 
```

**YY.MM**
```sql
SELECT CONVERT(VARCHAR(5), SYSDATETIME(), 2) AS [YY.MM]

-- Result : 
```

