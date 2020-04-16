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
## for döngüsü
## paste() fonksiyonu
## while döngüsü
## Fonksiyonlar
## Apply Fonksiyonları
R dilinde for, while gibi döngüler kullanılsada veri analizi gibi alanlarda daha çok apply fonksiyonları kullanılır. Apply fonksiyonları bir vektörün her bir elemanı için belirlenen fonksiyonu uygular.Bunlardan en çok kullanılanları:
  *apply
  *lapply
  *sapply'dir.

#### apply Fonksiyonu 
Bir fonksiyonu veri setinin satırlarına, sütunlarına veya her ikisine de uygulanmasını sağlar. Girdisi data frame veya matris, çıktısı vektör, liste veya array'dir.
apply fonksiyonunun argümanları:
```
apply(x, MARGIN, FUN)
```
*x: Veri seti
*MARGIN: MARGIN argÜmanı 1 değerini aldığında satır bazında, 2 değerini aldığında ise sütun  bazında ilgili fonksiyonu çalıştırır.
*FUN: Uygulanacak fonksiyon

Örneğin elemanları 1'den 20'ye kadar olan, 5 satırlı bir matrisin satırları toplamını bulalım.Öncelikle my_matrix isimli matrisimizi tanımlayalım.
```
my_matrix = matrix(1:20, nrow = 5)
```
Şimdi bu matrisin satırlarına apply fonksiyonunu uuygulayalım. Satırlara uygulayacağımız için MARGIN = 1 olarak ayarlayalım. Satır toplamını bulacağımız için sum fonksiyonunu kullanalım.
```
apply(my_matrix, MARGIN = 1, FUN = sum)
```
Bu işlemleri R Studio'da uyguladığınızda satırların toplamını görebilirsiniz.
Sütunların toplamını görebilmek için MARGIN'ı 2 yapmamız yeterlidir.
```
apply(my_matrix, MARGIN = 2, FUN = sum)
```
Herbir elemanın karekökünü bulmak  istersek sum fonksiyonu yerine sqrt fonksiyonunu kullanmamız gerekiyor.
```
apply(my_matrix, MARGIN = 2, FUN = sqrt)
```
Kendimizin oluşturduğu bir fonksiyonu da apply fonksiyonunda kullanabiliriz.
Örnek olarak bütün elemanların karesini alan ve elemanın kendisini karesinden çıkartan bir fonksiyon yazıp, my_matrix matrisimizdeki elemanlara ugulayalım.
```
apply(my_matrix, c(1,2), function(x) x^2-x)
```
Dikkat ederseniz burada işlemi tüm elemanlara uygulayacağımız için satır ve sütunları c(1,2) şeklinde bir vektör ile ifade ettik.

Kolon bazında öğrencilerin boy ve kilo ortalamalarını hesaplayan kodu yazalım.
Bunun için ogrenciler adındaki aşağıdaki data frame'i oluşturalım.
```
ogrenciler <- data.frame(Kilo=c(60, 70, 80,90,100), Boy=c(160, 175, 180, 190, 195))
```
apply fonksiyonunu uygulayalım.Kolon bazında olduğu için MARGIN argümanını 2 olarak ayarlayalım. Ortalama için mean() fonksiyonunu kullanalım.
```
apply(ogrenciler, MARGIN=2, FUN=mean)
```
#### lapply Fonksiyonu
Bir girdinin tüm elementlerine bir fonksiyonun uygulanmasını sağlar. Girdisi liste, vektör veya data frame, çıktısı listedir.

lapply fonksiyonunun argümanları:
```
lapply(x, FUN)
```
*x: Veri seti
*FUN: Uygulanacak fonksiyon

Şimdi karakter, numerik ve mantıksal şekilde eleman içeren 3 farklı vektörden bir liste oluşturalım.
```
a = c("t","e","s","t")
b = c(1,2,3,4,5)
c = c(F,T,F)
myList = list(a,b,c)
```
class() metoduyla a,b,c vektörlerinin sınıflarına bakabilirsiniz.
Listedeki her bir fonksiyonun uzunluğunu bulalım.
```
lapply(myList, length)
```
Her bir vektörün sınıfını öğrenmek için ise
```
lapply(myList, class)
```
fonksiyonunu uygulayabiliriz.

Aşağıda büyük harflerle yazılan süper kahraman isimlerinden oluşan vektördeki tüm süper kahraman isimlerini küçük harfle yazdıralım. Bunun için tolower fonksiyonunu kullanabiliriz.
```
kahramanlar <- c("SPIDERMAN", "BATMAN", "SUPERMAN", "IRONMAN", "ANTMAN")

kucukHarfli <- lapply(kahramanlar, tolower)
```
R Studio'da kucukHarfli değişkenine bakarsak vektördeki tüm elemanların küçük harfleriyle yazıldığını görebiliriz.

str_sub() fonksiyonu ve lapply() fonksiyonu yardımıyla her bir süperkahramanın ilk harfini elde edelim. str_sub() da dahil herhangi bir fonksiyonun argümanlarını kullanacağımız zaman bunu str_sub() fonksiyonunun yerine lapply() fonksiyonunun içine argüman olarak yazıyoruz.

Örnek: lapply(x, fun, argüman1, argüman2…)

Öncelikle eğer bilgisayarımıza indirmediysek install.packages() fonksiyonu ile stringr paketini indirelim. Ve library fonksiyonu ile paketi çağıralım.
```
install.packages("stringr")
library(stringr)
str_sub(kahramanlar, start=1, end = 1)
lapply(kahramanlar, str_sub, start=1, end=1)
```
Gördüğümüz gibi str_sub fonksiyonunu kullanarak her bir süperkahramanın ilk harfini elde etmiş olduk.

#### sapply Fonksiyonu
lapply foksiyonu gibi bir girdinin tüm elementlerine bir fonksiyonun uygulanmasını sağlar.lapply fonksiyonundan farkı çıktısıdır. Girdisi liste, vektör veya data frame, çıktısı vektör veya matristir.

sapply fonksiyonunun argümanları:
```
sapply(x, FUN)
```
*x: Veri seti
*FUN: Uygulanacak fonksiyon

Daha önce oluşturduğumuz ogrenciler data frame’ini kullanarak maksimum kilo ve boyu sapply fonksiyonunu () kullanarak bulalım.
```
sapply(ogrenciler, FUN=max)
```
Oluşturduğumuz ogrenciler veri setindeki tüm değişkenleri karakter veri tipine çevirelim.
```
ogrenciler[,c("Kilo","Boy")] <- sapply(ogrenciler[,c("Kilo", "Boy")], as.character)
```
str() fonksiyonu ile veri setindeki kolonlardaki değişkenlerin karakter veri tipine çevrildiğini görebiliriz.
```
str(ogrenciler)
```