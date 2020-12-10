---
title:  "Nmap ile Servis Sürüm Tespiti ve İşletim Sistemi Tespiti Nasıl Yapılır?"
date:   2020-01-30 
layout: post
categories: 
    - network-guvenligi
thumbnail: posts/nmap.png
---

# Nmap ile Servis Sürüm Tespiti ve İşletim Sistemi Tespiti Nasıl Yapılır?

**Nmap aracının en önemli özelliklerinden biri port tarama işlemidir. Port tarama işlemi sonucunda elde edilen açık portlara yönelik güvenlik açıklarının olup olmadığını tespit etmek için, öncelikle açık port üzerinde çalışan servislerin veya uygulamaların tespit edilmesi gerekmektedir. Nmap aracının önemli özelliklerinden biri de servis ve uygulama sürüm tespitidir. Tarama yapılan bir ağ üzerindeki makinelerde bulunan servislerin ve uygulamaların sürüm bilgilerinin tespiti için –sV parametresi kullanılmaktadır.** Ayrıca bu parametre dışında –A parametresi ile yapılan agresif taramanın içerisinde de –sV parametresi kullanılır. –sV parametresi ve –A parametresi arasındaki fark uygulamalı olarak görülmektedir.

```linux
root@Stormer:~# nmap -sV -T4 localhost
::
::
root@Stormer:~# nmap -A -T4 localhost
```

Nmap portun açık olup olmadığını tespit ettikten sonra tespit ettiği porta bağlanmaktadır. Bu bağlantı sonucunda Nmap, bağlandığı portu beş saniye dinlemektedir. Böylece portta herhangi bir servis çalıştığını tespit etmektedir. Çünkü FTP, SSH, SMTP, Telnet, POP3 ve IMAP sunucuları dahil olmak üzere birçok servis, kendilerini ilk açılış banner’ında tanımlamaktadır. Nmap bu durumu **Null Probe** olarak adlandırmaktadır. Çünkü Nmap herhangi bir probe verisi göndermeden gelen yanıtları dinlemektedir. Herhangi bir veri alındığında, Nmap-service-probes dosyasındaki 3.000 tane NULL probe imzası ile karşılaştırılır. İmzalardaki düzenli ifadeler, alınan yanıttan sürüm numaralarını seçmek için alt dizilim eşleşmeleri içerebilir. Bu işlemler biraz zaman alabilir.  Çünkü Nmap her probun sonucu için 5 saniye beklemektedir.  Problardan bir tanesi hedef portun SSL kullanıp kullanmadığını test etmek için kullanılır. OpenSSL varsa, Nmap SSL üzerinden bağlanıp şifrelemenin ardından neyin dinlediğini belirlemek için servis taramasını yeniden başlatmaktadır.

Nmap’in hangi probu kullanacağı Precise Algoritmasının kullanılmasıyla belirlenmektedir. TCP için ilk önce NULL probu denenmektedir. Nadir bir değere sahip olan veya tamamının mevcut olan yoğunluk değerine eşit olan problar, Nmap-service-probe’un belirlediği sıraya göre denemektedir. Bir probe’un eşleştiği tespit edildiğinde, algoritma sonra erer ve sonuç bildirilir. Sürüm tespitinde, belirtilen yoğunluk seviyesi ne kadar yüksek olursa, denenecek prob sayısı o kadar yüksek olur. Nmap’in varsayılan yoğunluk seviyesi 7’dir.

```linux
root@Stormer:~# nmap -sV --version-intensity 3 -v 192.168.16.128
```

Sürüm taramasında yoğunluk değeri 0 ile 9 arasında verilen değer ile ayarlanabilir. Yoğunluk değeri 0 olarak atanırsa, yalnızca NULL probu (TCP için) ve portu varsayılan bir port olarak listeleyen prob taraması yapılır. Ayrıca **--version-light** parametresinin kullanılması yoğunluk değerinin 2 olması ile eş değerdir. Yoğunluk seviyesinin 9 olarak ayarlanması için ise, **--version-all** parametresi kullanılabilir. Böylece bütün problar denenmektedir. Yalnız servis tespiti uzun sürmektedir.

