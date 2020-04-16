# Lesson 2

### Başlıklar: 
* [Vektör Modifikasyonu](#vektör-modifikasyonu)
* [Data Frame Modifikasyonu](#dataframe-modifikasyonu)
* [Mantıksal Testler](#mantıksal-testler)
* [Data Frameden Mantıksal Operatörlerle Kesit Alma](#kesit-alma)
* [Kontrol İfadeleri](#kontrol-ifadeleri)
* [for döngüsü](#for-döngüsü)
* [paste() fonksiyonu](#paste-fonksiyonu)
* [while döngüsü](#while-döngüsü)
* [Fonksiyonlar](#fonksiyonlar)
* [Apply Fonksiyonları](#apply-fonksiyonları)

## Vektör Modifikasyonu
Elimizde bir vektörümüz varken bu vektörden kesitler almayı ve endeksleme yöntemiyle vektörün istediğimiz elemanına ulaşmayı nasıl yapacağımızı görmüştük. Fakat bazen de elimizdeki vektörün istediğimiz bir veya birkaç elemanını değiştirmek isteyebiliriz, bunu nasıl yapacağımıza bakalım.

Dört elemanlı ve her elemanı 0 olan bir vektör oluşturalım ismi de vektörüm olsun:
```R
vektörüm <- c(0,0,0,0)
```
Yukarıdaki komutu kullanarak rahatlıkla bu vektörü oluşturabiliyorduk. Burada yaptığımız vektörüm değişkenine c() fonksiyonunun oluşturduğu vektörü atamak. c() fonksiyonuna da girdi olarak vektörün içinde olmasını istediğimiz elemanları, sırasıyla, verdiyorduk.
 
Hali hazırda elimizde olan vektörün herhangi bir elemanını değiştirmek istediğimizde de aslında aynı mantık ile hareket ediyoruz.
Değiştirmek istediğimiz elemanın endeksini biliyorsak, vektörün o endeksteki elemanına ulaşıp ona farklı bir değer atayacağız.

Diyelim ki vektörüm değişkenimizin birinci endeksindeki 0 sayısını 1000 ile değiştirmek istiyoruz.
vec[1] ifadesi bu elemana ulaşmamızı sağlayacaktır. Sonrasında yapmamız gereken tek şey 1000 sayısını bu elemana atamak, yani:
```R
vec[1] <- 1000
```
komutunu çalıştırmak.

Yukarıdaki iki komutu RStudioda çalıştırırsanız, her elemanı sıfır olan dört elemanlı vektörümüzün ilk elemanının artık 1000 olduğunu göreceksiniz.

Şimdi birden fazla elemanı modifiye etmeye bakalım.
Vektörlerde nasıl kesit aldığımızı hatırlıyorsunuz değil mi? Kesit almak aslında o vektörün belirli bir parçasını elde etmek demektir. Örneğin vektörüm vektörünün ilk iki elemanını elde etmek istersek kesit almamız gerekir, bunu şu şekilde yapıyorduk:
```R
vektörüm[1:2]

 veya

vektörüm[c(1,2)]
```
Yukarıdaki iki komut da aynı işlevi görüyordu ve bize vektörüm vektörünün ilk iki elemanından oluşan vektörü veriyordu.
Demek ki vektörümüzün istediğimiz sayıdaki elemanına da bu yolla ulaşıyorduk.
O zaman yukarıdaki bahsettiğimiz mantığı bir daha çalıştıralım ve sadece elimizdeki kesite yeni bir değer(eğer hepsinin değeri aynı olsun istiyorsak) veya değerler atayalım.

Vektörümüzün birinci elemanı 1000 ve ikinci elemanı sıfırdı, diyelim ki bu iki elemanın da değerini 5 yapmak istiyoruz.
Vektörümüzden ilgili kesiti alalım, kesit almak için kullandığımız ilk yazımı kullanacağım fakat yukarıdaki iki yazım da aynı sonucu verir, istediğinizi kullanabilirsiniz:
```R
vektörüm[1:2]

Ve şimdi bu kesite 5 sayısını atayalım.

vektörüm[1:2] <- 5
```
Yukarıdaki komutu çalıştıracak olursanız, ilk iki elemanın 5 olduğunu göreceksiniz. İkisine de aynı değeri atamış olduk. 
Bu işlemi şu komut ile de yapabilirdik:
```R
vektörüm[1:2] <- c(5,5)
```
Fakat R dilinde küçük bir kolaylık yapmışlar ve tek bir sayı verdiğimizde bütün ilgili elemanlara o sayıyı atıyor dolayısıyla bu şekilde yazmayı gerek görmedik.

Devam edelim, farklı sayılar atayalım, vektörümüzün kesitini modifiye edelim. Tahmin edeceğiniz üzere yapmamız gereken tek şey c() fonksiyonu ile istediğimiz değerlere sahip yeni bir vektör yaratıp bu kesite atamak. Diyelim ki ilk elemanımız 7 ikinci elemanımız 21 olsun istiyoruz, aşağıdaki komut istediğimizi yapacaktır.
```R
vektörüm[1:2] <- c(7,21)
```
Daha farklı bir örnek yapalım, alacağımız kesit ardışık endekslerden oluşmak zorunda değil, belki ilk ve son elemanı değiştirmek istiyoruz.
İlk ve son elemanı içeren kesiti alalım:
```R
vektörüm[c(1,4)]
```
Elimizde birinci ve dördüncü endeksteki elemanları içeren vektörümüz hazır.
Bu kesitteki elemanların ikisine de önce 0 değeri atayalım, sonra ilkine 3 ikincisine 8 değerini atayalım:
```R
vektörüm[c(1,4)] <- 5

ve

vektörüm[c(1,4)] <- c(3,8)
```


## Data Frame Modifikasyonu
## Mantıksal Testler

Mantıksal testler iki girdi arasında karşılaştırma yapmak için kullanılır ve çıktı olarak Boolean (TRUE veya FALSE) döndürür.

R dilinde sıkça kullanılan mantıksal testler şunlardır.


| __Operatör__ | __Açıklama__ | __Kullanımı__ |
|-------------|------------|------------|
|<| 1. girdi __küçüktür__ 2. girdi | a < b |
|>| 1. girdi __büyüktür__ 2. girdi | a > b |
|<=| 1. girdi __küçük eşittir__ 2. girdi | a <= b |
|>=| 1. girdi __büyük eşittir__ 2. girdi| a >= b  |
|==| 1. girdi __eşittir__ 2. girdi | a == b |
|!=| 1. girdi __eşit değildir__ 2.girdi | a != b |
|%in%| 1. girdi __içindedir__ 2.girdinin | a %in% b |


```R
5 < 3
[1] FALSE #5 küçüktür 3 karşılaştırması yanlış olduğu için FALSE döndü.


5 > 3
[1] TRUE #5 büyüktür 3 karşılaştırması doğru olduğu için TRUE döndü.

5 == 3
[1] FALSE #5 eşittir 3 karşılaştırması yanlış olduğu için FALSE döndü.

5 != 3  #5 eşit değildir 3 olduğu için sonuç TRUE döndü.
[1] TRUE
```

Aynı şekilde karakter yapısında değişkenler de mantıksal testlere tabi tutulabilir.

```R
"TEDU" == "TEDu"  #R küçük harf ve büyük harfe duyarlı(case sensitive)bir dildir. Bu yüzden FALSE döndü. 
[1]FALSE
"Kodluyoruz" == "Kodluyoruz"  #Her iki ifade birbirine eşit olduğu için TRUE döndü.
[1]TRUE
```

Bir vektör de mantıksal testlere tabi tutulabilir.

```R
vektorum <- c(3, 7, 12, 17, 20, 35)
12 > vektorum
[1]TRUE  TRUE FALSE FALSE FALSE FALSE  #Her vektör elemanı için sonuç döndürülür.

vektorum[4] == 20 #Vektörümüzde 4. eleman 20 olduğu için TRUE döndü.
[1]TRUE

6 == length(vektorum)  #Vektörümüz 6 eleman içerdiği için sonuç TRUE döndü.
[1]TRUE
```

Yukarıda örneğimizde vektörümüzün içinde bulunan her bir elemanın istediğimiz koşulu sağlayıp sağlamadığını nasıl test
edebileceğimizi gördük. Peki bu mantıksal ifadenin ardından dönen TRUE ve FALSE çıktılarını kullanarak vektörümüzde
bulunan elemanlara nasıl erişebiliriz?



```R
vektorum <- c(3, 7, 12, 17, 20, 35)
vektorum[15 < vektorum]  #Aradığımız mantıksal ifade ile vektörümüzü çağırırsak TRUE dönen değişkenleri çıktı olarak verecektir.
[1] 17 20 35
```

İki vektör de aynı şekilde mantıksal testlere tabi tutulabilir.

```R
c(4, 5, 8) == c(1, 5, 8)  #Karşılıklı denk gelen her bir eleman için istenilen karşılaştırma yapılır.
[1] FALSE  TRUE  TRUE

c(12, 17, 18) == c(2, 12, 16, 22)  #Uzunlukları farklı olan vektörleri doğrudan karşılaştıramayız.
[1]Warning message:

#Ancak aşağıdaki gibi bir karşılaştırma yapılabilir. 
#Her vektörden bir elemanı karşılaştırdığımız için hata almayacağız.
vek1 <- c(12, 17, 18)
vek2 <- c(2, 12, 16, 22) 
vek1[3] == vek2[4]  #18 eşit değildir 22 olduğu için FALSE döndü.
[1]FALSE
```

%in% operatörü ile aradığımız bir değerin bir vektörün/matrisin içinde olup olmadığını kontrol edebiliriz.

```R
vek1 <- c(12, 5, 18, 22, 47)

12 %in% vek1  #12 sayısı aradığımız vektörün içinde bulunuyor bu yüzden TRUE döndü.
[1] TRUE

c(1,2) %in% c(3,4,5)  #1. vektörde bulunan elemanları 2. vektörün içinde arar.
[1] FALSE FALSE

c(3,4,5) %in% c(1,3,9)
[1]TRUE FALSE FALSE
```

## Data Frameden Mantıksal Operatörlerle Kesit Alma

İstediğimiz analizleri doğru bir şekilde yapabilmek için veri setimizden mantıksal operatörleri kullanarak kesit alabiliriz.
Aldığımız kesitleri yeni bir değişkene atayabilir ve üzerinde çalışabiliriz.

Bir data frame oluşturarak başlayalım ve sonrasında üzerinde çalışalım.

```R

ogrenci_df <- data.frame(
 "öğrenci_no" = c(215,421,729,487,389,390), 
 "bölüm" = c("İktisat", "Bilgisayar Müh.", "İstatistik", "Biyoloji","Elektrik Elektronik Müh.", "İstatistik"), 
 "fakülte" = c("iibf", "Mühendislik", "Fen", "Fen","Mühendislik","Fen"),
 "not" = c(3.15, 2.8, 3.5, 3.8,3.2,2.3))
ogrenci_df


```


| öğrenci_no | bölüm | fakülte | not |
|-------------|------------|------------|------------|
| 215 | İktisat| iibf | 3.15 |
| 421 | Bilgisayar Müh. | Mühendislik | 2.80 |
| 729 | İstatistik | Fen | 3.50 |
| 487 | Biyoloji| Fen | 3.80 |
| 389 | Elekrik Elektronik Müh. | Mühendislik | 3.20 |

Yukarıdaki data frame için aşağıdaki soruları cevaplayalım.

```R

#2. öğrencinin bilgilerine bakalım.
ogrenci_df[2,] #Öğrenci data frame içine bak. 2. öğrencinin tüm değerlerini getir.

#Kaç farklı fakülteden öğrenci vardır?

unique(ogrenci_df[3]) #Data framein 3. kolonuna bak ve birbirinden farklı olanları döndür.(unique fonksiyonu)

#2. ve 3. öğrencinin bölümüne ve notuna nasıl bakabiliriz?

ogrenci_df[2:3,c(2,4)]

#Öğrencilerin not ortalamasını nasıl hesaplayabiliriz?

mean(ogrenci_df[,4]) #Data frame içinde 4. sütunun tüm değerlerini seçer.
[1]3.125

#Fen fakültesinde okuyan öğrencilerin hepsini nasıl görüntüleyebiliriz?

ogrenci_df[ogrenci_df$fakülte == "Fen",]

#Eğer data frame içinde sütunlarımıza bir isim ataması yapıldıysa aşağıdaki gibi de verilere ulaşılabilir.

ogrenci_df$ogrenci_no
[1]215 421 729 487 389 390

#3. öğrencinin okul numarasını 387 olarak nasıl değiştirebiliriz?

ogrenci_df[3,"öğrenci_no"] = 387

#Not ortalaması 3 ve üzeri olan öğrencilere nasıl ulaşabiliriz?

ogrenci_df[ogrenci_df$not > 3,]


# Data frame üzerinden öğrencilerin notlarını nasıl silebiliriz?

ogrenci_df$not <- NULL #sütun silmek istiyorsak kolan seçilir ve NULL atanır.

#4. öğrenciyi data frame üzerinden nasıl silebiliriz?

ogrenci_df <- ogrenci_df[-4,] #satır silmek istiyorsak tekrar atama ile seçilen satır silinebilir
```

#### subset Fonksiyonu

subset fonksiyonu kullanarakta data frame içinden kesitler alınabilir.
subset fonksiyonunu kullanabilmek için mutlaka logical (TRUE ve ya FALSE) bir girdi girmek gerekir.

 ```R
 
 #Notu 2.75 altında bulunan öğrencilere nasıl ulaşabiliriz?
 
subset(ogrenci_df, ogrenci_df$not < 2.75)


#Mühendislik fakültesinde okuyan öğrencilerin bölümlerine nasıl ulaşabiliriz?

subset(ogrenci_df[2], ogrenci_df$fakülte == "Mühendislik")
 
```

## Kontrol İfadeleri

Kontrol ifadeleri programın akışını belirlemekte kullanılır. İstenilen koşulun sağlanıp
sağlanmadığına göre bir sonraki kod bloğu çalışır. Bu kontrolleri yapabilmek için 3 ifadeden faydalanıyoruz.

__Not__: Kontrol ifadelerinin içine mantıksal testler yazdığımıza dikkat edelim.

#### if kontrolü
İstenilen koşul gerçekleştiğinde kod bloğu içinde bulunan kod çalıştırılır. Koşul sağlanmadıysa program normal
akışında devam eder.


__Soru:__
Adım adım verilen sayının pozitif mi yoksa negatif mi olduğunu gösteren bir program yazalım.

 ```R
#Adım 1: Sayımızın negatif olma durumunu kontrol edelim.
 
sayi <- 5
 
if(sayi < 0) { #Eğer sayı 0'dan küçükse if kod bloğundaki kodları çalıştır.
	print("Sayı negatiftir.")
}
 
print("Kontrol ifadelerini öğreniyorum") 
 
#if koşulu sağlanmadığı için bu kod bloğu çıktı olarak sadece aşağıdaki çıktıyı verecektir.
[1]"Kontrol ifadelerini öğreniyorum"
```

#### else if kontrolü
if koşulunun gerçekleşmediği durumda kontrol edilecek bir sonraki koşulu belirtmemizi sağlayan ifadedir.

_Not_: Birden fazla else if koşulu alt alta tanımlanabilir.

```R
#Adım 2: Sayının pozitif olma kontrolünü ekleyelim. 
 
if(sayi < 0) { #Eğer sayı 0'dan küçükse if kod bloğundaki kodları çalıştır.
	print("Sayı negatiftir.")
}else if(sayi > 0) { #Eğer sayı 0'dan büyükse else if kod bloğundaki kodları çalıştır.
	print("Sayı pozitiftir.")
}

```

#### else kontrolü
if koşulunun sağlanmadığı durumlarda çalıştırılacak kod bloğudur.
if koşulu ya da else if koşullarından bir tanesi istenileni sağlayıp çalışırsa else komutu çalışmaz.

```R
#Adım 3: Sayımızın 0 olma durumunu kontrol edelim.

if(sayi < 0) { #Eğer sayı 0'dan küçükse if kod bloğundaki kodları çalıştır.
  print("Sayı negatiftir.")
}else if(sayi > 0) { #Eğer sayı 0'dan büyükse else if kod bloğundaki kodları çalıştır.
  print("Sayı pozitiftir.")
}else {
  print("Girilen sayı 0'dır.") #Girilen sayı ne negatif ne de pozitiftir. Sayı 0'dır.
} 
```

__Soru:__
Bir arabanın hızına göre sürücüye aşağıdaki koşullar doğrultusunda çıktı veren bir program yazalım.

Araba hızımızın negatif olup olmadığı kontrolünü yapalım.\
Arabamızın hızı 85 olsun.\
Eğer arabanın hızı 0-30 arasındaysa ekrana "hızınız çok yavaş"\
Eğer arabanın hızı 30-50 arasındaysa ekrana "hızınız yavaş"\
Eğer arabanın hızı 70-100 arasındaysa ekrana "hızınız normal"\
Eğer arabanın hızı 100'den fazlaysa ekrana "hızınız yüksek, lütfen yavaşlayın!" yazsın.

```R
araba_hiz <- 85

if(araba_hiz >= 0) {
  if(araba_hiz <= 30) { # hız 0 ve 30 arasındaysa.
    print("hızınız çok yavaş.")
  } else if(araba_hiz <= 50) { # hız 30 - 50 arasındaysa
    print("hızınız yavaş.")
  } else if(araba_hiz <=100) { # hız 50 - 100 arasındaysa
    print("hızınız normal.")
  } else { #hız 100'den fazlasysa
    print("hızınız yüksek, lütfen yavaşlayın!")
  }
} else { # arabanın hızı >= 0 değilse yani negatif ise
  print("Arabanın hızı negatif olamaz.")
}
```

__Soru:__
Bir hava yolu şirketinin indirimli uçak biletlerini tuttuğu vektör size verilmiştir. Gitmek istediğiniz
şehrin bu vektörün içinde olup olmadığına bakınız. Eğer indirimli bilet satın alabiliyorsanız ekrana
"Yaşasın ucuza bilet buldum!" bulamıyorsanız "Şansız günümdeyim" yazdırınız.


```R
sehirler <- c("Paris", "Amsterdam", "Munich", "Prague", "Berlin", "Athens")

#1. çözüm bir değişkene gitmek istediğimiz şehri atayarak kontrolü sağlayabiliriz.
gitmek_istediğim <- "California"

if(gitmek_istediğim %in% sehirler) {
	print("Yaşasın ucuza bilet buldum!")
}else {
	print("Şanssız günümdeyim.")
}
[1]Şanssız günümdeyim.

#2. çözüm doğrudan gitmek istediğimiz şehir ile kontrolü sağlayabiliriz.
if("Munich" %in% sehirler) {
	print("Yaşasın ucuza bilet buldum!")
}else {
	print("Şanssız günümdeyim.")
}
[1]Yaşasın ucuza bilet buldum!
```

__Not__: Birden çok iç içe if else blokları yazılabilir. Okunabilirliği artırmak için program yazarken bırakılan boşluklara ve
süslü parantezlerin hizalarına dikkat etmek önemlidir.



## for döngüsü

Kendini tekrar eden benzer kod satırlarını tekrar yazmak yerine döngüleri kullanmayı tercih ederiz. Döngüler yardımıyla
zaman kazancı elde edebilir ve daha verimli kod yazılabilir. 


__Soru:__
Ekrana 5 kere "Merhaba Kodluyoruz" yazdıran programı yazınız.

```R
#Öncelikle sorumuzu döngüler kullanmadan çözelim.

print("Merhaba Kodluyoruz")
print("Merhaba Kodluyoruz")
print("Merhaba Kodluyoruz")
print("Merhaba Kodluyoruz")
print("Merhaba Kodluyoruz")
```

Yukarıdaki çözümde gördüğünüz şekilde bize verilen sorunun cevabını elde edebildik. Peki soru bizden 5 değil de 20
kere ekrana "Merhaba Kodluyoruz" yazdırmamızı isteseydi bu şekilde bir çözüm mantıklı olur muydu?

Hayır, bu tarz kendini tekrar eden kod bloklarında döngüler ile çok daha pratik çözümler üretebiliriz.

```R
for(index in 1:20) {
  print("Merhaba Kodluyoruz")
}
```

__Not:__ Programlama dünyasında kendimizi tekrar etmekten kaçınmamızı hatırlatacak bir prensip vardır.
Bu prensip "Don't Repat Yourself" yani "DRY" olarak literatüre geçmiştir. "Kendini Tekrar Etme" şeklinde
Türkçe'leştirebileceğimiz bu prensibi hatırlayarak daha verimli çalışan kodlar yazabiliriz.

__Soru:__
Elimizde öğrencilere ait notların bulunduğu bir vektör kod dizini içerisinde verilmiştir. Öğrencilerin notlarını
ekrana for döngüsü kullanarak yazdırınız.

```R
notlar <- c(85, 90, 25, 30, 59, 60, 28, 64, 75, 83, 92, 10, 50, 60, 52, 10, 88, 46, 12)

#çözüm1

for(not in notlar) {
	print(not)
}

#çözüm2

for(i in 1:length(notlar)) {
	print(notlar[i])
}

```

Çözüme baktığımız zaman problemin iki farklı for döngüsü ile çözüldüğünü görüyoruz. İki döngü de aynı
sonucu vermesine rağmen probleme göre bir çözüm diğer çözüme göre avantaj sağlayabilir.

__1. Çözüm İnceleme:__
Kısa ve daha okunabilir bir çözüm olmasına rağmen indekslere doğrudan bir erişim
sağlayamadığımız için indeks bazlı işlem yapmak zordur.


__2. Çözüm İnceleme:__
Okunabilirliği ve yazması daha zordur fakat indekslere doğrudan erişim sağlandığı için elemanlar
üzerinde işlem yapmak daha kolaydır.


__Soru:__
Aşağıdaki x dizisinde bulunan sayılardan 2 ile bölünebilen sayıları nasıl yazdırabiliriz?
```R
x <- 1:10 #1'den 10'a kadar olan sayılar x'in içinde tutuluyor.

for(sayi in x) {
	if(sayi %% 2 == 0) { # mod operatörünü kullandık.
		print(sayi)
	}
}
```
__Soru:__
Aşağıdaki x dizisinde bulunan sayılardan 2 ile bölünebilen sayıların toplamı kaçtır?

```R
x <- 1:20 #1'den 20'ye kadar olan sayılar x'in içinde tutuluyor.
toplam <- 0 #toplamı tutabilmek için bir toplam değişkeni oluşturduk ve 0'a atadık.
for(sayi in x) {
	if(sayi %% 2 == 0) { #2'ye bölünebilirlik kontrolü.
		toplam <- toplam + sayi #Eğer sayı 2'ye bölünebiliyorsa toplam değişkenini sayı kadar arttır.
	}
}
print(toplam)
```

Döngüler üzerindeki kontrolü arttıran bazı komutlar vardır.

#### break komutu

İstenilen koşul sağlandığında döngünün sonlandırılmasını sağlayan komuttur.

__Soru:__ 
Şehirler vektöründe 6 harfli olan ilk şehri for döngüsü ile nasıl yazdırabiliriz?

```R
sehirler <- c("İzmir", "Ankara", "İstanbul", "Antalya", "Kayseri", "Bursa","Amasya")

for(sehir in sehirler) {
  if(nchar(sehir) == 6) {
    print(sehir)
    break
  }
}
[1] "Ankara"

#nchar() fonksiyonu içine gönderilen argümanın kaç character'den oluştuğunu döndürür.
```
__Dikkat:__Yukarıdaki çözümümüzde "Ankara" çıktısını elde ettik. Eğer break komutu ile döngüyü kırmasaydık
"Amasya" çıktısını da elde edecektik.


#### next komutu
İstenilen koşulun sağlanması durumunda döngüyü sıradaki eleman ile çağırır.

__Soru:__
1'den 10'a kadar olan sayılardan tek sayıları for döngüsü ve next komutu yardımıyla nasıl yazdırabiliriz?

```R
sayilar <- 1:10

for(sayi in sayilar) {
  if(sayi %% 2 == 0) {
    next
  }
  print(sayi)
}
[1] 1
[1] 3
[1] 5
[1] 7
[1] 9
```


## paste() fonksiyonu

paste fonksiyonu içerisine gönderilen argümanları character tipine dönüştürür ve belirlenen bir ayraç
ile character tipinden verileri birleştirebilir.

İçine tek bir  argüman gönderilirse  paste() fonksiyonu as.character() fonksiyonu görevi görür.

İçine birden fazla argüman gönderilirse paste() fonksiyonu gönderilen argümanları birleştirir ve character
tipinde bir çıktı üretir. 

Argümanların arasına sep = "" parametresi ile ayraç konabilir.\
Herhangi bir ayraç belirtilmemesi durumunda varsayılan ayraç "" şeklindedir.
```R
sayi <- 5
class(sayi)
[1]"numeric"

class(as.character(sayi))
[1]"character"

class(paste(sayi))
[1]"character"


isim <- "Berke"
kilo <- 65

paste(isim, kilo,"kilogramdır.")
[1]"Berke 65 kilogramdır."

paste(isim, kilo,"kilogramdır.", sep= " *** ")
[1] "Berke *** 65 *** kilogramdır."

paste(c("x","y"), 1:10, sep=",")
[1]"x,1"  "y,2"  "x,3"  "y,4"  "x,5"  "y,6"  "x,7"  "y,8"  "x,9"  "y,10"
```

## while döngüsü


## Fonksiyonlar
## Apply Fonksiyonları

