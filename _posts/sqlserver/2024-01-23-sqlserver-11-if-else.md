---
title: 11 - IF...ELSE
author: yek
date: 2024-01-23 21:34:19 +03:00
categories: [Tutorial, SQL Server]
tags: [sqlserver, sql, microsoft, t-sql]
image:
  path: https://yemrekrks.sirv.com/posts/sqlserver/001fe0a1577fc15040c2b0512639a4dc.jpg
  alt: Sunset at the sandy beach, West Coast National Park, Yzerfontein, Western Cape, South Africa
---

<style>
r { color: red }
b { color: blue }
y { color: yellow }
</style>

# 11 - IF...ELSE

## 11.1 - Tek IF ifadesi
Diğer programlama dillerinin çoğunda olduğu gibi T-SQL de IF..ELSE ifadelerini destekler.

Örneğin aşağıdaki örnekte 1 = 1, Doğru olarak değerlendirilen ve kontrolün girdiği ifadedir.
BEGIN..END bloğu ve Print ifadesi 'Bir eşittir Bir' dizesini yazdırır.

```sql
IF ( 1 = 1)
BEGIN
  PRINT 'Bir eşittir Bir'
END
```

## 11.2 - Çoklu IF İfadeleri
Birbirinden tamamen bağımsız birden fazla ifadeyi kontrol etmek için birden fazla IF ifadesini kullanabiliriz.

Aşağıdaki örnekte, her IF ifadesi değerlendirilir ve eğer doğruysa BEGIN...END içindeki kod bloğu yürütülür. 

Bu özel örnekte, Birinci ve Üçüncü ifadeler doğrudur ve yalnızca yazdırılanlar doğrudur.
 ifadeler yürütülecek

```sql
IF (1 = 1) -- Bu doğru
BEGIN
  PRINT 'Birinci IF doğruysa' -- burası çalışacaktır
END

IF (1 = 2)
BEGIN
  PRINT 'İkinci IF doğruysa'
END

IF (3 = 3) -- Bu doğru
BEGIN
  PRINT 'Üçüncü IF doğruysa' -- burası çalışacaktır
END
```

## 11.3 - Tek IF..ELSE ifadesi
Tek bir ``IF..ELSE`` ifadesinde, eğer ifade ``True`` olarak değerlendirilirse, kontrol ilk sıraya girer. ``BEGIN..END`` bloğu ve yalnızca o bloğun içindeki kod yürütülür, ``ELSE`` bloğu basitçe göz ardı edilir.

Öte yandan, eğer ifadesi ``False`` olarak değerlendirilirse ``ELSE BEGIN..END`` bloğu yürütülür ve kontrol asla ilk ``BEGIN..END`` Bloğuna girmez.

Aşağıdaki örnekte ifade ``false`` olarak değerlendirilecek ve ``ELSE`` bloğu yazdırılacaktır.


```sql
IF ( 1 <> 1)
BEGIN
  PRINT 'Bir eşittir bir'
END
ELSE
BEGIN
  PRINT 'İlk ifade doğru değil'
END
```

## 11.4 - Çoklu IF...ELSE ifadeleri
Çoğu zaman, birden çok ifadeyi kontrol etmemiz ve bu ifadelerin sonuçlarına göre belirli eylemleri gerçekleştirmemiz gerekebilir. Bu durumu ele almak için birden çok IF...ELSE IF ifadesi kullanılır. 

Aşağıdaki örnekte, tüm ifadeler üstten alta doğru değerlendirilir. Bir ifade true olarak değerlendirildiğinde, o blok içindeki kod çalıştırılır. Eğer hiçbir ifade true olarak değerlendirilmezse, hiçbir şey çalıştırılmaz.

```sql
IF (1 = 1 + 1)
BEGIN
    PRINT 'İlk If Koşulu'
END
ELSE IF (1 = 2)
BEGIN
    PRINT 'İkinci If Else Bloğu'
END
ELSE IF (1 = 3)
BEGIN
    PRINT 'Üçüncü If Else Bloğu'
END
ELSE IF (1 = 1) -- Bu True
BEGIN
    PRINT 'Son Else Bloğu' -- Sadece bu ifade yazdırılacak
END
```


## 11.5 - Son ELSE olan birden fazla IF...ELSE ifadeleri
Eğer birden çok IF...ELSE IF ifadesi bulunuyorsa ve hiçbiri True olarak değerlendirilmezse, o zaman hiçbiri ifade True olmadığında çalışacak bir kod parçasını da yürütmek istiyorsak, basitçe sona bir ELSE bloğu ekleyebiliriz. 

Aşağıdaki örnekte, hiçbir IF veya ELSE IF ifadesi True olarak değerlendirilmediğinde sadece ELSE bloğu çalışır ve 'Başka bir ifade True değil' mesajını yazdırır:

```sql
IF (1 = 1 + 1)
BEGIN
    PRINT 'İlk If Koşulu'
END
ELSE IF (1 = 2)
BEGIN
    PRINT 'İkinci If Else Bloğu'
END
ELSE IF (1 = 3)
BEGIN
    PRINT 'Üçüncü If Else Bloğu'
END
ELSE
BEGIN
    PRINT 'Başka bir ifade True değil' -- Sadece bu ifade yazdırılacak
END
```
