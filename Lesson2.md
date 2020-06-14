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
İstediğimiz sayıda ve istediğimiz endekslerde olan verileri nasıl başka bir değer ile değiştireceğimizi gördük. Şimdi, elimizdeki verileri o verilerin kendilerini kullanarak nasıl değiştireceğimize bakalım.

Örneğin, vektörüm vektörümüzdeki ilk üç elemanını bir ile toplamak istiyoruz. Yani ilk üç elemanın bir ile toplanmış hallerini vektörümüzün ilk üç elemanına atamak istiyoruz. 

İlk yapmamız gereken şeyi biliyoruz, vektörümüzün ilk üç elemanına ulaşmak, yani vektörüm vektöründen ilgili kesiti alacağız ve buna istediğimizi atayacağız. Peki, atayacağımız vektör ne? Tekrar ilgili kesite ulaşıp buna bir eklemeli ve kesitimize atamalıyız. 
```R
vektörüm[c(1,2,3)]

Kesitimizi aldık.

vektörüm[c(1,2,3)] + 1

Bu komut ile de bu kesitin yani aslında küçük vektörümüzün her elemanına bir ekliyoruz.

vektörüm[c(1,2,3)] <- vektörüm[c(1,2,3)] + 1

Son olarak vektörümüzün ilgili kesitine yine bu vektörün her elemanını bir ile topladığımızda oluşan vektörü atıyoruz.
```

Bu şekilde, vektörümüzün elemanlarının veri tipine göre işlemler uygulayarak, hali hazırda elimizde olan kesitleri modifiye edip tekrar ilgili kesite atayabiliriz.

Vektörlerimizi son bir şekilde daha modifiye edebiliriz, yeni bir eleman eklemek. Aslında tekrar c() fonksiyonunu kullanacğız, fakat bu sefer c() fonksiyonuna verdiğimiz değerler sayılar değil, vektörler olacak, veya bir vektör ve bir sayı olacak.

Örneğin vektörüm vektörümüzün sonuna bir eleman daha ekleyelim, değeri de 999 olsun. 
Yapacağımız şey ilk elemanları vektörüm vektöründeki elemanlar olan son elemanı da 999 olan br vektör oluşturmak, c() fonksiyonunu burada kullanıyoruz.
```R
c(vektörüm, 999)

Bu komut istediğimiz vektörü oluşturacaktır.
```
Bundan sonra yapmamız gereken tek şey ismi vektörüm olan vektör değişkenimize bu yeni vektörü atamak.
```R
vektörüm <- c(vektörüm, 999)
```
Aynı bu şekilde vektörümüzün önüne de bir eleman ekleyebiliriz. Diyelim en öne bir eleman ekleyeceğiz ve bunun da değerinin 999 olmasını istiyoruz. Yani vektörümüzün eleman sayısı artacak fakat ilk elemanı değiştirmiş olacağız ve her elemanın endeksi bir sağa kaymış olacak.
Benzer şekilde aşağıdaki komut ile bunu başarabiliriz.
```R
vektörüm <- c(999, vektörüm)
```

Vektörümüzün ortasındaki bir yere de bir eleman ekleyebiliriz yapmamız gereken tek şey ilgili vektörü oluşturmak.
Örneğin,
```R
vektörüm <- c(vektörüm[c(1:3)], 999, vektörüm[c(4:6)])
```
Burada yeni dördüncü elemanımızı ekliyoruz, değeri de 999. Eski dördüncü eleman ve sonrasındaki elemanların endeksi bir sağa kayıyor ve vektörümüzün eleman sayısı artıyor.

## Data Frame Modifikasyonu

Elimizdeki "data frame"leri modifiye etmek isteyebiliriz. Yeni bir kolon eklemek, var olan bir kolonu silmek veya var olan satır ve sütunlardaki değerleri değiştirmek için hangi komutları kullandığımıza bakalım.

Öncelikle bir "data frame" oluşturalım. 

Diyelim ki iskambil destesini bir "data frame" şeklinde tutmak istiyoruz.
Dört tane kolonumuz olsun. İlk kolon kartımızın simgesi için, ikinci kolon kartımızın tipi için, üçüncü kolon kartımızın değeri için, dördüncü kolon da kartımızın destedeki sırası için olsun.

