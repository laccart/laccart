---
title:  "Nmap ile Host Keşif İşlemleri Nasıl Yapılır?"
date:   2020-01-28 
layout: post
categories: 
    - network-guvenligi
thumbnail: posts/nmap.png
---

# Nmap ile Host Keşif İşlemleri Nasıl Yapılır?

**Host keşfi, ağda bulunan sistemlere ping atılarak gerçekleştirilir. Fakat bu işlemler geniş ağlara yönelik yapıldığında veya var olan ağ üzerinde ICMP paketlerine yönelik cevap vermeyen bazı makineler olduğunda farklı yöntemler kullanılarak host keşfi yapılabilir. Hedef ağ üzerinde ping atmadan tarama işlemleri gerçekleştirilebilir. Ayrıca TCP, SYN/ACK, UDP gibi problar isteğe bağlı olarak kullanılabilir. Bu probların amacı, türüne göre problar gönderildikten sonra, alınan yanıt doğrultusunda verilen IP adresine sahip makinenin gerçekten açık olup olmadığını tespit etmektir.**

### 3.1 Hedef Hostları ve Ağları Belirlemek

Hedef hostları belirlemek için Nmap’e hedef ağın IP adresi veya Hostname bilgisi girilmelidir. Bir IP adresinin yerine IP adresi aralığı verilebilir. Ayrıca, Nmap uygulaması CIDR adreslemeyi desteklemektedir. CIDR, ip adresinden veya hostname’den sonra gelen /24, /18 vb. değerlerdir. Bu değerler sonucunda Nmap uygulaması kendi içerisinde bunun işlemlerini yaparak kaç hostun taranması gerektiğini hesaplayıp otomatik olarak tarama işlemini gerçekleştirmektedir. Örneğin, 192.168.10.0/24 IP adresini girdiğimizde 256 tane hostu taramaktadır. Ayrıca hostname adını belirterek tarama aynı şekile gerçekleştirilmektedir. Örneğin, kizilcinar.com/24 diyerek de bir tarama başlatılabilir.

```linux
root@Stormer:~# nmap 192.168.16.160-167
```
Yukarıdaki komut çalıştırıldığında belirtilen IP aralığındaki makinelere yönelik varsayılan nmap taraması gerçekleştirilir.

#### 3.1.1 IP Listesi Belirtmek

Bu tür tarama işlemleri genellikle geniş ağ taramalarında gerçekleştirilmektedir. Verilen yüzlerce veya binlerce IP adresini bir dosyaya kaydettikten sonra –iL parametresi kullanılarak tarama işlemleri başlatılabilir.

```linux
root@Stormer:~# cat tarama.txt 
192.168.16.160
192.168.16.161
192.168.16.162
192.168.16.163
192.168.16.164
192.168.16.165
root@Stormer:~# nmap -iL tarama.txt 
```

Nmap komutundan sonra –iL parametresi kullanılarak içerisinde IP adresleri bulunan bir dosya belirtilir ve tarama işlemi gerçekleştirilir.

#### 3.1.2 Rastgele Hedef Seçmek

Nmap aracı ile rastgele IP adresilerinin taranması için –iR parametresi kullanılarak yapılmaktadır.

#### 3.1.3 Kapsam Dışı Hedefleri Belirlemek

Genellikle gözden kaçan durumlardan biri olan kapsam dışı hedefleri belirleme işlemleri, riskli işlemleri önlemektedir. Tarama yapılacak ağda, taranması istenmeyen IP adresleri –exclude parametresi ile belirtilir. Birçok IP adresi olduğu durumlarda, IP adresleri bir dosyaya kaydedildikten sonra **--excludefile** parametresi ile bu işlemler gerçekleştirilmektedir.

```linux
root@Stormer:~# nmap 192.168.16.160-167 --exclude 192.168.16.161,192.168.16.160
```

**--exclude** parametresinden sonra belirtilen IP adresleri tarama dışında tutulmaktadır. Ayrıca taranması istenmeyen IP adresleri birden fazla olduğunda **--excludefile** parametresi kullanılabilir.

Taranacak bir ağdaki IP adreslerinin listelenmesi için –sL parametresi kullanılır.

