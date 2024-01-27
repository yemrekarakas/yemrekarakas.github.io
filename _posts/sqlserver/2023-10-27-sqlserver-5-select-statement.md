---
title: 5 - SELECT ifadesi
author: yek
date: 2023-10-27 10:22:38 +0300
categories: [Tutorial, SQL Server]
tags: [sqlserver, sql, microsoft, t-sql]
image:
  path: https://yemrekrks.sirv.com/posts/sqlserver/1e59d8840b851931a9615eacc0b4cc2b.jpg
  alt: Medieval castle Gravensteen (Castle of the Counts) in Ghent, Belgium
---

<style>
r { color: red }
b { color: blue }
y { color: yellow }
</style>


# Bölüm 5 - SELECT ifadesi (SELECT Statement)

SQL'de SELECT ifadesi, tablolar veya viewler gibi veri koleksiyonlarından sonuç kümelerini döndürür. İstenilen sonuçları daha da hassaslaştırmak için WHERE, GROUP BY veya ORDER BY gibi diğer çeşitli maddelerle birlikte kullanılabilir.

## 5.1 - Temel SELECT ifadesi
Bazı tablolardan (bu durumda sistem tablosu) tüm sütunları seçin:

```sql
SELECT *
FROM sys.objects
```
Veya yalnızca belirli sütunlardan bazılarını seçin:

```sql
SELECT object_id, name, type, create_date
FROM sys.objects
```

## 5.2 - WHERE kullanarak satırları filtreleme
WHERE cümleciği yalnızca bazı koşulları karşılayan satırları filtreler:

```sql
SELECT *
FROM sys.objects
WHERE type = 'IT'
```

## 5.3 - ORDER BY kullanarak sonuçları sıralayın
ORDER BY cümleciği, döndürülen sonuç kümesindeki satırları bazı sütunlara veya ifadelere göre sıralar:

```sql
SELECT *
FROM sys.objects
ORDER BY create_date
```

## 5.4 - GROUP BY kullanarak sonuçları gruplama
GROUP BY cümleciği satırları bir değere göre gruplandırır:

```sql
SELECT type, count(*) as c
FROM sys.objects
GROUP BY type
```

Kayıtların toplamını veya sayısını hesaplamak için her gruba bazı işlevler (toplama(sum), sayma(count), ortalama(avg) işlevleri gibi) uygulayabilirsiniz.

| type | c  |
| --   | -- |
| SQ   | 3  |
| S    | 72 |
| IT   | 16 |
| PK   | 1  |
| U    | 5  |

## 5.5 - HAVING kullanarak grupları filtreleme
HAVING cümleciği koşulu karşılamayan grupları kaldırır:

```sql
SELECT type, count(*) as c
FROM sys.objects
GROUP BY type
HAVING count(*) < 10
```

| type | c  |
| --   | -- |
| SQ   | 3  |
| PK   | 1  |
| U    | 5  |

## 5.6 - Yalnızca ilk N satırı döndürme
TOP ifadesi sonuçta yalnızca ilk N satırı döndürür:

```sql
SELECT TOP 10 *
FROM sys.objects
```

## 5.7 - OFFSET FETCH kullanarak sayfalandırma
OFFSET FETCH ifadesi TOP'un daha gelişmiş versiyonudur. N1 satırlarını atlamanızı ve sonraki N2 satırlarını almanızı sağlar:

İlk 50 satırdan sonraki 10 satır
```sql
SELECT *
FROM sys.objects
ORDER BY object_id
OFFSET 50 ROWS FETCH NEXT 10 ROWS ONLY
```

Sadece ilk 50 satırı atlamak için:

```sql
SELECT *
FROM sys.objects
ORDER BY object_id
OFFSET 50 ROWS
```

## 5.8 - FROM olmadan SELECT (veri kaynağı yok)
SELECT ifadesi FROM cümlesi olmadan çalıştırılabilir:

```sql
DECLARE @var int = 17;
SELECT @var as c1, @var + 2 as c2, 'third' as c3
```

Bu durumda, ifadelerin değerlerini/sonuçlarını içeren bir satır döndürülür.