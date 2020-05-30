---
layout: post
title: SSL/TLS ile HTTPS güvenliği nasıl sağlanır ? 
date: 2020-05-30 17:15:00 +0300
description: 
img:  ruxipen_unsplash.jpg
tags: [SSL,HTTPS] # add tag
---

Ofiste lunch and learn (öğle yemeği ve öğrenme) etkinliğinde konu olarak çok teknik olmasını istemediğimden herkes için internet kavramlarını seçmiştim. HTTPS'in şifreleme mantığını da tam olarak bu hazırlık sürecinde öğrendim. Çalışma mantığı çok hoşuma gitti ve burada genel hatlarıyla anlattıklarımı yazıya döktüm.

## HTTP, HTTPS ve SSL Nedir ? 

İnternet bilgisayarların haberleşme sağladığı koca bir ağ. Bu ağda bilgisayarlar arasında sürekli olarak bilgi alışverişi oluyor. Her türlü özel bilgilerimiz internet aracılığıyla diğer sunuculara, bilgisayarlara gönderiliyor veya alınıyor. Kredi kartı bilgileri, şifreler, yazışmalar vs. vs. Fakat bu ağlar arasındaki bilgi alışverişimiz üçüncü kişiler tarafından araya girererek dinlenebiliyor. Bu da önemli bilgilerimizin çalınmasına yol açıyor. 

İşte bunun önüne geçebilmek için geliştirilen teknolojilerden biri SSL. İnternette bilgisayarlar arası iletişimde şifreleme sağlayarak güvenliği sağlıyor. Bu sayede internette sizin özel verilerinizi gönderdiğiniz veya aldığınız bilgisayarla aranıza giren kişiler bu verileri okusa bile anlamlandıramıyor. Bir nevi iki kişi arasındaki şifreli konuşmaları dinleyen üçüncü kişi gibi. Ne konuşulduğunu duyuyor fakat anlayamıyor. 

Kısaca, bilgi alışverişini yapmamızı ve bunları chrome, firefox vs. gibi tarayıcılarda görüntüleyebilmemizi sağlayan teknolojiye HTTP diyoruz. SSL ise bu alışverişi şifreleyen katman. HTTP ile SSL bir olup bu transferlerin güvenli hale gelmesini sağlayıncada ortaya çıkan yeni kavrama HTTPS diyoruz.  :closed_lock_with_key:

<img src="{{site.baseurl}}/assets/img/ssl_https.png" alt="Http ve Https farkı"/>

## Nasıl Çalışıyorlar ?

İşin hoşuma giden kısmı burası. Bu arkadaşlar nasıl çalışıyor ? Online alışveriş yaparken kredi kartı bilgilerimiz nasıl şifreleniyor ? Bu şifrelenmiş bilgiler e-ticaret sitesinin sunucusuna gittiğinde karşı taraf şifrelenmiş bilgiyi tekrar nasıl anlamlı hale getiriyor ve bu işlem aynı anda iki yönlü olarak binlerce kişiyle nasıl yapılabiliyor?

Bu güvenli iletişimin nasıl sağlandığını iyi anlayabilmek için öncelikle simetrik ve asimetrik şifreleme kavramlarını anlamamız gerekiyor. 

#### Simetrik Şifreleme
Veriyi  gönderen  ile alan  aralarında anlaşıyorlar ve kendi aralarında gizli bir şifreleme anahtarı oluşturuyorlar. Şifreleme anahtarı,  her harfi veya sayıyı 3 artırmak olsun. Örneğin; 5 yazdıysam 8. "A" yazdıysam karşılığında "Ç" gibi .

Örneğin; Ali ile Ahmet aralarında bu gizli anahtarı kullanıyorlar. Ali Ahmet'e **"Merhaba"** yazdığında veri daha önce belirlenen anahtar ile şifrelenerek ortaya **"Öğtjçdç"** gibi anlamsız bir kelime ortaya çıkıyor. Dolayısıyla elinde anahtar olmayan diğer kişiler şifrelenmiş bu mesajı görünce anlamlandıramayor. **"Öğtjçdç"** kelimesi iletildiğinde Ahmet anahtara sahip olduğu için şifrelenen mesajı çözebiliyor ve 3 karakter geri alıp  mesajı **"Merhaba"** olarak anlamlı şekilde okuyabiliyor. 

Özetleyecek olursak. Haberleşen taraflar arasında gizli bir şifreleme anahtarı var ve bu anahtar diğer kişilerle paylaşılmıyor. Paylaşılırsa herkes şifreyi çözüp mesajı okuyabiliyor. İnternette amacımız herkesle şifreli konuşmak olduğu için anahtarıda herkese veremeyeceğimizi düşündüğümüzde simetrik şifreleme tek başına internette haberleşme için yeterli olmuyor. Çünkü amacımız milyonlarca kişiyle şifreli haberleşmek. İşte buradaki sorun a-simetrik şifrelemenin yardımı ile çözülmüş. 

<p align="center">
<img src="{{site.baseurl}}/assets/img/simetrik.png" alt="simetrik şifreleme" height = "300"/>
</p>