```linux
root@Stormer:~# nmap -sL 192.168.16.160/29
```

Eğer birbirinden farklı hedeflerin IP adresleri veya hostnameleri taranmak istenirse IP adresleri arasında boşluk bırakılarak tarama işlemi gerçekleştirilebilir. 

```linux
root@Stormer:~# nmap kizilcinar.com 192.168.16.160 192.168.16.163
```

### 3.2 Hedef Kuruluşun IP Adresinin Bulunması

Genellikle bir taramadan önce ana domain bilgisi verilmektedir. Verilen domain adresinin IP bilgileri üzerinden tarama işlemleri gerçekleştirilebilir.

### 3.2.1 DNS Bilgilerinin Elde Edilmesi

DNS’in birincil amacı, domain adlarını IP adreslerine çözümlemektir. Bu nedenle DNS bilgilerini araştırılması gerekmektedir. 

```linux
root@Stormer:~# host -t ns kizilcinar.com
kizilcinar.com name server eric.ns.cloudflare.com.
kizilcinar.com name server janet.ns.cloudflare.com.
root@Stormer:~# host -t a kizilcinar.com
kizilcinar.com has address 104.28.29.246
kizilcinar.com has address 172.67.160.65
kizilcinar.com has address 104.28.28.246
root@Stormer:~# host -t aaaa kizilcinar.com
kizilcinar.com has IPv6 address 2606:4700:3034::ac43:a041
kizilcinar.com has IPv6 address 2606:4700:3031::681c:1cf6
kizilcinar.com has IPv6 address 2606:4700:3037::681c:1df6
root@Stormer:~# host -t mx kizilcinar.com
kizilcinar.com mail is handled by 10 mx.yandex.net.
root@Stormer:~# host -t soa kizilcinar.com
kizilcinar.com has SOA record eric.ns.cloudflare.com. dns.cloudflare.com. 2035782894 10000 2400 604800 3600
```

Yukarıda gösterildiği gibi linux ortamında DNS kayıt türlerinin öğrenilmesi için host yardımcı uygulaması kullanılarak nameserver’lar hakkında bilgi elde edilmektedir. Ayrıca zone transferi ile ilgili işlemler aşağıda gösterilmektedir.

```linux
root@Stormer:~# host -t ns kizilcinar.com
kizilcinar.com name server eric.ns.cloudflare.com.
kizilcinar.com name server janet.ns.cloudflare.com.
root@Stormer:~# dig @eric.ns.cloudflare.com -t AXFR kizilcinar.com
..
..
```

Zone transfer işleminin başarılı veya hatalı olduğu gösterilmektedir. Bir domainin IP adresi var olan kapsam içerisinde var mı yok mu gibi işlemleri öğrenmek için DNS çözümlemesi, traceroute ve ilgili IP adres kayıtları için whois kullanılmaktadır.

```linux
root@Stormer:~# nmap -Pn -T4 --traceroute kizilcinar.com
```


Hedef domaine yönelik tarama gerçekleştirildiğinde –tracetoute parametresi kullanılarak hedef IP adresine kadarki ara IP adresileri gösterilecektir. Ayrıca bir IP adresini kullanarak bilgi toplama işlemi aşağıda gösterilmektedir.

```linux
root@Stormer:~# whois 104.28.29.246

#
# ARIN WHOIS data and services are subject to the Terms of Use
# available at: https://www.arin.net/resources/registry/whois/tou/
#
# If you see inaccuracies in the results, please report at
# https://www.arin.net/resources/registry/whois/inaccuracy_reporting/
#
# Copyright 1997-2020, American Registry for Internet Numbers, Ltd.
#


NetRange:       104.16.0.0 - 104.31.255.255
CIDR:           104.16.0.0/12
NetName:        CLOUDFLARENET
NetHandle:      NET-104-16-0-0-1
Parent:         NET104 (NET-104-0-0-0-0)
NetType:        Direct Assignment
OriginAS:       AS13335
Organization:   Cloudflare, Inc. (CLOUD14)
RegDate:        2014-03-28
Updated:        2017-02-17
Comment:        All Cloudflare abuse reporting can be done via https://www.cloudflare.com/abuse
Ref:            https://rdap.arin.net/registry/ip/104.16.0.0



OrgName:        Cloudflare, Inc.
OrgId:          CLOUD14
Address:        101 Townsend Street
City:           San Francisco
StateProv:      CA
PostalCode:     94107
Country:        US
RegDate:        2010-07-09
Updated:        2019-09-25
Ref:            https://rdap.arin.net/registry/entity/CLOUD14

```

