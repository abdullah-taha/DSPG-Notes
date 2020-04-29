# Ders 4

## Başlıklar:
* [Giriş](#giriş) 
* [ggplot2](#ggplot2) 
* Grafik çeşitleri - Abdullah
* Tek değişkenli grafikler (sürekli)-(continouse variables) - Abdullah
* Tek değişkenli grafikler (kesikli)-(discrete variable) - Abdullah
* İki Değişkenli Grafikler (ikisi sürekli) - Abdullah
* İki Değişkenli Grafikler (biri kesik biri sürekli) - Abdullah
* Grafik ile özet istatistik Belirtme - Abdullah
* [Bar Grafiği ile Oran Gösterme](#BarGrafiğiOranGösterme) - Petek
* [Facetting](#Facetting) - Petek 
* [Koordinat Sistemleri](#KoordinatSistemleri) - Petek
* [Temalar](#Temalar) - Petek
* TED: The best stats you've ever seen | Hans Rosling
* ggplot2 ile Yapılabilecek Diğer Şeyler 
* Case Study 1 (Genel Egzersiz) : mpg veri seti
* Case Study 2: gapminder verisi ile animasyonlu görselleştirme

## Giriş

*Inan hoca: "Geçirdiğimiz üç içerisinde R okur yazarlığını belli bir noktaya getirebilimişsinizdir. Üç hafta içinde çok muhteşem olmak ve yetkin hissetmek mümkün değil tabi ki. Ama en azından okur yazarlığınızı artık geliştirdiğinizi, hatalarınızı bulabildiğinizi, ve yaptıkça daha iyi olacağınızı hissediyorsanız doğru yoldasınız demek. Bundan sonra daha çok veri bilimi prespektifinden bakmaya başlıyor olacağız. Kullandığımız araçların teknik kısmını öğreniyor olacağız ama asıl amacımız bunları farklı durumlarda kullanıp onlardan nasıl bir hikaye çıkarabileceğimizi, nasıl bir değer üretebileceğimizi, ve nasıl bir bilgi ortaya çıkarabileceğimizi görmektir. Bu hafta veri görselleştirmeyi öğreniyor olacağız ve aklınıza bu tür sorular gelebilir: "peki şunu yapabilecek miyim?","peki bu kutuphanede bu özellik ta var mı?" . paketlerin öğrenmenin bir sonu yok, bir çok şey yapabilmek mümkün farklı paketlerle. Biz burada bu aletlerin nasıl kullanıldığını ve yaygın özelliklerini tartışıyor olacağız. Bundan sonra sizin yaratıcılığınıza bırakıp case study üzerine hikaye anlatmaya doğru ilerleyeceğiz."*

Geçen haftalarda veriyi kesip biömeyi, filtrelemeyi, istatistikleri çıkarmayı öğrenmiştik. Şimdi ise kesip biçtiğimiz veriyi karşı tarafa anlayabilecekleri şekilde sunmayı öğreneceğiz. bu süreçte veri görselleştirme büyük bir rol oynar.

## ggplot2
ggplot2 veri görselleştirme konsunda dünyada yaygın kullanınlan bir R paketi. Temelde 3 katmandan oluşur:
1. Data katmanı: bu katmanda kullanmak istediğimiz veri kaynağını beliriyoruz
1. Aesthetics katmanı: bu katmanda veri kayanağından plot etmek istediğimiz değişkenleri belirliyoruz. Üstelik neye göre renklendirelim ve neye göre şeffaflandıralım gibi soruları bu katmanda da belirlenir
1. Geometries katmanı: bu katmanda çizmek istediğimiz gradiği belirliyoruz (nokta grafiği, çizgi grafiği, bar grafizi vs.)
<img src=".images/ggplot_layers.png">

## Bar Grafiği ile Oran Gösterme

Bar grafiği ile elmas veri setimizin belirli kategorilerinde kaçar adet elmas olduğunu gösterebiliyorduk. 
Sıklıkları bar grafiği ile gösterebiliyorduk da diyebiliriz.
Şimdi bu sıklıkların oranlarını bar grafiği ile nasıl gösterebiliriz ona bakalım.

Sıklık oranlarını görmek aslında her kategori için o kategorinin elmasların yüzde kaçını oluşturduğunu görmek demek.

Kategorilerimizi berraklık değişkeni oluştursun, çeşitli berraklıkta elmaslarımız olduğunu biliyoruz. Bunların her birinden kaç tane olduğunu gösteren bar grafiğini çizelim.
```R
ggplot(diamonds)+
  geom_bar(aes(x=clarity))
```
Bu komut bize aşağıdaki grafiği veriyor.

<img src=".images/plot_bar_oran_1.png">

Oranları görmek istersek yapmamız gereken y eksenini oranları gösterecek şekilde tanımlamak. 

Bunu aes() fonksiyonundaki y argümanına ..prop.. atayarak yapacağız. ..prop.. R'a oranları göstermesini istediğimizi söylememize yarayan bir anahtar kelime. 

Fakat bir argümanı daha tanımlamamız gerekiyor bu da grup argümanı, eğer bu argümanı sağlamazsak R her bir kategori için bir grup oluşturacak ve oranları bu gruplara göre alacak dolayısıyla bütün oranları 1 olarak göreceğiz. Bizim istediğimiz bütün kategorileri bir grup olarak alması ve her kategorideki elmasları total elmaslara oranlaması, dolayısıyla R'a bir tane grup istediğimizi belirtmemiz gerekiyor.
```R
elmaslar +
  geom_bar(aes(x=clarity, y=..prop.., group=1))
```
Bu komut bize aşağıdaki grafiği veriyor. Y eksenindeki değerlerin değişimini gözlemleyebilirsiniz.

<img src=".images/plot_bar_oran2.png">

Grafiğimizi daha anlaşılır kılmak için y ekseninde kategorilerin total elmasların yüzde kaçını oluşturduklarını görmek istiyoruz. Y eksenindeki her değeri 100 ile çarparsak istediğimiz grafiği elde edebiliriz.
```R
elmaslar +
  geom_bar(aes(x=clarity, y=..prop..*100, group=1))
```
Bu komut bize aşağıdaki grafiği veriyor. Artık her berraklık kategorisindeki elmas sayılarının total elmasların yüzde kaçını oluşturduklarını net bir şekilde görebiliyoruz.

<img src=".images/plot_bar_oran3.png">

## Facetting

Facetting başka bir grafik oluşturma yöntemidir. Diyelim verimiz ile ilgili bir grafik çıkardık ve ihtiyacımız olan şey bu grağin farklı özelliklerin kategorileri için özelleşmiş versiyonlarını incelemek. Yani bir özelliğin her bir kategorisi için ayrı grafik çıkarmamız gereken durumlarda kullandığımız bir yöntem.

Örnek verecek olursak, karat ve fiyat arasındaki ilişkiyi incelediğimizi düşünelim ve istiyoruz ki tüm elmaslar için değil de ayrı ayrı her berraklık kategorisi için karat ve fiyat arasındaki ilişkiyi görelim.

Elmaslar veri setimizin berraklık kategorilerine bölünmüş kesitlerini alıp her birinin grafiğini çizmemiz gerekiyor. Fakat neyse ki bunu bizim için yapan bir fonksiyon var, üstelik bu grafikleri aynı görselde buluşturuyor. 

Fonksiyonumuz facet_grid() fonksiyonu, yapmamız gereken tek şey bu fonksiyona istediğimiz özelliği vermek. Bu fonksiyon da çizdiğimiz ilk bütüncül grafiği tek tek bu özelliğin kategorilerine uygulayıp kategori sayısı kadar grafiği karşımıza getirecek.
```R
elmaslar +
  geom_point(aes(x=carat, y=price)) +
  facet_grid(.~clarity)
```
Gördüğünüz üzere berraklık özelliğini argüman olarak facet_grid() fonksiyonuna verdik. Fakat ".~" ne anlama geliyor? 

facet_grid()in kaç tane kategori varsa o kadar grafik verdiğini söylemiştik, bu grafiklerin görselde nasıl görüneceklerini belirlemek için eklediğimiz bir ifade. Grafikler yatay veya dikey görünebilir. Yukarıdaki örnekte dikey şekilde görüyoruz.
```R
elmaslar +
  geom_point(aes(x=carat, y=price)) +
  facet_grid(clarity~.)
```

Bu durumda ise yatay şekilde görüyoruz.


Grafiklerimizi kutucuklara bölünmüş şekilde de görmek isteyebilirdik, bu durumda başka bir facetting fonksiyonu olan facet_wrap() fonksiyonunu kullanıyoruz. Ona da aynı şekilde berraklık argümanını sağlıyoruz.
```R
elmaslar +
  geom_point(aes(x=carat, y=price)) +
  facet_wrap(~clarity)
```

## Koordinat Sistemleri
## Temalar