#### Asimetrik  Şifreleme
Asimetrik şifrelemede ise tek anahtar yerine birbiriyle ilişkili 2 anahtar var. Bunlardan biri mesajı alan kişi dışında kimsede olmayan özel anahtar. Diğeri ise genel anahtar. Genel anahtar herkese açık. Genel anahtar ile şifrelenen bir veri yalnızca bu anahtarın ilişkili olduğu özel anahtar ile açılabiliyor. 

Yine Ali Ahmet'e şifreli şekilde mesaj göndermek istiyor. Ahmet bir çift anahtar oluşturuyor. Özel anahtarı sadece kendisinde tutuyor. Kimseyle paylaşmıyor. Genel anahtarı ise isteyen herkesle paylaşıyor. Hatta isterse veri hırsızları dahi bu anahtarı alabiliyor. Genel anahtarı alan Ali mesajı bu anahtar ile şifreleyerek Ahmet'e gönderiyor. Şifreyi çözen özel anahtar sadece Ahmet'de olduğu için birileri gönderilen şifreli mesajı okusa bile çözemiyor. Mesaj Ahmet'e geldiğinde kendisindeki özel anahtar ile bu şifrelenen mesajı çözüyor ve anlamlı hale getiriyor. 

Özel anahtar ile genel anahtar arasında matematiksel bir ilişki var. Nasıl çalıştıklarını detaylı öğrenmek isteyen arkadaşlar RSA algoritmasını inceleyebilir. 

Asimetrik şifreleme simetrik şifrelemeye kıyasla daha yavaş. O nedenle internette sürekli asimetrik şifreleme kullanmak dezavantaj. İşte SSL bu iki yöntemide kullanarak ortak bir yol buluyor. 

<p align="center">
<img src="{{site.baseurl}}/assets/img/asimetrik.png" alt="asimetrik şifreleme" height = "300"/>
</p>

## SSL nasıl güvenli bağlantı sağlıyor ?
**Bu bölüm biraz dikkat gerektiriyor. Kafalar karışabilir.** 

Lcwaikiki.com'a girdiğinizde tarayıcımız LCW sunucusuna (bilgisayarına) güvenli bağlantı isteği gönderir. Yani asimetrik şifrelemedeki genel anahtarı şifreli konuşmak için LCW'den ister. LCW sunucusu SSL sertifikasını bize gönderir. SSL sertifikası asimetrik şifreleme için hem LCW'nin genel anahtarını barındırır hem de gerçekten Lcwaikiki.com'a bağlandığımızı doğrular.  

SSL sertifikaları,  sertifika belgelendirme kuruluşları (CA -Certificate Authority) tarafından verilir. Bu CA meselesi ayrı bir konu. Şimdilik genel anahtarı elde etmemizi ve doğru yere bağlandığımızdan emin olmamızı sağlayan sertifikaları veren kuruluşlar olarak bilmemiz yeterli. 

<p align="center">
<img src="{{site.baseurl}}/assets/img/lcw_sertifika.png" alt="lcw chrome https sertifika"/>
</p>

Lcwaikiki.com'a bağlanıp sertifikamızla beraber LCW'nin genel anahtarını aldıktan hemen sonra tarayıcımız simetrik şifrelemeye geçiş yapmak için kendi anahtarını oluşturur. Bu anahtar çalınmasın diye LCW'nin bize gönderdiği genel anahtar ile şifrelenir ve LCW'ye gönderilir. LCW'ye tarayıcımızın oluşturduğu yeni anahtarı gönderirken birileri araya girip bu anahtarı alsa bile anlamlandıramaz. Çünkü bu yeni anahtar LCW'nin genel anahtarı ile şifrelendi ve şifrenin çözülmesi için LCW'nin özel anahtarına ihtiyaç var.

LCW'ye tarayıcının oluşturduğu yeni anahtar ulaştığında şifre LCW özel anahtarı ile çözülür ve tarayıcımızın oluşturduğu anahtarın bir diğeri artık LCW'ye güvenli şekilde geçmiş olur. Bundan sonra iki tarafta da simetrik şifreleme için birer anahtar olmuş olur ve asimetrik şifreleme ile başlayan süreç simetrik şifreleme ile devam eder. 

Tarayıcımız ile LCW arasındaki şifreli iletişim bir sonraki oturuma (session) kadar simetrik olarak tarayıcımızın oluşturduğu anahtar ile devam eder. Yeni oturumda süreç başa sarar. 

Umarım konu herkes için anlaşılır olmuştur. 

Altta konuya hazırlanırken faydalandığım linkleri bulabilirsiniz. 

[https://medium.com/@gokhansengun/https-nas%C4%B1l-%C3%A7al%C4%B1%C5%9F%C4%B1r-20e27c9f9668](https://medium.com/@gokhansengun/https-nasıl-çalışır-20e27c9f9668)

[https://www.youtube.com/watch?v=33VYnE7Bzpk](https://www.youtube.com/watch?v=33VYnE7Bzpk)

[https://www.youtube.com/watch?v=8I7BNgD2Yag](https://www.youtube.com/watch?v=8I7BNgD2Yag)

[https://www.youtube.com/watch?v=4nGrOpo0Cuc](https://www.youtube.com/watch?v=4nGrOpo0Cuc)