Nmap ile gerçekleştirilen taramada –sV parametresi ile servis sürüm tespiti, -T4 parametresi ile zamanlama ayarlaması, -F parametresi ile popüler 100 port taraması, -d parametresi ile debug modda çalışma yapılır. Ayrıca **--version-trace** parametresi kullanılarak sürüm tespiti sırasındaki işlemleri ekrana detaylı basar. 

```linux
root@Stormer:~# nmap -sSV -T4 -f -d --version-trace -n 192.168.16.128
```

Açık portların tespit edilmesinden sonra paralel olarak her bir porta yönelik servis taraması başlatılacaktır. Aşağıda NULL probu ile TCP bağlantı isteğinde bulunulduğu gösterilmektedir.

| ![atgr1]({{ site.url }}/assets/img/Nmap/part4/sekil5e.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 5.a : NULL probu için TCP bağlantısı* |


Şekil 5.a’da, NULL prob bağlantıları dört portta bulunan servislere yönelik başarılı bir şekilde uygulanmıştır.

| ![atgr2]({{ site.url }}/assets/img/Nmap/part4/sekil5f.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 5.b : NULL Probunun Kullanıldığı Gösterimi* |


Şekil 5.b’de NULL probe kullanılarak FTP servisinin sürüm bilgisi tespit edilmiştir.

| ![atgr3]({{ site.url }}/assets/img/Nmap/part4/sekil5g.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 5.c : SMTP sürüm keşfi* |


Şekil 5.c'de 25 numaralı portta posta sunucusunun tespit edildiği gösterilmiştir. Normal şartlarda ne tür bir posta sunucusu olduğu bilinmemektedir. Dikkatli bakıldığında posta sunucusunun sürüm bilgisi olarak ESMTP Postfix olduğu görülmektedir. Nmap, SMTP ile eşleştiği için yalnızca SMTP sunucularını eşleştirebilen problar denenir.

Nmap’te, –sV parametresi ile sürüm tespiti yapılırken sürüm tespiti sırasında nelerin yapıldığını detaylı olarak görüntülemek için **--version-trace** parametresi kullanılır.

### 5.1 Post-Processors

Açık portların tespit edilmesi ve açık portların üzerinde çalışan servislerin sürüm bilgilerinin tespit edilmesi dışında Nmap’in sunduğu ek hizmetler bulunmaktadır. Bu hizmetlerden bazıları, PRC Grinding ve SSL tünellemedir.

#### 5.1.1 RPC Grinding

SunRPC (Sun Remote Procedure Call), NFS dâhil birçok hizmetin çalışması için kullanılan ortak bir Unix protokolüdür. Nmap’in içerisinda nmap-rpc veritabanı bulunmaktadır. Birçok RPC hizmeti yüksek numaralı portları veya UDP protokolünü kullanmaktadır. RPC Grinding, kötü yapılandırılmış güvenlik duvarlarını atlatmak için kullanılabilir. RPC hizmeti üzerinde uzaktan kontrol edilip kod çalıştırılabilen kritik güvenlik açıkları bulunmaktadır.

```linux
root@Stormer:~# rpcinfo -p 192.168.16.128
```

Hostların birçok RPC hizmetini sunduğunu ve bu hizmetlerin kötüye kullanılma ihtimalinin yüksek olduğu görülmektedir. RPC hizmeti bilgileri çok hassas olduğundan dolayı birçok ağ yöneticisi 111 numaralı portu engelleyerek hassas bilgileri gizlemek ister. Nmap aracı üç adımda diğer RPC portları ile iletişime geçerek hassas bilgileri elde edebilir. Bunun için TCP/UDP port taraması yaparak açık portlar tespit etmelidir. Sürüm tespiti yapılarak, açık portlardan RPC hizmeti kullanan portları belirlemek gerekmektedir. Son olarak RPC Bruteforce Engine kullanılarak Nmap, içerisindeki nmap-rpc veritabınındaki RPC hizmetlerine ait bilgileri sırasıyla RPC hizmetinin çalıştığı açık portlara denemektedir.

#### 5.1.2 SSL Tünelleme

Nmap, SSL şifreleme protokolünü tespit etme ve ardından sürüm tespitini gerçekleştiren şifreli bir oturum başlatma özelliğine sahiptir.

```linux
root@Stormer:~# nmap -Pn -sSV -T4 -p http,https 192.168.16.128
```

Gerçekleştirilen tarama sonucunda SSL kullanıldığı tespit edilmiştir. Nmap aracının, SSL kullanıldığını tespit etmemesi durumunda **SERVICE** bölümünde **ssl/unknown** olarak belirtilir. Nmap, SSL tespiti için OpenSSL küttüphanelerini ücretsiz olarak kullanmaktadır.

## 6. İŞLETİM SİSTEMİ TESPİTİ

Yapılan güvenlik taramalarında açık portların bulunup üzerinde çalışan servisler tespiti dışında işletim sistemi tespiti de büyük bir önem arz etmektedir. Çünkü yapılan bir güvenlik tespitinde keşif ve bilgi toplama evresi önemlidir. İşletim sisteminin adı, sürümü vb. bilgilerin tespit edilmesi, hedef sistem üzerinde bulabilecek güvenlik açıklarının daha kolay tespit edilmesini sağlamaktadır. Güvenlik testlerinde uzmanlar tarafından güvenlik açıkları tespit edilip hedef sistemler ele geçirilmektedir. Ardından raporlanıp sistemden sorumlu yöneticiye bildirilmektedir. Hedef sistem hakkında elde edilen işletim sistemi bilgilerini bir saldırganın elde etmesiyle birlikte sistem üzerinde tespit ettiği güvenlik açıklarını kullanarak sistemlere zarar verebilir. Bundan dolayı, işletim sistemi ile ilgili bilgilerin ifşası önemsiz görülse bile sistemi ele geçirmeye kadar ki sürecin bir başlangıcıdır.

İşletim sistemi tespit etmenin nedenleri aşağıdaki gibidir:

- Hedef sistemin güvenlik açıklarını belirlemek

- Exploit Uyarlaması yapmak

- Ağ envânteri ve desteğinin belirlenmesi

- Yetkisiz ve tehlikeli cihazların tespit edilmesi

- Sosyal mühendislik

İşletim sistemi tespiti için –O parametresi kullanılmaktadır.

```linux
root@Stormer:~# nmap -O -v kizilcinar.com
```

Device type bölümü, router, yazıcı, güvenlik duvarı veya general purpose gibi bir veya daha fazla üst düzey aygıt türüyle sınıflandırılabilir. Running bölümünde, işletim sistemi ailesini (Windows/Linux) ve varsa işletim sistemini ( 10/2.4.X) göstermektedir. Birden fazla işletim sistemi ailesi varsa aralarına virgül koyularak gösterilmektedir. Herhangi bir eşleştirme olmadığı bir durumda ise, Running bölümü JUST GUESSING olarak değiştirilir. Genelde her işletim sistemi ailesinin sonununda doğruluk yüzdeliği eklenmektedir. OS CPE bölümünde, işletim sisteminin Common Platform Enumeration (CPE) temsili gösterilmektedir. Ayrıca donanım türünün CPE temsiline sahip olabilir. İşletim sistemi CPE’si cpe:/o ile başlarken donanım CPE’si cpe:/h ile başlamaktadır. OS Details bölümünde, eşleşen her fingerprint için ayrıntılı açıklama sağlamaktadır. TCP Sequence Prediction bölümünde, TCP başlangıç sıra numarası zayıf olan sistemler, TCP spoofing saldırılarına karşı savunmasızdır. Bu sistemler ile bağlantı kurulabilir ve farklı IP adresini taklit ederek veri gönderebilirler.  IP ID Sequence Generation bölümünde, birçok sistem istemeden IP paketlerinde 16 bitlik ID alanını nasıl oluşturduklarına bağlı olarak trafik seviyeleri hakkında hassas bilgileri verir. Bu alan Nmap’in ayırt edebildiği ID oluşturma algoritmasını açıklamaktadır. Uptime Guess bölümünde, işletim sistemi tespitinin bir parçası olarak, Nmap üst üste birkaç SYN/ACK TCP paketi alır ve üst bilgileri zaman damgası seçeneği olup olmadığını denetlemektedir. Network Distance bölümü,  Nmap’in hedef host ile bağlantı kurarken kaç tane yönlendirici ve host üzerinden geçtiğini gösterir.

İşletim sistemi tespiti taramalarında hedef sistemlere yönelik taramaların sınırlandırıması **--osscan-limit** parametresi kullanılarak sağlanmaktadır. İşletim sistemi tespitinde, en az bir açık ve bir kapalı TCP portu bulunursa daha etkili olur. Nmap, bu kurallara uymayan hostlara karşı işletim sistemi tespitini gerçekleştirmez. Bu durum –PN taramalarında birok hosta karşı önemli ölçüde zaman kazandırmaktadır.

```linux
root@Stormer:~# nmap -O --osscan-limit -n 192.168.16.128
```

İşletim sistemi tespitine yönelik taramalarda bazen işletim sistemi eşleştirmesi gerçekleşmemektedir. Bu durumlarda **--osscan-guess** veya **--fuzzy** parametreleri kullanılarak işletim sistemine en yakın sonuç tahmin edilmektedir. Tahminin güven düzeyi yüzdelik olarak verilmektedir.

```linux
root@Stormer:~# nmap -O --fuzzy -n 192.168.16.128
```

Yukarıda gösterildiği gibi **--fuzzy** parametresi kullanılmıştır. Fakat bu parametre tek bir hosta gerçekleştirilmiş olup işletim sistemi eşleştirmesi yapılmıştır. Eğer işletim sistemi eşleştirmesi yapılmasaydı, parametrenin gereği olarak en yakın sonuç güven düzeyi yüzdeliğiyle görüntülenecekti.

Hedef sisteme karşı maksimum işletim sistemi tespiti deneme sayısını belirlemek için **--max-os-tries** parametresi kullanılmaktadır. Nmap bir sisteme karşı işletim sistemi tespiti taraması gerçekleştirdiğinde, sistem ile ilgili bir eşleşme bulmadığında genellikle girişimi tekrarlamaktadır. Değeri düşük vermek taramayı hızlandıracaktır. Fakat işletim sistemini tanımlayabilen denemeler azalacaktır.

```linux
root@Stormer:~# nmap -O --max-os-tries 10 -n 192.168.16.128
```
Yukarıda işletim sistemi tespiti için yapılacak olan deneme sayısı maksimum 10 olarak ayarlanmıştır.

### 6.1 TCP/IP Fingerprinting Yöntemleri

Nmap OS fingerprinting, hedef makinenin bilinen açık ve kapalı portlarına 16’ya kadar TCP, UDP ve ICMP probları göndererek çalışmaktadır. Bu problar, standart protokol RFC’lerinde çeşitli belirsizliklerden yararlanmak için özel olarak tasarlanmıştır. Nmap, probları gönderdikten sonra dönen cevapları dinlemektedir. Dönen cevaplardaki nitelikler analiz edilir ve fingerprint oluştumak için birleştirilir. Her prob paketi en az bir kez izlenir ve cevap verilemezse tekrar gönderirlir. Tüm paketler, rastgele bir IP ID değerine sahip IPv4’tür. Açık bir TCP portuna giden problar, eğer böyle bir port bulunamamışsa port atlanır. Kapalı TCP veya UDP portları için Nmap ilk önce böyle bir portun bulunup bulunmadığını kontrol etmektedir.

### 6.2 IPv6 Fingerprinting Yöntemleri

Nmap, IPv6 için gelişmiş benzer ancak ayrı bir işletim sistemi tespiti motoruna sahiptir. Genellikle teknik açıdan aynıdır. Problar gönderilip yanıtlar alınır. Alınan yanıtlar veritabanı ile karşılaştırılır. Farklılıklar kullanılan spesifik problarda ve eşleştirme tarzlarında mevcuttur. IPv6 OS tespiti, IPv4 gibi kullanılır. Sadece -6 ve –O parametreleri birlikte kullanılmalıdır.




