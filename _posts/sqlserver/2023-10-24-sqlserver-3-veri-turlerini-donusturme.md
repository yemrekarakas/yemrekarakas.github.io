---
title: 3 - Veri türlerini dönüştürme (Converting data types)
author: yek
date: 2023-10-24 12:05:32 +0300
categories: [Tutorial, SQL Server]
tags: [sqlserver, sql, microsoft, t-sql]
image:
  path: https://yemrekrks.sirv.com/posts/sqlserver/1e7de65b3809636c874e30ba231b0a1d.jpg
  alt: Skógafoss waterfall with Aurora borealis or northern light, Iceland
---

<style>
r { color: red }
b { color: blue }
y { color: yellow }
</style>


# 3 - Veri türlerini dönüştürme (Converting Data Types)

## 3.1 - TRY PARSE

*<y>Version ≥ SQL Server 2012</y>*

Dize veri türünü (String) hedef veri türüne (Tarih veya Sayısal) dönüştürür.

Örneğin kaynak veri string tipinde ve tarih tipine çevirmemiz gerekiyor. Dönüştürme girişimi başarısız olursa boş değer geri döner.

**Sözdizimi**: TRY_PARSE (string_value AS data_type [ USING culture ])

- String_value – Bu bağımsız değişken, NVARCHAR(4000) türündeki kaynak değeridir.
- Data_type – Bu bağımsız değişken, tarih veya sayısal olarak hedef veri türüdür.
- Kültür – Değerin kültür biçimine dönüştürülmesine yardımcı olan isteğe bağlı bir argümandır. Diyelim ki tarihi Türkçe olarak görüntülemek için kültür türünü 'Tr-TR' olarak yazmanız gerekir.
- Geçerli bir kültür adı girmezseniz, o zaman PARSE bir hata verecektir.

```sql
DECLARE @fakeDate AS varchar(10);
DECLARE @realDate AS VARCHAR(10);

SET @fakeDate = 'bentarihdegilim';
SET @realDate = '24/10/2023';

SELECT TRY_PARSE(@fakeDate AS DATE); --NULL parse başarısız olduğundan

SELECT TRY_PARSE(@realDate AS DATE); -- NULL format uyuşmazlığı nedeniyle

SELECT TRY_PARSE(@realDate AS DATE USING 'Tr-TR'); -- 2023-10-24
```

## 3.2 - TRY CONVERT

*<y>Version ≥ SQL Server 2012</y>*

Değeri belirtilen veri türüne dönüştürür ve dönüştürme başarısız olursa NULL değerini döndürür. 

**Sözdizimi**: TRY_CONVERT ( data_type [ ( length ) ], expression [, style ] )

TRY_CONVERT(), dönüştürme başarılı olursa, belirtilen veri türünde bir değer döndürür; aksi takdirde null değerini döndürür.

- Data_type - Dönüştürülecek veri türü. Burada uzunluk, sonuç alınmasına yardımcı olan isteğe bağlı bir parametredir.
- Expression - Dönüştürülecek değer.
- Style - Biçimlendirmeyi belirleyen isteğe bağlı bir parametredir. Diyelim ki “18 Mayıs 2013” ​​gibi bir tarih formatı istiyorsunuz o zaman 111 stiline ihtiyacınız var.

```sql
DECLARE @sampletext AS VARCHAR(10);
SET @sampletext = '123456';

DECLARE @realDate AS VARCHAR(10);
SET @realDate = '13/09/2015';

SELECT TRY_CONVERT(INT, @sampletext); -- 123456

SELECT TRY_CONVERT(DATETIME, @sampletext); -- NULL

SELECT TRY_CONVERT(DATETIME, @realDate, 111); -- Sep, 13 2015

SELECT   
    CASE WHEN TRY_CONVERT(float, 'test') IS NULL   
    THEN 'Cast failed'  
    ELSE 'Cast succeeded'  
END AS Result;  -- Cast failed
```

Bir kaç örnek daha,

