# Lesson 1 

### Başlıklar: 
* Sosyal Fayda için Veri Bilimi nedir ?
* Programın gidişi ve program sonundaki projeler 
* Neden R ?
* [Giriş](#giriş) 
* [Veri tipleri](#veri-tipleri)
* [R objeleri oluşturma](#r-objeleri-oluşturma)
* [Vektörler](#vektörler)
* [Matrisler](#matrisler)
* [Faktörler](#faktörler-factors)
* [Dataframeler](#data-frames)
* [R notasyonları](#r-notasyonları)
* [R'da Listeler](#r'da-listeler)

## Sosyal Fayda için Veri Bilimi nedir ?
yazılacak
## Programın gidişi ve program sonundaki projeler 
yazılacak
## Neden R ?
yazılacak
## Giriş:
R, S programlama dili üzerine yazılan ve istatistiksel analizlerin R içindeki kütüphaneler yardımıyla 
kolay bir şekilde yapılmasını sağlayan bir platformdur. 
R studio, R dilini Compile etmek için kullanacağımız araçtır. 

R studio içinde dört tane kısımdan oluştuğunu görüyoruz,
* Sol üst kısım, kod yazacağımız kısımdır
* onun altında Console yer almaktadır, yazdığımız kodun çıktısını burada görebiliriz
* Sağ üst kısım Environment kısmı, compile edilen değişkenler burada görünüyor
* onun altında Help kısmı, R studio da bir şeyi aratmak istediğimizde kullanırız


R'ı başlangıçta bir hesap makinesi olarak düşünebiliriz, burada her türlü hesap yapılabilir. Toplama, Çıkarma ve Çarpma işlemlerini görelim. 

**NOT: yazdıklarımızı çalıştırmak için ya çalıştırmak istediğimiz kod parçasını seçip yukarıdaki *run* tuşuna tıklayabiliriz ya da kısaca CTRL+ENTER'a basabiliriz**
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

> *İnan Hoca: Hataları okumayı öğrenmemiz lazım, sürekli hatalarla karşılaşacağız ve onları okuyup hatayı gidermeye çalışacağız.*

  ## Veri tipleri
  R'da birçok veri tipi vardır, bunlara bakalım:
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
  * Integers: Doğal sayılara Integer diyoruz, integer sayılar numeric sayılır.
  Integer sayıları ayırt etmek için ardından L harfi ekleyebiliriz
  ```R
  5L
  > 5
  ```
  
  * Logical (Mantıksal): Mantıksal operatörler R’da TRUE ve FALSE ile ifade edilmektedir. Bir ifadenin doğruluğunu veya yanlışlığını       belirlemek amaçlı kullanılır. Aynı zamanda, TRUE 1 rakamına eşit olup FALSE ise 0 rakamına eşittir.
  R’da Mantıksal operatörler veriyi manipüle etmede (filtreleme, veriden kesit alma, döngüler) oldukça önemli ve kullanışlıdır.
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
  
  TRUE == 1
  > TRUE
  
  FALSE == 0
  > TRUE
  ```
  **NOT: İki objenin eşit olup olmadığını test etmek istersek "==" mantıksal operatörünü kullanıyoruz. "=" operatörü atama yapmak için kullanılan bir operatördür.**
  
  * Complex: kompleks ifadeleri çok kullanacak olmasak bile, varlığından bilmekte fayda var.
  ```R
  1+2i
  ```
  
  Farklı tiplerden veri toplarsak ne olur ?
  ```R
  16 + TRUE
  > 17
  
  12 + "12"
  > Error in 12 + "12" : non-numeric argument to binary operator
  ```
  12 + TRUE ifadesini toplayabildi çünkü daha önce gördüğümüz gibi TRUE ifadesi 1 olarak algılanır. Yalnızca 12+"12" ifadesi bir hata verdi çünkü tırnak içinde yazdığımız herhangi bir şey character olarak algılanır, sayı olarak değil. 
  
  Bir şeyin veri tipini incelemek istersek **class()** fonksiyonunu kullanabiliriz,
  ```R
  class(12)
  > numeric
  
  class("12")
  > character
  
  class(TRUE)
  > logical
  
  class("TRUE")
  > character
  ```
  
  ## R objeleri oluşturma
  R'da bazı fonksiyonlar objelerle çalışır. Dolayısıyla bir fonksiyonun çalışması için obje oluşturma (değişken atama)
  yapmamız gerekir. Bunu "<-" veya "=" sembolleriyle yapabiliriz. Fakat "=" operatörü ve "==" operatörünü karıştırmamak
  için genelde "<-" operatörünü kullanmanızı tavsiye ediyorum.
  
  Bir character değişkeni oluşturalım:
  ```R
  x <- "Data Science For The Public Good"
  ```
  "Data Science For The Public Good" ifadesini tanımlayıp x adlı değişkenine atadık. Bu kod parçasını çalıştırdıktan sonra R studio'nun Environment kısmında(sağ üst kısım) x değişkeni ve değerinin belirdiğini göreceksiniz. Şimdi x değişkenini ne zaman istersek kullanabilir ve değerini elde edebiliriz.
  ```R
  x
  > "Data Science For The Public Good"
  ```
  
  Bir numeric değişken oluşturalım,
  ```R
  a <- 5
  b <- 6
  c <- a+b
  c
  
  > 11
  ```
  
  **NOT: dikkat edelim ki R studionun environment kısmında compile ettiğimiz değişkenlerin bilgileri yer almaktadır.**
  
  > **Egzersiz:  numerik1 ve numerik2 objelerine (değişkenlerine) 4 ve 2 rakamlarını atayıp bu iki objeyi toplayalım. Bu toplamı "numeriklerim" isimli değişkene atayıp verinin tipini inceleyelim.**
  
  > **Egzersiz: a'nın 10 b'nin 2 olduğu iki numerik değişken oluşturalım. Bu iki değişkeni çarptıktan sonra c isimli değişkene atayalım. Sonrasında ise c'nin b üstünü (^) sembolü kullanarak alalım.**

  
  ## Vektörler
  R vektörleri aynı tip elemanlardan (veriden) oluşan bir koleyksiyondur. Bir vektör sadece aynı tip elemanlara sahip olabilir. 
  İleride işlenecek olan değişken ve Data Frame konularında değişkenleri oluşturmada kullanılır. 
  Bir vektör "c()" fonksiyonu ile oluşturulur.
  
  ```R
  vektörüm <- c("Hello","World")
  vektörüm
  
  >"Hello"  "World"
  
  class(vektörüm)
  > "character"
  ```
  
 > **Egzersiz: vektorum1 isimli ve elemanları "DSPG", "CADS", "TEDU" olan bir vektör oluşturalım. sonrasında isi genelvektor isimli   vektörümüzü vektorum ve vektorum1 vektörlerini kullanarak oluşturalım.**
 
 Numerik Vektör oluşturalım:
 
 ```R
 numerik_vek1 <- c(1,2,3,4,5)
 numerik_vek2 <- c(6,7,8,9,10)
 class(numerik_vek1)
 > "numeric"
 
 toplam_vek <- numerik_vek1 + numerik_vek2
 toplm_vek 
 > 7  9 11 13 15 
 
 toplam_vek <- toplam_vek + 1
 toplam_vek
 > 8 10 12 14 16
 ```
 
 Belli bir aralıkla bir sayı vektörü oluşturmak istersek:
 ```R
 benim_vektorum <- 1:10
 benim_vektorum
 > 1  2  3  4  5  6  7  8  9 10
 ```
 seq() fonksiyonu da kullanabiliriz. 
 ** NOT: fonksiyonun adından önce '?' işareti koyup ( ?seq() şeklinde ) çalıştırırsak, sağdaki pencerede fonksiyonun tanımı, aldığı parametreler, ve diğer kullanım bilgileri bulabiliriz. En ağağıya gelirsek örnekleri de bulabiliriz. 
 bu işlem help() fonksiyonu kullanarak ( help(seq) şelinde) da gerçekleştirilebilir**

```R
aralıklı <- seq(from=1, to=10, by=2)
aralıklı

> 1 3 5 7 9
````
  
> **Egzersiz: Kiloları 65, 70, 80, 90, ve 100 olan, boyları ise 1.65, 1.75, 1.85, 1.9, ve 2.0 olan öğrencilerin kilo ve boy isimli vektörlerini oluşturalım. Ayrı ayrı hem kilo hem boy ortalamalarını bulalım.
İpucu: Vektörler oluşturulduktan sonra mean() fonksiyonun içine oluşturduğumuz vektör isimlerini yazarak ortalamaları bulabiliriz.**

```R
kilo <- c(65, 70, 80, 90, 100)
boy <- c(1.65, 1.75, 1.85, 1.9, 2)

mean(kilo)
mean(boy)

> 81
> 1.83
```

## Matrisler
Çok boyutlu veri depolamanın yöntemlerinden biridir. Matris fonksiyonuyla oluşturulurlar argüman olarak satır ve sütun sayısıyla birlikte yuvalara yerleşecek olan verileri alırlar.
Matrix oluşturma fonksiyonuna bakalım,
```R
?matrix()
``` 
<img src="/Matrix_doc.JPG" width="400">

ilk parametre data parametresi, vektör gibi bir şey alıdığını görebiliyoruz. ikinci ve üçüncü parametre ise matrisin satır ve kolon sayısı ifade ediyor. Matirisimizi oluşturalım.

```R
matrix(1:16, nrow=4, ncol=5)

>      [,1] [,2] [,3] [,4] [,5]
[1,]    1    5    9   13    1
[2,]    2    6   10   14    2
[3,]    3    7   11   15    3
[4,]    4    8   12   16    4
```

cbind() ve rbind() fonksiyonları: vektöre ya da matrise satır ya da kolon ekleme işlemi için kullanılır.
```R
v <- 1:5
x <- 6:10
cbind(v, x)

>      v  x
  [1,] 1  6
  [2,] 2  7
  [3,] 3  8
  [4,] 4  9
  [5,] 5 10
  
  
rbind(v,x)

>   [,1] [,2] [,3] [,4] [,5]
v    1    2    3    4    5
x    6    7    8    9   10
```

> **Egzersiz: Önceden oluşturduğumuz "kilo" ve "boy" değişkenlerini hem matrix() fonksiyonu hem cbind() kullanarak matrislerimizi oluşturalım.**
```R
matrix(data=c(kilo, boy), ncol=2)
```
Eğer matris değişkenlerine isim vermek istersek:
```R
m <- matrix(data=c(kilo, boy), ncol=2, dimnames = list(c("P1", "P2", "P3", "P4", "P5"),c("kilo", "boy")))
m

>    kilo  boy
P1   65 1.65
P2   70 1.75
P3   80 1.85
P4   90 1.90
P5  100 2.00
```
## Faktörler (Factors)
Faktörler, R’da kategorik bilgilerin tutulduğu yerlerdir. Kısacası etiketli vektörlerdir. 
Çoğu zaman kategorik verileri gerek görselleştirme yaparken gerek istatistiksel analizler yaparken veri tipini factor olarak değiştiriyoruz.
kategorik bilgiler belli sayıda değer alan bilgilerdir. Örneğin, cinsiyet bilgisi çoğu zaman "erkek" ya da "kadın" olarak belirlenebilir.
Bir vektör oluşturalım, 
```R
f <- c("Hello", "World", "Hello", "Annie", "Hello", "World")
f
> "Hello" "World" "Hello" "Annie" "Hello" "World"

class(f)
> "character"
```
Şimdi bir faktöre dönüştürelim,
```R
f <- factor(f)
class(f)
> "factor"

f
> Hello World Hello Annie Hello World
Levels: Annie Hello World
```
Burada gördüğümüz gibi 3 tane unique değer vardır.
f vektörümüz artık faktör olduğu için table() fonksiyonu kullanarak f vektöründeki verilerin sıklığını elde edebiliriz.
(summary() fonksiyonu da kullanabiliriz)
```R
table(f)
> Annie Hello World 
    1     3     2
```
/#yazilacak: faktorlerin ordered ve levels argümalnarı

> **Egzersiz: Şampiyonlar ligini 7 kere kazanan Milan, 5 kere kazanan Liverpool ve 5 kere kazanan Barcelona’nın champions isimli vektörünü oluşturalım. Oluşturulan vektörü faktör veri tipi olarak değiştirelim. 
Sonrasında ise hem table() hem de summary() fonksiyonunu kullanarak toplam kazandıkları kupaları görelim.
İpucu: Herhangi bir veriyi tekrarlı girmek için rep() fonksiyonunu kullanabiliriz.**
```R
Milan <- c("Milan", "Milan", "Milan", "Milan", "Milan", "Milan", "Milan")
Liverpool <- c("Liverpool","Liverpool","Liverpool","Liverpool","Liverpool")
Barcelona <- c("Barcelona","Barcelona","Barcelona","Barcelona","Barcelona")

champions <- c(Milan, Liverpool, Barcelona)
champions <- factor(champions)

table(champions)
> Barcelona Liverpool     Milan 
        5         5         7 
        
summary(champions)
> Barcelona Liverpool     Milan 
        5         5         7 
```
 rep() kullanarak, 
 ```R
champions <- factor(c(rep("Milan", 7),rep("Liverpool", 5), rep("Barcelona",5)))
table(champions)
>  Barcelona Liverpool     Milan 
        5         5         7 
 ```
 
 
 Soru : Benim kodum çıktı vermiyor ve console kısmında '>' işareti yerine '+' işareti görünüyor.  
 Cevap : R studio kodun eksik olduğunu söylüyor ve kodu tamamlamanı bekliyor. Esc'e basarak yorum modundan çıkılabilirve kodu tekrar çalıştırabilirsiniz.
 
 ## Data Frames
 Data Frame değişkenlerden ve gözlemlerden oluşan yapılandırılmış bir tablodur. Bir Data Frame’in karakteristikleri aşağıdaki gibidir:
1. Kolon isimleri (değişken isimleri) boş olmamalıdır
1. Satır isimleri özgün olmalıdır
1. Data frame’de depolanan veri tipi numerik, faktör ya da karakter olmalıdır
1. Her kolon (değişken) ise eşit sayıda veri içermelidir

Bir dataframe oluşturalım, bunu yapmak için data.frame() fonksiyonunu kullanırız. Argümanları için (kolonun adı = kolonun değeri) şeklinde yazılarak dataframe'in yapısı ve değerleri belirlenir. 
```R
myframe <- data.frame(
  ogreni_id = c(1:5),
  ogreni_ad = c("Ayşe", "Fatma", "Ali", "Mehmet", "Zeynep"),
  kilo = c(48, 56, 75, 89, 53),
  boy = c(160, 165, 177, 196, 169),
  stringsAsFactors = T
)
myframe

>   ogreni_id ogreni_ad kilo boy
1         1      Ayse   48 160
2         2     Fatma   56 165
3         3       Ali   75 177
4         4    Mehmet   89 196
5         5    Zeynep   53 169
```
Gördüğümüz gibi bir argüman daha yazdık, stringAsFactors argümanı. data.frame() tanımına bakarsak bunu görürüz:

*stringsAsFactors 
logical: should character vectors be converted to factors? The ‘factory-fresh’ default is TRUE, but this can be changed by setting *

Yani bu argümanı TRUE olarak yazarsak, mydataframe içindeki girdiğimiz character tipinden değerler faktör olarak tutulmasını söylüyoruz, bu da işimize yarayabilir. Yalnızca argümanın default değeri TRUE olduğu için bu durumda yazmasak ta bir şey değişmezdi.

Bir dataframe'in yapısını incelemek için str() fonksiyonu kullanırız.
```R
str(myframe)
> 'data.frame':	5 obs. of  4 variables:
 $ ogreni_id: int  1 2 3 4 5
 $ ogreni_ad: Factor w/ 5 levels "Ali","Ayse","Fatma",..: 2 3 1 4 5
 $ kilo     : num  48 56 75 89 53
 $ boy      : num  160 165 177 196 169
```
dataframe'in özet istatistiklerine bakmak istiyorsak summary() fonksiyonu kullanabiliriz

```R
summary(myframe)
>    ogreni_id  ogreni_ad      kilo           boy       
 Min.   :1   Ali   :1   Min.   :48.0   Min.   :160.0  
 1st Qu.:2   Ayse  :1   1st Qu.:53.0   1st Qu.:165.0  
 Median :3   Fatma :1   Median :56.0   Median :169.0  
 Mean   :3   Mehmet:1   Mean   :64.2   Mean   :173.4  
 3rd Qu.:4   Zeynep:1   3rd Qu.:75.0   3rd Qu.:177.0  
 Max.   :5              Max.   :89.0   Max.   :196.0  
```

> *İnan hoca: Normalde veribiliminde kullandığımız dataframeler çok büyük olur ve bir dataframe'i yüklediğimiz zaman yaptığımız ilk şey, str() ve summary()'ne bakmaktır.*

Kullanacağımız dataframeler binlerce gözlemden oluşur olacak. O yüzden bütün gözlemler çağırmak yerine head() ve tail() fonksiyonlarını kullanarak ilk ve son birkaç tanesini çağırabiliriz.
```R
#head() fonksiyonu ilk 6 gözlemi getirir
head(myframe) 
>   ogreni_id ogreni_ad kilo boy
1         1      Ayse   48 160
2         2     Fatma   56 165
3         3       Ali   75 177
4         4    Mehmet   89 196
5         5    Zeynep   53 169

# n parametresini belirlersek ilk n satırı getirir
# örneğin: myframe'in ilk 3 gözlemini elde etmek istersek
head(myframe, n=3)
>   ogreni_id ogreni_ad kilo boy
1         1      Ayse   48 160
2         2     Fatma   56 165
3         3       Ali   75 177


#tail() fonksiyonu son 6 saırı getirir, 
tail(myframe)
>   ogreni_id ogreni_ad kilo boy
1         1      Ayse   48 160
2         2     Fatma   56 165
3         3       Ali   75 177
4         4    Mehmet   89 196
5         5    Zeynep   53 169

# n parametresini belirlersek son n saırı getirir
# örneğin: myframe'in son 2 gözlemini elde etmek istersek
tail(myframe, n=2)
>   ogreni_id ogreni_ad kilo boy
4         4    Mehmet   89 196
5         5    Zeynep   53 169

```

Dataframe'in kolonlarına '$' işareti ile de erişebiliriz.
```R
myframe$ogreni_id
> 1 2 3 4 5 
```
Dataframe'e yeni bir kolon eklemek istersek yine '$' işareti yardımıyla gerçekleştirebiliriz
```R
myframe$dersnotu <- c("A","B","C","A","A")
myframe

>   ogreni_id ogreni_ad kilo boy ders_notu
1         1      Ayse   48 160         A
2         2     Fatma   56 165         B
3         3       Ali   75 177         C
4         4    Mehmet   89 196         A
5         5    Zeynep   53 169         A
```

Bir kolonu silmek istersek yine '$' işareti kullanarak işlemi gerçekleştirebiliriz
```R
myframe$ders_notu <- NULL
myframe

>   ogreni_id ogreni_ad kilo boy
1         1      Ayse   48 160
2         2     Fatma   56 165
3         3       Ali   75 177
4         4    Mehmet   89 196
5         5    Zeynep   53 169
```

> **Egzersiz: Öğrencilerin matematik harf notlarını, "harf_notu" değişken ismiyle oluşturduğumuz data frame’e ekleyelim. 
Harf notunu faktör veri tipine çevirip. Data frame’in özetine bakalım **
```R
myframe$harf_notu <- c("A", "B", "C", "A", "A")
myframe$harf_notu <- factor(myframe$harf_notu)
str(myframe)
> 'data.frame':	5 obs. of  5 variables:
 $ ogreni_id: int  1 2 3 4 5
 $ ogreni_ad: Factor w/ 5 levels "Ali","Ayse","Fatma",..: 2 3 1 4 5
 $ kilo     : num  48 56 75 89 53
 $ boy      : num  160 165 177 196 169
 $ harf_notu: Factor w/ 3 levels "A","B","C": 1 2 3 1 1
 
 summary(myframe)
 >    ogreni_id  ogreni_ad      kilo           boy        harf_notu
 Min.   :1   Ali   :1   Min.   :48.0   Min.   :160.0   A:3      
 1st Qu.:2   Ayse  :1   1st Qu.:53.0   1st Qu.:165.0   B:1      
 Median :3   Fatma :1   Median :56.0   Median :169.0   C:1      
 Mean   :3   Mehmet:1   Mean   :64.2   Mean   :173.4            
 3rd Qu.:4   Zeynep:1   3rd Qu.:75.0   3rd Qu.:177.0            
 Max.   :5              Max.   :89.0   Max.   :196.0            
```

## R Notasyonları
R programlama dilinde endeksleme 1’den başlar. Dolayısıyla bir vektörde veya data framede bir veya birden fazla kesit seçeceğimiz zaman bunu dikkate alarak seçim yapmalıyız.

Bir vektörden eleman nasıl seçilir ?
```R
myvec <- 1:5
myvec[2]
> 2

Bir vektördeki bir elemanı nasıl değiştirilir ?
```R
myvec[2] <- 9
myvec
> 1 7 3 4 5
```

Data Frame'den nasıl kesit alınır ?
Köşeli parantezin soluna satırdan bir kesit, köşeli parantezin sağına kolonlardan kesit almak için kullanılır
```R
myframe
>   ogreni_id ogreni_ad kilo boy
1         1      Ayse   48 160
2         2     Fatma   56 165
3         3       Ali   75 177
4         4    Mehmet   89 196
5         5    Zeynep   53 169

# Data Frame'den 1. satırı seçme
myframe[1, ]
>   ogreni_id ogreni_ad kilo boy
1         1      Ayse   48 160

# Data Frame'den 2. kolonu seçme
myframe[, 2]
> Ayse   Fatma  Ali    Mehmet Zeynep
Levels: Ali Ayse Fatma Mehmet Zeynep

# Data Frame'den 2. satır 3. sütunu seçme
myframe[2,3]
> 56

# Direkt olarak kolon ismi ile de istediğimiz kolonu seçebiliyoruz.
myframe[,"kilo"]
>   48 56 75 89 53

# Kilo ve boy kolonlarını endeksleyerek elde etme
# 1.yol
myframe[,c(3,4)]
>   kilo boy
1   48 160
2   56 165
3   75 177
4   89 196
5   53 169

# 2.yol
myframe[,c("kilo","boy")]

# 3.yol
myframe[1:3,3:5]
```

> **Egzersiz: myframe veri setimizdeki 1. kişinin kilosunu 65 olarak değiştirelim**

Başka bir R notasyon ise negatif indekslemedir. '-' simgesiyle belirttiğimiz kolon ya da satır hariç diğer şeyleri seçmemizi sağlar
```R
# 1. satır hariç tüm satırları getir
myframe[-1,]
>   ogreni_id ogreni_ad kilo boy
2         2     Fatma   56 165
3         3       Ali   75 177
4         4    Mehmet   89 196
5         5    Zeynep   53 169

# 2. kolon hariç tüm kolonları ve satırları getir
myframe[,-2]
>   ogreni_id kilo boy
1         1   48 160
2         2   56 165
3         3   75 177
4         4   89 196
5         5   53 169

# ilk 3 satırı getirme
myframe[-1:-3,]
>   ogreni_id ogreni_ad kilo boy
4         4    Mehmet   89 196
5         5    Zeynep   53 169

 #ilk kolon hariç 1'den 4'e kadar olan satırları getir, kesit alınmış veri setimizden Ayşe'nin harf notunu elde edelim.
 # 1.yol
 myframe[1:4,-1]$harf_notu[1]
 > A
Levels: A B C

 # 2.yol
 myframe[1:4,-1][1,"harf_notu"]

```

## R'da Listeler
Hatırlayacağınız gibi R'da vektörlerin elemanları aynı tipte değişkenlerden oluşuyordu. Listeler ise bunun tam tersi şeklinde olmakta. Listeler karakter, nümerik, data frame vb. değişkinlerden oluşabilir. Liste oluşturmak için ise **list()** fonksiyonunu kullanıyoruz. Hemen örneklere bakalım.
```R
mylist<-list(sayılar=c(1:6),karakter=c("DSPG","CADS","TEDU"),mantık=c(T,F,T),myframe)
mylist
>$sayılar
[1] 1 2 3 4 5 6

$karakterler
[1] "DSPG" "CADS" "TEDU"

$mantıksal
[1]  TRUE FALSE  TRUE

[[4]]
       ogrenci_id ogrenci_ad kilo boy harf_notu
Ayşe            1       Ayşe   65 160         A
Fatma           2      Fatma   56 165         B
Ali             3        Ali   75 177         C
Mehmet          4     Mehmet   89 196         A
Zeynep          5     Zeynep   60 169         A
```

Gördüğünüz gibi listeye istediğimiz veri tipindeki elemanları eklyebiliyoruz. Ayrıca verdiğimiz istersek isim de verebiliyoruz. Listemizi çağırdığımızda isim verdiğimiz değişkenlerin isimlerini, vermediklerimizin ise endeksini görmekteyiz. Bahsedilen isimlendirme ve endeksleme ile listemizden parça alabiliriz.
```R
#mylist listesinden sayıları değişkenini elde edelim.
mylist$sayılar
#ya da
mylist[[1]]
>1 2 3 4 5 6

#mylist listesinden data frame'i elde edelim.Data Frame bir isme sahip değil çağırırken dikkat edelim.
mylist[[4]]
>       ogrenci_id ogrenci_ad kilo boy harf_notu
Ayşe            1       Ayşe   65 160         A
Fatma           2      Fatma   56 165         B
Ali             3        Ali   75 177         C
Mehmet          4     Mehmet   89 196         A
Zeynep          5     Zeynep   60 169         A
```

Gördüğünüz gibi isimlendirdiğimiz elemanı ismi ile çağırabiliyoruz. İsimsiz olanları ise endeksine göre çağırmalıyız. Şimdi ise çağırdığımız elemanlardan parçalar alalım.
```R
#-Egzersiz-
#Oluşturduğumuz mylist listemizden myframe veri setine erişip Mehmet'in boyuna ulaşalım.

mylist[[4]]["Mehmet", "boy"]
>196
#ya da
mylist[[4]]$boy[4]
>196
```

Myframe bir dataframe olduğu için listeden çağırdığımızda da dataframe bölümünde gördüğümüz kesit alma yöntemleri ile kesit alabiliyoruz. Şimdi ise bu kesit değerlerini nasıl değiştirebileceğimize bakalım.
```R
#-Egzersiz-
#Oluşturduğumuz mylist listemizden myframe veri setine erişip Zeynep'in kilosunu 60 olarak değiştirip
#yeniset isimli data frame olarak atayalım.

mylist[[4]]["Zeynep","kilo"] <- 60
yeniset <- mylist[[4]]

yeniset
>       ogrenci_id ogrenci_ad kilo boy harf_notu
Ayşe            1       Ayşe   65 160         A
Fatma           2      Fatma   56 165         B
Ali             3        Ali   75 177         C
Mehmet          4     Mehmet   89 196         A
Zeynep          5     Zeynep   60 169         A
```

Kesitimize ulaştıktan sonra atama operatörü ile yeni değerini verebiliriz. Ayrıca yeni halinide endeksleme ile yeni bir değişkene atayabiliriz. Eğer listedeki elemanların isimleri olsaydı endeksleme yerine isimler ile de atama yapabilirdik.
