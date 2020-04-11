# Lesson 3

## Başlıklar:
yazılacak

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

