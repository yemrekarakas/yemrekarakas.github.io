---
title: 1 - Microsoft SQL Server'e Başlarken
author: yek
date: 2023-10-19 22:44:53 +0300
categories: [Tutorial, SQL Server]
tags: [sqlserver, sql, microsoft, t-sql]
image:
  path: https://yemrekrks.sirv.com/posts/sqlserver/1e7bf60c971d96f9854605fef11715f3.jpg
  alt: Victoria Falls sunset with rainbow, Livingstone, Zambia
---

<style>
r { color: red }
b { color: blue }
y { color: yellow }
</style>

# 1 - Microsoft SQL Server'e Başlarken

### Versiyon Tarihi (Release Date)

| Versiyon               | No   | Kod Adı      | Yayın Tarihi | Son Destek Tarihi  | Genişletilmiş Son Tarih |
| ---------------------- | ---- | ------------ | -----------: | -----------------: | ----------------------: |
| <y>SQL Server 2022</y> | 16   | Dallas       | 16 Kas 2022  | 11 Oca 2028        | 11 Oca 2033             |
| SQL Server 2019        | 15   | Aris Seattle | 04 Kas 2019  | 28 Şub 2025        | 08 Oca 2030             |
| SQL Server 2017        | 14   | vNext        | 29 Eyl 2017  | <r>11 Eki 2022</r> | 12 Eki 2027             |
| SQL Server 2016        | 13   |              | 01 Haz 2016  | <r>13 Tem 2021</r> | 14 Tem 2026             |
| SQL Server 2014        | 12   |              | 05 Haz 2014  | <r>09 Tem 2019</r> | 09 Tem 2024             |
| SQL Server 2012        | 11   | Denali       | 20 May 2012  | <r>11 Tem 2017</r> | <r>12 Tem 2022</r>      |
| SQL Server 2008 R2     | 10.5 | Kilimanjaro  | 20 Tem 2010  | <r>08 Tem 2014</r> | <r>09 Tem 2019</r>      |
| SQL Server 2008        | 10   | Katmai       | 06 Kas 2008  | <r>08 Tem 2014</r> | <r>09 Tem 2019</r>      |
| SQL Server 2005        | 9    | Yukon        | 14 Oca 2006  | <r>12 Nis 2011</r> | <r>12 Nis 2016</r>      |
| SQL Server 2000        | 8    | Shiloh       | 30 Kas 2000  | <r>08 Nis 2008</r> | <r>09 Nis 2013</r>      |
| SQL Server 7.0         | 7    | Sphinx       | 27 Kas 1998  | <r>31 Ara 2005</r> | <r>11 Oca 2011</r>      |
| SQL Server 6.5         | 6.5  | Hydra        | 30 Haz 1996  | <r>01 Oca 2002</r> |                         |
| SQL Server 6.0         | 6    | SQL95        | 13 Haz 1995  | <r>31 Mar 1999</r> |                         |
 

## 1.1 - Basit Veri İşleme Dili (DML-Data Manipulation Language) <br> SELECT / INSERT / UPDATE / DELETE

Veri İşleme Dili (kısaca DML), INSERT, UPDATE ve DELETE gibi işlemleri içerir:

Bir tablo oluşturalım 'MerhabaDunya'

```sql
CREATE TABLE MerhabaDunya (
  Id INT IDENTITY,
  Description VARCHAR(1000)
)
```

DML işlemi INSERT, tabloya satır ekleme.
```sql
INSERT INTO MerhabaDunya (Description) VALUES ('Merhaba Dünya')
```

DML işlemi SELECT, tabloyu görüntülüyor.
```sql
SELECT * FROM MerhabaDunya
```

Tablodan belirli bir sütunu seçin.
```sql
SELECT Description FROM MerhabaDunya
```

DML işlemi UPDATE, tablodaki belirli bir satırın güncellenmesi.
```sql
UPDATE MerhabaDunya SET Description = 'Merhaba, Dünya!' WHERE Id = 1
```

Tablodan satırların seçilmesi UPDATE işeminden sonra açıklamanın değiştiğini görün.
```sql
SELECT * FROM MerhabaDunya
```