Whois ile hedef IP adresi hakkında bilgiler getirmektedir.

### 3.3 DNS Çözümlemesi

Host keşiflerinde DNS çözümlemesi işleminin kullanılması büyük önem taşımaktadır. Nmap, host keşif problarına yanıt veren her IP adresi için DNS çözümlemesi gerçekleştirmektedir. DNS çözümlemesini kontrol etme işleminde 4 parametre kullanılmaktadır. Bu parametreler aşağıdaki gibidir:

**-n Parametresi** : 
 - Nmap uygulamasının bulduğu IP adreslerine yönelik Reverse DNS çözümlemesi yapmamasını sağlamaktadır. Bu parameter genellikle tarama süresini azaltmaktadır.

**-R Parametresi** : 
- Nmap uygulamasının elde ettiği bütün IP adreslerine yönelik Reverse DNS çözümlemesi yapmasını sağlamaktadır. Varsayılan olarak Reverse DNS çözümlemesi yalnızca açık hostlara karşı gerçekleştirilmektedir.

**––system-dns Parametresi** : 
- Nmap uygulaması, varsayılan olarak makinemizde yapılandırılmış name server’lara sorgu gönderip yanıtları dinleyerek IP adreslerini çözümlemektedir. Performansı arttırmak için birçok istek parallel olarak yapılmaktadır. Bunun yerine ise bu parameter kullanılmaktadır. –system-dns parametresi IPv6 için kullanılmaktadır.

**––dns-server Parametresi** : 
- Nmap uygulaması, varsayılan olarak DNS sunucuları resolv.conf(Unix) dosyasından veya registry(Win32)’den belirtmektedir. Birden çok DNS sunucusu kullanıldığında daha hızlıdır. İstekler internetteki özyinelemeli DNS sunucusundan hemen hemen ayrılabileceği için gizliliği de arttırabilmektedir.

```linux
root@Stormer:~# nmap -Pn -n --system-dns 192.168.16.160
```

Yukarıda, DNS çözümleme işlemi gerçekleştirilmiştir. Yukarıda belirtilen parametreler kullanılarak sonuçlar elde edilmiştir.

### 3.4 Host Keşif Kontrolleri

Host keşif kontrollerinde kullanılan parametreler sonucunda, hedeflerin açık/kapalı olma durumu tespit edilebilir. Makinelere gönderilen probların yanıtlarının kontrol edilmesi gerekmektedir.

#### 3.4.1 Liste Taraması (-sL)

Hedef IP adreslerinin kontrolünü sağlamak amacıyla kullanılır. Hedef makinelere herhangi bir paket gönderilmeden belirtilen ağ üzerindeki hostların listelenmesini sağlayan bir host keşif şeklidir.

```linux
root@Stormer:~# nmap -Pn -sL 104.28.29.246/28
```

**–sL** parametresi ile liste taraması gerçekleştirilmektedir. Ayrıca –Pn parametresi kullanılarak ping tarama yapıp taramadaki işlevselliğin devam etmesi sağlanılacaktır.

Bir ön liste taraması, hangi hedeflerin tarandığını doğrulamaya yardımcı olmaktadır. Gelişmiş bir liste taramasının nedenlerinden biri gizliliktir. Bazı durumlar IDS sistemlerin tetiklenmesine neden olmaktadır. Liste taraması bu tür tetiklenmelere sebep vermeden hangi makinelerin hedef makine olacağı konusunda bilgi sağlamaktadır. Hedeflerin Reverse DNS çözümleme isteklerini fark etmeleri durumunda **––dns-server** parametresini kullanarak isimsiz özyinelemeli DNS sunucuları arasında geçiş yapılabilmektedir.

