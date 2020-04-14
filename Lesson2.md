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
## Data Frameden Mantıksal Operatörlerle Kesit Alma
## Kontrol İfadeleri

Mantıksal testler iki girdi arasında karşılaştırma yapmak için kullanılır ve çıktı olarak Boolean (TRUE or FALSE) döndürür.

R dilinde sıklıkla kullanılan mantıksal testler şunlardır.

<1. girdi küçüktür 2. girdia < b>1. girdi büyüktür 2. girdia > b<=1. girdi küçük eşittir 2. girdia <= b>=1. girdi büyük eşittir 2. girdia >= b==1. girdi eşittir 2. girdia == b!=1. girdi eşit değildir 2.girdia != b

```R
a <- 5; b <-3 # a ve b değişkenlerimize sırasıyla 5 ve 3 atadık.

a < b
[1] FALSE

```



## for döngüsü
## paste() fonksiyonu
## while döngüsü
## Fonksiyonlar
## Apply Fonksiyonları