Dört tane simgemiz ve on üç tane kart tipimiz olduğu için, her bir simge için on üç tane girdimiz olmalı. rep() fonksiyonunu kullanarak simge vektörümüzü bu özelliğe göre oluşturalım.
```R
simge <- c(rep("Maça",13),rep("Sinek",13),rep("Karo",13),rep("Kupa",13))
```
rep() fonksiyonunu kullanmak işimizi kolaylaştırıyor, on üç kere "Maça" yazmaktansa rep("Maça", 13) yazıyoruz ve bu işi bizim yerimize yapıyor.

Şimdi kart tipi için olan kolonumuzu oluşturacak kart vektörümüzü oluşturalım. Her bir simge için on üç tane kart tipi var öncelikle bu on üç kart tipini içeren vektörümüzü oluşturalım.
```R
kart_tipleri <- c("papaz","kiz","vale","on","dokuz","sekiz","yedi","alti","bes","dört","üç","iki","as")
```
Bu kart tipleri her bir simge için var olmalı, burada da rep() fonksiyonunu kullanarak bu kart tiplerinin dört kere ard arda kart vektöründe bulunmalarını sağlıyoruz.
```R
kart <- rep(kart_tipleri, 4)
```
Her kartımızın bir değeri var, papazın değeri on üç ve papaz, kız, vale, 10, 9, ... şeklinde geriye doğru giderken değerimiz bir azalsın ve en son asın değeri bir olsun. On üç tane değerimiz var bunları içeren vektörümüzü oluşturalım.
```R
değerler <- 13:1
```
Bu değerlerin de her simge için tekrar etmesi gerekiyor, rep() fonksiyonunu kullanarak değer kolonumuzu oluşturacak değer vektörünü oluşturalım.
```R
deger <- rep(değerler, 4)
```

Son kolonumuzu yani kartlarımızın sırasını gösteren kolonu "data frame"imizi oluşturduktan sonra ekleyelim.
"Data frame"imizi oluşturuyoruz.
```R
deste <- data.frame(
  kart,
  simge,
  deger
)
```

Şimdi deste ismindeki "data frame"imize yeni bir kolon eklemeyi görelim. Yukarıda söylediğimiz gibi bu kolon kartlarımızın sırasını gösterecek.

"$" işareti ile tanımladığımız kolonumuza istediğimiz vektörü atıyoruz ve istediğimiz değerlere sahip kolonu "data frame"imize eklemiş oluyoruz.

Kolonumuzun ismi sıra olsun.
```R
deste$sıra 
```
ifadesiyle bu kolonu tanımlıyoruz, fakat bu "data frame"imize yeni bir kolon eklemiyor çünkü kolonumuzun elemanlarını daha atamadık.
Elli iki tane kartımız olduğu için birden elli ikiye kadar olan sayılardan oluşan bir vektör oluşturalım ve kolonumuzun elemanları olarak bu vektörü atayalım.
```R
deste$sıra <- 1:52
```
Evet bu komutla yeni kolonumuzu eklemiş oluyoruz.

Şimdi bir kolonu silmek istersek ne yapmamız gerektiğine bakalım.
En son eklediğimiz sıra kolonunu silmek istediğimizi varsayalım.

Silmek istediğimiz kolona özel bir değer atayacağız ve bu kolon "data frame"imizden silinmiş olacak.
Bu özel değer NULL değeri, bu değere boş değer diyebiliriz. Rda ve çoğu programlama dilinde tanımlı bir değer NULL.
Silmek istediğimiz kolona bu değeri atadığımızda kolonun değerlerini boş değere eşitlemiş oluyoruz ve kolonumuz siliniyor.
```R
deste$sıra <- NULL
```
Sıra kolonunu bu şekilde silmiş oluyoruz.

Şimdi "data frame"imizdeki bir verinin değerini değiştirmeye bakalım.

Diyelim ki on üçüncü, yirmi altıncı, otuz dokuzuncu ve elli ikinci satırlardaki değerleri bir olan kartlarımızın değerlerini on dört yapmak istiyoruz.
Öncelikle bu satırlara ulaşmamız gerekiyor.
Aşağıdaki ifadelerle bu satırlara erişebiliyorduk.
```R
deste[13,]
deste[26,]
deste[39,]
deste[52,]
deste[c(13,26,39,52),]
```
Tek tek erişebiliriz veya c() fonksiyonunu kullanarak hepsine birden erişebilirz.