#### 3.4.2 Port Taramasını Devre Dışı Bırakmak (-sn)

Nmap, bir ağ üzerinde aktif hostların hızlı bir şekilde tespit edilmesi için –sn parametresini kullanır. Tespit edilen hostların IP adresleri belirtilmektedir. Bu işleme **ping scan** denir. Port taraması gerçekleştirmeden Nmap uygulamasının içerisindeki scriptlerden ve traceroute problarından faydalanılmaktadır. Ping Scan, liste taramasına göre daha aktif bir tarama türüdür. Bu tarama türü ile hedeflerin IP adresleri ve hostname bilgileri elde edilir. 

```linux
root@Stormer:~# nmap -sn -T4 kizilcinar.com/28
```

**–sn** parametresi kullanılarak ping taraması gerçekleştirilmiştir. Ekran görüntüsünde görüldüğü gibi port taraması yapılmamıştır.

#### 3.4.3 Ping Devre Dışı Bırakmak (-Pn)

Nmap uygulaması ile pingsiz tarama gerçekleştirilmesi, host keşfinin yapılmasının istenmemesi anlamına gelmektedir. Bir ağdaki bütün makineleri sırasıyla diğer işlemlere tabi tutulmaktadır. Host keşfi atlanılmaktadır. Bir ağda verilen IP adresleri üzeride host keşfi yapıldığında pingsiz taramaya göre zaman kaybı meydana gelecektir. Ayrıca nmap ile –Pn parametresi kullanıldığında pingsiz tarama işlemleri gerçekleştirilmektedir. 

#### 3.4.4 Bazı Parametreler

**––disable-arp-ping** : 
- Genellikle host keşfinde ARP paketleri gönderilip alınan yanıtlar doğrultusunda hostun aktif olup olmadığı tespit edilmektedir. Bu seçenek, bir yönlendiricinin bütün ARP isteklerine özel olarak yanıt verdiği ve hedefin ARP taraması yapılıyormuş gibi görünmesini sağlayan ağlarda kullanılmaktadır. Varsayılan ARP taramasını devre dışı bırakmaktadır. 

```linux
root@Stormer:~# nmap -Pn --disable-arp-ping 192.168.16.160
```

**––resolve-all** : 
- Bir hostname hedefi birden fazla adrese çözümlenirse, hepsi taranmaktadır. Varsayılan olan, yalnızca ilk çözülen adresi taramaktır.

**––traceroute** : 
- Hedefe ulaşması en uygun portu ve protokolü belirlemek için tarama sonuçlarından gelen bilgileri kullanarak tarama sonrasında gerçekleştirilmektedir. Bağlantı taramaları (-sT) ve boşta taramalar (-sl) dışındaki bütün tarama türleriyle çalışmaktadır. Traceroute, ICMP zamanını aşmak için tarayıcı ve hedef host arasındaki ara atlama noktalarından aşılmış mesajları almak için düşük TTL (yaşam süresi) sürüme sahip paketler göndererek çalışmaktadır. Standart traceroute uygulamaları 1 TTL ile başlayıp hedef hosta ulaşana kadar arttırmaktadır. Bu TTL işlemleri, Nmap uygulaması üzerinde geriye doğru yapıldığında sürecin hızlandırılması için akıllı önbellek algoritması kullanılmıştır.

### 3.5 Host Keşif Teknikleri

Genellikle bir ğ üzerinde bulunan makinelerin aktif olup olmadığını tespit edilmek açacıyla makinelere ICMP echo isteği gönderilmektedir. Daha sonra yapılan istek sonucunda bir yanıt alınmaktadır. Alınan yanıt sonucunda makinenin aktif veya pasif olduğu tespit edilmektedir. Güvenlik duvarları bu istekleri nadiren engellemektedir. Bundan dolayı host keşif çalışmaları için kullanılan bir tekniktir. **–sn –PE** parametreleri ICMP ping taramasını belirtmektedir. –R parametresi ise tüm makinelere yönelik reverse DNS çözümlemesi yapmasını istemektedir.

