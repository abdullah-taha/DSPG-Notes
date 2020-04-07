# Lesson 1 

### Başlıklar: 
* Sosyal Fayda için Veri Bilimi nedir ?
* Programın gidişi ve program sonundaki projeler 
* Neden R ?
* R ve R studio Yüklenmesi
* Giriş 
* Veri tipleri
* R objeleri oluşturma
* Mantıklsal Öpretarörleri
* Vektörler
* Matrisler
* Faktörler
* Dataframeler
* R notasyonları
* R'da Listeler

# Giriş:
R, S programlama dili üzerine yazılan ve istatistiksel analizlerin R içindeki kütüphaneler yardımıyla 
kolay bir şekilde yapılmasını sağlayan bir platformdur. 
R studio, R dilini Compile etmek için kullanacağımız araçtır. 

R studio içinde dört tane kısımdan oluştuğunu görüyoruz,
* Sol üst kısım, kod yazacağımız kısımdır
* onun altında Console yer almaktadır, yazdığımız kodun çıktısı burada görebiliriz
* Sağ üst kısım Environment kısmı, compile edilen değişkenler burada görünyor
* onun altında Help kısmı, R studio da bir şeyi aratmak istediğimizde kullanırız


R'ı başlangıçta bir hesap makinesi olarak düşünebiliriz, burada her türlü hesaplar yapabiliriz. Toplama, Çıkarma ve Çarpma işlemleri görelim.

```R
#Toplama
3452 + 3452
> 6904

#Çıkarma
15-70
> -55

#Çarpma
15 * 22
> 330
```

Bölme işlemi bildiğimiz bölme sembölü (%) ile de deneyelim, 
```R
#Bölme
5%2
> Error: unexpected input in "5%2"
```
Bir hata aldık! hatayı okuyalım, "Error: unexpected input in "5%2"" diyor, yani beklenmedik bir sembolü okuduğunu söylüyor 
R'da bölme işareti '%' ile gösterilmez, '/' sembölü ile gösterilir.

> İnan Hoca: Hataları okumayı öğrenmemiz lazım, sürekli hatalarla karşılaşacağız ve onları okuyup hatayı gidermeye çalışacağız.

  ## Veri tipleri
  R'da bir sürü veri tipleri vardır, bunlara bakalım:
  * Character : Text ifadeleri character tipinden olur
  ```R
  "CADS DPSG"
  "TEDU"
  ```
  * Numeric: Decimal sayılar numeric tipindendir
  ```R
  12.5
  20
  ```
  * Integers: Doğal sayılara İnteger diyoruz, integer sayılar numeric sayılır.
  Integer sayıları ayırt etmek için ardından L harfi ekleyebiliriz
  ```R
  5L
  > 5
  ```
  
  *Logical: Boolean ifadelerine logical denir. iki değer alır, ya TRUE ya da FALSE.
  T ya da F şeklinde de ifade edilebilir
  ```R
  TRUE
  > TRUE
  
  FALSE
  > FALSE
  
  T
  > TRUE
  
  F
  > FALSE
  ```
  
  R'da TRUE ifadesi 1 olarak, FALSE ifadesi 0 olarak algılanır. Bunu şu şekilde görebiliyoruz
  ```R
  TRUE + TRUE
  > 2
  
  FALSE + FALSE
  > 0
  
  TRUE + FALSE 
  > 1
  ```
  
  * Complex: kompleks ifadeleri çok kullanacak olmasak bile, varlığından bilmekte fayda var.
  ```R
  1+2i
  ```
  
  
  
