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
* [gather()](#gather())
* spread()
* unite()
* dplyr ile join işlemleri


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

## transmute() fonksiyonu
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
## count() fonskiyonu
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
## top_n() fonskiyonu
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

# gather()

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