```linux
root@Stormer:~# nmap -sn -PE -R -v google.com microsoft.com
```

#### 3.5.1 TCP SYN Ping

-PS parametresi kullanılarak, SYN bayraklı boş bir TCP paketi gönderilmektedir. Varsayılan olarak 80. port hedef alınmaktadır. Bu bayrak uzak bir sistem ile bağlantı kurulmak istendiğini göstermektedir. Port açık olursa SYN/ACK paketiyle yanıt verilecektir. Üç el sıkışma tamamlayıp bir ACK paketi yerine RST paketini göndererek bağlantıyı düşürecektir. Alınan RST ve SYN/ACK yanıtları makinenin açık olduğunu belirtmektedir.

```linux
root@Stormer:~# nmap -sn -PS80 -R -v google.com microsoft.com
```

#### 3.5.2 TCP ACK Ping

-PA parametresi kullanılarak gerçekleştirilen bir host keşif tekniğidir. SYN ping’e benzemektedir. Tek fark bayrakların değişik olmasıdır. Hedef sisteme ACK bayraklı TCP paketi gönderilir. Eğer hedef sistem açıksa RST paketi ile yanıt verir. 80. port hedef alınarak yapılmaktadır. –PS parametresi ile yapılan taramalar engellendiğinde kullanılmaktadır.

```linux
root@Stormer:~# nmap -sn -PA microsoft.com
```
#### 3.5.3 UDP Ping

UDP paketleri kullanılarak sistemlerin aktif olup olmadığı tespit edilir. –PU parametresi kullanılarak gerçekleştirilmektedir. –PS ve –PA parametreleri ile aynı formattadır. Varsayılan olak 40. Ve 125. Portlar kullanılmaktadır. Genellikle gönderilen paketler boştur. Fakat 53. Ve 161. Portlar genel bağlantı noktaları olduğu için özel payload gönderilmektedir. –data-length parametresi, tüm portlar için ratsgele payload göndermektedir. Bu tarama türünün en önemli avantajı güvenlik duvarını atlatması ve TCP portlarını tarayan filtreleri olmasıdır.

#### 3.5.4 ICMP Ping Türleri

Nmap, host keşiflerinde ICMP paketlerini kullanır. Hedef hostlara ICMP type 8 (Echo isteği) gönderip ICMP type 0(Echo yanıtı) beklemektedir. Fakat birçok güvenlik duvarı bu isteği engellemektedir. -PE parametresi kullanılarak ICMP echo isteği gerçekleştirilir. Bu işlem ICMP ping sorgusu olarak bilinmektedir. Ayrıca açık hostların keşfi zaman damgası ve adres damgası üzerinden de tespit edilebilmektedir. Bu işlemler –PP ve –PM parametreleri kullanılarak gerçekleştirilir.

```linux
root@Stormer:~# nmap -PP -PE -PM 192.168.16.160
```

#### 3.5.5 IP Protocol Ping

IP paketlerinin içerisindeki IP başlığında belirtilen protokol numarası ile yapılan bir tarama türüdür. Herhangi bir protokol belirtilmediği zaman ICMP, IGMP, IP-inIP protokolleri için birden fazla IP paketi gönderilmektedir. Bu yöntemde kullanılan prob ile aynı protokolü kullanan yanıtlar üzerinde veya hedef hosta yönelik erişilemediğini belirten ICMP paketlerine bakılarak bir makinenin çalışıp çalışılmadığı tespit edilmektedir.

#### 3.5.6 ARP Ping

En yaygın kullanılan taramalardan biridir. Bu tarama işlemi –PR parametresi kullanılarak gerçekleştirilir. Bu tarama ham bir IP paketi gönderilerek gerçekleştirilmektedir. Böylece hedef IP adresine karşılık gelen makinenin fiziksel adresi tespit edilir.

