# Lesson 3

## Başlıklar:
* [Giriş](#giriş)
* [Bir veriseti nasıl yüklenir?](#veri-manipülasiyonu)
* [dyplr paketi](#dyplr-fonksiyonları)
* [select()](#select-fonskyionu)
* [filter()](#filter-fonskiyonu)
* [Pipe %>% operatörü](#pipe--fonksiyonu-operatörü)
* [mutate()](#mutate-fonksiyonu)
* [arrange()](#arrange-fonksiyonu)
* [transmute()](#transmute-fonksiyonu)
* [summarise()](#summarise-fonksiyonu)
* [group_by()](#group_by-fonksiyonu)
* [count()](#count-fonksiyonu)
* [top_n()](#top_n-fonksiyonu)
* [genel egzersiz](#genel-egzersiz)
* [tidyr paketi](#tidyr-paketi)
* [gather()](#gather-fonksiyonu)
* [spread()](#spread-fonksiyonu)
* [unite()](#unite-fonksiyonu)
* [Veri setindeki eksik, aykırı gözlemler](#Veri-setindeki-eksik-aykırı-gözlemler)
* [dplyr ile join işlemleri](#dplyr-ile-join-işlemleri)
* [Vaka Çalışması](#Flights-veri-seti-ile-vaka-çalışması)



## Giriş
Bugüne kadar R objelerini nasıl tanımlayabildiğimizi, fonksiyonların ne olduğunu ve nasıl kullanabildiğimizi öğrendik. Kendi slot makinemizi de yazabildik! kolay bir iş değildi elbette.
Böylece R okur yazarlığını büyük bir kısmını öğrenmiş olduk ve artık veri analitiği kısmına yavaşça başlıyor olacağız.

Verisetlerin ne olduğunu biliyor, üzerine basit işlemler gerçekleştirir, soru sorup cevabını mantıksal operatörler kullanarak bulabiliyoruz.
Bugün ise bu işlemleri hızlı bir şekilde yapacağız. R'ın içinde tanımlı olan paketleri kullanarak verimli ve etkin bir şekile veri analitiği ihtiyaçlarımızı çözüyor olacağız.
Haftaya ise görselleştirmeye geçeceğiz, böylece temel R okur yazarlığını bitirmiş olacağız. Sonrasından Veri bilimin temel yöntemlerini R kullanarak yapıyor olacağız.

## Veri Manipülasiyonu
Bu kısımda veriyi içeri aktarma, düzenleme, temizleme, filtreleyerek kesit alma, özet istatistikler çıkartma ve veri setimizle ilgili sorabileceğimiz sorulara cevap vermemizi sağlayacak birtakım araçlar kullanacağız. Bunu ise R'da genel olarak kullanılan "dplyr" paketi fonksiyonları ile yapacağız.
İlk başta "dpylr" kutuphanesini indirelim
```R
install.packages("dplyr")
```

R studio'da bir fonksiyonu indirdiğinizde bir daha indirmenize gerek yok, bilgisayarınızın içinde kalır. Yalnızca kullanmak isterseniz R studio'yu her açtığında çağırmanız lazım .
çağıralım,
```R
library(dplyr)
```

**Soru:** Fonkiyonu indirdim ama bana uyarı geliyor 

**Cevap:** Hatalar (errors) ve uayrılar (warnings) arasındaki farkı bilmemiz lazım,  warning'ler hata olmak zorunda değiller. R studio bize diyor ki "ben böyle böyle yaptım haberin olsun", yani kodu çalıştırdı ama emin olamadığı kısmı bize bilgilendiriyor. 

**Soru:** Ben fonksiyonu çağırdım ama hiç bir şey gelmedi ekrana 

**Cevap:** Ekrana hiç bir şey gelmemek iyidir. bir hata almadığımız zaman kutuphane doğru bir şekilde çağırıldı demek

Kullanacağımız bazı "dplyr" fonksiyonları aşağıdaki gibidir:
filter() ---> Veriyi belli bir kurala göre filtreleyerek veri setinden kesit almamızı sağlar
select() ---> İstediğimiz değişkenleri seçmemizi sağlar
mutate() ---> Veri setimize yeni bir değişken eklememizi sağlar
arrange() ----> Veriyi bir değişkene göre sıralamamızı sağlar
summarise() ---> Veri setinden özet istatistikler çıkarmamızı sağlar

## Bir veriseti nasıl yüklenir ?
Bu bölümü anlatırken msleep (Mammals Sleep) veri setini kullanacağız. Bu veri setini 3 farklı yoldan içeri aktaracağız:

1. CADS@TEDU GitHub [Hesabından](https:\\raw.githubusercontent.com\cads-tedu\DSPG\master\Veri%20Manip%C3%BClasyonu\msleep.csv):

İnternet üzerinden bir veri seti indirmek için "RCurl" kutuphanesinin getURL() fonskiyonunu kullanacağız,
```R
install.packages("RCurl")
library(RCurl)
url <- getURL("https://raw.githubusercontent.com/cads-tedu/DSPG/master/Veri%20Manip%C3%BClasyonu/msleep.csv")
```
**NOT:** link'deki yer alan '\\' (backslash) sembölleri '/' (slash)  ile değiştirmemiz lazım. Aksi halde linki doğru şekilde okuyamaz

```R
msleep <- read.csv(text=url)
head(msleep)
>                         name      genus  vore        order conservation sleep_total sleep_rem sleep_cycle awake brainwt  bodywt
1                    Cheetah   Acinonyx carni    Carnivora           lc        12.1        NA          NA  11.9      NA  50.000
2                 Owl monkey      Aotus  omni     Primates         <NA>        17.0       1.8          NA   7.0 0.01550   0.480
3            Mountain beaver Aplodontia herbi     Rodentia           nt        14.4       2.4          NA   9.6      NA   1.350
4 Greater short-tailed shrew    Blarina  omni Soricomorpha           lc        14.9       2.3   0.1333333   9.1 0.00029   0.019
5                        Cow        Bos herbi Artiodactyla domesticated         4.0       0.7   0.6666667  20.0 0.42300 600.000
6           Three-toed sloth   Bradypus herbi       Pilosa         <NA>        14.4       2.2   0.7666667   9.6      NA   3.850
```
**soru:** CSV nedir ve neden böyle bir şeye ihtiyacımız var?

**cevap:** CSV dosyası comma seperated values demek, excel ve diğer çok şey verileri bu şekilde tutuyor. En az yer kaplayan yöntemlerden bir tanesi. Çok büyük verietin varsa CSV şeklinde tutmayı tercih edersin. Herhangi bir csv dosyası excelde açabilirsin. 

2. Bilgisayardan  

Eğer csv dosyası sizin bilgisayarınızda bulunuyorsa yine read.csv() kullanarak elde edebiliriz
```R
msleep <- read.csv("C:/Users/X/Desktop/msleep.csv") 
head(msleep)

>                         name      genus  vore        order conservation sleep_total sleep_rem sleep_cycle awake brainwt
1                    Cheetah   Acinonyx carni    Carnivora           lc        12.1        NA          NA  11.9      NA
2                 Owl monkey      Aotus  omni     Primates         <NA>        17.0       1.8          NA   7.0 0.01550
3            Mountain beaver Aplodontia herbi     Rodentia           nt        14.4       2.4          NA   9.6      NA
4 Greater short-tailed shrew    Blarina  omni Soricomorpha           lc        14.9       2.3   0.1333333   9.1 0.00029
5                        Cow        Bos herbi Artiodactyla domesticated         4.0       0.7   0.6666667  20.0 0.42300
6           Three-toed sloth   Bradypus herbi       Pilosa         <NA>        14.4       2.2   0.7666667   9.6      NA
   bodywt
1  50.000
2   0.480
3   1.350
4   0.019
5 600.000
6   3.850  
```
3. Excel'den

Eğer dosya excel formatındaysa "readxl" paketinden read_excel() fonksiyonu ile veriyi içeri aktarabiliriz.

 Excel dosyası sheetlerden oluşur. dosyayı açarsak birinci ve ikinci sheetlerin boş olduğunu görebiliriz. bizim istediğimiz veriler "msleep" isimli sheette yer aldığını görüyorüz. Hangi sheetten veri çekmek istediğimizi read_excel fonksiyonuna "sheet" parametresiyle söyleyebiliriz.

```R
install.packages("readxl")
library(readxl)
msleep_1 <- read_excel("C:/X/Undefined/Desktop/msleep_1.xlsx",sheet="msleep")
head(msleep_1)

>                          name      genus  vore        order conservation sleep_total sleep_rem sleep_cycle awake brainwt
1                    Cheetah   Acinonyx carni    Carnivora           lc        12.1        NA          NA  11.9      NA
2                 Owl monkey      Aotus  omni     Primates         <NA>        17.0       1.8          NA   7.0 0.01550
3            Mountain beaver Aplodontia herbi     Rodentia           nt        14.4       2.4          NA   9.6      NA
4 Greater short-tailed shrew    Blarina  omni Soricomorpha           lc        14.9       2.3   0.1333333   9.1 0.00029
5                        Cow        Bos herbi Artiodactyla domesticated         4.0       0.7   0.6666667  20.0 0.42300
6           Three-toed sloth   Bradypus herbi       Pilosa         <NA>        14.4       2.2   0.7666667   9.6      NA
   bodywt
1  50.000
2   0.480
3   1.350
4   0.019
5 600.000
6   3.850
```
Şimdi veri setimizin structure'ına be summery'ne bakalım,
```R
str(msleep)
> 'data.frame':	83 obs. of  11 variables:
    $ name        : Factor w/ 83 levels "African elephant",..: 12 57 52 36 17 77 55 81 21 67 ...
    $ genus       : Factor w/ 77 levels "Acinonyx","Aotus",..: 1 2 3 4 5 6 7 8 9 10 ...
    $ vore        : Factor w/ 4 levels "carni","herbi",..: 1 4 2 4 2 2 1 NA 1 2 ...
    $ order       : Factor w/ 19 levels "Afrosoricida",..: 3 15 17 19 2 14 3 17 3 2 ...
    $ conservation: Factor w/ 6 levels "cd","domesticated",..: 4 NA 5 4 2 NA 6 NA 2 4 ...
    $ sleep_total : num  12.1 17 14.4 14.9 4 14.4 8.7 7 10.1 3 ...
    $ sleep_rem   : num  NA 1.8 2.4 2.3 0.7 2.2 1.4 NA 2.9 NA ...
    $ sleep_cycle : num  NA NA NA 0.133 0.667 ...
    $ awake       : num  11.9 7 9.6 9.1 20 9.6 15.3 17 13.9 21 ...
    $ brainwt     : num  NA 0.0155 NA 0.00029 0.423 NA NA NA 0.07 0.0982 ...
    $ bodywt      : num  50 0.48 1.35 0.019 600 ...


summary()
>                         name             genus         vore             order          conservation  sleep_total      sleep_rem      sleep_cycle    
 African elephant         : 1   Panthera    : 3   carni  :19   Rodentia    :22   cd          : 2    Min.   : 1.90   Min.   :0.100   Min.   :0.1167  
 African giant pouched rat: 1   Spermophilus: 3   herbi  :32   Carnivora   :12   domesticated:10    1st Qu.: 7.85   1st Qu.:0.900   1st Qu.:0.1833  
 African striped mouse    : 1   Equus       : 2   insecti: 5   Primates    :12   en          : 4    Median :10.10   Median :1.500   Median :0.3333  
 Arctic fox               : 1   Vulpes      : 2   omni   :20   Artiodactyla: 6   lc          :27    Mean   :10.43   Mean   :1.875   Mean   :0.4396  
 Arctic ground squirrel   : 1   Acinonyx    : 1   NA's   : 7   Soricomorpha: 5   nt          : 4    3rd Qu.:13.75   3rd Qu.:2.400   3rd Qu.:0.5792  
 Asian elephant           : 1   Aotus       : 1                Cetacea     : 3   vu          : 7    Max.   :19.90   Max.   :6.600   Max.   :1.5000  
 (Other)                  :77   (Other)     :71                (Other)     :23   NA's        :29                    NA's   :22      NA's   :51      
     awake          brainwt            bodywt        
 Min.   : 4.10   Min.   :0.00014   Min.   :   0.005  
 1st Qu.:10.25   1st Qu.:0.00290   1st Qu.:   0.174  
 Median :13.90   Median :0.01240   Median :   1.670  
 Mean   :13.57   Mean   :0.28158   Mean   : 166.136  
 3rd Qu.:16.15   3rd Qu.:0.12550   3rd Qu.:  41.750  
 Max.   :22.10   Max.   :5.71200   Max.   :6654.000  
```

**NOT:** Dataframe'i daha düzgün bir şekilde görmek istiyorsak R studio'nun Environment kısmındaki değişkene tıklayarak düzgün halini görebiliriz.

<img src=".images/msleep.JPG" width="630">

**soru:** istatistiklere neden bakıyoruz ?

**cevap:** verisetiyi ilk yüklediğinde yaptığın ilk şey onu incelemektir. İçinde ne var ne yok bakıyorsun. Aklında bazı sorular oluşabilir bu aşamada. 

**NOT:** read_csv() ve read_excel() fonksiyonlari Dataframe tipindendir.

Değişkenlerin açıklamalarına bakalım, 

name: isimler

genus: memelinin cinsi

vore: karnivor, omnivor, herbivor

order: taksonomik sıralama

conservation: memelilerin korunma durumu

sleep_total: saat cinsinden toplam uyku süresi

sleep_rem: saat cinsinden rem uykusu süresi

sleep_cycle: saat cinsinden uyku döngüsü süresi

awake: saat cinsinden uyanık kalınan süre

brainwt: kilogram cinsinden beyin ağırlığı

bodywt: kilogram cinsinden vücut ağırlığı

## dyplr fonksiyonları

### select() fonskyionu

Yapacağımız ilk işlem verimizden bir seçme yapmaktır.
select fonksiyonları, içindeki yardımcı fonksiyonlar ile değişkenleri istediğimiz gibi seçip, sıralayabiliriz.
Tanımına bakaım,
```R
?select()
```
İlk argümanı olarak bizim datamız, ardından verilerimizin isimlerini girmemizi sölyüyor.

Örneğin, eğer "vore" ve "brainwt" değişkenlerini görmek istersek
```R
select(msleep, vore, brainwt)
>       vore brainwt
1    carni      NA
2     omni 0.01550
3    herbi      NA
4     omni 0.00029
5    herbi 0.42300
6    herbi      NA
7    carni      NA
8     <NA>      NA
9    carni 0.07000
10   herbi 0.09820
11   herbi 0.11500
        .
        .
72   herbi      NA
73    <NA> 0.00033
74    omni 0.18000
75 insecti 0.02500
76   herbi      NA
77   herbi 0.16900
78    omni 0.00260
79    omni 0.00250
80   carni      NA
81   carni 0.01750
82   carni 0.04450
83   carni 0.05040
```
Gördüğümüz gibi sadece "vore" ve "brainwt" değişkenlerini içeren bir Dataframe döndürdü bize!
```R
class(select(msleep, vore, brainwt))
> "data.frame"
```

select fonksiyonun içinde yardımıcı fonksiyonlar kullanabiliriz. Belli bir özelliklere sahip kolonları seçmek istediğimizde yardımcı olur. Bunlardan birkaç tanesi aşağıdaki gibidir:

* starts_with(): belirlenen bir karakterle başlayan
* ends_with(): belirli bir karakterle biten
* contains(): içinde belli bir karakter geçen
* matches(): regex karakterlerle eşleşen
* num_range(): belli bir numerik aralıktataki değişkenleri çeken
* one_of(): bir karakter vektöründeki karakterlerle eşleşen
* everything(): tüm değişkenleri getirir
* last_col(): son değişkeni getirir

Örneğin: İçinde "or" karakterleri içeren değişkenleri getirelim.
(contains() fonksiyonu yardımıyla yapabiliriz )
```R
select(msleep, contains("or"))

>     vore           order
1    carni       Carnivora
2     omni        Primates
3    herbi        Rodentia
4     omni    Soricomorpha
5    herbi    Artiodactyla
        .
        .

81   carni       Carnivora
82   carni       Carnivora
83   carni       Carnivora
```

"sleep" ile başlayan değişkenleri seçelim, 
```R
select(msleep, starts_with(("sleep")))

>   sleep_total sleep_rem sleep_cycle
1         12.1        NA          NA
2         17.0       1.8          NA
3         14.4       2.4          NA
4         14.9       2.3   0.1333333
5          4.0       0.7   0.6666667
            .
            .
81         6.3       1.3          NA
82        12.5        NA          NA
83         9.8       2.4   0.3500000
```

>**Egzersiz:** "sleep" ile başlayan değişkenleri seçtikten sonra diğer değişkenleri bu değişkenlerden sonra ekleyelim. ve "sleep" adlı bir değişkene atalım.

bunu nasıl yapabiliriz ? everything() fonksiyonunu kullanarak bu işlemi gerçekleştirebiliriz.
Birden fazla select işlemi yapmak istiyorsak virgülle ayırarak yapabiliriz.

```R
sleep <- select(msleep, starts_with(("sleep")) , everything())
sleep
```
<img src=".images/sleep.JPG" width=630>


Belli bir aralıktaki değişkenleri seçmek istersek, örneğin: "genus"tan "conservation" a kadar olan değişkenleri seçmek istersek şu şekilde gerçekleştirebiliriz

```R
select(msleep, genus:conservation)

>           genus    vore           order conservation
1       Acinonyx   carni       Carnivora           lc
2          Aotus    omni        Primates         <NA>
3     Aplodontia   herbi        Rodentia           nt
4        Blarina    omni    Soricomorpha           lc
5            Bos   herbi    Artiodactyla domesticated
                    .
                    .
81       Genetta   carni       Carnivora         <NA>
82        Vulpes   carni       Carnivora         <NA>
83        Vulpes   carni       Carnivora         <NA>

# 2. yol indeksleme kullanarak : 
#select(msleep, 2:5) 
```

> **Egzersiz:** Sonu "wt" karakteriyle biten değişkenleri getirip bu değişkenlerin ilk 10 satırına bakalım.
```R
wt <- select(msleep, ends_with("wt"))
head(wt,10)

>    brainwt  bodywt
1       NA  50.000
2  0.01550   0.480
3       NA   1.350
4  0.00029   0.019
5  0.42300 600.000
6       NA   3.850
7       NA  20.490
8       NA   0.045
9  0.07000  14.000
10 0.09820  14.800
```

### filter() fonskiyonu
filter() fonksiyonu veri setindeki satırları/gözlemleri istenilen şekilde filtrelemek için kullanılır. 
Ayrıca, sütunlar/değişkenleri ayıklamak için kullanılan select() fonksiyonunun
satırlar/gözlemler için kullanılan halidir diyebiliriz. 

filter() ile sadece seçtiğimiz gözlemlerden oluşan yeni bir data frame oluşturmuş oluruz.

filter() fonksiyonu veri setindeki değişken ismi ile beraber bizden bir mantıksal test ifadesi ister. 

filter() fonksiyonundan tam anlamıyla yararlanmak için eşit “==”, eşit değil “!=”, büyük “>”, küçük “<”, büyük eşit “>=” vb. mantıksal operatörlerin iyi bilinmesi gerekir. Daha detaylı bakmak için ?Comparison yapabiliriz yazıp mantıksal operatörleri hatırlayabiliriz!
```R
?Comparison
```
<img src=".images/Comparison.JPG" width=500>

Örnek: Günlük uyku süresi 16 saatten fazla olan hayvanları hangileridir?

```R
filter(msleep, sleep_total > 16)
```
<img src=".images/msleep1.JPG">

Gördüğümüz gibi, uyku süresi 16 saatten fazla olan 8 tane hayvan varmış.

Birden fazle filter yapmak istersek yine virgül koyarak filtrelerimizi yapabiliriz.

> **Egzersiz:** Uyanık kalma süreleri ortalama uyanık kalma süresinden büyük eşit olan hayvanlar hangileridir?
```R
filter(msleep, awake >= mean(awake))
```
<img src=".images/msleep2.JPG">

> **Egzersiz:** Hem toplam uyku süresi 16 saati geçen hem de vücut ağırlığı 1 kilogramin üzerinde olan memelileri filtreleyip çıktıyı inceleyelim
```R
filter(msleep, sleep_total > 16, bodywt>1)

#2.yol : filter(msleep, sleep_total > 16 & bodywt>1)
```

<img src=".images/msleep3.JPG">

>**Egzersiz:** "Perissodactyla" ve "Cingulata" taksonomik sırasına göre memelileri filreleyelim
Burada "Perissodactyla" **ve** "Cingulata" dediği için **&** operatörünü kullanıp böyle bir hataya düşebiliriz
```R
filter(msleep, order == "Perissodactyla" & order=="Cingulata")
```

Yalnızca bu hayvan aynı zamanda hem "Perissodactyla" hem de "Cingulata" olamlı anlamına geliyor. Dolayısıyla **veya** operatörü kullanmamız lazım.

```R
filter(msleep, order == "Perissodactyla" | order=="Cingulata")

#2.yol: filter(msleep, order %in% c("Perissodactyla" , "Cingulata")) 
```
<img src=".images/msleep4.JPG">

> **Egzersiz:** Beslenme şekli omnivor("omni") olanları seçip ilk altı satırına bakalım.
```R
head(filter(msleep,vore=="omni"))
```
<img src=".images/msleep5.JPG">

> **Egzersiz:** Vücut ağırlığı en büyük olan hayvan hangisidir? 

```R
max(msleep$bodywt)
> 6654
```
6654 kilogrammış, acaa bir fil olabilir mi ? hemen bakalım,
```R
filter(msleep, bodywt == max(bodywt))
```
<img src=".images/msleep6.JPG">
Tahmin ettiğimiz gibi bir filmiş.

### Pipe %>% Fonksiyonu (operatörü)
Türkçe karşılığı olarak zincir fonksiyonu olarak da isimlendirilir. 
Bu fonksiyon soldakinin çıktısını, sağdakine girdi olarak veriyor.

Bu şekilde birden fazla işlem dizinini ifade edebilirsiniz. 
Kodunuzu basitlesteştirmenizi ve işlemlerinizi daha açık bir şekilde görebilmenizi sağlar.  

Pipe fonksiyonu çokça kullanacağız, kodun okunabilirliği açısından da oldukça faydalı bir operatördür.

Nasıl yazıldığına bir bakalım
```R
msleep %>%
    select(name, sleep_total)
```
Böylece "name" ve "sleep_total" değişkenleri seçmiş olduk. Bu elde ettiğimiz dataframe'in ilk altı satırına bakmak istersek arkasına head() fonksiyonu yapıştırırız.
```R
msleep %>%
    select(name, sleep_total) %>%
    head()

>                        name sleep_total
1                    Cheetah        12.1
2                 Owl monkey        17.0
3            Mountain beaver        14.4
4 Greater short-tailed shrew        14.9
5                        Cow         4.0
6           Three-toed sloth        14.4
```

*Eren Hoca: Pipe kullanarak 6-7 satırlık işlemler yazacağız. Size tavsiyem, elde etmek istediğiniz yere kadar satırları işaretleyip çalıştırmanızdır. Böylece kafanız karışmaz*

**Egzersiz:** İsim (name), toplam uyku süresi (sleep_total) ve korunma durumlarını (conservation) seçelim ve korunma durumu “domesticated” olanları filtreleyelim. İlk altı satırına bakalım!

```R
msleep %>%
  select(name, sleep_total, conservation) %>%
  filter(conservation == "domesticated") %>%
  head()

>        name sleep_total conservation
1        Cow         4.0 domesticated
2        Dog        10.1 domesticated
3 Guinea pig         9.4 domesticated
4 Chinchilla        12.5 domesticated
5      Horse         2.9 domesticated
6     Donkey         3.1 domesticated
```

## arrange() fonksiyonu
arrange() fonksiyonu satırları/gözlemleri içeriklerine göre sıralamak için kullanılır. 
Böylece veri setini istenilen kriterde sıralayabilirsiniz.

Örneğin: msleep dataframe'inden vücüt ağırlıklarını küçükten büyüğe sıralayalım
```R
msleep %>%
  arrange(bodywt)
```
<img src=".images/msleep7.JPG">

Gördüğümüz gibi gözlemleri "bodywt" kolonuna göre küçükten büyüğe sıralamış olduk.
 Eğer sıralamayı büyükten küçüğe yapmak istersek arrange() fonksiyonun içine desc() fonksiyonu yerleştirerek elde edebiliriz.
 ```R
msleep %>%
  arrange(desc(bodywt))
 ```
<img src=".images/msleep8.JPG">

name kolonunu 'a' dan 'z' ye sıralamak istersek,
```R
msleep %>%
  arrange(name)
```

> **Egzersiz:** Veri setimizden name, genus, vore ve brainwt değişkenleri seçip. Beyin ağırlığı 1'den büyük olanları filtreledikten sonra beyin ağırlığına göre büyükten küçüğe sıralatalım.
```R
msleep %>%
  select(name,genus,vore,brainwt) %>%
  filter(brainwt > 1) %>%
  arrange(desc(brainwt))

>              name     genus  vore brainwt
1 African elephant Loxodonta herbi   5.712
2   Asian elephant   Elephas herbi   4.603
3            Human      Homo  omni   1.320
```

Maksimum beyin ağırlığına sahip canlıyı bulmak isteseydik nasıl yapardık?

sadece bir pipe daha ekleyerek bunu elde edebiliriz,
```R
msleep %>%
  select(name,genus,vore,brainwt) %>%
  filter(brainwt > 1) %>%
  arrange(desc(brainwt)) %>%
  head(1)

>              name     genus  vore brainwt
1 African elephant Loxodonta herbi   5.712


# 2.yol : 
msleep %>%
  select(name,genus,vore,brainwt) %>%
  filter(brainwt > 1) %>%
  arrange(desc(brainwt)) %>%
  filter(brainwr == max(brainwt))
```

## mutate() fonksiyonu
#Mutate fonksiyonu ile veri setimize yeni sütunlar/değişkenler ekleyebiliriz.

Rem uykusunun toplam uyku miktarına oranını bulalım ve rem_proportion adlı yeni bir sütuna atayalım. 

```R
msleep <- msleep %>%
  mutate(rem_proportion = sleep_rem / sleep_total)

msleep
```
<img src=".images/msleep9.JPG">


```R
#2.yol: geçen hafta görmüştük
msleep$rem_proportion <- msleep$sleep_rem / msleep$sleep_total
```

mutate() fonksiyonunun bir başka özelliği ise ihtiyaç duyulduğunda aynı anda aynı kodun içerisinde birden fazla yeni değişken üretebilmesidir.

İlk örneğe ilaveten, gram cinsinde bodywt sütunu olacak şekilde bodywt_grams adli ikinci bir değer yaratalım.
```R
msleep %>%
  mutate(rem_proportion = sleep_rem / sleep_total,
         bodywt_gram = bodywt * 1000)
```
<img src=".images/msleep10.JPG">

> **Egzersiz:** msleep veri setine brainwt_grams isimli brainwt değişkeninin 1000'le çarpılmış halini ekleyelim.
Ayrıca, sleep_cycle_ratio isimli sleep_cycle'ı 100 ile çarparak yüzde oran belirtecek şekilde ekleyelim.
Bu işlemlerden sonra msleep_yeni isimli veri setine atayalım.
```R
msleep_yeni <- msleep %>%
  mutate(bodywt_gram = bodywt * 1000,
         sleep_cycle_ratio = sleep_cycle * 100)
msleep_yeni
```
<img src=".images/msleep11.JPG">

mutate() kullanarak numerik değişkenleri kategorikleştirme işlemleri de gerçekleştirebiliriz. Bunun birkaç yöntemi var, onları görmeden size ifelse() fonksiyonundan bahsetmek istiyorum.

ifelse() fonksiyonunun tanımına bir bakalım,
```R
?ifelse() 
```
<img src=".images/ifelse().JPG" width=500>

Burada bir sınama yapıyoruz(ilk argüman), ve bu sınama doğru çıktıysa ikinci argümanı, yanlış çıktıysa üçüncü argümanı döndürecek bize. örneğin,
```R
ifelse(5 > 4, "doğru", "yanlış")

> "doğru"
```
Peki numerik değişkenleri kategorik değerlere dönüştüme işlemine dönelim, bunun iki yöntemle yapabiliriz;

1. ifelse() yöntemi:

```R
msleep$miskinlik <- ifelse(msleep$sleep_total < 16, "Miskin", "Miskin_Degil")
msleep
```
<img src=".images/msleep12.JPG" >
Yani yeni oluşturduğumuz "mikinlik" kolonuna uyku süresi 16 küçük ise "Miskin", değilse "Miskin_değil" değerleri atayacağız.


2. case_when() yöntemi: 
Diğer bir yöntem ise case_when() yöntemi, bu yöntemde birden fazla sınama yapabiliriz. kullanim şekli şu şekilde,

case_when(ilk sınama ~ sonuç1, 
         ikinci sınama ~ sonuç2,
         üçüncü sınama ~ sonuç3 ) 
```R
msleep %>%
  mutate(miskinlik = case_when(sleep_total > 16 ~ "Miskin",
                               sleep_total < 16 ~ "Miskin_Degil"))

msleep %>%
  mutate(uyanıklık = case_when(awake > 4 & awake < 10 ~ "Az_uyanik",
                               awake > 10 & awake < 13 ~ "Ortalama_uyanik",
                               awake > 13 & awake < 16 ~ "Daha_uyanik",
                               awake > 16 ~ "Cok_uyank"))

```

<img src=".images/msleep13.JPG" >

## summarise() fonksiyonu
mutate() fonksiyonu ile dataframe'in sonuna yeni değişkenler ekleyebiliyorduk. Sadece yeni değişkenleri elde etmek istiyorsak transmute() fonksiyonunu kullanabiliriz.

```R
msleep %>% 
  transmute(brainwt_grams = brainwt * 1000,
            sleep_cycle_ratio = sleep_cycle * 1000)

>   brainwt_grams sleep_cycle_ratio
1             NA                NA
2          15.50                NA
3             NA                NA
4           0.29          133.3333
5         423.00          666.6667
```

Gördüğümüz gibi sadece yeni oluşturduğumuz değişkenleri içeren bir dataframe elde ettik.

## summarise() fonksiyonu
Bu fonksiyonla, istatistiksel bir özet olusturulur.

Değişkenler için mod, maksimum, minumum, ortalama, standart sapma vb. hesaplar yapmamızı sağlar.

Canlıların ortalama uyku sürelerini summarise fonksiyonu ile bulalım.
```R
msleep %>%
  summarise(ortalama_uyku = mean(sleep_total))

>   ortalama_uyku
1      10.43373
```
Summarize fonksiyonu içerisinde kullanilabilecek diğer istatistiksel fonksiyonlar vardır. 

Bunlar; sd(), min(), max(), median(), sum(), n(), first(), last() ve n_distinct() vb. fonksiyonlardır.

> **Egzersiz:** Ortalama rem uyku süresini(avg,_rem), en düşük rem uyku süresini (min_rem) ve en yüksek rem uyku sürelerini (max_rem) 
hesaplıyalım ve sırasıyla bu değerlere atayalım. 
İlk altı satıra bakalım.

İpucu: sleep_rem değişkeninde kayıp veriler (NA) olduğundan "mean(), min(), max()" fonksiyonu argümanlarından na.rm=TRUE yapmamız gerekiyor (na.rm gondermedigimiz zaman NA döndürür).

```R
msleep %>% 
  summarise(avg_rem = mean(sleep_rem, na.rm=T), 
            min_rem = min(sleep_rem, na.rm = T),
            max_rem = max(sleep_rem, na.rm= T))

>   avg_rem min_rem max_rem
1 1.87541     0.1     6.6
```

> **Egzersiz:** Canlıların kaç farklı yemek türü yediğini n_distinct() fonksiyonu ile bulup değişkenin ismini çeşit koyalım.
```R
msleep %>%
  summarise(çeşit = n_distinct(vore))

>   çesit
1     5
```

> **Egzersiz:** Canlıların rem uykusunda kalma sürelerinin oranını değişken olarak sleep_rem_ratio ismiyle ekledikten sonra rem uykusunda kalma sürelerinin oranlarının ortalamasını bulalım.
```R
msleep %>% 
  mutate(sleep_rem_ratio = sleep_rem/ sleep_total) %>%
    summarise(avg_ratio = mean(sleep_rem_ratio , na.rm=T))

>   avg_ratio
1 0.1740226
```

## group_by() fonksiyonu
Bu fonksiyon ile istediginiz bir kolona göre gruplama işlemi yapabiliriz. Böylece o grup içerisindeki özet istatistikleri elde edebiliriz.

Örnek üzerine yapalım daha anlaşılır olur. msleep veri setini taksonomik sırasına(order) göre gruplayalım ve yukarıda yaptığımız gibi taksonomik sırasına göre gruplanmış canlıların toplam uyku sürelerinin özet istatistiklerini elde edelim. (n,min, max, sd, mean)
```R
msleep %>%
  group_by(order) %>%
  summarise(toplam = n(),
            ort_sleep = mean(sleep_total),
            min_sleep = min(sleep_total),
            max_sleep = max(sleep_total),
            sd_sleep = sd(sleep_total))

>    order           toplam ort_sleep min_sleep max_sleep sd_sleep
   <fct>            <int>     <dbl>     <dbl>     <dbl>    <dbl>
 1 Afrosoricida         1     15.6       15.6      15.6  NaN    
 2 Artiodactyla         6      4.52       1.9       9.1    2.51 
 3 Carnivora           12     10.1        3.5      15.8    3.50 
 4 Cetacea              3      4.5        2.7       5.6    1.57 
 5 Chiroptera           2     19.8       19.7      19.9    0.141
 6 Cingulata            2     17.8       17.4      18.1    0.495
 7 Didelphimorphia      2     18.7       18        19.4    0.990
 8 Diprotodontia        2     12.4       11.1      13.7    1.84 
 9 Erinaceomorpha       2     10.2       10.1      10.3    0.141
10 Hyracoidea           3      5.67       5.3       6.3    0.551
11 Lagomorpha           1      8.4        8.4       8.4  NaN    
12 Monotremata          1      8.6        8.6       8.6  NaN    
13 Perissodactyla       3      3.47       2.9       4.4    0.814
14 Pilosa               1     14.4       14.4      14.4  NaN    
15 Primates            12     10.5        8        17      2.21 
16 Proboscidea          2      3.6        3.3       3.9    0.424
17 Rodentia            22     12.5        7        16.6    2.81 
18 Scandentia           1      8.9        8.9       8.9  NaN    
19 Soricomorpha         5     11.1        8.4      14.9    2.70 
```

Gördüğümüz gibi, 19 tane taksonomik sıralamamız var. toplam değişkenine atadığımız n() fonskiyonu gözlemlerin sayısını temsil ediyor. verisetimizde 1 tane "Afrosoricida" hayvanı var, onun ortalama uyku süresi 15.6, standart sapması tek bir gözlem olduğu için hesaplayamadık (NAN döndürdü). 

> **Egzersiz:** Canlıları yemek şekillerine (vore) grupladıktan sonra beyin ağırlıklarının özet istatistiklerini çıkaralım. Sonrasında ise ortalama beyin ağırlıklarını büyükten küçüğe sıralayalım.

```R
msleep %>%
  group_by(vore) %>%
  summarise(toplam = n(),
            ort_brainwt = mean(brainwt ,na.rm = T),
            min_brainwt = min(brainwt,na.rm = T),
            max_brainwt= max(brainwt,na.rm = T),
            sd_brainwt = sd(brainwt,na.rm = T)) %>%
  arrange(desc(ort_brainwt))

>  vore    toplam ort_brainwt min_brainwt max_brainwt sd_brainwt
  <fct>    <int>       <dbl>       <dbl>       <dbl>      <dbl>
1 herbi       32     0.622      0.0004         5.71     1.57   
2 omni        20     0.146      0.000140       1.32     0.325  
3 carni       19     0.0793     0.0108         0.325    0.103  
4 insecti      5     0.0216     0.00025        0.081    0.0349 
5 NA           7     0.00763    0.00033        0.021    0.00859
```
## count() fonksiyonu
Kategorik değişkenleri, group_by() ve n() fonksiyonlarını kullanmadan count() fonksiyonu ile sayabilmemizi sağlar. Yani adı üzerine, sayıyor!

Hangi yemek tercihinden kaç tane var? (vore)
```R
msleep %>% 
  count(vore)
>  vore        n
  <fct>   <int>
1 carni      19
2 herbi      32
3 insecti     5
4 omni       20
5 NA          7

#sıralamak istiyorsak sort argümanı TRUE yaparız
msleep %>% 
  count(vore, sort=T)
>  vore        n
  <fct>   <int>
1 herbi      32
2 omni       20
3 carni      19
4 NA          7
5 insecti     5
```

bu neye denk? (neyle yapabiliyorduk)
```R
msleep %>% 
  group_by(vore) %>%
  summarise(n())
```
Peki ya hem yemek tercihlerine göre(vore) hem de korunma sayılarına(conservation) canlıların sayılarını görmek istiyorsak nasıl yapabiliriz? Virgül ile bu işlemi gerçekleştirebiliriz. Yani count(vore,conservation) yazdığımız zaman, bu iki değişkeninin her bir kombinasiyonu için bir satır getirecek.
```R
msleep %>% 
  count(vore, conservation)
>   vore  conservation     n
   <fct> <fct>        <int>
 1 carni cd               1
 2 carni domesticated     2
 3 carni en               1
 4 carni lc               5
 5 carni nt               1
 6 carni vu               4
 7 carni NA               5
 8 herbi cd               1
 9 herbi domesticated     7
10 herbi en               2
# ... with 12 more rows
```
## top_n() fonksiyonu

**top_n()** fonksiyonu belli bir değişkene göre belirlenen bir sayıda en yüksek değerli verileri getirir. Hemen nasıl çalıştığını inceleyelim.

Uyanık kalma süreleri en fazla olan ilk 10 canlıyı getirelim.
```R
msleep %>%
  top_n(awake, n=10)
>
               name         genus  vore          order conservation sleep_total sleep_rem sleep_cycle awake brainwt   bodywt
1               Cow           Bos herbi   Artiodactyla domesticated         4.0       0.7   0.6666667 20.00  0.4230  600.000
2          Roe deer     Capreolus herbi   Artiodactyla           lc         3.0        NA          NA 21.00  0.0982   14.800
3    Asian elephant       Elephas herbi    Proboscidea           en         3.9        NA          NA 20.10  4.6030 2547.000
4             Horse         Equus herbi Perissodactyla domesticated         2.9       0.6   1.0000000 21.10  0.6550  521.000
5            Donkey         Equus herbi Perissodactyla domesticated         3.1       0.4          NA 20.90  0.4190  187.000
6           Giraffe       Giraffa herbi   Artiodactyla           cd         1.9       0.4          NA 22.10      NA  899.995
7       Pilot whale Globicephalus carni        Cetacea           cd         2.7       0.1          NA 21.35      NA  800.000
8  African elephant     Loxodonta herbi    Proboscidea           vu         3.3        NA          NA 20.70  5.7120 6654.000
9             Sheep          Ovis herbi   Artiodactyla domesticated         3.8       0.6          NA 20.20  0.1750   55.500
10     Caspian seal         Phoca carni      Carnivora           vu         3.5       0.4          NA 20.50      NA   86.000
```

Çıktımıza baktığımızda uyanık kalma süreleri en büyük olan ilk 10 canlının tüm özellikleri geldi. Şimdi ise bir egzersiz yapalım.

>**Egzersiz** Beyin ağırlığı en fazla olan ilk 10 canlıyı vücut ağırlığına göre büyükten küçüğe sıralayalım.

```R
msleep %>%
  top_n(brainwt, n=10) %>%
  arrange(desc(bodywt))
>
               name        genus  vore          order conservation sleep_total sleep_rem sleep_cycle awake brainwt   bodywt
1  African elephant    Loxodonta herbi    Proboscidea           vu         3.3        NA          NA  20.7   5.712 6654.000
2    Asian elephant      Elephas herbi    Proboscidea           en         3.9        NA          NA  20.1   4.603 2547.000
3               Cow          Bos herbi   Artiodactyla domesticated         4.0       0.7   0.6666667  20.0   0.423  600.000
4             Horse        Equus herbi Perissodactyla domesticated         2.9       0.6   1.0000000  21.1   0.655  521.000
5            Donkey        Equus herbi Perissodactyla domesticated         3.1       0.4          NA  20.9   0.419  187.000
6               Pig          Sus  omni   Artiodactyla domesticated         9.1       2.4   0.5000000  14.9   0.180   86.250
7         Gray seal Haliochoerus carni      Carnivora           lc         6.2       1.5          NA  17.8   0.325   85.000
8             Human         Homo  omni       Primates         <NA>         8.0       1.9   1.5000000  16.0   1.320   62.000
9        Chimpanzee          Pan  omni       Primates         <NA>         9.7       1.4   1.4166667  14.3   0.440   52.200
10           Baboon        Papio  omni       Primates         <NA>         9.4       1.0   0.6666667  14.6   0.180   25.235
```
Bingo!! Tahmin edilebileceği gibi aramıza Afrika'dan katılan Afrika Filleri yarışmamızın 1.si oluyor. :tada: :tada:

## Genel Egzersiz

Aşağıdaki soruları çözmeye çalışalım.

1. İnsan canlısının özellikleri nedir? ("name" kolonu "Human")

2. Hangi canlı cinsi (genus) en fazla sayıda?

3. Yeme şekillerine göre (vore), "herbi" ve "carni" olanlarla çalışmak istiyoruz. Hangi canlı türü ortalama daha fazla uyuyor? Hangi
canlı türünün ortalama beyin ağırlığı daha fazla? "herbi" olarak beslenen canlılardan hangisi en fazla uyanık kalıyor (awake) ve uyanık
kalma süresi nedir?
**İpucu** Tüm soruyu tek bir pipe operatörü ile yapmak zorunda değiliz.

4. BrBo_ratio isimli bir değişken oluşturalım. Bu değişken bir canlının beyin ağırlığının vücut ağırlığına oranını temsil etsin. Bu
oranın ortalama orandan fazla olanlara "brbo_fazla", ortalama orandan az olanlara "brbo_az" olarak kategorikleştirip bu değişkenin
ismini brbo_kategorik koyduktan sonra, canlılardan kaç tanesinin brbo_fazla kaç tanesi brbo_az olduğunu bulalım.

5. Korunma durumlarına göre (conservation) canlıların vücut ağırlığı özet istatistiklerini çıkaralım.(min, max, mean, median, sd).
Ortalama vücut ağırlıklarına göre büyükten küçüğe doğru sıralattığımızda hangi korunma durumundaki canlıların vücut ağırlığı ortalaması
en yüksektir?

**Çözümler**

1. 
```R
# Burada insan canlısının özellikleri istendiği için filter ile özellikleri getirebiliriz.
msleep %>%
  filter(name == "Human")
>
   name genus vore    order conservation sleep_total sleep_rem sleep_cycle awake brainwt bodywt
1 Human  Homo omni Primates         <NA>           8       1.9         1.5    16    1.32     62
```

2.
```R
# Bizden bir sayma işlemi isteniyor. Genus özelliğimizde kategorik. En kısa yoldan count() işimizi görecektir
#count() fonksiyonunu hatırlarsak sort argümanı ile sıralamayı da büyükten küçüğe ayarlayabiliriz.

msleep %>%
  count(genus, sort=T)
># A tibble: 77 x 2
     genus            n
   <fct>        <int>
 1 Panthera         3
 2 Spermophilus     3
 3 Equus            2
 4 Vulpes           2
 5 Acinonyx         1
 6 Aotus            1
 7 Aplodontia       1
 8 Blarina          1
 9 Bos              1
10 Bradypus         1
# ... with 67 more rows
````

3.
```R
#Sorumuzun ilk kısmı yani carni ve herbiyi beraber inceleyeceğimiz kısım.
msleep %>%
  filter(vore %in% c("herbi", "carni")) %>%
  group_by(genus) %>%
  summarise(ort_uyku = mean(sleep_total),
            ort_beyin_agir= mean(brainwt, na.rm = T)) %>%
  arrange(desc(ort_uyku))

># A tibble: 45 x 3
   genus        ort_uyku ort_beyin_agir
   <fct>           <dbl>          <dbl>
 1 Lutreolina       19.4      NaN      
 2 Dasypus          17.4        0.0108 
 3 Tamias           15.8      NaN      
 4 Spermophilus     15.4        0.00485
 5 Eutamias         14.9      NaN      
 6 Neofiber         14.6      NaN      
 7 Onychomys        14.5      NaN      
 8 Aplodontia       14.4      NaN      
 9 Bradypus         14.4      NaN      
10 Mesocricetus     14.3        0.001  
# ... with 35 more rows

#ikinci kısmımıza bakalım burada da sadece herbi ile ilgileneceğiz.
msleep %>%
  filter(vore == "herbi" & awake == max(awake))

> 
      name   genus  vore        order conservation sleep_total sleep_rem sleep_cycle awake brainwt  bodywt
1 Giraffe Giraffa herbi Artiodactyla           cd         1.9       0.4          NA  22.1      NA 899.995
```

4.
```R
# Yeni bir değişken oluşturacağımız zaman hangi fonksiyonu kullandığımız hatırlayalım
msleep %>%
  mutate(BrBo_ratio = brainwt / bodywt,
         brbo_kategorik = case_when(BrBo_ratio > mean(BrBo_ratio, na.rm=T)~"brbo_fazla",
                                    BrBo_ratio < mean(BrBo_ratio, na.rm = T)~"brbo_az")) %>%
  count(brbo_kategorik) %>%
  filter(!is.na(brbo_kategorik)) %>%
  mutate(oran = n/sum(n)*100)
#filter işleminde is.na fonksiyonunu kullandık. Veri setimizde NA değerler olduğu için filterdan sonraki mutate işleminde is.na #kullanmasaydık değerlerin çoğu NA gelecekti. NA ile bir işlem yaptığımızda sonuç NA gelir bunu hatırlayalım

># A tibble: 2 x 3
  brbo_kategorik     n  oran
  <chr>          <int> <dbl>
1 brbo_az           37  66.1
2 brbo_fazla        19  33.9
```

5.
```R
# Özet istatistiklere bakmak gerektiğinde summarise fonksiyonunu kullandığımızı hatırlayalım.
msleep %>%
  group_by(conservation) %>%
  summarise(min_bodywt = min(bodywt, na.rm = T),
            max_bodywt = max(bodywt, na.rm = T),
            ort_bodywt = mean(bodywt, na.rm = T),
            med_bodywt = median(bodywt, na.rm = T),
            sd_bodywt = sd(bodywt, na.rm = T),
            sıklık = n(),
            uyanıklık_ort = mean(awake, na.rm = T)) %>% 
  arrange(desc(ort_bodywt))

># A tibble: 7 x 8
  conservation min_bodywt max_bodywt ort_bodywt med_bodywt sd_bodywt sıklık uyanıklık_ort
  <fct>             <dbl>      <dbl>      <dbl>      <dbl>     <dbl>  <int>         <dbl>
1 vu                1.67       6654     1026.       86        2483.       7          17.1
2 cd              800           900.     850.      850.         70.7      2          21.7
3 en                0.12       2547      692.      111.       1238.       4          11.0
4 domesticated      0.42        600      147.       34.8       226.      10          16.4
5 nt                0.022       100       25.4       0.808      49.7      4          11.0
6 NA                0.01        173.      11.9       0.9        34.5     29          12.8
7 lc                0.005        85        8.05      0.77       19.1     27          12.6
Warning message:
Factor `conservation` contains implicit NA, consider using `forcats::fct_explicit_na` 
```
## tidyr paketi

Uygulamada veri setleri iki şekilde karşımıza çıkar: Uzun (Long) ve Geniş (Wide) olarak. Uzun veri setleri sırasıyla her bir değişkenin
her birim için değerleri alt alta barındırdığı veri setleridir. Geniş veri setleri ise her bir birim için değişkenlerin ayrı ayrı
sıralandığı veri setleridir. Uygulamada sağladığı kolaylıklar açısından uzun veri setleri geniş veri setlerine tercih edilmektedir.R’de
veri setlerinin geniş veya uzun hale getirilmesi için genellikle tidyr paketi kullanılır.

Kullanacağımız bazı fonksiyonlar:
spread()
gather()
unite().

tidyr paketini indirelim ve çağıralım.
```R
install.packages("tidyr")
library(tidyr)
```

weather isimli veri setimizi direkt olarak "datacamp" sitesinden indirelim ve weather isimli değişkene atayalım.
```R
if(!file.exists("weather.rds")){download.file("https://assets.datacamp.com/production/repositories/34/datasets/b3c1036d9a60a9dfe0f99051d2474a54f76055ea/weather.rds", "weather.rds")
  dateDownloaded <- date()}
weather <- readRDS('weather.rds')
```
Her zamanki gibi elimize veri seti geçtiği zaman yapacağımız ilk iş onun yapısına bakmaktır.
```R
str(weather)

>
'data.frame':	286 obs. of  35 variables:
 $ X      : int  1 2 3 4 5 6 7 8 9 10 ...
 $ year   : int  2014 2014 2014 2014 2014 2014 2014 2014 2014 2014 ...
 $ month  : int  12 12 12 12 12 12 12 12 12 12 ...
 $ measure: chr  "Max.TemperatureF" "Mean.TemperatureF" "Min.TemperatureF" "Max.Dew.PointF" ...
 $ X1     : chr  "64" "52" "39" "46" ...
 $ X2     : chr  "42" "38" "33" "40" ...
 $ X3     : chr  "51" "44" "37" "49" ...
 $ X4     : chr  "43" "37" "30" "24" ...
 $ X5     : chr  "42" "34" "26" "37" ...
 $ X6     : chr  "45" "42" "38" "45" ...
 $ X7     : chr  "38" "30" "21" "36" ...
 $ X8     : chr  "29" "24" "18" "28" ...
 $ X9     : chr  "49" "39" "29" "49" ...
 $ X10    : chr  "48" "43" "38" "45" ...
 $ X11    : chr  "39" "36" "32" "37" ...
 $ X12    : chr  "39" "35" "31" "28" ...
 $ X13    : chr  "42" "37" "32" "28" ...
 $ X14    : chr  "45" "39" "33" "29" ...
 $ X15    : chr  "42" "37" "32" "33" ...
 $ X16    : chr  "44" "40" "35" "42" ...
 $ X17    : chr  "49" "45" "41" "46" ...
 $ X18    : chr  "44" "40" "36" "34" ...
 $ X19    : chr  "37" "33" "29" "25" ...
 $ X20    : chr  "36" "32" "27" "30" ...
 $ X21    : chr  "36" "33" "30" "30" ...
 $ X22    : chr  "44" "39" "33" "39" ...
 $ X23    : chr  "47" "45" "42" "45" ...
 $ X24    : chr  "46" "44" "41" "46" ...
 $ X25    : chr  "59" "52" "44" "58" ...
 $ X26    : chr  "50" "44" "37" "31" ...
 $ X27    : chr  "52" "45" "38" "34" ...
 $ X28    : chr  "52" "46" "40" "42" ...
 $ X29    : chr  "41" "36" "30" "26" ...
 $ X30    : chr  "30" "26" "22" "10" ...
 $ X31    : chr  "30" "25" "20" "8" ...
```

Veri yapısına baktığımızda 35 değişkenimiz ve 286 adet gözlemimiz olduğunu görebiliriz. Değikenlerimize baktığımız zaman year ve month olarak zaman değişkenleri var. İlk X değişkeninin ne olduğunu bilmiyoruz. Onun dışında çok fazla X ile başlayan değişken var. Son olarak measure değişkeninde çok fazla sayıda biricik(unique) özellik olduğunu görebiliriz. Verimizin ilk gözlemlerine bakarak bu değişkenleri anlamaya çalışalım.

```R
head(weather)

>
  X year month           measure X1 X2 X3 X4 X5 X6 X7 X8 X9 X10 X11 X12 X13 X14 X15 X16 X17 X18 X19 X20 X21
1 1 2014    12  Max.TemperatureF 64 42 51 43 42 45 38 29 49  48  39  39  42  45  42  44  49  44  37  36  36
2 2 2014    12 Mean.TemperatureF 52 38 44 37 34 42 30 24 39  43  36  35  37  39  37  40  45  40  33  32  33
3 3 2014    12  Min.TemperatureF 39 33 37 30 26 38 21 18 29  38  32  31  32  33  32  35  41  36  29  27  30
4 4 2014    12    Max.Dew.PointF 46 40 49 24 37 45 36 28 49  45  37  28  28  29  33  42  46  34  25  30  30
5 5 2014    12    MeanDew.PointF 40 27 42 21 25 40 20 16 41  39  31  27  26  27  29  36  41  30  22  24  27
6 6 2014    12     Min.DewpointF 26 17 24 13 12 36 -3  3 28  37  27  25  24  25  27  30  32  26  20  20  25
  X22 X23 X24 X25 X26 X27 X28 X29 X30 X31
1  44  47  46  59  50  52  52  41  30  30
2  39  45  44  52  44  45  46  36  26  25
3  33  42  41  44  37  38  40  30  22  20
4  39  45  46  58  31  34  42  26  10   8
5  34  42  44  43  29  31  35  20   4   5
6  25  37  41  29  28  29  27  10  -6   1
````
Veriye baktığımızda ilk değişkenimizin gereksiz olduğunu anlayabiliyoruz. X ile başlayan değişkenlere baktığımızda ise bunların günler olduğunu anlayabiliyoruz. Ayrıca measure değişkenine bakarsak buradaki değerlerin değişken olması gerektiğini anlayabiliyoruz. Artık veri üzerinde yapmamız gereken işlemler belirginleşti. O zaman işe koyulalım. :computer: :computer:

## gather() fonksiyonu

Karşımıza çıkan ilk problemi çözelim. X ile başlayan değerler göz sağlığı için çok faydalı durmamakta. Bu değerler aslında değişken olmalı. Bu değerler bir ayın günlerini temsil etmekte.
Bu sütunları tek bir gün (day) değişkeni oluşturacak şekilde toplamamız gerekiyor. Bunu ise gather() fonksiyonu ile yapabiliriz.

İlk önce bu fonksiyonun hangi argümanlar aldığına bakalım.
```R
?gather
```
**buraya resim gelecek**
İlk argümanımız *data* yani üzerinde çalışacağımız veri setimiz.
*key* ve *value* argümanlarına bakarsak *key* değişkenlerimizin yeni kaydedileceği sütunu *value* bu değişkenlere karşılık gelen değerlerin kaydedileceği sütunu göstermekte. Buraya isimleri string olarak girmeliyiz.
Bu argümanlardan sonra değiştireceğimiz değişkenleri argüman olarak vermeliyiz. Örneğimizde daha rahat anlayacaksınız.

```R
weather <- gather(weather, key="day", value="value", X1:X31, na.rm = T)
head(weather)

>
X year month           measure day value
1 1 2014    12  Max.TemperatureF  X1    64
2 2 2014    12 Mean.TemperatureF  X1    52
3 3 2014    12  Min.TemperatureF  X1    39
4 4 2014    12    Max.Dew.PointF  X1    46
5 5 2014    12    MeanDew.PointF  X1    40
6 6 2014    12     Min.DewpointF  X1    26
```

Artık sonsuza kadar giden X değerlerimizi day değişkeninde topladık. Şimdi ise gereksiz olan ilk değişken X'den kurtulalım. Veri setinde bir değişken silmemiz için o değişkene NULL atamamız yeterli olacaktır.

```R
weather$X <- NULL
```

Artık "day" isimli gün kolonumuzu elde ettik. Fakat her günün önünde "X" karakteri mevcut.Bu "X" karakterlerini stringr paketinden str_remove_all() fonksiyonu kullanarak silelim.

```R
install.packages("stringr")
library(stringr)
weather$day <- str_remove_all(weather$day, "X")

head(weather)

>
   year month           measure day value
1 2014    12  Max.TemperatureF   1    64
2 2014    12 Mean.TemperatureF   1    52
3 2014    12  Min.TemperatureF   1    39
4 2014    12    Max.Dew.PointF   1    46
5 2014    12    MeanDew.PointF   1    40
6 2014    12     Min.DewpointF   1    26
```

Veri setimiz şekillenmeye başladı. Şimdi ise ara vermeden devam edelim. :star: :star:

## spread() fonksiyonu

Veri setimizdeki ikinci sorun ise measure değişkeni. Measure değişkeni "Max.TemperatureF", "Mean.TemperatureF" gibi aslında değişken ismi olması gereken gözlemler içeriyor.
Bu değişkendeki farklı her bir gözlemi ayrı bir değişken haline getirmemiz gerekiyor. Bunu spread() fonksiyonu ile yapabiliriz. 

Yine ilk olarak bu fonksiyonun argümanlarına bakalım.

```R
?spread
```
**Ekran görüntüsü gelecek**

İlk argümanımız *data* yani veri setimiz.
*key* argümanı yeni değişkenlere ayrılacak olan sütunu, *value* argümanı ise yeni oluşacak değişkenlere ait olan gözlemleri barındırdan sütunu ifade ediyor. İsimleri gather() fonksiyonunda olduğu gibi string olarak girmeliyiz.
Biz veri setimizde measure değişkenini value değişkenindeki gözlemlerle çoklu sütunlar haline getirmek istiyoruz. spread() fonksiyonunu şu şekilde uyarlayabiliriz.

```R
weather <- spread(weather, measure, value)
head(weather)

>
  year month day CloudCover    Events Max.Dew.PointF Max.Gust.SpeedMPH Max.Humidity Max.Sea.Level.PressureIn
1 2014    12   1          6      Rain             46                29           74                    30.45
2 2014    12  10          8      Rain             45                29          100                    29.58
3 2014    12  11          8 Rain-Snow             37                28           92                    29.81
4 2014    12  12          7      Snow             28                21           85                    29.88
5 2014    12  13          5                       28                23           75                    29.86
6 2014    12  14          4                       29                20           82                    29.91
  Max.TemperatureF Max.VisibilityMiles Max.Wind.SpeedMPH Mean.Humidity Mean.Sea.Level.PressureIn Mean.TemperatureF
1               64                  10                22            63                     30.13                52
2               48                  10                23            95                      29.5                43
3               39                  10                21            87                     29.61                36
4               39                  10                16            75                     29.85                35
5               42                  10                17            65                     29.82                37
6               45                  10                15            68                     29.83                39
  Mean.VisibilityMiles Mean.Wind.SpeedMPH MeanDew.PointF Min.DewpointF Min.Humidity Min.Sea.Level.PressureIn
1                   10                 13             40            26           52                    30.01
2                    3                 13             39            37           89                    29.43
3                    7                 13             31            27           82                    29.44
4                   10                 11             27            25           64                    29.81
5                   10                 12             26            24           55                    29.78
6                   10                 10             27            25           53                    29.78
  Min.TemperatureF Min.VisibilityMiles PrecipitationIn WindDirDegrees
1               39                  10            0.01            268
2               38                   1            0.28            357
3               32                   1            0.02            230
4               31                   7               T            286
5               32                  10               T            298
6               33                  10            0.00            306
``` 

Gördüğümüz gibi measure değişkeni altındaki 22 farklı gözlem ayrı sütun isimleri olacak şekilde ayrıldı. value değişkenindeki değerler de karşılığı olan year, month ve day değişkenlerine göre bu yeni 22 değişkenin altına atandı. 

Veri setimiz neredeyse istediğimiz formata ulaştı, bir diğer fonksiyonumuzla düzenlemeye devam edelim.

## unite() fonksiyonu

Veri setimizdeki diğer bir sorun ise tarihle alakalı değişkenlerin ayrı olması. Tarih bilgisinin year, month ve day olarak 3 ayrı değişkenle ifade edilmesi hem görsellik açısından yeterince anlaşılır durmuyor hem de ileride yapacağımız filtreleme gibi işlemlerin uzamasına sebep olabilir. 
Bu 3 ayrı değişkeni birleştirerek tek bir değişken haline getirmek istiyoruz. Bunun için unite() fonksiyonunu kullanacağız.

Öncelikle bu fonksiyonun argümanlarını inceleyelim.

```R
?unite
```
**Ekran görüntüsü gelecek**

*data* argümanı veri setimizi ifade ediyor.
*col* argümanı yeni oluşacak değişkenimizin ismini gösteriyor, string veya sembol olarak girilmesi gerekiyor.
*...* şeklinde gösterilen argüman birleştirmek istediğimiz değişkenleri ifade ediyor. 
*sep* argümanı ise birleşecek olan değerler arasında kullanılacak ayracı belirtmemizi sağlıyor. 
Veri setimiz üzerinde fonksiyonu uygulayarak bu argümanları daha iyi anlayabiliriz. 

```R
weather <- unite(weather, date, year, month, day, sep = "-")
head(weather)

>
        date CloudCover    Events Max.Dew.PointF Max.Gust.SpeedMPH Max.Humidity Max.Sea.Level.PressureIn Max.TemperatureF
1  2014-12-1          6      Rain             46                29           74                    30.45               64
2 2014-12-10          8      Rain             45                29          100                    29.58               48
3 2014-12-11          8 Rain-Snow             37                28           92                    29.81               39
4 2014-12-12          7      Snow             28                21           85                    29.88               39
5 2014-12-13          5                       28                23           75                    29.86               42
6 2014-12-14          4                       29                20           82                    29.91               45
  Max.VisibilityMiles Max.Wind.SpeedMPH Mean.Humidity Mean.Sea.Level.PressureIn Mean.TemperatureF Mean.VisibilityMiles
1                  10                22            63                     30.13                52                   10
2                  10                23            95                      29.5                43                    3
3                  10                21            87                     29.61                36                    7
4                  10                16            75                     29.85                35                   10
5                  10                17            65                     29.82                37                   10
6                  10                15            68                     29.83                39                   10
  Mean.Wind.SpeedMPH MeanDew.PointF Min.DewpointF Min.Humidity Min.Sea.Level.PressureIn Min.TemperatureF Min.VisibilityMiles
1                 13             40            26           52                    30.01               39                  10
2                 13             39            37           89                    29.43               38                   1
3                 13             31            27           82                    29.44               32                   1
4                 11             27            25           64                    29.81               31                   7
5                 12             26            24           55                    29.78               32                  10
6                 10             27            25           53                    29.78               33                  10
  PrecipitationIn WindDirDegrees
1            0.01            268
2            0.28            357
3            0.02            230
4               T            286
5               T            298
6            0.00            306
```

year, month ve day değişkenlerinin değerleri satır bazında, belirlemiş olduğumuz "-" ayraç işaretiyle, date isimli değişken altında birleşti. 

Bir tarih değişkeni elde ettik ancak oluşturduğumuz bu değişkenin sınıfı da tarih formatında mı acaba?

```R
class(weather$date)

>
[1] "character"
```

Gördüğümüz gibi elde ettiğimiz değişken karakter formatında. R'ın bu değişkeni tarih olarak algılayabilmesi için bu şekilde işlememiz gerekiyor. Tarih formatıyla ilgili işlemleri "lubridate" paketini kullanarak yapıyoruz. Öncelikle paketi indirelim ve çağıralım.

```R
install.packages("lubridate")
library(lubridate)
```

date değişkenimizi tarih formatına ymd() fonksiyonunu kullanarak çevireceğiz. Bu fonksiyonu kullanmamızın sebebi date değişkenindeki gözlemlerimizin "year-month-day" sırasında olması. Farklı sıralarda olan gözlemler için ydm(), mdy(), myd(), dmy(), dym() fonksiyonları gibi gözlemlerimize uygun olan fonksiyonu kullanabiliriz.

date değişkenimizdeki değerleri tarih formatına çevirerek tekrar date değişkenine tanımlıyoruz. 

```R
weather$date <- ymd(weather$date)
class(weather$date)

>
[1] "Date"
```

date değişkeninin sınıfı tarih formatına dönüştü. 

Veri setimizin yapısını tekrar incelediğimizde “PreciptationIn” değişkeninde ondalıklı sayılar ile birlikte “T” harfinin de olduğunu görüyoruz. 

```R
str(weather)

>
'data.frame': 366 obs. of 23 variables:
$ date : Date, format: "2014-12-01" "2014-12-10" "2014-12-11" "2014-12-12" ...
$ CloudCover : chr "6" "8" "8" "7" ...
$ Events : chr "Rain" "Rain" "Rain-Snow" "Snow" ...
$ Max.Dew.PointF : chr "46" "45" "37" "28" ...
$ Max.Gust.SpeedMPH : chr "29" "29" "28" "21" ...
$ Max.Humidity : chr "74" "100" "92" "85" ...
$ Max.Sea.Level.PressureIn : chr "30.45" "29.58" "29.81" "29.88" ...
$ Max.TemperatureF : chr "64" "48" "39" "39" ...
$ Max.VisibilityMiles : chr "10" "10" "10" "10" ...
$ Max.Wind.SpeedMPH : chr "22" "23" "21" "16" ...
$ Mean.Humidity : chr "63" "95" "87" "75" ...
$ Mean.Sea.Level.PressureIn: chr "30.13" "29.5" "29.61" "29.85" ...
$ Mean.TemperatureF : chr "52" "43" "36" "35" ...
$ Mean.VisibilityMiles : chr "10" "3" "7" "10" ...
$ Mean.Wind.SpeedMPH : chr "13" "13" "13" "11" ...
$ MeanDew.PointF : chr "40" "39" "31" "27" ...
$ Min.DewpointF : chr "26" "37" "27" "25" ...
$ Min.Humidity : chr "52" "89" "82" "64" ...
$ Min.Sea.Level.PressureIn : chr "30.01" "29.43" "29.44" "29.81" ...
$ Min.TemperatureF : chr "39" "38" "32" "31" ...
$ Min.VisibilityMiles : chr "10" "1" "1" "7" ...
$ PrecipitationIn : chr "0.01" "0.28" "0.02" "T" ...
$ WindDirDegrees : chr "268" "357" "230" "286" ...
```

İleride yapacağımız analizler için bu "T" değerleri yanlış sonuçlara sebep olabilir o yüzden bu değerleri diğerleri gibi bir sayıyla değiştirmek istiyoruz.
PrecipitationIn değişkeni hakkında biraz araştırma yaptığımızda bu değişkenin meteoroloji alanında yağışı temsil ettiğini ve ilgili bölgenin çok az yağış alacağını belirttiğini öğreniyoruz. 
Bu yüzden "T" harfi yerine bu gözlemlere 0 (sıfır) atarsak daha anlamlı sonuçlar elde edebiliriz. Bu işlemi "stringr" paketinde bulunan str_replace_all() fonsiyonunu kullanarak yapacağız. 

Bu fonksiyonun argümanlarını inceleyelim.

```R
?str_replace_all
```
**Ekran görüntüsü gelecek**

*string* argümanı değişiklik yapacağımız vektörü ifade ediyor. 
*pattern* argümanı değiştimek istediğimiz, fonksiyonun vektör içinde arayacağı ifadeyi/kalıbı gösteriyor.
*replacement* argümanı ise *pattern* argümanındaki ifadeyi ne ile değiştirmek istediğimizi ifade ediyor.
Fonksiyonu veri setimiz üzerinde uygulayalım.

```R
weather$PrecipitationIn <- str_replace_all(weather$PrecipitationIn, "T", "0")
head(weather$PrecipitationIn, 10)

>
[1] "0.01" "0.28" "0.02" "0" "0" "0.00" "0.00" "0" "0.43" "0.01"
```

PrecipitationIn değişkenindeki "T" ifadelerini "0" sayısıyla değiştirdik. 


head() fonksiyonuyla veri setimizin son haline bakalım.

```R
#İlk 5 satıra bakalım.
head(weather, 5) 

        date CloudCover    Events Max.Dew.PointF Max.Gust.SpeedMPH Max.Humidity Max.Sea.Level.PressureIn Max.TemperatureF
1 2014-12-01          6      Rain             46                29           74                    30.45               64
2 2014-12-10          8      Rain             45                29          100                    29.58               48
3 2014-12-11          8 Rain-Snow             37                28           92                    29.81               39
4 2014-12-12          7      Snow             28                21           85                    29.88               39
5 2014-12-13          5                       28                23           75                    29.86               42
  Max.VisibilityMiles Max.Wind.SpeedMPH Mean.Humidity Mean.Sea.Level.PressureIn Mean.TemperatureF Mean.VisibilityMiles
1                  10                22            63                     30.13                52                   10
2                  10                23            95                      29.5                43                    3
3                  10                21            87                     29.61                36                    7
4                  10                16            75                     29.85                35                   10
5                  10                17            65                     29.82                37                   10
  Mean.Wind.SpeedMPH MeanDew.PointF Min.DewpointF Min.Humidity Min.Sea.Level.PressureIn Min.TemperatureF Min.VisibilityMiles
1                 13             40            26           52                    30.01               39                  10
2                 13             39            37           89                    29.43               38                   1
3                 13             31            27           82                    29.44               32                   1
4                 11             27            25           64                    29.81               31                   7
5                 12             26            24           55                    29.78               32                  10
  PrecipitationIn WindDirDegrees
1            0.01            268
2            0.28            357
3            0.02            230
4               0            286
5               0            298
```

Görsel açıdan daha düzgün bir veri seti elde etmek amacıyla date ve Events değişkenleri ilk iki sütunlarda olacak şekilde verimizi düzenlemek istiyoruz. Bu düzenlemeyi "dplyr" paketinde bulunan select() fonksiyonuyla yapacağız. 

```R
weather <- weather %>%
  select(date, Events, everything())
```
select() fonksiyonuyla ilk sütun için date, ikinci sütün için Events değişkenlerini seçtikten sonra geri kalan değişkenlerin isimlerini teker teker yazmak yerine everything() fonksiyonunu kullanarak diğer değişkenleri de sıralıyoruz. Bu işlemi tekrar weather veri setimize atamayı da unutmamamız lazım. 

Veri setimizin yapısını inceleyelim.
```R
str(weather)

>
'data.frame':	366 obs. of  23 variables:
 $ date                     : Date, format: "2014-12-01" "2014-12-10" "2014-12-11" "2014-12-12" ...
 $ Events                   : chr  "Rain" "Rain" "Rain-Snow" "Snow" ...
 $ CloudCover               : chr  "6" "8" "8" "7" ...
 $ Max.Dew.PointF           : chr  "46" "45" "37" "28" ...
 $ Max.Gust.SpeedMPH        : chr  "29" "29" "28" "21" ...
 $ Max.Humidity             : chr  "74" "100" "92" "85" ...
 $ Max.Sea.Level.PressureIn : chr  "30.45" "29.58" "29.81" "29.88" ...
 $ Max.TemperatureF         : chr  "64" "48" "39" "39" ...
 $ Max.VisibilityMiles      : chr  "10" "10" "10" "10" ...
 $ Max.Wind.SpeedMPH        : chr  "22" "23" "21" "16" ...
 $ Mean.Humidity            : chr  "63" "95" "87" "75" ...
 $ Mean.Sea.Level.PressureIn: chr  "30.13" "29.5" "29.61" "29.85" ...
 $ Mean.TemperatureF        : chr  "52" "43" "36" "35" ...
 $ Mean.VisibilityMiles     : chr  "10" "3" "7" "10" ...
 $ Mean.Wind.SpeedMPH       : chr  "13" "13" "13" "11" ...
 $ MeanDew.PointF           : chr  "40" "39" "31" "27" ...
 $ Min.DewpointF            : chr  "26" "37" "27" "25" ...
 $ Min.Humidity             : chr  "52" "89" "82" "64" ...
 $ Min.Sea.Level.PressureIn : chr  "30.01" "29.43" "29.44" "29.81" ...
 $ Min.TemperatureF         : chr  "39" "38" "32" "31" ...
 $ Min.VisibilityMiles      : chr  "10" "1" "1" "7" ...
 $ PrecipitationIn          : chr  "0.01" "0.28" "0.02" "0" ...
 $ WindDirDegrees           : chr  "268" "357" "230" "286" ...
```

Gördüğümüz gibi, date formatına getirdiğimiz date değişkeni hariç diğer değişkenler karakter formatında. Events değişkeni hariç diğer bütün değişkenler tam veya ondalıklı sayılardan oluşuyor. Bu değişenleri numerik veri tipine dönüştürek istiyoruz. Bunu 3 farklı yoldan yapabiliriz.

1. yol:
İlk olarak "dplyr" paketindeki mutate_at fonksiyonuyla yapabiliriz. Argümanlara ilk olarak veri setimizi giriyoruz. İkinci argüman "vars()", formatını değiştirmek istediğimiz değişkenlerin isimlerini girmemize yarıyor. Bu değişkenlerin hepsi yan yana olduğu için "CloudCover:WindDirDegrees" şeklinde ilk ve son sütunlardaki değişkenlerin isimlerinin arasına : işareti koyarak yazabiliriz. Üçüncü argüman "funs()", yapmak istediğimiz işlemi/fonksiyonu girmemizi sağlar. Numerik veri tipine dönüştürmek istediğimiz için as.numeric() fonksiyonunu kullanacağız. 

```R
weather <- mutate_at(weather, vars(CloudCover:WindDirDegrees), funs(as.numeric))
```

2.yol:
lapply() fonksiyonuyla veri setimizdeki numerik veri tipine çevirmek istediğimiz son 21 sütuna as.numeric() fonksiyonunu uygulayabiliriz. cbind() fonksiyonunu da kullanarak ilk 2 sütunla bu 21 sütunu birleştirebiliriz. 

```R
weather <- cbind(weather[1:2], lapply(weather[3:23], as.numeric))
```

3. yol:
Son olarak her döngüde bir sütunu numerik veri tipine çevirecek olan bir for döngüsü de kurabiliriz. 
```R
for(i in 3:23) {
  weather[,i] <- as.numeric(weather[,i])
}
```

Değişkenlerin numerik veri tipine dönüşüp dönüşmediğini kontrol etmek için tekrar veri setimizin yapısını inceleyelim.
```R
str(weather)

>
'data.frame':	366 obs. of  23 variables:
 $ date                     : Date, format: "2014-12-01" "2014-12-10" "2014-12-11" ...
 $ Events                   : chr  "Rain" "Rain" "Rain-Snow" "Snow" ...
 $ CloudCover               : num  6 8 8 7 5 4 2 8 8 7 ...
 $ Max.Dew.PointF           : num  46 45 37 28 28 29 33 42 46 34 ...
 $ Max.Gust.SpeedMPH        : num  29 29 28 21 23 20 21 10 26 30 ...
 $ Max.Humidity             : num  74 100 92 85 75 82 89 96 100 89 ...
 $ Max.Sea.Level.PressureIn : num  30.4 29.6 29.8 29.9 29.9 ...
 $ Max.TemperatureF         : num  64 48 39 39 42 45 42 44 49 44 ...
 $ Max.VisibilityMiles      : num  10 10 10 10 10 10 10 10 10 10 ...
 $ Max.Wind.SpeedMPH        : num  22 23 21 16 17 15 15 8 20 23 ...
 $ Mean.Humidity            : num  63 95 87 75 65 68 75 85 85 73 ...
 $ Mean.Sea.Level.PressureIn: num  30.1 29.5 29.6 29.9 29.8 ...
 $ Mean.TemperatureF        : num  52 43 36 35 37 39 37 40 45 40 ...
 $ Mean.VisibilityMiles     : num  10 3 7 10 10 10 10 9 6 10 ...
 $ Mean.Wind.SpeedMPH       : num  13 13 13 11 12 10 6 4 11 14 ...
 $ MeanDew.PointF           : num  40 39 31 27 26 27 29 36 41 30 ...
 $ Min.DewpointF            : num  26 37 27 25 24 25 27 30 32 26 ...
 $ Min.Humidity             : num  52 89 82 64 55 53 60 73 70 57 ...
 $ Min.Sea.Level.PressureIn : num  30 29.4 29.4 29.8 29.8 ...
 $ Min.TemperatureF         : num  39 38 32 31 32 33 32 35 41 36 ...
 $ Min.VisibilityMiles      : num  10 1 1 7 10 10 10 5 1 10 ...
 $ PrecipitationIn          : num  0.01 0.28 0.02 0 0 0 0 0 0.43 0.01 ...
 $ WindDirDegrees           : num  268 357 230 286 298 306 324 79 311 281 ...
```

## Veri setindeki eksik, aykırı gözlemler

Veri setimiz üzerinde yapacağımız analizlerden doğru ve anlamlı sonuçlar alabilmemiz için her zaman eksik ve aykırı gözlemleri kontrol etmemiz gerekir.
Eksik gözlemler veri setinde "NA" şeklinde görünebilir ya da hiçbir girdi olmayabilir.
Veri setimizde "NA" olup olmadığını is.na() fonksiyonuyla kontrol edelim.

```R
is.na(weather)

>
date Events CloudCover Max.Dew.PointF Max.Gust.SpeedMPH Max.Humidity Max.Sea.Level.PressureIn Max.TemperatureF
  [1,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
  [2,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
  [3,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
  [4,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
  [5,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
  [6,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
  [7,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
  [8,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
  [9,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [10,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [11,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [12,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [13,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [14,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [15,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [16,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [17,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [18,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [19,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [20,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [21,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [22,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [23,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [24,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [25,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [26,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [27,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [28,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [29,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [30,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [31,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [32,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [33,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [34,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [35,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [36,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [37,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [38,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [39,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [40,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [41,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [42,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
 [43,] FALSE  FALSE      FALSE          FALSE             FALSE        FALSE                    FALSE            FALSE
       Max.VisibilityMiles Max.Wind.SpeedMPH Mean.Humidity Mean.Sea.Level.PressureIn Mean.TemperatureF
  [1,]               FALSE             FALSE         FALSE                     FALSE             FALSE
  [2,]               FALSE             FALSE         FALSE                     FALSE             FALSE
  [3,]               FALSE             FALSE         FALSE                     FALSE             FALSE
  [4,]               FALSE             FALSE         FALSE                     FALSE             FALSE
  [5,]               FALSE             FALSE         FALSE                     FALSE             FALSE
  [6,]               FALSE             FALSE         FALSE                     FALSE             FALSE
  [7,]               FALSE             FALSE         FALSE                     FALSE             FALSE
  [8,]               FALSE             FALSE         FALSE                     FALSE             FALSE
  [9,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [10,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [11,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [12,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [13,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [14,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [15,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [16,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [17,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [18,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [19,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [20,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [21,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [22,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [23,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [24,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [25,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [26,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [27,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [28,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [29,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [30,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [31,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [32,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [33,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [34,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [35,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [36,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [37,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [38,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [39,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [40,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [41,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [42,]               FALSE             FALSE         FALSE                     FALSE             FALSE
 [43,]               FALSE             FALSE         FALSE                     FALSE             FALSE
       Mean.VisibilityMiles Mean.Wind.SpeedMPH MeanDew.PointF Min.DewpointF Min.Humidity Min.Sea.Level.PressureIn
  [1,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
  [2,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
  [3,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
  [4,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
  [5,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
  [6,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
  [7,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
  [8,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
  [9,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [10,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [11,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [12,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [13,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [14,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [15,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [16,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [17,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [18,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [19,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [20,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [21,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [22,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [23,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [24,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [25,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [26,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [27,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [28,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [29,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [30,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [31,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [32,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [33,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [34,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [35,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [36,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [37,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [38,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [39,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [40,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [41,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [42,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
 [43,]                FALSE              FALSE          FALSE         FALSE        FALSE                    FALSE
       Min.TemperatureF Min.VisibilityMiles PrecipitationIn WindDirDegrees
  [1,]            FALSE               FALSE           FALSE          FALSE
  [2,]            FALSE               FALSE           FALSE          FALSE
  [3,]            FALSE               FALSE           FALSE          FALSE
  [4,]            FALSE               FALSE           FALSE          FALSE
  [5,]            FALSE               FALSE           FALSE          FALSE
  [6,]            FALSE               FALSE           FALSE          FALSE
  [7,]            FALSE               FALSE           FALSE          FALSE
  [8,]            FALSE               FALSE           FALSE          FALSE
  [9,]            FALSE               FALSE           FALSE          FALSE
 [10,]            FALSE               FALSE           FALSE          FALSE
 [11,]            FALSE               FALSE           FALSE          FALSE
 [12,]            FALSE               FALSE           FALSE          FALSE
 [13,]            FALSE               FALSE           FALSE          FALSE
 [14,]            FALSE               FALSE           FALSE          FALSE
 [15,]            FALSE               FALSE           FALSE          FALSE
 [16,]            FALSE               FALSE           FALSE          FALSE
 [17,]            FALSE               FALSE           FALSE          FALSE
 [18,]            FALSE               FALSE           FALSE          FALSE
 [19,]            FALSE               FALSE           FALSE          FALSE
 [20,]            FALSE               FALSE           FALSE          FALSE
 [21,]            FALSE               FALSE           FALSE          FALSE
 [22,]            FALSE               FALSE           FALSE          FALSE
 [23,]            FALSE               FALSE           FALSE          FALSE
 [24,]            FALSE               FALSE           FALSE          FALSE
 [25,]            FALSE               FALSE           FALSE          FALSE
 [26,]            FALSE               FALSE           FALSE          FALSE
 [27,]            FALSE               FALSE           FALSE          FALSE
 [28,]            FALSE               FALSE           FALSE          FALSE
 [29,]            FALSE               FALSE           FALSE          FALSE
 [30,]            FALSE               FALSE           FALSE          FALSE
 [31,]            FALSE               FALSE           FALSE          FALSE
 [32,]            FALSE               FALSE           FALSE          FALSE
 [33,]            FALSE               FALSE           FALSE          FALSE
 [34,]            FALSE               FALSE           FALSE          FALSE
 [35,]            FALSE               FALSE           FALSE          FALSE
 [36,]            FALSE               FALSE           FALSE          FALSE
 [37,]            FALSE               FALSE           FALSE          FALSE
 [38,]            FALSE               FALSE           FALSE          FALSE
 [39,]            FALSE               FALSE           FALSE          FALSE
 [40,]            FALSE               FALSE           FALSE          FALSE
 [41,]            FALSE               FALSE           FALSE          FALSE
 [42,]            FALSE               FALSE           FALSE          FALSE
 [43,]            FALSE               FALSE           FALSE          FALSE
 [ reached getOption("max.print") -- omitted 323 rows ]
 ```
 
Bu çıktı içinden TRUE değerlerini saymamız mümkün değil. Ancak sum() fonksiyonuyla TRUE değerlerini toplayabiliriz. (FALSE değerleri 0, TRUE değerleri 1 olarak algılanıyor, hatırlayalım.)

```R
sum(is.na(weather))

>
[1] 6
```

6 adet TRUE değerini olduğunu bulduk. Bu değerlerin hangi satırlarda olduğuna which() fonksiyonuyla bakabiliriz.``

```R
which(is.na(weather))

>
[1] 1625 1669 1737 1739 1772 1822
```

"NA" değerlerinin hangi değişkenlerde olduğunu bulmak için veri setimizin özetine bakalım.

```R
summary(weather)

>
      date               Events            CloudCover    Max.Dew.PointF  Max.Gust.SpeedMPH  Max.Humidity    
 Min.   :2014-12-01   Length:366         Min.   :0.000   Min.   :-6.00   Min.   : 0.00     Min.   :  39.00  
 1st Qu.:2015-03-02   Class :character   1st Qu.:3.000   1st Qu.:32.00   1st Qu.:21.00     1st Qu.:  73.25  
 Median :2015-06-01   Mode  :character   Median :5.000   Median :47.50   Median :25.50     Median :  86.00  
 Mean   :2015-06-01                      Mean   :4.708   Mean   :45.48   Mean   :26.99     Mean   :  85.69  
 3rd Qu.:2015-08-31                      3rd Qu.:7.000   3rd Qu.:61.00   3rd Qu.:31.25     3rd Qu.:  93.00  
 Max.   :2015-12-01                      Max.   :8.000   Max.   :75.00   Max.   :94.00     Max.   :1000.00  
                                                                         NA's   :6                          
 Max.Sea.Level.PressureIn Max.TemperatureF Max.VisibilityMiles Max.Wind.SpeedMPH Mean.Humidity  
 Min.   :29.58            Min.   :18.00    Min.   : 2.000      Min.   : 8.00     Min.   :28.00  
 1st Qu.:30.00            1st Qu.:42.00    1st Qu.:10.000      1st Qu.:16.00     1st Qu.:56.00  
 Median :30.14            Median :60.00    Median :10.000      Median :20.00     Median :66.00  
 Mean   :30.16            Mean   :58.93    Mean   : 9.907      Mean   :20.62     Mean   :66.02  
 3rd Qu.:30.31            3rd Qu.:76.00    3rd Qu.:10.000      3rd Qu.:24.00     3rd Qu.:76.75  
 Max.   :30.88            Max.   :96.00    Max.   :10.000      Max.   :38.00     Max.   :98.00  
                                                                                                
 Mean.Sea.Level.PressureIn Mean.TemperatureF Mean.VisibilityMiles Mean.Wind.SpeedMPH MeanDew.PointF   Min.DewpointF   
 Min.   :29.49             Min.   : 8.00     Min.   :-1.000       Min.   : 4.00      Min.   :-11.00   Min.   :-18.00  
 1st Qu.:29.87             1st Qu.:36.25     1st Qu.: 8.000       1st Qu.: 8.00      1st Qu.: 24.00   1st Qu.: 16.25  
 Median :30.03             Median :53.50     Median :10.000       Median :10.00      Median : 41.00   Median : 35.00  
 Mean   :30.04             Mean   :51.40     Mean   : 8.861       Mean   :10.68      Mean   : 38.96   Mean   : 32.25  
 3rd Qu.:30.19             3rd Qu.:68.00     3rd Qu.:10.000       3rd Qu.:13.00      3rd Qu.: 56.00   3rd Qu.: 51.00  
 Max.   :30.77             Max.   :84.00     Max.   :10.000       Max.   :22.00      Max.   : 71.00   Max.   : 68.00  
                                                                                                                      
  Min.Humidity   Min.Sea.Level.PressureIn Min.TemperatureF Min.VisibilityMiles PrecipitationIn  WindDirDegrees 
 Min.   :16.00   Min.   :29.16            Min.   :-3.00    Min.   : 0.000      Min.   :0.0000   Min.   :  1.0  
 1st Qu.:35.00   1st Qu.:29.76            1st Qu.:30.00    1st Qu.: 2.000      1st Qu.:0.0000   1st Qu.:113.0  
 Median :46.00   Median :29.94            Median :46.00    Median :10.000      Median :0.0000   Median :222.0  
 Mean   :48.31   Mean   :29.93            Mean   :43.33    Mean   : 6.716      Mean   :0.1016   Mean   :200.1  
 3rd Qu.:60.00   3rd Qu.:30.09            3rd Qu.:60.00    3rd Qu.:10.000      3rd Qu.:0.0400   3rd Qu.:275.0  
 Max.   :96.00   Max.   :30.64            Max.   :74.00    Max.   :10.000      Max.   :2.9000   Max.   :360.0 
 ```
 
6 "NA" değerinin de Max.Gust.SpeedMPH değişkeninde olduğunu görüyoruz. 

Eksik gözlemler, aykırı gözlemler, beklenmeyen gözlemler ve bariz hatalar gibi durumlarda işlem yaparken aşağıdakiler dikkate alınmalıdır:
-Değişkenlerin içerikleri önemlidir.
-Değişkenlerin aralıkları akla yatkın olmalıdır.
-Veri seti ölçütleri doğrulanmalıdır.

Max.Gust.SpeedMPH değişkeninde eksik gözlemleri satır bazında silebiliriz ya da doldurabiliriz. Genel olarak eksik gözlemleri ortalama ile doldurmak yaygın olan bir kullanımdır.
Biz de Max.Gust.SpeedMPH değişkenindeki eksik gözlemleri ortalama ile doldurmak istiyoruz. mean() fonksiyonuyla bu değişkenin ortalama değerini bulup, veri seti içindeki "NA" değerlerini seçerek bunlara atayabiliriz. Sonunda kontrol amaçlı tekrar veri setimizin özetini inceleyelim.

```R
weather[is.na(weather$Max.Gust.SpeedMPH), "Max.Gust.SpeedMPH"] <- mean(weather$Max.Gust.SpeedMPH, na.rm = T)
summary(weather)

>
  date               Events            CloudCover    Max.Dew.PointF  Max.Gust.SpeedMPH  Max.Humidity    
 Min.   :2014-12-01   Length:366         Min.   :0.000   Min.   :-6.00   Min.   : 0.00     Min.   :  39.00  
 1st Qu.:2015-03-02   Class :character   1st Qu.:3.000   1st Qu.:32.00   1st Qu.:21.00     1st Qu.:  73.25  
 Median :2015-06-01   Mode  :character   Median :5.000   Median :47.50   Median :26.00     Median :  86.00  
 Mean   :2015-06-01                      Mean   :4.708   Mean   :45.48   Mean   :26.99     Mean   :  85.69  
 3rd Qu.:2015-08-31                      3rd Qu.:7.000   3rd Qu.:61.00   3rd Qu.:31.00     3rd Qu.:  93.00  
 Max.   :2015-12-01                      Max.   :8.000   Max.   :75.00   Max.   :94.00     Max.   :1000.00  
 Max.Sea.Level.PressureIn Max.TemperatureF Max.VisibilityMiles Max.Wind.SpeedMPH Mean.Humidity  
 Min.   :29.58            Min.   :18.00    Min.   : 2.000      Min.   : 8.00     Min.   :28.00  
 1st Qu.:30.00            1st Qu.:42.00    1st Qu.:10.000      1st Qu.:16.00     1st Qu.:56.00  
 Median :30.14            Median :60.00    Median :10.000      Median :20.00     Median :66.00  
 Mean   :30.16            Mean   :58.93    Mean   : 9.907      Mean   :20.62     Mean   :66.02  
 3rd Qu.:30.31            3rd Qu.:76.00    3rd Qu.:10.000      3rd Qu.:24.00     3rd Qu.:76.75  
 Max.   :30.88            Max.   :96.00    Max.   :10.000      Max.   :38.00     Max.   :98.00  
 Mean.Sea.Level.PressureIn Mean.TemperatureF Mean.VisibilityMiles Mean.Wind.SpeedMPH MeanDew.PointF   Min.DewpointF   
 Min.   :29.49             Min.   : 8.00     Min.   :-1.000       Min.   : 4.00      Min.   :-11.00   Min.   :-18.00  
 1st Qu.:29.87             1st Qu.:36.25     1st Qu.: 8.000       1st Qu.: 8.00      1st Qu.: 24.00   1st Qu.: 16.25  
 Median :30.03             Median :53.50     Median :10.000       Median :10.00      Median : 41.00   Median : 35.00  
 Mean   :30.04             Mean   :51.40     Mean   : 8.861       Mean   :10.68      Mean   : 38.96   Mean   : 32.25  
 3rd Qu.:30.19             3rd Qu.:68.00     3rd Qu.:10.000       3rd Qu.:13.00      3rd Qu.: 56.00   3rd Qu.: 51.00  
 Max.   :30.77             Max.   :84.00     Max.   :10.000       Max.   :22.00      Max.   : 71.00   Max.   : 68.00  
  Min.Humidity   Min.Sea.Level.PressureIn Min.TemperatureF Min.VisibilityMiles PrecipitationIn  WindDirDegrees 
 Min.   :16.00   Min.   :29.16            Min.   :-3.00    Min.   : 0.000      Min.   :0.0000   Min.   :  1.0  
 1st Qu.:35.00   1st Qu.:29.76            1st Qu.:30.00    1st Qu.: 2.000      1st Qu.:0.0000   1st Qu.:113.0  
 Median :46.00   Median :29.94            Median :46.00    Median :10.000      Median :0.0000   Median :222.0  
 Mean   :48.31   Mean   :29.93            Mean   :43.33    Mean   : 6.716      Mean   :0.1016   Mean   :200.1  
 3rd Qu.:60.00   3rd Qu.:30.09            3rd Qu.:60.00    3rd Qu.:10.000      3rd Qu.:0.0400   3rd Qu.:275.0  
 Max.   :96.00   Max.   :30.64            Max.   :74.00    Max.   :10.000      Max.   :2.9000   Max.   :360.0  
```

Max.Gust.SpeedMPH değişkeninde "NA" değerlerinin artık olmadığını görüyoruz. Ancak o sırada gözümüze başka aykırı değerler çarpıyor. İlk olarak Max.Humidity değişkeninin maksimum değerinin 1000 olduğunu görüyoruz. Nem oranının 1000 olması mümkün değil, muhtemelen yanlışlıkla fazladan bir 0 eklenmiş. Bunu düzetmek için 1000 olan değer yerine 100 yazdıracağız. 
İkinci olarak Mean.VisibilityMiles değişkeninde minimum değerin negatif bir sayı olduğunu fark ediyoruz. Visibility Miles kavramını araştırdığımızda negatif değerler alamayacağını öğreniyoruz. O yüzden bu değeri de değişkenin ortalama değeriyle değiştireceğiz.

Bu iki işlemi iki farklı yöntemle yapacağız. İkisi de birbirine uyarlanabilir.

```R
weather[weather$Max.Humidity == 1000, "Max.Humidity"] <- 100

weather$Mean.VisibilityMiles[which(weather$Mean.VisibilityMiles == -1)] <- mean(weather$Mean.VisibilityMiles)
```

Tekrar veri setimizin özetini inceleyelim.

```R
summary(weather)

>
       date               Events            CloudCover    Max.Dew.PointF  Max.Gust.SpeedMPH  Max.Humidity   
 Min.   :2014-12-01   Length:366         Min.   :0.000   Min.   :-6.00   Min.   : 0.00     Min.   : 39.00  
 1st Qu.:2015-03-02   Class :character   1st Qu.:3.000   1st Qu.:32.00   1st Qu.:21.00     1st Qu.: 73.25  
 Median :2015-06-01   Mode  :character   Median :5.000   Median :47.50   Median :26.00     Median : 86.00  
 Mean   :2015-06-01                      Mean   :4.708   Mean   :45.48   Mean   :26.99     Mean   : 83.23  
 3rd Qu.:2015-08-31                      3rd Qu.:7.000   3rd Qu.:61.00   3rd Qu.:31.00     3rd Qu.: 93.00  
 Max.   :2015-12-01                      Max.   :8.000   Max.   :75.00   Max.   :94.00     Max.   :100.00  
 Max.Sea.Level.PressureIn Max.TemperatureF Max.VisibilityMiles Max.Wind.SpeedMPH Mean.Humidity  
 Min.   :29.58            Min.   :18.00    Min.   : 2.000      Min.   : 8.00     Min.   :28.00  
 1st Qu.:30.00            1st Qu.:42.00    1st Qu.:10.000      1st Qu.:16.00     1st Qu.:56.00  
 Median :30.14            Median :60.00    Median :10.000      Median :20.00     Median :66.00  
 Mean   :30.16            Mean   :58.93    Mean   : 9.907      Mean   :20.62     Mean   :66.02  
 3rd Qu.:30.31            3rd Qu.:76.00    3rd Qu.:10.000      3rd Qu.:24.00     3rd Qu.:76.75  
 Max.   :30.88            Max.   :96.00    Max.   :10.000      Max.   :38.00     Max.   :98.00  
 Mean.Sea.Level.PressureIn Mean.TemperatureF Mean.VisibilityMiles Mean.Wind.SpeedMPH MeanDew.PointF   Min.DewpointF   
 Min.   :29.49             Min.   : 8.00     Min.   : 1.000       Min.   : 4.00      Min.   :-11.00   Min.   :-18.00  
 1st Qu.:29.87             1st Qu.:36.25     1st Qu.: 8.000       1st Qu.: 8.00      1st Qu.: 24.00   1st Qu.: 16.25  
 Median :30.03             Median :53.50     Median :10.000       Median :10.00      Median : 41.00   Median : 35.00  
 Mean   :30.04             Mean   :51.40     Mean   : 8.888       Mean   :10.68      Mean   : 38.96   Mean   : 32.25  
 3rd Qu.:30.19             3rd Qu.:68.00     3rd Qu.:10.000       3rd Qu.:13.00      3rd Qu.: 56.00   3rd Qu.: 51.00  
 Max.   :30.77             Max.   :84.00     Max.   :10.000       Max.   :22.00      Max.   : 71.00   Max.   : 68.00  
  Min.Humidity   Min.Sea.Level.PressureIn Min.TemperatureF Min.VisibilityMiles PrecipitationIn  WindDirDegrees 
 Min.   :16.00   Min.   :29.16            Min.   :-3.00    Min.   : 0.000      Min.   :0.0000   Min.   :  1.0  
 1st Qu.:35.00   1st Qu.:29.76            1st Qu.:30.00    1st Qu.: 2.000      1st Qu.:0.0000   1st Qu.:113.0  
 Median :46.00   Median :29.94            Median :46.00    Median :10.000      Median :0.0000   Median :222.0  
 Mean   :48.31   Mean   :29.93            Mean   :43.33    Mean   : 6.716      Mean   :0.1016   Mean   :200.1  
 3rd Qu.:60.00   3rd Qu.:30.09            3rd Qu.:60.00    3rd Qu.:10.000      3rd Qu.:0.0400   3rd Qu.:275.0  
 Max.   :96.00   Max.   :30.64            Max.   :74.00    Max.   :10.000      Max.   :2.9000   Max.   :360.0 
````

Son olarak View() fonksiyonuyla veri setimizi gözlemlediğimizde Events değişkeninde eksik gözlemler olduğunu görüyoruz. Ancak bu değerler "NA" olarak değil boşluk olarak işlenmiş. Bu yüzden bu değerler yerine "None" diye bir değer atamak istiyoruz.

```R
weather[weather$Events == "", "Events"] <- "None"
```

Veri setimiz yapacağımız analizler için hazır! Tabi ki yapacağımız analizlerin amacına göre veri setimizi farklı şekillerde düzenleyebilir, içinden kesitler alarak farklı veri setleri oluşturabilir ya da başka veri setleriyle birleştirebiliriz. Bu birleştirme işlemini "dplyr" paketiyle nasıl yaptığımızı bir sonraki bölümde bulabilirsiniz. 

## dplyr ile join işlemleri

Aynı değişkeni içeren birden fazla veri setine sahip olduğumuz durumlarda, bu veri setlerini “dplyr” paketinin join() fonksiyonlarıyla birleştirebiliriz. İstediğimiz değişkenleri ve gözlemleri seçerek bu birleştirme işlemini yapmamızı sağlayan bazı fonksiyonlar şunlardır:
- left_join()
- right_join()
- inner_join()
- full_join()
- anti_join()
- semi_join()

Bu fonksiyonları kendi tanımlayacağımız iki veri seti üzerinde deneyeceğiz. Daha önce gördüğümüz data frame gibi yöntemler yerine “tibble” formatında veri setlerimizi tanımlayacağız. Tibble’lar okunabilirliği yüksek olan küçük tablolar için kullanılabilen bir formattır. Değişkenlerin altında gözlemler satır satır dizilmiş halde bulunur. 
Tibble oluşturmak için tribble() fonksiyonunu kullanacağız. Bu fonksiyon aslında “tibble” paketi içinde bulunuyor. Ancak ek paketleri içinde “tibble”, “dplyr” ve “tidyr” gibi paketleri de barındıran “tidyverse” paketini de çağırabiliriz. Tabi ki ilk defa kullanacağımız bir paketse, öncelikle install.packages() fonksiyonuyla yüklememiz gerekiyor. 

```R
install.packages("tidyverse")
library(tidyverse)
```

“superheroes” ve “publishers” adında süper kahramanlarla ilgili iki tibble tanımlayacağız. Değişken isimlerimizi belli etmek için başına ‘~’ işareti koymamız gerekiyor. Daha sonra her değişkenin gözlemi değişken isminin altına gelecek şekilde yani bir tablo halinde gözlemlerimizi giriyoruz. 

```R
superheroes <- tribble(
  ~name,     ~alignment,    ~gender,    ~publisher,
  "Magneto",   "bad",       "male",      "Marvel",
  "Storm",     "good",      "female",    "Marvel",
  "Mystique",  "bad",       "female",    "Marvel",
  "Batman",    "good",      "male",      "DC",
  "Joker",     "bad",       "male",      "DC",  
  "Catwoman",  "bad",       "female",    "DC",
  "Hellboy",    "good",     "male",      "Dark Horse Comics"
)

publishers <- tribble(
  ~name,   ~yr_founded,
  "DC",         1934L,  
  "Marvel",     1939L,
  "Image",      1992L
)
```

“superheroes” veri setimiz süper kahramanın ismini, kötü ya da iyi olarak karakterini, cinsiyetini ve  yayıncı kuruluşu içeriyor. “publishers” veri setimiz ise yayıncı kuruluşların ismini ve kuruluş yıllarını içeriyor. 
Bu veri setleri içinde aynı olan bir değişkenimiz var ancak değişken isimleri farklı. “superheroes” veri setindeki publisher değişkeniyle, “publishers” veri setindeki name değişkeni aslında aynı değişkenler. Veri setlerimizi bu değişkene göre birleştireceğiz. 

**inner_join() fonksiyonu**

Bu fonksiyon her iki tablodaki eşleşen satırları ve her iki tablodaki değişkenleri getirir. Veri setlerimiz üzerinde deneyelim. 
Veri setlerinde aynı olan değişkenlerimizin ismi farklıydı. Eğer aynı olsaydı, inner_join() foksiyonuna argüman olarak sadece veri setlerinin isimlerini girmemiz yeterliydi. Ancak aynı olmadığı için ‘by=’ argümanıyla hangi değişkenlere göre birleştirmek istediğimizi belirtmemiz lazım. 

```R
inner_join(superheroes, publishers, by = "publisher" = “name”)

>
# A tibble: 6 x 5
  name     alignment gender publisher yr_founded
  <chr>    <chr>     <chr>  <chr>          <int>
1 Magneto  bad       male   Marvel          1939
2 Storm    good      female Marvel          1939
3 Mystique bad       female Marvel          1939
4 Batman   good      male   DC              1934
5 Joker    bad       male   DC              1934
6 Catwoman bad       female DC              1934
```

Argüman olarak ilk yazdığımız veri setinin değişkenleri başta olacak şekilde tablolarımız publisher ve name değişkenlerine göre birleşti. Ortak olan değişkenin ismi de yine ilk argüman olarak yazdığımız veri setindeki publisher ismini aldı. 
Burada dikkat etmemiz gereken nokta, superheroes veri setindeki "Dark Horse Comics" gözleminin olduğu satır ve publishers veri setindeki "Image" gözleminin olduğu satırın birleşmiş halinde bulunmaması. inner_join fonksiyonu sadece her iki tabloda eşleşen satırları getirdiği için bu satırlar bulunmuyor. 

**semi_join() foksiyonu**

Her iki tablodaki eşleşen satırları getirir ama ikinci tablodaki ortak olan sütun hariç diğer sütunları getirmez.

```R
semi_join(superheroes, publishers, by = c("publisher" = "name"))

>
# A tibble: 6 x 4
  name     alignment gender publisher
  <chr>    <chr>     <chr>  <chr>    
1 Magneto  bad       male   Marvel   
2 Storm    good      female Marvel   
3 Mystique bad       female Marvel   
4 Batman   good      male   DC       
5 Joker    bad       male   DC       
6 Catwoman bad       female DC  
```

Gördüğümüz gibi, inner_join() fonksiyonundaki gibi sadece eşleşen satırlar geldi ancak, ikinci tablodaki yani publishers veri setindeki sütunlar gelmedi. (Burada ortak olan sütun haricinde tek bir sütun vardı: yr_founded)

**left_join() fonksiyonu**

İlk tablodaki tüm satırlar ile birlikte her iki tablodaki tüm değişkenleri/sütunları getirir.

```R
left_join(superheroes, publishers, by = c("publisher" = "name"))

>
# A tibble: 7 x 5
  name     alignment gender publisher         yr_founded
  <chr>    <chr>     <chr>  <chr>                  <int>
1 Magneto  bad       male   Marvel                  1939
2 Storm    good      female Marvel                  1939
3 Mystique bad       female Marvel                  1939
4 Batman   good      male   DC                      1934
5 Joker    bad       male   DC                      1934
6 Catwoman bad       female DC                      1934
7 Hellboy  good      male   Dark Horse Comics         NA
```

inner_join() fonksiyonunda olduğu gibi her iki veri setindeki bütün değişkenler geldi ancak farklı olarak ilk veri setindeki tüm satırlar geldi. "Dark Horse Comics" gözleminin publishers veri setinde karşılığı olmadığı için yr_founded değişkeninde gözlemi “NA” olarak gözüküyor. 

**right_join() fonksiyonu**

İkinci tablodaki tüm satırlar ile birlikte her iki tablodaki tüm değişkenleri/sütunları getirir. 

```R
right_join(superheroes, publishers, by = c("publisher" = "name"))

>
# A tibble: 7 x 5
  name     alignment gender publisher yr_founded
  <chr>    <chr>     <chr>  <chr>          <int>
1 Batman   good      male   DC              1934
2 Joker    bad       male   DC              1934
3 Catwoman bad       female DC              1934
4 Magneto  bad       male   Marvel          1939
5 Storm    good      female Marvel          1939
6 Mystique bad       female Marvel          1939
7 NA       NA        NA     Image           1992
```

left_join() fonksiyonunun tersi olan right_join() fonksiyonunda da her iki veri setindeki tüm değişkenler geldi. Bu seferde, “Image” gözleminin karşılığı superheroes veri setinde bulunmadığı için bu veri setinin değişkenlerinde değeri “NA” geldi. 

**anti_join() fonksiyonu**

İlk tabloda olup ikinci tabloda olmayan satırları ve ilk tablonun sütunlarını getirir.

```R
anti_join(superheroes, publishers, by = c("publisher" = "name"))

>
# A tibble: 1 x 4
  name    alignment gender publisher        
  <chr>   <chr>     <chr>  <chr>            
1 Hellboy good      male   Dark Horse Comics
```

"Dark Horse Comics" gözlemi ikinci tablonun name değişkeninde bulunmuyordu, bu yüzden sadece bu satır geldi.
Tabloların yerini değiştirerek superheroes tablosundaki ortak olmayan satıra da erişebiliriz. Tabi ki birinci ve ikinci argümanın yeri değiştiği için ‘by=’ argümanında da sütun isimlerinin yerini değiştirmemiz gerekiyor.

```R
anti_join(publishers, superheroes, by = c(“name” = "publisher"))

>
# A tibble: 1 x 2
  name  yr_founded
  <chr>      <int>
1 Image       1992
```

**full_join() fonksiyonu**

Her iki tablodaki tüm satırları ve sütunları getirir.

```R
full_join(superheroes, publishers, by = c("publisher" = "name"))

>
# A tibble: 8 x 5
  name     alignment gender publisher         yr_founded
  <chr>    <chr>     <chr>  <chr>                  <int>
1 Magneto  bad       male   Marvel                  1939
2 Storm    good      female Marvel                  1939
3 Mystique bad       female Marvel                  1939
4 Batman   good      male   DC                      1934
5 Joker    bad       male   DC                      1934
6 Catwoman bad       female DC                      1934
7 Hellboy  good      male   Dark Horse Comics         NA
8 NA       NA        NA     Image                   1992
```

Tüm satırlar ve sütunları birleştirdik. Karşılığı olmayan gözlemler yerine otomatik olarak “NA” değeri geldi. 

Veri setimizde ortak olan değişken haricinde aynı isimli değişkenler olursa ne olur? Oluşturduğumuz iki tabloyu bu şekilde revize ederek sonuca bakalım.

```R
superheroes_yeni <- tribble(
  ~name,     ~alignment,    ~gender,    ~publisher_id,   
  "Magneto",   "bad",       "male",         "101",         
  "Storm",     "good",      "female",       "101",         
  "Mystique",  "bad",       "female",       "101",         
  "Batman",    "good",      "male",         "102",         
  "Joker",     "bad",       "male",         "102",          
  "Catwoman",  "bad",       "female",       "102",        
  "Hellboy",    "good",     "male",         "103"
)

publishers_yeni <- tribble(
  ~publisher_id,   ~name,                ~yr_founded,
  "101",           "Marvel",               1939L,
  "102",           "DC",                   1934L,  
  "103",           "Dark Horse Comics",    1986L,
  "104",           "Image",                1992L
)
```

Tabloların yeni halini oluşturarak ikisine de publisher_id değişkenini ekledik ve superheroes tablosundan yayıncı isimlerini çıkardık. Artık ortak olan değişkenimiz publisher_id ancak her iki tabloda da name isimli aslında ortak olmayan iki değişken var. inner_join() fonksiyonuyla tabloları birleştirip sonucu inceleyelim. 

```R
inner_join(superheroes_yeni, publishers_yeni, by = "publisher_id")

>
# A tibble: 7 x 6
  name.x   alignment gender publisher_id name.y            yr_founded
  <chr>    <chr>     <chr>  <chr>        <chr>                  <int>
1 Magneto  bad       male   101          Marvel                  1939
2 Storm    good      female 101          Marvel                  1939
3 Mystique bad       female 101          Marvel                  1939
4 Batman   good      male   102          DC                      1934
5 Joker    bad       male   102          DC                      1934
6 Catwoman bad       female 102          DC                      1934
7 Hellboy  good      male   103          Dark Horse Comics       1986
```

İsmi aynı olan değişkenlerin sonuna farklı olduklarını belli etmek için otomatik olarak '.x' ve '.y' ifadeleri geldi. Tahmin edebileceğiniz gibi, '.x' ifadesi 1. veri setindeki değişkene, '.y' ifadesi 2. tablodaki değişkene geliyor. Ancak dışarıdan bakan biri için yeterince açıklayıcı değiller.
Bu sorunumuzu join fonksiyonunu yazarken 'suffix=" argümanı ekleyerek çözebiliriz. Bu argüman aynı isimli değişkenlerin isimlerinin sonuna getirmek istediğimiz ekleri girmemizi sağlıyor.

```R
inner_join(superheroes_yeni, publishers_yeni, by = "publisher_id", suffix = c("_superhero", "_publisher"))

>
# A tibble: 7 x 6
  name_superhero alignment gender publisher_id name_publisher    yr_founded
  <chr>          <chr>     <chr>  <chr>        <chr>                  <int>
1 Magneto        bad       male   101          Marvel                  1939
2 Storm          good      female 101          Marvel                  1939
3 Mystique       bad       female 101          Marvel                  1939
4 Batman         good      male   102          DC                      1934
5 Joker          bad       male   102          DC                      1934
6 Catwoman       bad       female 102          DC                      1934
7 Hellboy        good      male   103          Dark Horse Comics       1986
```

Gördüğümüz gibi, name değişkenlerinin isimlerini ait oldukları tablolara göre değiştirdik. 

## Flights veri seti ile vaka çalışması

Bu derste öğrediğimiz bütün fonksiyonları kullanacağımız bir vaka çalışması yapacağız. 'nycflights13' kütüphanesindeki 'flights' veri setini kullanacağız. Öncelikle gerekli paketleri yükleyip, çağıralım.

```R
install.packages("nycflights13")
library(nycflights13)
library(tidyverse)
```

Veri setimizin değişkenlerini ve neyi ifade ettiklerini inceleyelim. 

```R
?flights
```

Veri setimiz 2013 yılında New York'tan gerçekleşen uçuşların bilgisini içeriyor. Değişkenlerimiz şu bilgileri açıklıyor:
* "year", "month", "day": Kalkış tarihini temsil ediyor.
* "dep_time", "arr_time": Gerçek kalkış ve varış zamanlarını temsil ediyor.
* "sched_dep_time", "sched_arr_time": Planlanan kalkış ve varış zamanlarını temsil ediyor.
* "dep_delay", "arr_delay": Kalkış ve varış gecikmelerini dakika cinsinden temsil ediyor.
* "carrier": Havayolu şirketinin kısa ismini temsil ediyor.
* "flight": Uçuş numarasını temsil ediyor.
* "tailnum": Uçak kuyruk numarasını temsil ediyor.
* "air_time": Uçağın havada kalma süresinin dakika cinsinden değerini temsil ediyor.
* "distance": Mil cinsinden iki havaalanı arasındaki uzaklığı temsil ediyor.
* "hour", "minute": Planlanan kalkışın saat ve dakikasını temsil ediyor.
* "time_hour": Uçuşun planlanan tarih ve saatini temsil ediyor.

Veri setimizin yapısına bakarak ya da veri setini görüntüleyerek değişkenleri ve gözlemleri daha iyi anlayabiliriz. 

```R
View(flights)
str(flights)

>
tibble [336,776 x 19] (S3: tbl_df/tbl/data.frame)
 $ year          : int [1:336776] 2013 2013 2013 2013 2013 2013 2013 2013 2013 2013 ...
 $ month         : int [1:336776] 1 1 1 1 1 1 1 1 1 1 ...
 $ day           : int [1:336776] 1 1 1 1 1 1 1 1 1 1 ...
 $ dep_time      : int [1:336776] 517 533 542 544 554 554 555 557 557 558 ...
 $ sched_dep_time: int [1:336776] 515 529 540 545 600 558 600 600 600 600 ...
 $ dep_delay     : num [1:336776] 2 4 2 -1 -6 -4 -5 -3 -3 -2 ...
 $ arr_time      : int [1:336776] 830 850 923 1004 812 740 913 709 838 753 ...
 $ sched_arr_time: int [1:336776] 819 830 850 1022 837 728 854 723 846 745 ...
 $ arr_delay     : num [1:336776] 11 20 33 -18 -25 12 19 -14 -8 8 ...
 $ carrier       : chr [1:336776] "UA" "UA" "AA" "B6" ...
 $ flight        : int [1:336776] 1545 1714 1141 725 461 1696 507 5708 79 301 ...
 $ tailnum       : chr [1:336776] "N14228" "N24211" "N619AA" "N804JB" ...
 $ origin        : chr [1:336776] "EWR" "LGA" "JFK" "JFK" ...
 $ dest          : chr [1:336776] "IAH" "IAH" "MIA" "BQN" ...
 $ air_time      : num [1:336776] 227 227 160 183 116 150 158 53 140 138 ...
 $ distance      : num [1:336776] 1400 1416 1089 1576 762 ...
 $ hour          : num [1:336776] 5 5 5 5 6 5 6 6 6 6 ...
 $ minute        : num [1:336776] 15 29 40 45 0 58 0 0 0 0 ...
 $ time_hour     : POSIXct[1:336776], format: "2013-01-01 05:00:00" "2013-01-01 05:00:00" "2013-01-01 05:00:00" ...
 ```
 
 Veri setimizle alakalı aşağıdaki soruları cevaplamaya çalışalım.
 
 1. 1 Ocak'ta olan uçuşlar hangileridir? Yılbaşı günü toplam kaç uçuş olmuştur?
 2. Kasım ve Aralık ayında olan uçuşları "kas_ar" isimli bir data frame olarak kaydettikten sonra ilk 5 ve son 5 gözlemine bakalım.
 3. kas_ar veri setinin son 5 gözlemine baktığımızda Kasım ve Aralık aylarında planlanan kalkışı olan fakat kalkış saati belli olmayan uçuşlar olduğunu görüyoruz. Peki kasım ve aralık aylarında kaç uçuşun kalkış saati belli değil ve en çok kalkış saati belli olmayan hava yolu şirketi hangisidir?
**Not** flights veri setinde havayolu şirketlerinin kısaltması mevcut. airlines veri setinden kısaltmalara ve şirketin tam ismine "joining" yöntemlerini kullanarak ulaşabiliriz.
 4. ExpressJet Airlines Inc. 480 kalkış saati belli olmayan uçuşuyla acaba güvenilmez bir havayolu şirketi mi? Bunu kalkış geçikmesi ve varış gecikmesi ile bulabiliriz. Bu şirketin kaç uçuşu gecikmeli kalkıp, inmiştir? 
 5. ExpressJet Airlines Inc. şirketinin 28440 gecikmeli uçuşu olduğunu öğrendik. Peki daha fazla gecikmeli uçuşu olan havayolu şirketi var mı?
 6. United Air Lines Inc. şirketinin çok fazla gecikmeli uçuşu olduğunu gördük. Peki bu şirketin diğerlerine göre uçuş sayısı daha fazla mı?
 7. Aslında gecikme oranını bulup ona göre şirketleri kıyaslamak daha mantıklı olacaktır. Dolayısıyla toplam uçuş içerisindeki gecikmeli uçuş oranlarını bulup sıralatalım.
 8. En hızlı uçuşlar hangileri? (air_time) 
 air_time, carrier, distance, origin, ve dest kolonlarına bakalım.
 9. En uzun mesafeli ilk 10 uçuş ve en kısa mesafeli ilk 10 uçuş hangileri? (distance)
 air_time, carrier, distance, origin, ve dest kolonlarına bakalım.
 10. İçinde "delay" kelimesi geçen değişkenlerle birlikte, distance, air_time, carrier, origin ve dest değişkenlerini seçip "ucuslar" isimli data frame oluşturalım.
 11. gain ve speed değişkenlerini oluşturup ucuslar veri setine tekrar atayalım.
* gain = dep_delay - arr_delay
* speed = distance / air_time * 60
 12. ucuslar veri setindeki speed değişkeninin özet istatistiklerini elde edelim. (mean, median, min, max, sd)
 13. Maximum hızlı uçuş hangisi?
 14. Havayolu şirketlerinin hız bakımından (speed) özet istatistikleri nedir?
 15. US kodlu havayolu şirketinin en fazla standart sapmaya sahip olduğunu gördük. Dolayısıyla bu şirket en fazla farklı noktalara uçuşu olan şirket olabilir. Gerçekten öyle mi?
 16. Havayolu şirketlerinin ortalama kalkış gecikme süreleri nedir?(dep_delay) Büyükten küçüğe doğru sıralayalım.
 17. Hangi ay ve günlerde ortalama kalkış gecikmesi fazla oluyor? (dep_delay)
 18. Hangi havaalanlarına ortalama varış gecikmesi fazla oluyor? (arr_delay ve dest değişkenleri) Ortalama varış gecikmesi bakımından ilk 10 havalimanını getirelim.
 19. CAE kodlu havalimanın tam ismi nedir? airports veri setinden join ile elde edelim. Bir önceki kodumuza devam edelim.
 20. Hangi havaalanlarında ortalama kalkış gecikmesi fazla oluyor? (dep_delay ve origin değişkenleri) Ortalama kalkış gecikmesi bakımından ilk 10 havalimanını getirelim.
 21. Bu uçuşlarda en fazla kullanılan uçak üreticisi ve modeli nedir? (manufacturer ve model değişkenleri)
**ipucu** planes veri seti tailnum id değişkeni ile birleştirilerek bulunabilir.
 22. Uçuşlardan önce mutlaka uçağın son durumu kontrol edilir ondan sonra kalkışa geçilir. Uçak üreticisi ve modeli bakımından kalkış süresi gecikme ortalaması fazla uçak üreticisi ve modeli nedir?
 23. flights veri setini kullanarak uzaklık (distance) değişkeninin özet istatistiklerini inceleyelim.
 24. Ortalama uzaklıktan fazla olan uzaklıkları "uzun", ortalama uzaklıktan düşük olan uzaklıkları "kısa" diye kategorikleştirip "mesafe_tipi" isimli değişken oluşturalım. Bu yeni oluşturduğumuz değişkenli veri setini yeni_ucuslar isimli veri setine atayalım.
 25. yeni_ucuslar ve planes veri setlerini kullanarak hangi ucak üreticisinin hangi modellerinin uzun mesafede, kısa mesafede ya da her iki mesafe türünde de kullanıldığını bulalım.
 
**Çözümler**

1.
Uçakların kalkış tarihini gösteren month ve day değişkenleri 1 Ocak'ı temsil edecek şekilde verimizi filtreleyebiliriz. 

```R
flights %>%
  filter(month == 1 & day ==1)
  
>
# A tibble: 842 x 19
    year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time arr_delay carrier flight
   <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>     <dbl> <chr>    <int>
 1  2013     1     1      517            515         2      830            819        11 UA        1545
 2  2013     1     1      533            529         4      850            830        20 UA        1714
 3  2013     1     1      542            540         2      923            850        33 AA        1141
 4  2013     1     1      544            545        -1     1004           1022       -18 B6         725
 5  2013     1     1      554            600        -6      812            837       -25 DL         461
 6  2013     1     1      554            558        -4      740            728        12 UA        1696
 7  2013     1     1      555            600        -5      913            854        19 B6         507
 8  2013     1     1      557            600        -3      709            723       -14 EV        5708
 9  2013     1     1      557            600        -3      838            846        -8 B6          79
10  2013     1     1      558            600        -2      753            745         8 AA         301
# ... with 832 more rows, and 8 more variables: tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>,
#   distance <dbl>, hour <dbl>, minute <dbl>, time_hour <dttm>

flights %>%
  filter(month == 1 & day ==1) %>%
  count()
  
>
# A tibble: 1 x 1
      n
  <int>
1   842
```
1 Ocak'ta 842 uçuş gerçekleşmiştir. 

2.
Önceki soruda olduğu gibi Kasım ve Aralık aylarındaki uçuşlara da month değişkenini filtreleyerek ulaşabiliriz.

```R
kas_ar <- flights %>%
  filter(month == 11 | month == 12)

head(kas_ar, 5)

>
# A tibble: 5 x 19
   year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time arr_delay carrier flight tailnum
  <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>     <dbl> <chr>    <int> <chr>  
1  2013    11     1        5           2359         6      352            345         7 B6         745 N568JB 
2  2013    11     1       35           2250       105      123           2356        87 B6        1816 N353JB 
3  2013    11     1      455            500        -5      641            651       -10 US        1895 N192UW 
4  2013    11     1      539            545        -6      856            827        29 UA        1714 N38727 
5  2013    11     1      542            545        -3      831            855       -24 AA        2243 N5CLAA 
# ... with 7 more variables: origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
#   minute <dbl>, time_hour <dttm>

tail(kas_ar, 5)

>
# A tibble: 5 x 19
   year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time arr_delay carrier flight tailnum
  <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>     <dbl> <chr>    <int> <chr>  
1  2013    12    31       NA            705        NA       NA            931        NA UA        1729 NA     
2  2013    12    31       NA            825        NA       NA           1029        NA US        1831 NA     
3  2013    12    31       NA           1615        NA       NA           1800        NA MQ        3301 N844MQ 
4  2013    12    31       NA            600        NA       NA            735        NA UA         219 NA     
5  2013    12    31       NA            830        NA       NA           1154        NA UA         443 NA     
# ... with 7 more variables: origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
#   minute <dbl>, time_hour <dttm>
```

head() ve taik() fonksiyonlarıyla ilk ve son 5 gözleme baktığımızda, veri setimizin 1 Kasım - 31 Aralık tarihleri arasındaki uçuşları içerdiğini görüyoruz.

3. 
Öncelikle airlines veri setini inceleyelim.

```R
str(airlines)
View(airlines)

>
tibble [16 x 2] (S3: tbl_df/tbl/data.frame)
 $ carrier: chr [1:16] "9E" "AA" "AS" "B6" ...
 $ name   : chr [1:16] "Endeavor Air Inc." "American Airlines Inc." "Alaska Airlines Inc." "JetBlue Airways" ...
```

kas_ar veri setimizde, planlanan kalkış saati olan ancak gerçek kalkış saati belli olmayan uçuşlar dep_time değişkeninde "NA" gözlemine sahip. Kalkış saati belli olmayan uçuş sayısını bulalım.

```R
sum(is.na(kas_ar$dep_time))

>
[1] 1258
```

1258 uçuşun kalkış saati belli değil. Peki, bu uçuşlar hangi havayolu şirketine ait? Havayolu şirketinin tam ismini airlines veri setinden getirebiliriz. 

```R
kas_ar %>%
  filter(is.na(dep_time)) %>%
  count(carrier, sort = T) %>%
  left_join(airlines)
  
>
Joining, by = "carrier"
# A tibble: 13 x 3
   carrier     n name                       
   <chr>   <int> <chr>                      
 1 EV        480 ExpressJet Airlines Inc.   
 2 MQ        186 Envoy Air                  
 3 AA        125 American Airlines Inc.     
 4 UA        114 United Air Lines Inc.      
 5 9E        109 Endeavor Air Inc.          
 6 US         92 US Airways Inc.            
 7 B6         74 JetBlue Airways            
 8 WN         30 Southwest Airlines Co.     
 9 DL         19 Delta Air Lines Inc.       
10 VX         16 Virgin America             
11 YV          7 Mesa Airlines Inc.         
12 FL          5 AirTran Airways Corporation
13 F9          1 Frontier Airlines Inc.  
```

ExpressJet Airlines Inc. şirketi 480 kalkış saati belli olmayan uçuşla ilk sırada. 

4.
Kalkış gecikmesi ya da varış gecikmesi sıfırdan fazlaysa, uçuş rötarlı demektir. 

```R
flights %>%
  filter(carrier == "EV") %>%
  filter(dep_delay > 0 | arr_delay > 0) %>%
  count()
  
>
# A tibble: 1 x 1
      n
  <int>
1 28440
```

ExpressJet Airlines Inc. şirketinin 28440 gecikmeli uçuşu var.

5.
Sadece ExpressJet Airlines Inc. şirketine göre filtrelemeden bütün şirketler için gecikmeli uçuşları filtreleyelim ve tam isimlerini getirelim.

```R
flights %>%
  filter(dep_delay > 0 | arr_delay > 0) %>%
  group_by(carrier) %>%
  count(sort = T) %>%
  left_join(airlines)
  
>
Joining, by = "carrier"
# A tibble: 16 x 3
# Groups:   carrier [16]
   carrier     n name                       
   <chr>   <int> <chr>                      
 1 UA      32877 United Air Lines Inc.      
 2 B6      28618 JetBlue Airways            
 3 EV      28440 ExpressJet Airlines Inc.   
 4 DL      21528 Delta Air Lines Inc.       
 5 AA      14200 American Airlines Inc.     
 6 MQ      12780 Envoy Air                  
 7 9E       8645 Endeavor Air Inc.          
 8 US       8359 US Airways Inc.            
 9 WN       7569 Southwest Airlines Co.     
10 VX       2754 Virgin America             
11 FL       2163 AirTran Airways Corporation
12 F9        477 Frontier Airlines Inc.     
13 YV        293 Mesa Airlines Inc.         
14 AS        290 Alaska Airlines Inc.       
15 HA        129 Hawaiian Airlines Inc.     
16 OO         11 SkyWest Airlines Inc.
```

United Air Lines Inc. şirketinin daha fazla gecikmeli uçuşu olduğunu görüyoruz. ExpressJet Airlines Inc., gecikmeli uçuş sıralamasında 3. sırada yer alıyor. 

6.
Veri setimizi havayolu şirketlerine göre (carrier) gruplayarak her birinin uçuş sayısını bulabiliriz. 

```R
flights %>%
  group_by(carrier) %>%
  count(sort = T) %>%
  left_join(airlines)

>
Joining, by = "carrier"
# A tibble: 16 x 3
# Groups:   carrier [16]
   carrier     n name                       
   <chr>   <int> <chr>                      
 1 UA      58665 United Air Lines Inc.      
 2 B6      54635 JetBlue Airways            
 3 EV      54173 ExpressJet Airlines Inc.   
 4 DL      48110 Delta Air Lines Inc.       
 5 AA      32729 American Airlines Inc.     
 6 MQ      26397 Envoy Air                  
 7 US      20536 US Airways Inc.            
 8 9E      18460 Endeavor Air Inc.          
 9 WN      12275 Southwest Airlines Co.     
10 VX       5162 Virgin America             
11 FL       3260 AirTran Airways Corporation
12 AS        714 Alaska Airlines Inc.       
13 F9        685 Frontier Airlines Inc.     
14 YV        601 Mesa Airlines Inc.         
15 HA        342 Hawaiian Airlines Inc.     
16 OO         32 SkyWest Airlines Inc. 
```

Gecikmeli uçuş sıralamasıyla, uçuş sayısı sıralamasının genellikle aynı olduğunu görüyoruz. Peki, uçuş sayısı fazla olan havayolu şirketinin gecikmeli uçuşu da daha fazla olur diyebilir miyiz?

7.
Gecikme oranlarını bulmak için öncelikle sadece gecikmeli uçuşların olduğu bir veri seti oluşturarak havayolu şirketine göre gruplayalım.

```R
gecikmeliler <- flights %>%
  group_by(carrier) %>%
  filter(dep_delay > 0 | arr_delay > 0) %>%
  count(sort=TRUE) %>%
  left_join(airlines)

gecikmeliler

>
# A tibble: 16 x 3
# Groups:   carrier [16]
   carrier     n name                       
   <chr>   <int> <chr>                      
 1 UA      32877 United Air Lines Inc.      
 2 B6      28618 JetBlue Airways            
 3 EV      28440 ExpressJet Airlines Inc.   
 4 DL      21528 Delta Air Lines Inc.       
 5 AA      14200 American Airlines Inc.     
 6 MQ      12780 Envoy Air                  
 7 9E       8645 Endeavor Air Inc.          
 8 US       8359 US Airways Inc.            
 9 WN       7569 Southwest Airlines Co.     
10 VX       2754 Virgin America             
11 FL       2163 AirTran Airways Corporation
12 F9        477 Frontier Airlines Inc.     
13 YV        293 Mesa Airlines Inc.         
14 AS        290 Alaska Airlines Inc.       
15 HA        129 Hawaiian Airlines Inc.     
16 OO         11 SkyWest Airlines Inc. 
```

Daha sonra bir önceki soruda yaptığımız gibi havayolu şirketlerini toplam uçuş sayılarına göre gruplayıp ayrı bir veri seti olarak atayalım.

```R
toplam_ucus <- flights %>%
  count(carrier, sort=TRUE) %>%
  left_join(airlines)

toplam_ucus

>
# A tibble: 16 x 3
   carrier     n name                       
   <chr>   <int> <chr>                      
 1 UA      58665 United Air Lines Inc.      
 2 B6      54635 JetBlue Airways            
 3 EV      54173 ExpressJet Airlines Inc.   
 4 DL      48110 Delta Air Lines Inc.       
 5 AA      32729 American Airlines Inc.     
 6 MQ      26397 Envoy Air                  
 7 US      20536 US Airways Inc.            
 8 9E      18460 Endeavor Air Inc.          
 9 WN      12275 Southwest Airlines Co.     
10 VX       5162 Virgin America             
11 FL       3260 AirTran Airways Corporation
12 AS        714 Alaska Airlines Inc.       
13 F9        685 Frontier Airlines Inc.     
14 YV        601 Mesa Airlines Inc.         
15 HA        342 Hawaiian Airlines Inc.     
16 OO         32 SkyWest Airlines Inc.
```

gecikmeliler veri setindeki gecikmeli uçuş sayılarını ifade eden n değişkenini, toplam_ucus veri setine 'gecikmeli' değişken ismiyle ekleyelim. Artık toplam_ucus veri setinde hem toplam ucuş sayıları hem de gecikmeli uçuş sayıları mevcut. Bunları birbirine oranlayabiliriz. 

```R
toplam_ucus$gecikmeli <- gecikmeliler$n

toplam_ucus$oran <- toplam_ucus$gecikmeli / toplam_ucus$n

toplam_ucus %>%
  arrange(desc(oran))

>
# A tibble: 16 x 5
   carrier     n name                        gecikmeli  oran
   <chr>   <int> <chr>                           <int> <dbl>
 1 AS        714 Alaska Airlines Inc.              477 0.668
 2 FL       3260 AirTran Airways Corporation      2163 0.663
 3 WN      12275 Southwest Airlines Co.           7569 0.617
 4 UA      58665 United Air Lines Inc.           32877 0.560
 5 VX       5162 Virgin America                   2754 0.534
 6 EV      54173 ExpressJet Airlines Inc.        28440 0.525
 7 B6      54635 JetBlue Airways                 28618 0.524
 8 MQ      26397 Envoy Air                       12780 0.484
 9 YV        601 Mesa Airlines Inc.                290 0.483
10 9E      18460 Endeavor Air Inc.                8359 0.453
11 DL      48110 Delta Air Lines Inc.            21528 0.447
12 AA      32729 American Airlines Inc.          14200 0.434
13 F9        685 Frontier Airlines Inc.            293 0.428
14 US      20536 US Airways Inc.                  8645 0.421
15 HA        342 Hawaiian Airlines Inc.            129 0.377
16 OO         32 SkyWest Airlines Inc.              11 0.344
```

Gecikmeli uçuş oranlarına göre havayolu şirketlerini sıraladığımızda normalde gecikmeli uçuş sayılarında 14. sırada olan Alaska Airlines Inc. şirketinin ilk sırada olduğunu görüyoruz. Gecikmeli uçuş oranına göre havayolu şirketlerini sıralamanın daha doğru bir analiz olduğunu söyleyebiliriz. 

8.
```R
flights %>%
  arrange(air_time) %>%
  select(air_time, carrier, distance, origin, dest)
  
>
# A tibble: 336,776 x 5
   air_time carrier distance origin dest 
      <dbl> <chr>      <dbl> <chr>  <chr>
 1       20 EV           116 EWR    BDL  
 2       20 EV           116 EWR    BDL  
 3       21 EV           116 EWR    BDL  
 4       21 EV            80 EWR    PHL  
 5       21 EV           116 EWR    BDL  
 6       21 EV            80 EWR    PHL  
 7       21 US           184 LGA    BOS  
 8       21 9E            94 JFK    PHL  
 9       21 EV           116 EWR    BDL  
10       21 EV           116 EWR    BDL  
# ... with 336,766 more rows
```

air_time değişkenine göre sıralayarak en kısa süreli uçuşları elde etmiş olduk. Ancak, tam olarak hızlı uçuşları bulmak istersek 7. soruda yaptığımız gibi air_time ve distance değişkenlerinin oranını kullanarak sıralamak daha doğru olacaktır.

9.
distance değişkenine göre sıralayarak en uzun ve en kısa mesafeli uçuşları bulabiliriz.

```R
flights %>%
  arrange(desc(distance)) %>%
  select(distance, air_time, carrier, distance, origin, dest) %>%
  head(10)
 
>
# A tibble: 10 x 5
   distance air_time carrier origin dest 
      <dbl>    <dbl> <chr>   <chr>  <chr>
 1     4983      659 HA      JFK    HNL  
 2     4983      638 HA      JFK    HNL  
 3     4983      616 HA      JFK    HNL  
 4     4983      639 HA      JFK    HNL  
 5     4983      635 HA      JFK    HNL  
 6     4983      611 HA      JFK    HNL  
 7     4983      612 HA      JFK    HNL  
 8     4983      645 HA      JFK    HNL  
 9     4983      640 HA      JFK    HNL  
10     4983      633 HA      JFK    HNL  

flights %>%
  arrange(distance) %>%
  select(distance, air_time, carrier, distance, origin, dest) %>%
  head(10)

>
# A tibble: 10 x 5
   distance air_time carrier origin dest 
      <dbl>    <dbl> <chr>   <chr>  <chr>
 1       17       NA US      EWR    LGA  
 2       80       30 EV      EWR    PHL  
 3       80       30 EV      EWR    PHL  
 4       80       28 EV      EWR    PHL  
 5       80       32 EV      EWR    PHL  
 6       80       29 EV      EWR    PHL  
 7       80       22 EV      EWR    PHL  
 8       80       25 EV      EWR    PHL  
 9       80       30 EV      EWR    PHL  
10       80       27 EV      EWR    PHL 
```

10.
```R
ucuslar <- flights %>%
  select(contains("delay"), distance, air_time, carrier, origin, dest)

ucuslar

>
# A tibble: 336,776 x 7
   dep_delay arr_delay distance air_time carrier origin dest 
       <dbl>     <dbl>    <dbl>    <dbl> <chr>   <chr>  <chr>
 1         2        11     1400      227 UA      EWR    IAH  
 2         4        20     1416      227 UA      LGA    IAH  
 3         2        33     1089      160 AA      JFK    MIA  
 4        -1       -18     1576      183 B6      JFK    BQN  
 5        -6       -25      762      116 DL      LGA    ATL  
 6        -4        12      719      150 UA      EWR    ORD  
 7        -5        19     1065      158 B6      EWR    FLL  
 8        -3       -14      229       53 EV      LGA    IAD  
 9        -3        -8      944      140 B6      JFK    MCO  
10        -2         8      733      138 AA      LGA    ORD  
# ... with 336,766 more rows
```

İçinde "delay" kelimesi geçen iki adet değişkenimiz olduğunu görüyoruz.

11.
```R
ucuslar <- ucuslar %>%
  mutate(gain = dep_delay - arr_delay,
         speed = distance / air_time * 60)

ucuslar

>
# A tibble: 336,776 x 9
   dep_delay arr_delay distance air_time carrier origin dest   gain speed
       <dbl>     <dbl>    <dbl>    <dbl> <chr>   <chr>  <chr> <dbl> <dbl>
 1         2        11     1400      227 UA      EWR    IAH      -9  370.
 2         4        20     1416      227 UA      LGA    IAH     -16  374.
 3         2        33     1089      160 AA      JFK    MIA     -31  408.
 4        -1       -18     1576      183 B6      JFK    BQN      17  517.
 5        -6       -25      762      116 DL      LGA    ATL      19  394.
 6        -4        12      719      150 UA      EWR    ORD     -16  288.
 7        -5        19     1065      158 B6      EWR    FLL     -24  404.
 8        -3       -14      229       53 EV      LGA    IAD      11  259.
 9        -3        -8      944      140 B6      JFK    MCO       5  405.
10        -2         8      733      138 AA      LGA    ORD     -10  319.
# ... with 336,766 more rows
```

12.
```R
ucuslar %>%
  summarise(mean_speed = mean(speed, na.rm = T),
            median_speed = median(speed, na.rm = T),
            max_speed = max(speed, na.rm = T),
            min_speed = min(speed, na.rm = T),
            sd_speed = sd(speed, na.rm = T))
           
>
# A tibble: 1 x 5
  mean_speed median_speed max_speed min_speed sd_speed
       <dbl>        <dbl>     <dbl>     <dbl>    <dbl>
1       394.         404.      703.      76.8     60.6
```

13.
En hızlı uçuşu ucuslar veri setini kullanarak iki yolla bulabiliriz.

```R
#1. yol:
ucuslar %>%
  top_n(1, speed)

#2. yol:
ucuslar %>%
  filter(speed == max(speed, na.rm = T))
  
>
# A tibble: 1 x 9
  dep_delay arr_delay distance air_time carrier origin dest   gain speed
      <dbl>     <dbl>    <dbl>    <dbl> <chr>   <chr>  <chr> <dbl> <dbl>
1         9       -14      762       65 DL      LGA    ATL      23  703.

```

14.
speed değişkeninin özet istatistiklerini bulmuştuk. Şimdi havayolu şirketlerinin kendi içindeki speed verilerine göre istatistikleri bulmamız gerekiyor. Bunu carrier değişkenine göre gruplayarak yapabiliriz. Maksimum hıza göre de veri setini sıralayabiliriz.

```R
ucuslar %>%
  group_by(carrier) %>%
  summarise(mean_speed = mean(speed, na.rm = T),
            median_speed = median(speed, na.rm = T),
            max_speed = max(speed, na.rm = T),
            min_speed = min(speed, na.rm = T),
            sd_speed = sd(speed, na.rm = T)) %>%
  arrange(desc(sd_speed))
  
>
# A tibble: 16 x 6
   carrier mean_speed median_speed max_speed min_speed sd_speed
   <chr>        <dbl>        <dbl>     <dbl>     <dbl>    <dbl>
 1 US            342.         338.      527.      76.8     71.7
 2 9E            345.         350.      518.      92.5     65.1
 3 B6            400.         416.      557.      84.7     64.9
 4 YV            332.         320.      473.     120       62.9
 5 EV            363.         367.      650.     123.      52.2
 6 MQ            368.         375.      508      132.      50.8
 7 UA            421.         429.      551.     107.      48.4
 8 AA            417.         423.      556.     117.      47.5
 9 DL            418.         424.      703.     123.      43.6
10 WN            401.         403.      504.     172.      38.4
11 FL            394.         394.      532.     280.      34.2
12 F9            425.         424.      498.     350.      27.8
13 OO            366.         370.      419      275.      25.9
14 VX            446.         446.      519.     363.      23.1
15 AS            444.         445.      520.     368.      21.7
16 HA            480.         481.      515.     433.      15.8
```

15.
n_distinct() fonksiyonuyla dest değişkeninin kaç farklı gözleme sahip olduğunu bulabiliriz.

```R
flights %>%
  group_by(carrier) %>%
  summarise(farklı_nokta = n_distinct(dest)) %>%
  arrange(desc(farklı_nokta))

>
# A tibble: 16 x 2
   carrier farklı_nokta
   <chr>          <int>
 1 EV                61
 2 9E                49
 3 UA                47
 4 B6                42
 5 DL                40
 6 MQ                20
 7 AA                19
 8 WN                11
 9 US                 6
10 OO                 5
11 VX                 5
12 FL                 3
13 YV                 3
14 AS                 1
15 F9                 1
16 HA                 1
```

Hıza göre standart sapmanın, daha fazla farklı noktaya uçuşa sahip olmakla alakası olmadığını görüyoruz. 

16.
```R
flights %>%
  group_by(carrier) %>%
  summarise(mean_delay = mean(dep_delay, na.rm = T)) %>%
  arrange(desc(mean_delay))
  
>
# A tibble: 16 x 2
   carrier mean_delay
   <chr>        <dbl>
 1 F9           20.2 
 2 EV           20.0 
 3 YV           19.0 
 4 FL           18.7 
 5 WN           17.7 
 6 9E           16.7 
 7 B6           13.0 
 8 VX           12.9 
 9 OO           12.6 
10 UA           12.1 
11 MQ           10.6 
12 DL            9.26
13 AA            8.59
14 AS            5.80
15 HA            4.90
16 US            3.78
```

F9 havayolu şirketinin ortalama olarak daha gecikmeli uçuşlar gerçekleştirdiğini söyleyebiliriz.

17.
Bir önceki soruda havayolu şirketine göre gruplayarak ortalama gecikme sürelerini bulmuştuk. Şimdi month ve day değişkenlerine göre gruplamamız gerekiyor.

```R
flights %>%
  group_by(month, day) %>%
  summarise(mean_delay = mean(dep_delay, na.rm = T)) %>%
  arrange(desc(mean_delay))
  
>
# A tibble: 365 x 3
# Groups:   month [12]
   month   day mean_delay
   <int> <int>      <dbl>
 1     3     8       83.5
 2     7     1       56.2
 3     9     2       53.0
 4     7    10       52.9
 5    12     5       52.3
 6     5    23       51.1
 7     9    12       50.0
 8     6    28       48.8
 9     6    24       47.2
10     7    22       46.7
# ... with 355 more rows
```

8 Mart'ta ortalama gecikme süresinin en fazla olduğunu görüyoruz.

18.
```R
flights %>%
  group_by(dest) %>%
  summarise(mean_arr_delay = mean(arr_delay, na.rm = T)) %>%
  top_n(10, mean_arr_delay)
  
>
# A tibble: 10 x 2
   dest  mean_arr_delay
   <chr>          <dbl>
 1 CAE             41.8
 2 CAK             19.7
 3 DSM             19.0
 4 GRR             18.2
 5 JAC             28.1
 6 MSN             20.2
 7 OKC             30.6
 8 RIC             20.1
 9 TUL             33.7
10 TYS             24.1
```

CAE havaalanında ortalama varış gecikmesinin en fazla olduğunu görüyoruz. Büyük ve uçuş sayısının fazla olduğu bir havaalanı olabilir.

19.
airports veri setini inceleyelim.

```R
View(airports)
str(airports)

>
tibble [1,458 x 8] (S3: tbl_df/tbl/data.frame)
 $ faa  : chr [1:1458] "04G" "06A" "06C" "06N" ...
 $ name : chr [1:1458] "Lansdowne Airport" "Moton Field Municipal Airport" "Schaumburg Regional" "Randall Airport" ...
 $ lat  : num [1:1458] 41.1 32.5 42 41.4 31.1 ...
 $ lon  : num [1:1458] -80.6 -85.7 -88.1 -74.4 -81.4 ...
 $ alt  : num [1:1458] 1044 264 801 523 11 ...
 $ tz   : num [1:1458] -5 -6 -6 -5 -5 -5 -5 -5 -5 -8 ...
 $ dst  : chr [1:1458] "A" "A" "A" "A" ...
 $ tzone: chr [1:1458] "America/New_York" "America/Chicago" "America/Chicago" "America/New_York" ...
 - attr(*, "spec")=
  .. cols(
  ..   id = col_double(),
  ..   name = col_character(),
  ..   city = col_character(),
  ..   country = col_character(),
  ..   faa = col_character(),
  ..   icao = col_character(),
  ..   lat = col_double(),
  ..   lon = col_double(),
  ..   alt = col_double(),
  ..   tz = col_double(),
  ..   dst = col_character(),
  ..   tzone = col_character()
  .. )
```

flights veri setinde dest değişkeninde bulunan havaalanı kodlarının, airports veri setinde faa değişkeninde bulunduğunu görüyoruz. Birleştirme işlemini bu değişkenlere göre yapacağız.

```R
flights %>%
  group_by(dest) %>%
  summarise(mean_arr_delay = mean(arr_delay, na.rm = T)) %>%
  top_n(10, mean_arr_delay) %>%
  left_join(airports, by = c("dest" = "faa"))
  
>
# A tibble: 10 x 9
   dest  mean_arr_delay name                            lat    lon   alt    tz dst   tzone           
   <chr>          <dbl> <chr>                         <dbl>  <dbl> <dbl> <dbl> <chr> <chr>           
 1 CAE             41.8 Columbia Metropolitan          33.9  -81.1   236    -5 A     America/New_York
 2 CAK             19.7 Akron Canton Regional Airport  40.9  -81.4  1228    -5 A     America/New_York
 3 DSM             19.0 Des Moines Intl                41.5  -93.7   958    -6 A     America/Chicago 
 4 GRR             18.2 Gerald R Ford Intl             42.9  -85.5   794    -5 A     America/New_York
 5 JAC             28.1 Jackson Hole Airport           43.6 -111.   6451    -7 A     America/Denver  
 6 MSN             20.2 Dane Co Rgnl Truax Fld         43.1  -89.3   887    -6 A     America/Chicago 
 7 OKC             30.6 Will Rogers World              35.4  -97.6  1295    -6 A     America/Chicago 
 8 RIC             20.1 Richmond Intl                  37.5  -77.3   167    -5 A     America/New_York
 9 TUL             33.7 Tulsa Intl                     36.2  -95.9   677    -6 A     America/Chicago 
10 TYS             24.1 Mc Ghee Tyson                  35.8  -84.0   981    -5 A     America/New_York
```

CAE kodlu havaalanının Columbia Metropolitan olduğunu bulduk.

20.
```R
flights %>%
  group_by(origin) %>%
  summarise(mean_delay = mean(dep_delay, na.rm = T)) %>%
  top_n(10, mean_delay) %>%
  left_join(airports, by = c("origin" = "faa"))

>
# A tibble: 3 x 9
  origin mean_delay name                  lat   lon   alt    tz dst   tzone           
  <chr>       <dbl> <chr>               <dbl> <dbl> <dbl> <dbl> <chr> <chr>           
1 EWR          15.1 Newark Liberty Intl  40.7 -74.2    18    -5 A     America/New_York
2 JFK          12.1 John F Kennedy Intl  40.6 -73.8    13    -5 A     America/New_York
3 LGA          10.3 La Guardia           40.8 -73.9    22    -5 A     America/New_York
```

Newark Liberty International havaalanından kalkan uçuşların ortalama kalkış gecikmesinin daha yüksek olduğunu görüyoruz.

21.
planes veri setini inceleyelim.

```R
View(planes)
str(planes)

>
tibble [3,322 x 9] (S3: tbl_df/tbl/data.frame)
 $ tailnum     : chr [1:3322] "N10156" "N102UW" "N103US" "N104UW" ...
 $ year        : int [1:3322] 2004 1998 1999 1999 2002 1999 1999 1999 1999 1999 ...
 $ type        : chr [1:3322] "Fixed wing multi engine" "Fixed wing multi engine" "Fixed wing multi engine" "Fixed wing multi engine" ...
 $ manufacturer: chr [1:3322] "EMBRAER" "AIRBUS INDUSTRIE" "AIRBUS INDUSTRIE" "AIRBUS INDUSTRIE" ...
 $ model       : chr [1:3322] "EMB-145XR" "A320-214" "A320-214" "A320-214" ...
 $ engines     : int [1:3322] 2 2 2 2 2 2 2 2 2 2 ...
 $ seats       : int [1:3322] 55 182 182 182 55 182 182 182 182 182 ...
 $ speed       : int [1:3322] NA NA NA NA NA NA NA NA NA NA ...
 $ engine      : chr [1:3322] "Turbo-fan" "Turbo-fan" "Turbo-fan" "Turbo-fan" ...
```

Veri setlerinde hem year hem de tailnum değişkenleri olmak üzere iki ortak isimli değişken olduğu için, 'by=' argümanını da kullanmamız gerekiyor.

```R
flights %>%
  inner_join(planes, , by = "tailnum") %>%
  count(manufacturer, model, sort=TRUE)
  
>
# A tibble: 147 x 3
   manufacturer                  model               n
   <chr>                         <chr>           <int>
 1 AIRBUS                        A320-232        31278
 2 EMBRAER                       EMB-145LR       28027
 3 EMBRAER                       ERJ 190-100 IGW 23716
 4 AIRBUS INDUSTRIE              A320-232        14553
 5 EMBRAER                       EMB-145XR       14051
 6 BOEING                        737-824         13809
 7 BOMBARDIER INC                CL-600-2D24     11807
 8 BOEING                        737-7H4         10389
 9 BOEING                        757-222          9150
10 MCDONNELL DOUGLAS AIRCRAFT CO MD-88            8932
# ... with 137 more rows
```

En fazla kullanılan uçağın AIRBUS tarafında üretilen A320-232 modeli olduğunu görüyoruz.

22.
```R
flights %>%
  inner_join(planes) %>%
  group_by(manufacturer, model) %>%
  summarise(mean_dep_delay = mean(dep_delay, na.rm = T)) %>%
  arrange(desc(mean_dep_delay))
  
>
# A tibble: 11 x 3
# Groups:   manufacturer [5]
   manufacturer     model           mean_dep_delay
   <chr>            <chr>                    <dbl>
 1 AIRBUS INDUSTRIE A321-231                 24.0 
 2 BOMBARDIER INC   CL-600-2D24              19.7 
 3 BOEING           737-924ER                13.8 
 4 AIRBUS           A320-214                 13.3 
 5 EMBRAER          ERJ 190-100 IGW          10.5 
 6 BOEING           737-8H4                   8.53
 7 BOEING           737-990ER                 8.48
 8 AIRBUS           A321-231                  7.31
 9 AIRBUS           A320-232                  5.42
10 AIRBUS           A321-211                  1.57
11 AIRBUS           A330-243                 -1.86
```

AIRBUS INDUSTRIE tarafından üretilen A321-231 model uçakla gerçekleştirilen uçuşların ortalama kalkış gecikmesinin daha fazla olduğunu görüyoruz. Uçuş öncesi kontroller bu model için daha uzun sürüyor olabilir.

23.
distance değişkeninde bilinmeyen değerler olup olmadığını kontrol edip, istatistikleri ona göre bulalım. (na.rm=T argümanının gerekliliğini görmek için)

```R
sum(is.na(flights$distance))

>
[1] 0

flights %>%
  summarise(mean_dist = mean(distance),
            max_dist = max(distance),
            min_dist = min(distance),
            median_dist = median(distance),
            sd_dist = sd(distance))
     
>
# A tibble: 1 x 5
  mean_dist max_dist min_dist median_dist sd_dist
      <dbl>    <dbl>    <dbl>       <dbl>   <dbl>
1     1040.     4983       17         872    733.
```

En uzun uçuşun 4983 mil olduğunu görüyoruz. 

24.
```R
yeni_ucuslar <- flights %>%
  mutate(mesafe_tipi = case_when(distance > mean(distance) ~ "uzun",
                                 distance < mean(distance) ~ "kısa"))
```

25.
```R
yeni_ucuslar %>%
  inner_join(planes, by = "tailnum) %>%
  count(manufacturer, model, mesafe_tipi, sort=T)
  
>
# A tibble: 248 x 4
   manufacturer     model           mesafe_tipi     n
   <chr>            <chr>           <chr>       <int>
 1 EMBRAER          EMB-145LR       kısa        27927
 2 EMBRAER          ERJ 190-100 IGW kısa        21065
 3 AIRBUS           A320-232        uzun        19091
 4 AIRBUS           A320-232        kısa        12187
 5 EMBRAER          EMB-145XR       kısa        10504
 6 BOMBARDIER INC   CL-600-2D24     kısa        10497
 7 BOEING           737-824         uzun         8986
 8 AIRBUS INDUSTRIE A320-232        uzun         8626
 9 BOEING           757-222         uzun         8377
10 BOMBARDIER INC   CL-600-2C10     kısa         8130
# ... with 238 more rows
```

EMBRAER üreticisinin EMB-145LR model uçağının kısa mesafe uçuşlarda sıkça kullanıldığını görüyoruz. AIRBUS üreticisinin A320-232 model uçağının ise hem kısa hem uzun mesafelerde kullanıldığını görüyoruz. 