```sql
create table t (dt varchar(32));
insert into t values ('08/03/2017 3:51 PM'),('28/03/2017 7:30 AM');

set dateformat dmy;
select 
    dt
  ,tryparse      = convert(char(23), try_parse(dt as datetime2(3)), 121) 
  ,tryparse100   = convert(char(23), try_parse(dt as datetime2(3)), 100) 
  ,tryparseEn    = convert(char(23), try_parse(dt as datetime2(3) using 'en-GB'), 121) 
  ,tryparseTr    = convert(char(23), try_parse(dt as datetime2(3) using 'tr-TR'), 100) 
  ,tryconvert    = convert(char(23), try_convert(datetime2(3), dt), 121) 
  ,tryconvert121 = convert(char(23), try_convert(datetime2(3), dt, 121), 121) 
  ,trycast       = convert(char(23), try_cast(dt as datetime2(3)), 121)
from t
```

## 3.3 - TRY CAST

*<y>Version ≥ SQL Server 2012</y>*

Değeri belirtilen veri türüne dönüştürür ve dönüştürme başarısız olursa NULL değerini döndürür.

**Sözdizimi**: TRY_CAST ( expression AS data_type [ ( length ) ] )

TRY_CAST(), dönüştürme başarılı olursa, belirtilen veri türüne bir değer dönüşümü döndürür; aksi takdirde null değerini döndürür.

- Expression - Kaynak değer.
- Data_type - Kaynak değerin aktaracağı hedef veri türü.
- Uzunluk - Hedef veri türünün uzunluğunu belirten isteğe bağlı bir parametredir.

```sql
DECLARE @sampletext AS VARCHAR(10);
SET @sampletext = '123456';

SELECT TRY_CAST(@sampletext AS INT); -- 123456

SELECT TRY_CAST(@sampletext AS DATE); -- NULL
```

## 3.4 - CAST
Cast() işlevi, bir veri türü değişkenini veya verileri bir veri türünden başka bir veri türüne dönüştürmek için kullanılır.

**Sözdizimi**: CAST ( [Expression] AS Datatype)

```sql
DECLARE @A varchar(2)
DECLARE @B varchar(2)

SET @A = '25a'
SET @B = '15'

SELECT CAST(@A as int) + CAST(@B as int) as Result
--'25a' is casted to 25 (string to int)
--'15' is casted to 15 (string to int)

--Result
--40

DECLARE @C varchar(2) = 'a'

SELECT CAST(@C as int) as Result

--Result
--Conversion failed when converting the varchar value 'a' to data type int.
```

Başarısız olursa hata fırlatır.

## 3.5 - CONVERT
Verileri bir türden diğerine dönüştürdüğünüzde, bir depolama prosedürü veya başka bir rutin içinde datetime türünden varchar türüne veri dönüştürme ihtiyacı olabilir. Bu tür durumlar için CONVERT() fonksiyonu kullanılır. CONVERT() fonksiyonu, tarih/saat verilerini çeşitli formatlarda görüntülemek için kullanılır. Sözdizimi şu şekildedir:

**Sözdizimi**: 
CONVERT(data_type(length), expression, style)
CONVERT(veri_türü(uzunluk), ifade, stil)

Stil - karakter verisine dönüştürme için datetime veya smalldatetime için stil değerleri. Bir stil değerine dört basamaklı bir yıl içeren bir yüzyıl (yyyy) eklemek için 100 ekleyin.

Örneğin, bir datetime ifadesini varchar tipine çevirmek için:

```sql
DECLARE @tarih datetime = GETDATE();
DECLARE @tarihVarchar varchar(20);

-- Stil 101: MM/DD/YYYY
SET @tarihVarchar = CONVERT(varchar, @tarih, 101);
SELECT @tarihVarchar AS 'Format 101';

-- Stil 103: DD/MM/YYYY
SET @tarihVarchar = CONVERT(varchar, @tarih, 103);
SELECT @tarihVarchar AS 'Format 103';

-- Stil 120: YYYY-MM-DD HH:MI:SS
SET @tarihVarchar = CONVERT(varchar, @tarih, 120);
SELECT @tarihVarchar AS 'Format 120';

-- Stil 120: 13:27:16
SELECT CONVERT(VARCHAR(20), GETDATE(), 108)
```