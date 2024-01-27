---
title: 2 - Veri Tipleri (Data Types)
author: yek
date: 2023-10-20 17:47:47 +0300
categories: [Tutorial, SQL Server]
tags: [sqlserver, sql, microsoft, t-sql]
image:
  path: https://yemrekrks.sirv.com/posts/sqlserver/1e7db0f80f2499ecc72caac9a30f0c62.jpg
  alt: Great Barrier Reef aerial view
---

<style>
r { color: red }
b { color: blue }
y { color: yellow }
</style>


# 2 - Veri Tipleri (Data Types)
Bu bölümde, SQL Server'ın kullanabileceği veri türlerini, veri aralıklarını, uzunluklarını ve (varsa) sınırlamaları anlatılacaktır.

## 2.1 - Tam Sayısallar
Tam sayısal veri türlerinin iki temel sınıfı vardır: Tam sayılar (Integer) ve sabit hassasiyet ve basamak sayısı (Fixed Precision and Scale)

### Tam Sayı Veri Türleri
- bit
- tinyint
- smallint
- int
- bigint

Integer hiçbir zaman kesirli kısım içermeyen ve her zaman sabit miktarda depolama alanı kullanan sayısal değerlerdir.  

tam sayı veri türlerinin aralığı ve depolama boyutları bu tabloda gösterilmektedir:

| Veri Tipi | Aralık                                                                    | Kapladığı Miktar  |
| --------- | ------------------------------------------------------------------------- | ----------------- |
| bit       | 0 - 1                                                                     | 1 bit             |
| tinyint   | 0 - 255                                                                   | 1 byte            |
| smallint  | -2^15 (-32,768) - 2^15-1 (32,767)                                         | 2 byte            |
| int       | -2^31 (-2,147,483,648) - 2^31-1 (2,147,483,647)                           | 4 byte            |
| bigint    | -2^63 (-9,223,372,036,854,775,808) - 2^63-1 (9,223,372,036,854,775,807)   | 8 byte            |


### Sabit Hassasiyet ve Ölçek Veri Türleri
- numeric
- decimal
- smallmoney
- money
  
SQL Server'da "Fixed Precision and Scale" veri tipleri, sabit bir hassasiyet ve ölçek ile sayısal verileri temsil etmek için kullanılan veri tipleridir. Bu veri tipleri, özellikle finansal verileri veya diğer alanlarda hassas matematiksel hesaplamalar gerektiren durumlar için uygundur.

Bu veri türleri sayıları tam olarak temsil etmek için kullanışlıdır. Değerler belirtilen aralıklara sığabildiği sürece değerde yuvarlama sorunları yaşanmaz.

**decimal** ile **numeric** eşdeğerdir, aynıdır.

| Veri Tipi                                    | Aralık                  | Kapladığı Miktar             |
| -------------------------------------------- | ----------------------- | ---------------------------- |
| Decimal [(p [, s])] veya Numeric [(p [, s])] | -10^38 + 1 - 10^38 - 1  | Hassasiyet tablosuna bakınız |

**precision** toplam basamak sayısını belirtir.

**scale** virgülden sonraki basamak sayısını belirtir.

Ondalık veya sayısal bir veri türü tanımlarken precision [p] ve scale [s]'yi belirtmeniz gerekir. Bir precision belirtmezseniz, varsayılan precision 18'dir.

Scale, virgülden sonraki basamak sayısıdır. 0,00 ile 999,99 arasında bir sayı kaydetmeniz gerekiyorsa, precision olarak 5 (beş rakam) ve scale olarak 2 (virgülden sonra iki rakam) belirtmeniz gerekir. Varsayılan scale 0 sıfırdır.

Precision(Hassasiyet) Tablosu

| Aralık  | Kapladığı Miktar |
| ------- | ---------------- |
| 1 - 9   | 5 byte           |
| 10 - 19 | 9 byte           |
| 20 - 28 | 13 byte          |
| 29 - 38 | 17 byte          |


### Parasal Veri Türleri
Bu veri türleri özellikle muhasebe, finans ve diğer parasal veriler için tasarlanmıştır. Bu türlerin sabit bir ölçeği vardır. 4 - ondalık basamaktan sonra her zaman dört rakamı göreceksiniz. Çoğu para birimiyle çalışan çoğu sistem için, 2 ölçekli sayısal değer yeterli olacaktır.

| Veri Tipi  | Aralık                                               | Kapladığı Miktar |
| ---------- | ---------------------------------------------------- | ---------------- |
| money      | -922,337,203,685,477.5808 - 922,337,203,685,477.5807 | 8 byte           |
| smallmoney | -214,748.3648 - 214,748.3647                         | 4 byte           |

## 2.2 - Yaklaşık Sayısal Veri Tipleri
- float [(n)]
- real
  
SQL Server'da, "Approximate Numerics" olarak bilinen yaklaşık sayısal veri tipleri bulunmaktadır. Bu veri tipleri genellikle kesirli sayıları veya hassasiyet gerektirmeyen sayıları temsil etmek için kullanılır. Bu veri türleri kayan nokta sayılarını depolamak için kullanılır.
Eğer çok büyük sayıları veya virgülden sonra belirsiz sayıda basamak içeren sayıları işlemeniz gerekiyorsa, bunlar en iyi seçeneğiniz olabilir


| Veri Tipi | Aralık                                                   | Kapladığı Miktar |
| --------- | -------------------------------------------------------- | ---------------- |
| float[(n)]| -1.79E+308 - -2.23E-308, 0 ve 2.23E-308 - 1.79E+308      | (n) e bağlı      |
| real      | -3.40E + 38 - -1.18E - 38, 0 ve 1.18E - 38 to 3.40E + 38 | 4 byte           |

Kayan sayılar için n değer tablosu. herhangi bir değer belirtilmemişse varsayılan değer 53 olacaktır.

Not: float(24) ile real eşittir.

| n değeri | Aralık    | Kapladığı Miktar |
| -------- | --------- | ---------------- |
| 1 - 24   | 7 digit   | 4 byte           |
| 25 - 53  | 15 digits | 8 byte           |


## 2.3 - Tarih ve Zaman
Bu türler SQL Server'ın tüm sürümlerinde bulunur.
- datetime
- smalldatetime

Bu türler SQL Server 2012'den sonraki tüm SQL Server sürümlerinde bulunmaktadır.
- date
- datetimeoffset
- datetime2
- time


## 2.4 - Karakter (metin, dizi)
- char
- varchar
- text


## 2.5 - Unicode Karakter (metin, dizi)
- nchar
- nvarchar
- ntext


## 2.6 - Binary (metin, dizi)
- binary
- varbinary
- image


## 2.7 - Diğer Veri Tipleri
- cursor
- timestamp
- hierarchyid
- uniqueidentifier
- sql_variant
- xml
- table
- Spatial Types