```linux
root@Stormer:~# nmap -sn -n --send-ip 192.168.16.161
..
..
root@Stormer:~# nmap -sn -n PR --packet-trace --send-eth 192.168.16.161
```

 Bu tarama örneğinin ilkinde –send-ip parametresi kullanılmasıyla, yerel ağ olmasına rağmen IP seviyesinde paketler gönderilmektedir. Yalnızca bir hedefe yönelik yapılan bir tarama olduğu için taramanın sonuçlanması kısa sürmüştür. Fakat kurumsal bir yerel ağdaki makine sayısının fazlalığı bu süreyi arttırmaktadır. İşletim sistemi hosttan vazgeçmediği için bir saniye arayla üç ARP isteği göndermektedir. Ağ yöneticileri normal şartlarda ping paketlerini engellemektedir. Fakat ARP istekleri veya yanıtları genellikle engellenmemektedir. Ayrıca bu taramalar gerçekleştirildiğinde –spoof-mac parametresini kullanılarak tarama yapan makinenin MAC adresi gizlenebilmektedir.

#### 3.5.7 SCTP INIT Ping

Bu tarama türü –PY kullanılarak gerçekleştirilmektedir. Bu parametre kullanılarak içerisinde INIT öbeği bulunan bit SCTP paketi gönderilmektedir. Varsayılan olarak 80. portu hedef almaktadır. Örnek olarak **–PY22,80** gibi bir parametre ile 22. ve 80. portlar ile bağlantı kurulacaktır. Portlar açık ise INIT-ACK yanıtı dönmektedir. Nmap, bu yanıta işlevsel bir SCTP paketi göndermek yerine ABORT yanıtını vererek bağlantıyı bitirmektedir. Böylece bu iki yanıt ile hedef makine üzerindeki portların açık olduğu tespit edilmektedir.

### 3.6 Host Keşif Stratejileri

Nmap aracı, host keşiflerini daha iyi şekilde tespitinin sağlanması için belirli parametreleri kullanmaktadır. Bu parametreler host keşiflerinde kullanıcının yapmak istediği tarama türlerine göre kullanılmaktadır. Bu parametreler aşağıda belirtilmiştir.

**-v/––verbose** :

Nmap aracında bulunan bu parametre, tarama sonucunda gelen çıktının ayrıntılı bir şekilde gelmesini sağlamaktadır. Bu doğrultuda host hakkında ek bilgiler verilmektedir.

```linux
root@Stormer:~# nmap -v 192.168.16.160
```

**–-source-port port_no** :

Firewall yöneticileri, DNS ve FTP portlarını açık tutmak için firewall üzerinden özel kurallar oluşturmaktadır. Fakat firewall bypass işlemlerinden biri de source port manipülasyon işlemidir.

**–-data-length boyut_degeri** :

Bu parametre ile her pakete rastgele olarak veri eklenmektedir. Ayrıca TCP, UDP ve ICMP gibi tarama türleriyle birlikte kullanılabilmektedir. Birçok IDS sistemini atlatmak için kullanılan yöntemlerden biridir. Örneğin, data değeri 56 olan bir echo istek paketine rastgele olarak 32 değeri atanırsa, IDS sistemi paketin bir Windows işletim sistemine sahip bir makineden geldiğini işaretler. Fakat gelen istek paketindeki gerçek data değerinin 56 olması isteğin bir Linux işletim sistemine sahip makineden geldiğini gösterebilir.

```linux
root@Stormer:~# nmap -PS135 --data-length 32 192.168.16.0/24
```

#### 3.6.4 (–ttl değer) Parametresi

Giden TTL değerinin ayarlanması, IPv4 düzeyindeki ping taramalarında kullanılmaktadır. Yapılan taramanın yerel ağ sınırları içerisinde yapılmasını sağlamayı hedeflemektedir. Giden –ttl değeri azaltılarak, herhangi bir döngü ile karşılaşıldığında yönlendirici CPU’sunun işlem yükünün azaltılması hedeflenmektedir.

```linux
root@Stormer:~# nmap -Pn --reason --ttl 128 192.168.16.160
```

#### 3.6.5 Hazır Zamanlama Parametreleri

Ping taramasını hızlandırıp sonuçların alınmasına yönelik yapılmış bir parametre düzenlemesidir. –T parametresi ile kullanılır. Hazır zamanlama şablonları, paranoid(0), sneaky(1), polite(2), normal(3), aggressive(4) ve insane(5) olarak 6 tanedir.

