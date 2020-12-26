---
title:  "DNS Amplification Attack Nedir ve Nasıl Yapılır?"
date:   2020-02-21 
categories: 
    - network-guvenligi
thumbnail: posts/dns.png
---

## DNS Amplification Attack Nedir?

**DNS Amplification, Distributed Denial-of-Service (DDoS) saldırı türlerinden biridir. Saldırının amacı tüm DDoS saldırılarında olduğu gibi, kullanıcıların ağ üzerinde bulunan sistemlere erişimlerini yavaşlatmak veya ağa bağlı bir sisteme, servise, web sitesine veya uygulamaya erişimlerini engellemektir.** Çoğu DDoS saldırısı, kullanıcı ağının kaldırabileceği trafik yükünden daha büyük trafik yükü ile bombardımana maruz kaldığı için hacimseldir(volumetric). Bir konserin veya bir futbol maçının bitiminden sonra stadyumun yanındaki altı şeritli bir otoyolda binlerce otomobilin trafiği tıkayıp, normal trafik akışını bozması DDoS saldırısına örnek verilebilir.

| ![atgr1]({{ site.url }}/assets/img/DNSAmplification/resim1.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim1 : DNS Amplification Saldırısının Gösterimi* |

<br/>

Resim1’de bir saldırgan, botnet kullanarak herkese açık DNS çözümleyicilerine, kaynak IP adresi alanına hedef IP adresini atayarak bir istek gönderiyor. Saldırganın göndermiş olduğu istek, dönecek yanıtın büyük boyutlarda olması için oluşturulmuş zararlı bir istektir. Saldırganın yapmış olduğu bu saldırı türü DNS Amplification saldırısı olarak bilinir. Böylece hedef nereden geldiği bilinmeyen yanıtlara maruz kalabilir.

Bir DNS Amplification saldırısı 6 adımda gerçekleştirilir:

- Saldırgan, DNS çözümleyicisine göndereceği isteğin kaynak IP adresini hedef IP adresi olarak ayarlar.

- UDP paketleriyle yapılan isteklerin yanıtlarının büyük boyutlu olması için “ANY” argümanı gibi argümanları ileten DNS çözümleyicileri tespit edilir.

- Saldırgan, Botnet kullanarak hazırlanan UDP paketleri ile DNS çözümleyicilerine istekte bulunur.

- Gönderilen isteğe karşı DNS çözümleyicileri, hedefin IP adresine yanıt verir.

- Hedef, istek göndermediği DNS çözümleyicilerinden yanıt alır.

- Hedefin bulunduğu ağın alt yapısı gelen trafiğin yoğunluğu nedeniyle yavaşlayabilir ve hizmet reddi verebilir.

DNS Amplification saldırısı, son hedefe ulaşmak için farklı teknikler kullanılarak gerçekleştirilebilir. Altı şeritli bir otoyolda binlerce aracın bir anda trafiği doldurarak trafiğin normal akışını yavaşlatması yerine, altı şeritli yolda altı tane yük kamyonunun yan yana gittiği düşünüldüğünde trafiğin normal akışı aynı şekilde yavaşlayıp tıkanacaktır. Dolayısıyla ortalama boyutta çok sayıda paketin kullanılması ile yapılan DDoS saldırısı yerine, DNS Amplification saldırısı olarak nitelendirilen büyük boyutlu paketlerle yapılan saldırı aynı etkiyi gösterebilir.


## DNS Amplification Saldırısı Nasıl Çalışır?

Bir DNS Amplification saldırısında saldırganlar, internetin adres defteri olarak nitelendirilen DNS’in normal çalışmasını, hedef web sitesine karşı bir silah gibi kullanabilirler. Amaç, hedef sistemde servis kesintisi olana kadar ağ bant genişliğini sahte DNS lookup istekleriyle doldurmaktır. DNS amplification saldırısının ne yaptığını anlamak için DNS kavramını bilmek gerekir. DNS, alan adlarının (www.example.com) sahip oldukları IP adreslerinin çözümlenmesini sağlayan bir protokoldür.

Herhangi bir domain adresine istek yapıldığında, yapılan istek belirli bir hiyerarşiye göre ilerlemektedir. Öncelikle yereldeki DNS önbelleğine bakılır. IP adresi yerel ön bellekte bulunmazsa işletim sistemi üzerinde bulunan hosts dosyasına bakılır. Eğer IP adresi burada da bulunamazsa istek ISP’lerdeki DNS sunucularına yönlendirilir. IP adresi bulunana kadar belirlenen hiyerarşiye göre ilerlenir. Bir şirketin kendi domain ağı, kendi çalışanları için DNS isteklerini çözebilir. Ancak internet, saldırganlar da dahil olmak üzere herkes için DNS isteklerini cevaplayan açık DNS sunucularıyla doludur. Saldırganlar, herkese açık DNS çözümleyicilerine, kaynak IP manipülasyonu yaparak zararlı istekler gönderebilir. Böylece yapılacak zararlı DNS istekleriyle hedef sistem yavaşlatılabilir, hatta servis kesintisi yapılabilir.

Saldırganların amaçlarından biri, küçük boyutta DNS istekleri yaparak daha büyük boyutta DNS cevapları almaktır. Tipik bir DNS isteği genellikle onlarca bayt boyutunda olup yalnızca birkaç satır metin ile küçük boyutta yapılabilir. Ama yapılan DNS isteğine karşı verilen cevap daha büyük boyutlarda olabilir. Bu işlem için bir örnek Resim2’de yer almaktadır.

| ![atgr2]({{ site.url }}/assets/img/DNSAmplification/resim2.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim2 : DNS Amplification Saldırısının Gösterimi* |

<br/>

Resim 2: Varsayılan DNS isteğine dönen DNS yanıtının boyut karşılaştırması
Resim2’deki varsayılan DNS isteğinin boyutu 30 bayt ve alınan yanıtın boyutu 45 bayttır. Bant genişliğindeki amplifikasyon faktörü, verilen yanıt boyutunun, isteğin boyutundan fazla olmasına neden olabilir. Saldırganların amacı, hedefe daha büyük boyutlarda yanıtlar gönderilmesini sağlamaktır. Bu nedenle saldırganlar amplifikasyon faktörünü tetikleyerek DNS Amplification saldırısı gerçekleştirebilirler. Bu saldırıyı yapmanın bir yolu, example.com adresinin IP adresini öğrenmek yerine DNS kayıtlarını elde etmek için istekte bulunmaktır. Yapılan isteğe verilen yanıt; alt alan adları (subdomains), yedekleme sunucuları (backup servers), posta sunucuları (mail servers), takma adları (aliases) vb. bilgiler içerebilir. Aniden yapılan 10 baytlık bir DNS isteği, 10, 20 hatta 50 kat daha büyük boyutta bir yanıtın dönmesine neden olabilir. Resim 3’te zararlı bir DNS isteği ve alınan yanıtın karşılaştırıldığı bir görsel yer almaktadır.

| ![atgr3]({{ site.url }}/assets/img/DNSAmplification/resim3.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim3 : Zararlı DNS isteğine verilen DNS yanıtının gösterimi* |

<br/>

Resim3’te 30 baytlık özel hazırlanmış DNS isteğine karşılık DNS çözümleyicisi 1500 baytlık bir yanıt döndürmektedir. Saldırgan, hedef  sisteme yönelik bir istek yerine birden çok istekte bulunduğunda sunucunun isteklere döndüreceği yanıtın boyutu katlanır. Böylece saldırgan ağ trafiğini yavaşlatabilir, hatta ağ trafiğini engelleyebilir.

Resim3’te yanlış olan durum ise dönen cevapların hedef sisteme değil, saldırgana dönmesidir. Çünkü saldırgan, DNS çözümleyicisine göndermiş olduğu isteğin kaynak IP adresini, hedef IP adresi olarak değiştirir. Her gün internet üzerinde yapılan milyonlarca DNS isteği göz önüne alındığında DNS trafiğinin yıldırım hızında gerçekleşmesi gerekir. Bundan dolayı DNS çözümleyicileri UDP protokolünü kullanır. Çünkü UDP protokolünün asıl görevi sunucu ve istemci arasındaki mesajları getirip götürmektir. UDP protokolü mesajın ulaşıp ulaşmadığını ve verilerin doğrulanıp doğrulanmadığını kontrol etmez. Gönderilen verilerin kontrolü yapılmadığı için hızlı çalışan bir protokoldür. UDP, sunucu ve istemci arasındaki bilgi alışverişinden önceki iletişimi takip etmediği için, gönderilen bir isteğin hangi kaynak IP’den geldiği bilinmez.

Saldırganlar kaynak IP adresi manipülasyonu yaparak, DNS isteklerinde kaynak IP adresi olarak hedef IP adresini kullanarak istekleri DNS çözümleyicisine gönderirler. DNS çözümleyicisi, isteğe vereceği yanıtı kaynak IP adresine gönderir. Yani, hedef sistem IP adresi göndermediği halde saldırganlar tarafından yapılan isteklerin yanıtlarını alır.

| ![atgr4]({{ site.url }}/assets/img/DNSAmplification/resim4.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim4 : Hedef IP adresini kullanarak DNS Çözümleyicisine istek göndermek* |

<br/>

Resim 4’te hedef IP adresi ile DNS çözümleyicisine istekte bulunan bir saldırgan yer almaktadır. Saldırgan, hedef sistemin IP adresini 30 baytlık zararlı DNS paketinin içerisine kaynak IP adresi olarak atadıktan sonra DNS çözümleyicisine istek gönderir. DNS çözümleyicisi ise aldığı zararlı isteğe yanıt olarak, hedef sisteme 1500 bayt boyutunda bir yanıt gönderir. Böylece saldırıyı gerçekleştiren saldırganın kim olduğu bilinmez. Bu tür saldırıların avantajı, saldırganın fazla sayıda kaynağa ihtiyaç duymamasıdır. Saldırının gerçekleştirilmesi için bir kaynak IP adresinin olması yeterlidir. Nispeten az miktarda kaynak ve çaba ile bir saldırgan, hedef sitenin performansını önemli ölçüde azaltabilir, hatta tamamen kapanmasını sağlayacak miktarda DNS isteği gönderebilir.

| ![atgr5]({{ site.url }}/assets/img/DNSAmplification/resim5.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim5 : Birden çok DNS Çözümleyicisi ile yapılan DNS Amplification saldırısı* |

<br/>

Resim5’te, bir saldırgan, hedef sistemdeki ağ trafiğini yavaşlatmak veya sistemi tamamen servis dışı bırakmak için birden çok DNS çözümleyicisi üzerinden DNS Amplification saldırısı gerçekleştirmektedir. Saldırgan, performans harcamak yerine birden çok kaynaktan düşük boyutta yapacağı zararlı isteklerle hedef sistemi servis dışı bırakabilir.

Bir saldırının şiddetini arttırmak için, UDP paketlerinin bozulmadan iletilemeyecek kadar büyük olması gerekir. Böylece saldırgan DNS yanıtlarını önemli ölçüde artırabilir. Yanıt paketi belirli bir boyuta ulaştığında daha küçük boyutlara bölünerek gönderilir. Her iki durumda da saldırgan istediği sonuca erişebilir. Çünkü parçalanan paketler yeniden birleşir. Böylece, saldırgan az sayıda kaynak kullanarak başarılı bir şekilde saldırı gerçekleştirebilir.

DNS Amplification saldırısını tespit etmek kolay olsa bile, saldırıya maruz kalan hedefin IP adresine gelen yoğun trafikten dolayı saldırganın IP adresinin tespit edilmesi zorlaşabilir. Çünkü saldırgan, sahte IP adresi kullanarak DNS Amplification saldırısı gerçekleştirebilir. DNS Amplification saldırıları, internette herkes tarafından erişilebilen DNS çözümleyicilerinin sayısı çok olduğu için kolaylıkla gerçekleştirilebilir. Bu nedenle DNS Amplification saldırısı, bir sistemin internet erişimini yavaşlatmak, hatta erişimi sonlandırmak için kullanılan popüler saldırılardan biridir.

## DNS Amplification Saldırı Örneği

DNS Amplification saldırısı yapabilmek için öncelikle saldırganın internet üzerinde herkese açık DNS çözümleyicilerini tespit etmesi gerekir.

| ![atgr6]({{ site.url }}/assets/img/DNSAmplification/resim6.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim6 : Herkese Açık DNS Çözümleyicileri* |

<br/>

Resim 6’da herkese açık DNS çözümleyicileri yer almaktadır. Böylece, saldırgan oluşturacağı zararlı bir istek ile Resim6’da yer alan DNS çözümleyicilerine istekte bulunabilir. Resim 6’da, ülkelere göre yapılan filtreleme sonucunda herkese açık DNS çözümleyicileri yer almaktadır. Resim7’de DNS çözümleyicisine istekte bulunmak için derlenmiş olan bir C kodu yer almaktadır.

```linux
root@Stormer:~# wget https://raw.githubusercontent.com/nullsecuritynet/tools/master/dos/dnsdrdos/release/dnsdrdos.c
..
..
root@Stormer:~# gcc dnsdrdos.c -o dnsdrdos -Wall -ansi
```

Yukarıda, “wget” komutu kullanılarak DNS çözümleyicisine istekte bulunmak için kullanılacak olan “dnsdrdos.c” dosyası indirildi. “gcc” programı ile kod parçası derlendikten sonra çalıştırılmaya hazır hale getirildi. Resim7’de herkese açık DNS çözümleyicilerinden 6 tanesi gösterilmektedir.

| ![atgr7]({{ site.url }}/assets/img/DNSAmplification/resim7.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim7 : Herkese açık birkaç DNS çözümleyicisi* |


Resim7’de, 6 tane DNS çözümleyicisinin IP adresleri publicDns.txt dosyası içerisinde bulunmaktadır. Böylece yapılan DNS istekleri, publicDns.txt dosyası içerisindeki DNS çözümleyicilerine gönderilir. 

```linux
root@Stormer:~# ./dnsdrdos -f publicDns.txt -s 192.168.30.1 -l 100000000000
```

**–f** parametresi ile herkesin erişimine açık olan birkaç DNS çözümleyicisinin IP adresinin olduğu dosya belirtildi. –s parametresi ile ise DNS çözümleyicilerine gönderilecek istek üzerinde kaynak IP manipülasyonu yapılarak 192.168.30.1 IP adresi kaynak IP adresi olarak belirtildi. -l parametresi ile gönderilecek paket sayısı belirtildi.

 Hedef makine üzerinden ağ trafiğinin izlenmesi sonucunda elde edilen DNS çözümleyicileri yanıtları Resim 8’de yer almaktadır.

| ![atgr8]({{ site.url }}/assets/img/DNSAmplification/resim8.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim8 : Saldırı esnasında hedef makine üzerinden ağın izlenmesi* |

<br/>

Resim8’de Wireshark aracı kullanılarak ağ trafiğinin izlenmesi sonucunda yakalanan istekler yer almaktadır. Hedef makine üzerinden DNS çözümleyicilerine herhangi bir istek yapılmadığı halde, hedef makinenin IP adresi kullanılarak, DNS çözümleyicilerine 70 bayt boyutunda paketler gönderildi. Böylece hedef sisteme yönelik DNS Amplification saldırısı yapıldığı doğrulandı.

## DNS Amplification Saldırısına Karşı Nasıl Savunulur?

DNS Amplification saldırısı hedef sistemin trafiğini yavaşlatmaya, hatta durdurmaya yönelik bir DoS saldırısı olarak sonuçlansa bile, DoS-DDoS saldırılarına karşı alınan bazı önlemler DNS Amplification saldırılarına karşı bir çözüm olmayabilir. Örnek olarak, kaynak IP adresinin engellenmesi verilebilir. DNS Amplification saldırısı, herkese açık DNS çözümleyicileri ile gerçekleştirilirse, kaynak IP adresinin engellenmesi durumunda saldırıyı gerçekleştiremeyen bir kullanıcının hedef sisteme yapmış olduğu istek hakkı engellenebilir. Bundan dolayı gelen paket içeriği filtrelenmelidir. Ayrıca açık DNS çözümleyicilerinin sayısı azaltılabilir.

Öncelikle tüm sistemlerin sadece kurum içerisinde yer alan yerel DNS sunucularını kullanmaları sağlanmalıdır. Diğer DNS trafiği, kurum içerisindeki yerel DNS sunucularının ağından ayrılmalıdır. Birçok kurumsal güvenlik duvarı, sahte kaynak IP adreslerinin kullanılmasıyla internet üzerinden gelen trafiğe izin verebilir. Normal şartlarda yerel ağda bulunan bir istemcinin IP adresi, yerel ağın adresi ile eşleşen bir IP adresi olur. Ancak güvenlik duvarını atlatan bir saldırgan, kaynak IPmanipülasyonu yaparak saldırıyı gerçekleştirebilir. Kötü yapılandırılmış perimeter güvenlik duvarları, internetten gelen trafiğin kontrol edilmeden geçmesine izin verebilir. Bu nedenden dolayı kurumlar, kendi ağlarından kaynaklanan ve internete bağlı trafiğin gerçekte yerel ağda bulunan bir IP adresinden yapılıp yapılmadığını tespit etmelidirler.

Bir kurumun ağına gelen tüm DNS yanıtları, giden istekleri işleyen yerel DNS sunucularına yönlendirilmeli ve hiçbir zaman diğer uç noktalara yönlendirilmemelidir. Çünkü kurumsal ağdaki bir istemci, dış ağa istekte bulunmak için yerel DNS sunucusuna DNS isteğinde bulunmaktadır. Yerel DNS sunucuları ise, yapılan DNS isteğinin yanıtını hem önbellekte kaydeder, hem de istemciye gönderir. Dolayısıyla yapılan her isteğin yanıtı olmalıdır. İstemcinin aldığı yanıtlar, yerel DNS sunucusu yönlendirilip, yerel DNS sunucusuna yapılan DNS istekleriyle karşılaştırılmalı ve DNS önbellekte, yanıtın olup olmadığı kontrol edilmelidir. Eşleşme olmadığı takdirde, trafik engellenmelidir. Ayrıca DNS saldırılarına yönelik koruma sağlayan güvenlik duvarları kullanılabilir.

Bazı kurumlarda DNS trafiğinin hacmi, konum olarak dağıtılmış birçok DNS sunucusu arasında paylaştırılarak denge sağlanabilir. Bu işlem için DNS Anycast uygulaması kullanılabilir. Böylece oluşan trafik tüm sunuculara dağıtılarak sunucular arasında dengeleme sağlanabilir. Ayrıca gelen trafik miktarı ağ bağlantısını dolduruyorsa, kurumlar trafiği engellemek için ISP’lerle bağlantılı bir şekilde çalışabilirler. Ancak ISP çözümleri genellikle ucuz olmasına rağmen yeterince esnek değildir. Bu nedenle birçok kurum üçüncü parti DDoS koruma hizmeti kullanır. Bu sayede bir saldırı gerçekleştiğinde istekler kurum ağına gelmeden saldırının engellenmesi sağlanabilir.