DML işlemi DELETE, tablodan satırın silinmesi.
```sql
DELETE FROM MerhabaDunya WHERE Id = 1
```

DELETE işleminden sonra tablo içeriğine bakın.
```sql
SELECT * FROM MerhabaDunya
```

Aşağıdaki örnekler tabloların nasıl sorgulanacağını göstermektedir:
```sql
USE Northwind;
GO
SELECT TOP 10 * FROM Customers
ORDER BY CompanyName
```

![Result](/assets/img/posts/sqlserver/ssms.png)

Northwind veritabanından CompanyName sütununa göre sıralanan Customers tablosunun ilk 10 kaydını getirecektir.

[Northwind](https://github.com/yemrekarakas/yemrekarakas.github.io/blob/main/assets/data/Northwind.rar) Microsoft'un örnek veritabanlarından biridir, [burada](https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/databases/northwind-pubs/instnwnd.sql) script halini bulabilirsiniz.

**Not:**
USE Northwind; sonraki tüm sorgular için varsayılan veritabanını değiştirir.

Daha sonra [Veritabanı].[Şema].[Tablo] biçimindeki sözdizimini kullanarak da veritabanını seçebilirsiniz:

```sql
SELECT TOP 10 * FROM Northwind.dbo.Customers 
ORDER BY CompanyName

SELECT TOP 10 * FROM Northwind.dbo.Suppliers
ORDER BY City
```

Farklı veritabanlarından veri sorguluyorsanız bu kullanışlıdır. Şema ve tam sözdizimi kullanılırken belirtilmesi gerekir. Bunu, klasörünüzün içindeki bir klasör olarak düşünebilirsiniz. dbo varsayılan şemadır. Varsayılan şema atlanabilir. Diğer tüm kullanıcı tanımlı şemalar belirtilmelidir.

Veritabanı tablosu ayrılmış kelimeler (bunlara reserve keywords denir) gibi adlandırılan sütunlar içeriyorsa, örneğin Date, köşeli parantez içinde kulanılır:
```sql
-- DESC tersten sırala
SELECT TOP 10 [Date] FROM dbo.MyTable
ORDER BY [Date] DESC
```

Sütun adında boşluk bulunması durumunda da aynı durum geçerlidir. (boşluk önerilmez!). Bir alternatif sözdizimi köşeli parantez yerine çift tırnak kullanmaktır, örneğin:
 
```sql
-- DESC tersten sırala
SELECT TOP 10 "Date" FROM dbo.MyTable
ORDER BY "Date" DESC
 ```
eşdeğerdir ancak çok yaygın olarak kullanılmaz.

Çift tırnak ve tek tırnak arasındaki farka dikkat edin. Tek tırnak metin (string, dize) için kullanılır,
```sql
-- DESC tersten sırala
SELECT TOP 10 "Date" FROM dbo.MyTable
WHERE User = 'johndoe'
ORDER BY "Date" DESC
```
geçerli bir sözdizimidir. 

T-SQL'in NChar ve NVarchar veri türleri için bir N önekine sahip olduğuna dikkat edin;
```sql
SELECT TOP 10 * FROM NORTHWND.dbo.Customers
WHERE CompanyName LIKE N'AL%'
ORDER BY CompanyName
```
AL ile başlayan şirket adına sahip tüm şirketleri döndürür. (% bir joker karakterdir, bir DOS komut satırında yıldız işaretini kullandığınız gibi, örneğin DIR AL*) LIKE için birkaç joker karakter mevcuttur, öğrenmek için [buraya](https://learn.microsoft.com/en-us/sql/t-sql/language-elements/like-transact-sql?view=sql-server-ver16&redirectedfrom=MSDN#pattern) bakın.

### Joins (birleştirmek, katılmak)
Tek bir tabloda değil de birden çok tabloda bulunan alanları sorgulamak istiyorsanız join kullanışlıdır. 

Örneğin:
Northwind veritabanındaki Territories tablosundaki tüm sütunları sorgulamak istiyorsunuz. Ama farklı bir tablo olan Region tablosundan RegionDescription alanına ihtiyacınız olduğunu fark ettiniz.
İhtiyacınız ortak bir anahtar alan : RegionID
bu bilgileri aşağıdaki gibi tek bir sorguda birleştirmek için kullanabilirsiniz. (TOP 5 yalnızca ilk 5 satırı döndürür, tüm satırları almak istiyorsanız kaldırabilirsiniz)

```sql
SELECT TOP 5 
  Territories.*,
  Region.RegionDescription
FROM Territories
  INNER JOIN Region ON Territories.RegionID = Region.RegionID
ORDER BY TerritoryDescription
```

Territories tablosundaki tüm sütunları ve Region tablosundaki RegionDescription sütununu gösterecektir. 
Sonuç TerritoryDescription alanına göre sıralanır.

### Table Aliases (tablo takma adları)
Sorgunuz iki veya daha fazla tabloya referans gerektirdiğinde Tablo Takma Adını kullanabilirsiniz. Takma adlar, tam tablo adı yerine kullanılabilecek tablolara yapılan kısa isimlendirmelerdir.

Takma ad kullanmanın sözdizimi şöyledir: ``<TableName> [as] <alias>``

AS isteğe bağlı bir anahtar kelimedir.
Örneğin önceki sorgu şu şekilde yeniden yazılabilir:

```sql
SELECT TOP 5 
  T.*,
  R.RegionDescription
FROM Territories T
  INNER JOIN Region R ON T.RegionID = R.RegionID
ORDER BY TerritoryDescription
```

Aynı tabloyu iki kez kullanıyor olsanız bile takma adlarının sorgudaki tüm tablolar için benzersiz olması gerekir.

```sql
SELECT 
  S.FirstName AS SupervisorFirstName, --Kolon alanını yeniden adlandırır
  S.LastName AS SupervisorLastName, --Kolon alanını yeniden adlandırır
  E.*
FROM Employees E 
  INNER JOIN Employees S ON E.ReportsTo = S.EmployeeId
WHERE E.EmployeeId = 6
```

### Unions (birleşim)
Daha önce gördüğümüz join farklı tablolardan sütunlar ekler. Peki farklı tablolardan satırları birleştirmek isterseniz, bu durumda UNION kullanabilirsiniz.
Diyelim ki bir parti planlıyorsunuz ve sadece çalışanlar değil, aynı zamanda müşterileri de davet etmek istiyorsunuz.
Bunu yapmak için bu sorguyu çalıştırabilirsiniz:

```sql
SELECT FirstName + ' ' + LastName as ContactName, Address, City FROM Employees
UNION
SELECT ContactName, Address, City FROM Customers
```

Çalışanların ve müşterilerin adlarını, adreslerini ve şehirlerini tek bir tabloda döndürecektir. 

Sütun numarası, sütun adları, sıra ve veri türü, tüm select ifadelerinde eşleşmelidir.

> Yinelenen satırlar (varsa) otomatik olarak ortadan kaldırılır,bunu istemiyorsanız UNION yerine UNION ALL kullanın.
{: .prompt-warning }

### Table Variables (tablo değişkenleri)
Geçici verilerle (özellikle prosedurler de) uğraşmanız gerekiyorsa tablo değişkenlerini kullanmak yararlı olabilir:

Gerçek bir tablo ile tablo değişkeni arasındaki fark, bunun yalnızca geçici işlem için bellekte bulunmasıdır.

Örnek:
```sql
DECLARE @Region TABLE (
  RegionID int,
  RegionDescription NChar(50)
)
```
Bellekte bir tablo oluşturur. Bu durumda @ öneki bir değişken olduğu için zorunludur. Tüm DML'leri gerçekleştirebilirsiniz.

örneğin, satır eklemek, silmek ve seçmek için,

```sql
INSERT INTO @Region values(3, 'Northern')
INSERT INTO @Region values(4, 'Southern')
```

Normalde, bunu gerçek bir tablodan doldurursunuz:

```sql
INSERT INTO @Region
SELECT * FROM dbo.Region WHERE RegionID > 2;
```
Bu, dbo.Region gerçek tablosundan filtrelenmiş değerleri okur ve bunu @Region bellek tablosuna ekler.

Daha ileri işlemler için kullanılabileceğiniz bir örnek:

```sql
SELECT * FROM Territories t
JOIN @Region r on t.RegionID = r.RegionID
```

Bu durumda tüm Northern(Kuzey) ve Southern(Güney) bölgeleri geri dönecektir.

NOT: Microsoft, tablo değişkenindeki veri satırlarının sayısı 100'den az ise tablo değişkenlerinin kullanılmasını önerir.
Daha büyük miktarda veriyle çalışacaksanız bunun yerine temporary table (geçici bir tablo) kullanın.

## 1.2 - Bir tablodaki tüm satırları ve sütunları seçmek
Sözdizimi:
```sql
SELECT * FROM table_name
```

Yıldız * işareti kullanmak, tablodaki tüm sütunları seçmek için bir kısayol görevi görür.

Bu, tabloya bir takma ad eklediğinizde de aynı şekilde çalışır,
```sql
SELECT * FROM Employees AS e
```

Veya belirli bir tablodan tümünü seçmek istiyorsanız, alias + ".* " kullanabilirsiniz:
```sql
SELECT 
  e.*, et.TerritoryID 
FROM Employees e
INNER JOIN EmployeeTerritories et ON et.EmployeeID = e.EmployeeID 
```

Veritabanı nesnelerine tam olarak nitelenmiş adlar kullanılarak da erişilebilir:
```sql
SELECT * FROM [server_name].[database_name].[schema_name].[table_name]
```

Sunucu ve/veya veritabanı adlarının değiştirilmesi, sorgularda sorunlara neden olacağından bu pek önerilmez.

Sorguların tek bir sunucuda yürütülmesi durumunda tablo_adı'ndan önceki alanların atlanabileceğini unutmayın, sırasıyla veritabanı ve şema.
Ancak bir veritabanının birden fazla şemaya sahip olması yaygındır ve bu durumlarda şema adı mümkün olduğunca atlanmamalıdır.

> SELECT *'in yerine sütun adlarını her zaman açıkça belirtmek daha güvenlidir.
{: .prompt-info }

```sql
SELECT col1, col2, col3 FROM table_name
```

## 1.3 - Belirli satırları güncelleme
```sql
UPDATE HelloWorlds
SET HelloWorld = 'HELLO WORLD!!!'
WHERE Id = 5
```

Yukarıdaki kod HelloWorlds tablosunda "Id = 5" olan kayıt için "HelloWorld" alanının değerini "HELLO WORLD!!!" ile günceller.
> Bir güncelleme ifadesinde, aşağıdaki durumlar dışında tüm tablonun güncellenmesini önlemek için "where" ifadesinin kullanılması tavsiye edilir.
{: .prompt-warning }

## 1.4 - Tüm satırları silme
```sql
DELETE FROM HelloWorlds
```
Bu, tablodaki tüm verileri silecektir. Bu kodu çalıştırdıktan sonra tablo hiçbir satır içermeyecektir. 

DROP TABLE'ın aksine, bu, tablonun kendisini ve yapısını korur ve bu tabloya yeni satırlar eklemeye devam edebilirsiniz.

Tablodaki tüm satırları silmenin başka bir yolu da:
```sql
TRUNCATE TABLE HelloWords
```

DELETE işleminin farklılığı:
1. Truncate işleminde log dosyasında saklanmaz.
2. IDENTITY(KİMLİK) alanı mevcutsa bu sıfırlanacaktır.
3. TRUNCATE, tablonun tamamına uygulanır bir kısmına uygulanamaz. (bunun yerine DELETE komutuyla WHERE yan tümcesini ilişkilendirin)

TRUNCATE işleminin kısıtlamaları.
1. Tabloda FOREIGN KEY referansı varsa
2. Eğer tablo INDEXED VIEW de kullanılmışsa
3. Eğer tablo TRANSACTIONAL REPLICATION or MERGE REPLICATION kullanılarak yayınlanıyorsa
4. Tabloda tanımlanan herhangi bir TRIGGER tetiklenmez.

[daha fazla bilgi](https://learn.microsoft.com/en-us/sql/t-sql/statements/truncate-table-transact-sql?view=sql-server-ver16&redirectedfrom=MSDN)

## 1.5 - Koddaki yorumlar (Comments)

Transact-SQL iki tür yorum yazmayı destekler. Yorumlar veritabanı motoru tarafından dikkate alınmaz ve insanların okuması için kullanılır.

Comments önünde -- bulunur ve yeni bir satırla karşılaşılıncaya kadar dikkate alınmaz:
```sql
-- Bu bir yorum
SELECT *
FROM MyTable -- Bu başka bir yorum
WHERE Id = 1;
```
Eğik çizgi yıldız yorumları /* ile başlar ve */ ile biter. Bu sınırlayıcılar arasındaki tüm metinler yorum olarak kabul edilir.
```sql
/* Bu bir 
çoklu satır 
blok 
yorumudur.
*/
SELECT Id = 1, [Message] = 'First row'
UNION ALL
SELECT 2, 'Second row'
/* Bu tek satır yorum */
SELECT 'More';
```

Eğik çizgi yıldız yorumları iç içe yerleştirilebilir ve eğik çizgi yıldız yorumunun içindeki bir başlangıç ​​/*'ın */ ile bitmesi gerekir.

Aşağıdaki kod hataya neden olacaktır
```sql
/*
SELECT *
FROM CommentTable
WHERE Comment = '/*'
*/
```

Alıntı içinde olsa bile eğik çizgi yıldız yorumun başlangıcı olarak kabul edilir. Bu yüzden sonlandırılması gerekiyor, doğru yol şu olurdu.
```sql
/*
SELECT *
FROM CommentTable
WHERE Comment = '/*'
*/ */
```

## 1.6 - PRINT (yazdır)
Çıkış konsoluna bir mesaj yazdırır.
SQL Server Management Studio'yu kullanarak sonuçlar sekmesi yerine mesajlar sekmesine bakın.

```sql
PRINT 'Hello World!';
```

## 1.7 - Bir koşulla eşleşen satırları seçin
Genel olarak sözdizimi şöyledir:
```sql
SELECT <column names>
FROM <table name>
WHERE <condition>
```

Örnek:
```sql
SELECT FirstName, Age
FROM Users
WHERE LastName = 'Smith'
```

Koşullar karmaşık olabilir
```sql
SELECT FirstName, Age
FROM Users
WHERE LastName = 'Smith' AND (City = 'New York' OR City = 'Los Angeles')
```

## 1.8 - Tüm satırları güncelleme
Basit bir güncelleme şekli, tablonun belirli bir alanındaki tüm değerleri arttıralım. Bunu yapabilmek için şunları yapmamız gerekiyor:

Alanı ve artış değerini tanımlayın

Aşağıda score alanını 1 (tüm satırlarda) artıran bir örnek verilmiştir:
```sql
UPDATE Scores
SET score = score + 1
```
Belirli bir satır için yanlışlıkla bir GÜNCELLEME yaparsanız verilerinizi bozabileceğiniz için bu tehlikeli olabilir.

## 1.9 - Truncate Table
```sql
TRUNCATE TABLE HelloWorlds
```

Bu kod HelloWorlds tablosundaki tüm verileri silecektir. TRUNCATE TABLE, Delete from Table'e neredeyse benzer aradaki fark Truncate ile Where cümlelerini kullanamamanızdır. TRUNCATE TABLE de daha az işlem günlüğü tutlur.

> Bir kimlik(ID) sütunu varsa, bunun ilk değerine sıfırlanacağını unutmayın (örneğin, otomatik olarak artan ID 1'den yeniden başlar). Kimlik sütunlarının başka bir tabloda yabancı anahtar olarak kullanılması durumunda bu durum tutarsızlığa yol açabilir.
{: .prompt-warning }

## 1.10 - Temel Sunucu Bilgilerini Alma
Çalışan MS SQL Server sürümünü döndürür.
```sql
SELECT @@VERSION
```

MS SQL Server örneğinin adını döndürür.
```sql
SELECT @@SERVERNAME
```

MS SQL Server'ın çalıştığı Windows hizmetinin adını döndürür.
```sql
SELECT @@SERVICENAME
```

SQL Server'ın çalıştığı makinenin fiziksel adını döndürür.
```sql
SELECT serverproperty('ComputerNamePhysicalNetBIOS');
```

## 1.11 - Yeni tablo oluşturun ve eski tablolardan kayıtları ekleyin
Eski tablonun yapısına sahip yeni bir tablo oluşturur ve tüm satırları yeni tabloya ekler.
 ```sql
SELECT * INTO NewTable FROM OldTable
 ```

Bazı Kısıtlamalar
1. Yeni tablo olarak bir tablo değişkeni veya tablo değerli parametre belirtemezsiniz.
2. Kaynak tablo olsa bile bölümlenmiş bir tablo oluşturmak için SELECT…INTO komutunu kullanamazsınız.
3. Kaynak tabloda tanımlanan indeksler, kısıtlamalar ve tetikleyiciler yeni tabloya aktarılmaz. SELECT...INTO ifadesinde de belirtilemezler.
4. ORDER BY ifadesinin belirtilmesi, satırların belirtilen sıraya göre eklendiğini garanti etmez.
5. Seçim listesine hesaplanan bir sütun eklendiğinde, yeni tablodaki karşılık gelen sütun hesaplanmış bir sütun değildir. Yeni sütundaki değerler, şu anda hesaplanan değerlerdir.

[daha fazla bilgi](https://learn.microsoft.com/en-us/sql/t-sql/queries/select-into-clause-transact-sql?view=sql-server-ver16&redirectedfrom=MSDN)


## 1.12 - Verileri güvenli bir şekilde değiştirmek için Transaction kullanma
Transaction, bir veya daha fazla SQL komutunu içerebilir ve bu komutlar bir arada çalışır. Transaction, verilerin bütünlüğünü korumak ve birden çok komutun başarılı bir şekilde tamamlanmasını sağlamak için kullanılır. Transaction başladığında, veritabanı değişikliklerini geçici bir alanda saklar ve işlem başarıyla tamamlandığında bu değişiklikler kalıcı hale gelir. Ancak işlem başarısız olursa, tüm değişiklikler geri alınır.

SQL Server'da işlem yönetimi için aşağıdaki komutlar kullanılır:

- **BEGIN TRANSACTION**: Bir transaction başlatmak için kullanılır. Transaction başlattığınızda, veritabanı değişiklikleri transaction tarafından izlenir.

```sql
BEGIN TRANSACTION;
```

- **COMMIT TRANSACTION**: Bir transaction başarıyla tamamladığınızda kullanılır. Bu komut, transaction tarafından yapılan değişiklikleri kalıcı hale getirir.

```sql
COMMIT TRANSACTION;
```

- **ROLLBACK TRANSACTION**: Bir transaction iptal etmek veya geri almak için kullanılır. Transaction başarısız olursa veya geri alınması gerekiyorsa kullanılır.

```sql
ROLLBACK TRANSACTION;
```

İşte bir örnek, bir banka müşteri hesabından para çekme işlemini SQL Server işlemleriyle nasıl gerçekleştirebileceğinizi gösteren bir örnek:

```sql
BEGIN TRANSACTION;

DECLARE @HesapBakiye DECIMAL(10, 2);
SET @HesapBakiye = (SELECT Bakiye FROM Hesaplar WHERE HesapNo = '12345');

IF @HesapBakiye >= 100.00
BEGIN
   UPDATE Hesaplar SET Bakiye = Bakiye - 100.00 WHERE HesapNo = '12345';
   INSERT INTO HesapHareketleri (HesapNo, IslemTarihi, IslemMiktari) VALUES ('12345', GETDATE(), -100.00);
   COMMIT TRANSACTION;
   PRINT 'Para çekme işlemi başarıyla tamamlandı.';
END
ELSE
BEGIN
   ROLLBACK TRANSACTION;
   PRINT 'Yetersiz bakiye, işlem iptal edildi.';
END;
```
Bu örnek, transaction başladığında hesap bakiyesini kontrol eder, yeterli bakiye varsa para çekme işlemini gerçekleştirir. Aksi takdirde işlemi iptal eder. İşlem başarılı olduğunda değişiklikler kalıcı hale gelir, aksi takdirde geri alınır.

SQL Server transactions, veritabanı işlemlerinin güvenli ve bütünlüğünü bir şekilde yönetmek için önemli bir araçtır.

**Note:**
İzolasyon seviyeleri ([isolation levels](https://learn.microsoft.com/en-us/previous-versions/sql/sql-server-2008-r2/ms189122(v=sql.105)?redirectedfrom=MSDN)) veritabanı işlemlerinin birbirleriyle etkileşimini ve verilere erişimini nasıl kontrol edeceğinizi belirler. İşte bu konsepti daha fazla açıklayan birkaç önemli nokta:

1. **Veritabanı Kilidi (Locking)**: İşlemler, veritabanında değişiklikler yaparken veya verilere erişirken kilitleme işlemleri kullanır. Bu, diğer işlemlerin aynı verilere aynı anda erişmesini önler. İşlemler kilitlenmiş verilere yazabilir, ancak diğer işlemler aynı verilere yazamaz veya okuyamaz.

2. **İzolasyon Seviyeleri**: Veritabanı yönetim sistemleri, farklı izolasyon seviyeleri sunar. Bu seviyeler işlemler arasındaki kilitleme davranışını kontrol eder. Yaygın olarak kullanılan izolasyon seviyeleri şunlardır:
- Read Uncommitted: Bu seviyede işlem, diğer işlemler tarafından yapılan değişiklikleri görür. Bu, en düşük izolasyon seviyesidir ve veri bütünlüğünü riske atabilir.
- Read Committed: Bu seviyede işlem, başka işlemler tarafından yapılan değişiklikleri görmez. Ancak, işlem kilitlenen verilere erişirken diğer işlemler tarafından engellenebilir.
- Repeatable Read: Bu seviyede işlem, başka işlemler tarafından yapılan yeni satır eklemelerini görmez. Ancak, mevcut satırların değiştirilmesini görebilir.
- Serializable: Bu seviye, işlemleri tamamen izole eder ve hiçbir işlem diğerinin üzerine yazma yapamaz. Ancak bu seviye, performans açısından maliyetli olabilir.

3. **Kilitlerin Etkisi**: İşlemler ne kadar uzun süre çalışırsa, kilitlenen verilere erişim süresi o kadar uzar. Uzun süren işlemler, diğer işlemlerin beklemesine ve hatta ölümcül kilitlenmelere [deadlock](https://www.red-gate.com/simple-talk/databases/sql-server/performance-sql-server/sql-server-deadlocks-by-example/) neden olabilir.

Sonuç olarak, veritabanı işlemlerinin mümkün olduğunca kısa ve etkili olması önemlidir. Transaction lar uzun süre çalışmamalı ve gereksiz yere fazla veriyi kilitlememelidir. Doğru izolasyon seviyesini seçmek de önemlidir, çünkü daha yüksek izolasyon seviyeleri, performans açısından daha maliyetli olabilir, ancak veri bütünlüğünü artırabilir. Bu nedenle, transaction ve izolasyon seviyelerini dikkatlice planlamak ve optimize etmek veritabanı performansını iyileştirmeye yardımcı olabilir.


## 1.13 - Tablo Satır Sayısını Alma
Veritabanındaki belirli bir tablonun toplam satır sayısını bulmak için kullanılabilir.

```sql
SELECT COUNT(*) AS [TotalRowCount] FROM table_name
```

Tüm tablolar için satır sayısını elde etmek de mümkündür.

```sql
SELECT [Tables].name AS [TableName],
  SUM( [Partitions].[rows] ) AS [TotalRowCount]
FROM sys.tables AS [Tables]
JOIN sys.partitions AS [Partitions] ON [Tables].[object_id] = [Partitions].[object_id] AND [Partitions].index_id IN ( 0, 1 )
--WHERE [Tables].name = N'table name' /* Belirli bir tabloyu aramak için yorumu (--) kaldırın */
GROUP BY [Tables].name;
```