```linux
root@Stormer:~# nmap -Pn -T4 192.168.16.160
```

**–-max/min-parallelism deger** :

Yapılan taramalarda taranacak makinelerin paralel bir şekilde taramasını sağlamak için düzenlenmiş bir parametredir. Parametreyi kullanarak zaman değeri olarak makine sayısı belirtilmektedir.

**–-min/max/initial-rtt-timeout zaman_degeri** :

Bu parametreler, Nmap aracının yaptığı istek sonucunda gelecek yanıt paketinin ne kadar bekleyeceğini kontrol etmektedir.

#### 3.6.8 Çıktı Parametreleri

-oA, -oN, -oG, -oX vb. parametrelerden oluşur. Örneğin, -oX parametresinin kullanılması ile Nmap çıktıları XML formatında oluşturulur.

**–-randomize-hosts** :

Ağda bir tarama yapılırken, –randomize-hosts parametresinin kullanılması taramayı belirginsizleştirir. Bu yöntem IDS ve IPS sistemlerinden kaçınmak için kullanılır. Nmap, varsayılan olarak bir ağdaki makineleri ardışık olarak tarar. Bazı IDS ve IPS sistemleri ise, yapılan taramayı tespit edip engelleyebilir.

**–-reason** :

Varsayılan taramalar sonucunda gelen çıktıda hedef portun çalışır durumda olup olmadığını göstermektedir. Bu parametre ile nmap taramasında hedef makinesinin hangi keşif testlerine yanıt verdiğini açıklamaktadır. Nmap çalışma sırasında yanıt paketi olarak bir ICMP echo paketi yanıtı rapor edilebilir. Fakat ikinci bir tarama sonucunda önce bir RST paketi alınabilir ve Nmap aracının bunu bildirmesine neden olabilir. Bundan dolayı detaylı olarak hedef makinenin ne tür yanıtlar verip vermediğini görebilmek için bu parametrenin kullanılması fayda sağlamaktadır.

```linux
root@Stormer:~# nmap -Pn --reason 192.168.16.160
```

**––packet-trace** :

Nmap, –packet-trace parametresini kullanarak tarama süresince neler yapıldığını detaylı olarak gösterir. –packet-trace parametresi, Sıra numaraları, TTL değerleri ve TCP bayrakları gibi ayrıntılar dâhil olmak üzere, alınan ve gönderilen her paketin gösterilmesini sağlar.

**-D decoy1, decoy2,,,** :

Decoy olarak tanımlanan IP adresleri, saldırgan IP adresini gizlemek için kullanılan sahte IP adresleri olarak tanımlanabilir. Saldırgan IP adresi, 192.168.16.163’tür. Fakat, -D parametresi ile verilen 192.168.16.138, 192.168.16.2 gibi sahte IP adresleri kullanılarak saldırgan IP ile tarama yapılmıştır. Böylelikle ağı izleyen yetkililerin saldırganı tespit etmesi zorlaşır.

```linux
root@Stormer:~# nmap -Pn -D192.168.16.138,192.168.16.2 192.168.16.160
```

**-6** :

Taramalarda ipv4 adresi yerine ipv6 kullanmayı sağlar.

**-S kaynak_IP, -e gonderen_cihaz_adi** :

Kaynak IP adresini ve gönderen cihazın adını belirterek yapılan tarama türleridir.

#### 3.7 Ping Parametrelerini Seçmek ve Birleştirmek

Hedefi daha etkili bir şekilde tarama için parametrelerin tümünü bilmekten daha fazlası gerekebilir. Bu parametreleri bir arada belirterek ve birleştirerek başarı oranları farklılık göstermektedir. Bu farklılık Şekil 3.7’de gösterilmektedir.

| ![atgr1]({{ site.url }}/assets/img/Nmap/part2/sekil37a.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil3.7.a : Taramada Kullanılacak Olan Probların Başarı Yüzdeleri* |

<br/>

| ![atgr2]({{ site.url }}/assets/img/Nmap/part2/sekil37b.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil3.7.b : Taramada Kullanılacak Birleştirilmiş Problar ve Başarı Yüzdeleri* |
