Bu kartların değerlerini değiştirmek istemiştik, o zaman bu satırlardaki değer kolonuna erişelim.
```R
"$" işareti ile erişebiliriz:
deste[c(13,26,39,52),]$deger

Kolonun endeksi ile erişebiliriz:
deste[c(13,26,39,52),3]

Kolonun ismi ile erişebiliriz:
deste[c(13,26,39,52),"deger"]

Önce kolona erişip ardından satırlara erişebiliriz.
deste$deger[c(13,26,39,52)]
```
Yukarıda dört farklı şekilde istediğimiz satırların değer kolonuna erişme ifadelerini görüyoruz.
Şimdi bu değerleri modifiye edelim.
Yapmamız gereken tek şey istediğimiz değeri az önce seçtiğimiz satırlardaki kolona atamak.
```R
deste[c(13,26,39,52),]$deger <- 14
```

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
| 421 | Bilgisayar Müh. | Mühendislik | 2.8 |
| 729 | İstatistik | Fen | 3.5 |
| 487 | Biyoloji| Fen | 3.8 |
| 389 | Elektrik Elektronik Müh. | Mühendislik | 3.2 |
| 390 | İstatistik | Fen | 2.3 |

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
  } else { #hız 100'den fazlaysa
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
__Dikkat:__
Yukarıdaki çözümümüzde "Ankara" çıktısını elde ettik. Eğer break komutu ile döngüyü kırmasaydık
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

Argümanların arasına sep = "" parametresi ile ayraç konabilir.

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
While döngüleri istenilen koşul bozulana kadar tekrar eden döngülerdir. Koşul bozulana kadar while döngüsü içindeki istenilenler tekrar edilir ve koşul değişkeni güncellenir. Örnekler ile rahat bir şekilde anlaşılacaktır. Hadi örneklere geçelim. :sunglasses: :sunglasses:

__Örnek:__ Şehir içinde trafikteyiz. Hızımız da 64 kmh fakat şehir içinde hız sınırı 30 kmh. Aracımız hızımız 30 kmh altına düşene kadar bize "Yavaş Gidin!" uyarısı versin ve her uyarız için hızımızı 7 kmh düşürelim.

```R
hız<-64
while(hız>30){
  print("yavaş gidin")
  hız<-hız-7
}

>
[1] "yavaş gidin"
[1] "yavaş gidin"
[1] "yavaş gidin"
[1] "yavaş gidin"
[1] "yavaş gidin"
```
While parantezleri içine gerçekleşmesini istediğimiz koşulu yazdık. Süslü parantezler içine de bu döngü içinde gerçekleşmesini istediğimiz kodları yazıyoruz. Bize 5 kere yavaş gidin uyarısı verdi. Toplamda hızımız 35 kmh azaldı. Son durumda hızımız 29 kmh oldu. O da 30 kmh'den büyük olmadığı için döngümüz sonlandı.

__Örnek:__ 1' den ona kalan olan sayıların toplamını bulan bir for döngüsü yazalım.
```R
toplam<-0
i<-1
while(i<=10){
  toplam<-toplam+i
}
print(toplam)

>
#Burada bir çıktı göremiyorsunuz değil mi :)
```
Yukarıdaki kodu çalıştırdığınızda konsolda bir çıktı göremiyorsunuz değil mi? :dizzy_face: :dizzy_face: 
Kodu incelemeden önce hemen **konsola tıklayıp ESC tuşuna basınız.** Böylelikle programı durdurmuş olursunuz.

Koda dikkatli baktığınızda i değerini değiştirmediğimizi farkedebiliyoruz. i değeri güncellenmediği için de döngümüz sona ermiyor yani sonsuz döngüye giriyor. :scream: :scream:

While döngüsü başlığı altında da yazdığımız gibi while döngüsüne verdiğimiz koşulları döngü içinde güncellememiz gerekiyor. Yoksa siz gerisini biliyorusunuz. :bowtie: :bowtie:

<p align="center">
	<img src=".images/funny.jpg" width="400">
</p>

Bu döngü ile en çok kullanılan komutlardan biri ise **break** komutudur. Döngümüzü istediğimiz durumda sonlandırmamızı sağlar. Hemen örneğini inceleyelim.

__Örnek:__ 1'den 6'ya kadar olan sayıların karelerini ekrana yazdırcak while döngüsünü yazalım. 4. sayıya geldiğinde ise döngümüzden ayrılalım.

```R
kare <- 1
while (kare <= 6){
  print (kare^2)
  kare <- kare + 1 
  if (kare==4){
    break
  }
}

>
[1] 1
[1] 4
[1] 9
```
Döngümüz her adımda sayıların karelerini aldı. sayımız 4 olduğunda if koşulu sağlandı ve if koşulu içindeki break komutu çalıştı. Böylelikle döngümüz sona erdi.


## Fonksiyonlar
Fonksiyonlara genel olarak  verilen girdiyi kod bloğu içindeki işlemlerde işleyerek çıktı oluşturan yapılar diyebiliriz.
R'da fonksiyon yazabilmek için **function()** fonksiyonunu kullanıyoruz. Yazdığımız fonksiyonu ise onu kullanacağımız isme atıyoruz.
Hemen birkaç örneğe bakalım. :rocket: :rocket:

```R
#Kullanıcı tarafından girilen sayısının,kullanıcı tarafından girilen diğer sayı kuvvetini
#alan fonksiyonumuzu yazalım.

kuvvet <- function(x,y){
	return(x^y)
}
kuvvet(x=2, y=2)
>
[1] 4
```
function() içine kullanıcadan gelecek veya dışarıdan gelecek değerleri temsil eden değişkenleri yazıyoruz. Daha sonra süslü parantezler
içine kod bloğumuzu yazıyoruz. Bize verilmesini istenen sonucu ise return ile dönderiyoruz. :scroll: :scroll:
Peki ya parametrelerimizden birine default değer vermek istersek? Yani biz o parametreye bir değer vermeyince önceden tanımlanmış
değeri kullansın. Hemen bakalım!!

```R
#fonksiyonumuz kuvvet değeri verilmediği sürece girilen sayının karesini bize döndersin. Eğer bir kuvvet değeri verilir
#ise bize o kuvvet sonucunu versin

kuvvet <- function(x,y=2){
	return(x^y)
}
kuvvet(x=3)
>
[1] 9

kuvvet(x=3,y = 3)
>
[1] 27

```
Parametreleri yazdığımız function() içine eşitlik halinde istediğimiz değeri yazarak default değer oluşturabiliriz. Temel olarak
fonksiyonlar bahsedilen yapıda çalışır. İçini ise istenilen şekilde değiştiririz. Şimdi bol bol egzersiz zamanı. :computer: :crystal_ball:

__Örnek :__ Kullanıcı tarafından a, b, c sayıları şeklinde girilen ve a ve b'nin toplamını c ile çarpan islem fonksiyonunu yazalım.
```R
islem <- function(a, b, c){
  sonuc <- (a + b)*c
  return(sonuc)
}

islem(a=1, b=2, c=3)
>
[1] 9
```
Burada return() içine istenilen (a+b)xc işlemini de yazabiliriz.

__Örnek :__ Kullanıcı tarafından belirlenen bir vektörün ortalamasını hesaplayan my_mean isimli fonksiyonu yazalım. Hazır fonksiyon
kullanmadan yazalım. length() fonksiyonunu kullanabilirsiniz.

```R
my_mean <- function(vec){
  toplam <- 0
  for(sayı in vec){
    toplam <- toplam + sayı
  }
  ortalama <- toplam/length(vec)
  return(ortalama)
}

my_mean(c(1,2,3,4,5,6))
>
[1] 3.5
```
Girdi olarak vectör vermemiz lazım. O yüzden function içinde vec isimli bir parametre kullandık. Okunabilirlik açısından da rahat 
olur hem. for döngüsü içinde de toplama işlemini yaptık. Son olarak ortalamamızı dönderdik.

## Apply Fonksiyonları
R dilinde for, while gibi döngüler kullanılsada veri analizi gibi alanlarda daha çok apply fonksiyonları kullanılır. Apply fonksiyonları bir vektörün her bir elemanı için belirlenen fonksiyonu uygular.Bunlardan en çok kullanılanları:
* apply
* lapply
* sapply'dir.

#### apply Fonksiyonu 
Bir fonksiyonu veri setinin satırlarına, sütunlarına veya her ikisine de uygulanmasını sağlar. Girdisi data frame veya matris, çıktısı vektör, liste veya array'dir.
apply fonksiyonunun argümanları:
```
apply(x, MARGIN, FUN)
```
* x: Veri seti
* MARGIN: MARGIN argümanı 1 değerini aldığında satır bazında, 2 değerini aldığında ise sütun  bazında ilgili fonksiyonu çalıştırır.
* FUN: Uygulanacak fonksiyon

Örneğin elemanları 1'den 20'ye kadar olan, 5 satırlı bir matrisin satırları toplamını bulalım.Öncelikle my_matrix isimli matrisimizi tanımlayalım.
```R
my_matrix = matrix(1:20, nrow = 5)
my_matrix
     [,1] [,2] [,3] [,4]
[1,]    1    6   11   16
[2,]    2    7   12   17
[3,]    3    8   13   18
[4,]    4    9   14   19
[5,]    5   10   15   20
```
Şimdi bu matrisin satırlarına apply fonksiyonunu uygulayalım. Satırlara uygulayacağımız için MARGIN = 1 olarak ayarlayalım. Satır toplamını bulacağımız için sum fonksiyonunu kullanalım.
```R
apply(my_matrix, MARGIN = 1, FUN = sum)
[1] 34 38 42 46 50
```
Sütunların toplamını görebilmek için MARGIN'ı 2 yapmamız yeterlidir.
```R
apply(my_matrix, MARGIN = 2, FUN = sum)
[1] 15 40 65 90
```
Herbir elemanın karekökünü bulmak  istersek sum fonksiyonu yerine sqrt fonksiyonunu kullanmamız gerekiyor.
```R
apply(my_matrix, MARGIN = 2, FUN = sqrt)

        [,1]     [,2]     [,3]     [,4]
[1,] 1.000000 2.449490 3.316625 4.000000
[2,] 1.414214 2.645751 3.464102 4.123106
[3,] 1.732051 2.828427 3.605551 4.242641
[4,] 2.000000 3.000000 3.741657 4.358899
[5,] 2.236068 3.162278 3.872983 4.472136
```
Kendimizin oluşturduğu bir fonksiyonu da apply fonksiyonunda kullanabiliriz.
Örnek olarak bütün elemanların karesini alan ve elemanın kendisini karesinden çıkartan bir fonksiyon yazıp, my_matrix matrisimizdeki elemanlara ugulayalım.
```R
apply(my_matrix, c(1,2), function(x) x^2-x)

     [,1] [,2] [,3] [,4]
[1,]    0   30  110  240
[2,]    2   42  132  272
[3,]    6   56  156  306
[4,]   12   72  182  342
[5,]   20   90  210  380
```
Dikkat ederseniz burada işlemi tüm elemanlara uygulayacağımız için satır ve sütunları c(1,2) şeklinde bir vektör ile ifade ettik.

Kolon bazında öğrencilerin boy ve kilo ortalamalarını hesaplayan kodu yazalım.
Bunun için ogrenciler adındaki aşağıdaki data frame'i oluşturalım.
```R
ogrenciler <- data.frame(Kilo=c(60, 70, 80,90,100), Boy=c(160, 175, 180, 190, 195))
ogrenciler

  Kilo Boy
1   60 160
2   70 175
3   80 180
4   90 190
5  100 195
```
apply fonksiyonunu uygulayalım.Kolon bazında olduğu için MARGIN argümanını 2 olarak ayarlayalım. Ortalama için mean() fonksiyonunu kullanalım.
```R
apply(ogrenciler, MARGIN=2, FUN=mean)
Kilo  Boy 
  80  180 
```
#### lapply Fonksiyonu
Bir girdinin tüm elementlerine bir fonksiyonun uygulanmasını sağlar. Girdisi liste, vektör veya data frame, çıktısı listedir.

lapply fonksiyonunun argümanları:
```
lapply(x, FUN)
```
* x: Veri seti
* FUN: Uygulanacak fonksiyon

Şimdi karakter, numerik ve mantıksal şekilde eleman içeren 3 farklı vektörden bir liste oluşturalım.
```R
a = c("t","e","s","t")
b = c(1,2,3,4,5)
c = c(F,T,F)
myList = list(a,b,c)

[[1]]
[1] "t" "e" "s" "t"

[[2]]
[1] 1 2 3 4 5

[[3]]
[1] FALSE  TRUE FALSE
```
class() metoduyla a,b,c vektörlerinin sınıflarına bakabilirsiniz.
Listedeki her bir fonksiyonun uzunluğunu bulalım.
```R
lapply(myList, length)

[[1]]
[1] 4

[[2]]
[1] 5

[[3]]
[1] 3
```
Her bir vektörün sınıfını öğrenmek için ise laaply fonksiyonunu uygulayabiliriz.
```R
lapply(myList, class)
[[1]]
[1] "character"

[[2]]
[1] "numeric"

[[3]]
[1] "logical"
```

Aşağıda büyük harflerle yazılan süper kahraman isimlerinden oluşan vektördeki tüm süper kahraman isimlerini küçük harfle yazdıralım. Bunun için tolower fonksiyonunu kullanabiliriz.
```R
kahramanlar <- c("SPIDERMAN", "BATMAN", "SUPERMAN", "IRONMAN", "ANTMAN")
kahramanlar
[1] "SPIDERMAN" "BATMAN"    "SUPERMAN"  "IRONMAN"   "ANTMAN"  

kucukHarfli <- lapply(kahramanlar, tolower)
kucukHarfli
[[1]]
[1] "spiderman"

[[2]]
[1] "batman"

[[3]]
[1] "superman"

[[4]]
[1] "ironman"

[[5]]
[1] "antman"
```
 kucukHarfli değişkenine bakarsak vektördeki tüm elemanların küçük harfleriyle yazıldığını görebiliriz.

str_sub() fonksiyonu ve lapply() fonksiyonu yardımıyla her bir süperkahramanın ilk harfini elde edelim. str_sub() da dahil herhangi bir fonksiyonun argümanlarını kullanacağımız zaman bunu str_sub() fonksiyonunun yerine lapply() fonksiyonunun içine argüman olarak yazıyoruz.

Örnek: lapply(x, fun, argüman1, argüman2…)

Öncelikle eğer bilgisayarımıza indirmediysek install.packages() fonksiyonu ile stringr paketini indirelim. Ve library fonksiyonu ile paketi çağıralım.
```R
install.packages("stringr")
library(stringr)
str_sub(kahramanlar, start=1, end = 1)
[1] "S" "B" "S" "I" "A"
lapply(kahramanlar, str_sub, start=1, end=1)
[[1]]
[1] "S"

[[2]]
[1] "B"

[[3]]
[1] "S"

[[4]]
[1] "I"

[[5]]
[1] "A"
```
Gördüğümüz gibi str_sub ve laaply fonksiyonunu kullanarak her bir süperkahramanın ilk harfini elde etmiş olduk.

#### sapply Fonksiyonu
lapply fonksiyonu gibi bir girdinin tüm elementlerine bir fonksiyonun uygulanmasını sağlar.lapply fonksiyonundan farkı çıktısıdır. Girdisi liste, vektör veya data frame, çıktısı vektör veya matristir.

sapply fonksiyonunun argümanları:
```
sapply(x, FUN)
```
* x: Veri seti
* FUN: Uygulanacak fonksiyon

Daha önce oluşturduğumuz ogrenciler data frame’ini kullanarak maksimum kilo ve boyu sapply fonksiyonunu () kullanarak bulalım.
```R
sapply(ogrenciler, FUN=max)
Kilo  Boy 
 100  195 
```
Oluşturduğumuz ogrenciler veri setindeki tüm değişkenleri karakter veri tipine çevirelim.
```R
ogrenciler[,c("Kilo","Boy")] <- sapply(ogrenciler[,c("Kilo", "Boy")], as.character)
```
str() fonksiyonu ile veri setindeki kolonlardaki değişkenlerin karakter veri tipine çevrildiğini görebiliriz.
```R
str(ogrenciler)
'data.frame':	5 obs. of  2 variables:
 $ Kilo: chr  "60" "70" "80" "90" ...
 $ Boy : chr  "160" "175" "180" "190" ...
```
