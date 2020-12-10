---
title:  "Nmap ile Güvenlik Duvarları ve IDS Sistemleri Nasıl Atlatılır?"
date:   2020-02-01 
layout: post
categories: 
    - network-guvenligi
thumbnail: posts/nmap.png
---

# Nmap ile Güvenlik Duvarları ve IDS Sistemleri Nasıl Atlatılır?

**Yapılan taramalarda güvenlik duvarları ağın haritalanmasını zorlaştırmaktadır. Sistemler hakkında keşif taramaları gerçekleştirildiğinde güvenlik duvarları ve IDS sistemleri bu tarama işlemlerini engelleyebilmektedir. Nmap, bu karmaşık ağları anlamaya yardımcı olmak ve filtrelerin amaçlandığı gibi çalıştığını doğrulamak için birçok özellik sunmaktadır. Tüm büyük IDS’ler Nmap taramalarını tespit etmek için tasarlanmış kurallarla birlikte gelmektedir. Çünkü taramalar bazen saldırılara öncülük etmektedir.**

### 8.1 Güvenlik Duvarı Kurallarını Belirlemek

Güvenlik duvarı kurallarını atlamak için nasıl çalıştıklarını bilmek gerekir. Nmap ulaşılabilir, ancak kapalı olan ve aktif olarak filtrelenen portları birbirinden ayırır. Etkili bir teknik, normal bir SYN port taraması ile başlamak, daha sonra ağı daha iyi anlamak için ACK taraması ve IP ID sıralaması gibi teknikler kullanmaktır.

#### 8.1.1 TCP SYN Scan

TCP SYN(Stealth) Scan, TCP portlarını taramanın en hızlı yolu olduğu için en popüler tarama türüdür. Connect taramasından daha gizlidir ve tüm işlevsel TCP yığınlarına karşı çalışmaktadır. SYN veya Stealth taraması, bir SYN paketi gönderip gelen cevabı göz önüne alarak bu prosedürü kullanmaktadır. SYN/ACK geri gönderilirse, TCP connection bağlantısı ile açık porta uzaktan bağlanmaya çalışır. Tarayıcı tamamen kurulmadan önce bağlantıyı down etmek için bir RST gönderir; genellikle uygulama logları bağlantı girişiminin görünmesini önlemektedir. Port kapalıysa, bir RST gönderilir. Eğer filtre edilirse, SYN paketi düşürülmüş olacak ve herhangi bir cevap gönderilmeyecektir. Bu şekilde, Nmap portun açık, kapalı ya da filtreli olup olmadığını tespit etmektedir. 

```linux
root@Stormer:~# nmap -sS 192.168.16.160
```

• RST döndüren gizli güvenlik duvarları

Kapalı TCP portları (bir RST paketi döndüren) ve filtrelenmiş portlar (hiçbir şey veya bir ICMP hatası döndürmeyen) arasındaki Nmap ayrımı genellikle doğru olsa da, birçok güvenlik duvarı cihazı şimdi RST paketlerini hedef hosttan geliyormuş gibi yapabilir ve portun kapalı olduğunu iddia edebilir. Bu özelliğin bir örneği, istenmeyen paketleri reddetmek için birçok yöntem sunan Linux iptables sistemidir. Iptables kılavuzu özelliği, –reject-with tipi olarak belirtmektedir. Verilen tip uygun ICMP hata mesajını döndüren icmp-net-unreachable, icmp-host-unreachable, icmp-port-unreachable, icmp-proto-unreachable, icmp-net-prohibited veya icmp-host-prohibited olabilir, unreachable varsayılandır). Tcp-reset parametresi yalnızca TCP protokolüyle eşleşen kurallarda kullanılabilir: bu bir TCP RST paketinin geri gönderilmesine neden olur. Bu, çoğunlukla bozuk posta hostlarına, posta gönderirken sıkça oluşan ident (113/tcp) problarını engellemek için kullanışlıdır (aksi halde postanızı kabul etmeyecektir). RST paketlerini güvenlik duvarları ve IDS/IPS ile oluşturmak, ağ operatörlerinin kafasını karıştırabildiğinden ve tarayıcıların, düşmüş paketlerin neden olduğu zaman aşımını beklemeden hemen bir sonraki porta geçmesine izin verdiğinden özellikle 113 numaralı port dışında yaygın değildir. Böyle bir sahtecilik, RST paketinin, makine tarafından gönderilen diğer paketlerle karşılaştırıldığında dikkatli bir şekilde analiz edilmesiyle tespit edilebilir.

#### 8.1.2 TCP ACK Scan

TCP ACK Scan, güvenlik duvarı kurallarının durumlu olup olmadığını ve hangi portların filtrelendiğini belirlemek için kullanılır. Dezavantajı ise açık portları kapalı portlardan ayırt edememesidir. –scanflags parametresini kullanmıyorsanız, varsayılan olarak bu tarama türü problardaki ACK bayrağını ayarlamaktadır. Filtrelenmemiş sistemleri tararken, açık ve kapalı portların her ikisi de bir RST paketi döndürmektedir. Nmap aracı da bunları unfiltered olarak etiketlemektedir. Bu durum, ACK paketleri tarafından erişilebilecekleri anlamına gelmektedir. Ancak Açık veya kapalı olup olmamaları belirsizdir. Yanıt vermeyen veya belirli ICMP hata mesajlarını geri gönderen portlar (type 3, codes 0, 1, 2, 3, 9, 10 veya 13), filtrelenmiş olarak etiketlenir. Ayrıca bu taramayı gerçekleştirmek için –sA parametresini kullanmanız yeterli olacaktır. 

```linux
root@Stormer:~# nmap -sA -n -v 192.168.16.160
```

#### 8.1.3 IP ID Püf Noktaları

IP başlıkları içindeki ID alanında şaşırtıcı miktarda bilgi açığa çıkabilir. Idle Scan tekniğide kullanıldığı gibi güvenlik duvarlarını kandırmak için RST paketlerinin saldırgan bir sistemden gelmediğini göstermeye çalışılır. Bir başka yol da, güvenlilir kaynak adreslerinin güvenlik duvarının kontrol mekanizmalarına takılmadan çalışmasıdır. Örneğin, bir şirketin üretim ağındaki makineler, şirket ağındaki IP adreslerine güvenebilir veya bir sistem yöneticisinin kişisel makinesine güvenebilir. Güvenilir kaynak adres sorununun somut bir örneği, bir şirketin özel UDP hizmetinin, bir yapılandırma dosyasına girilen özel bloklardan geliyorsa, kullanıcıların kimlik doğrulaması mekanizması atlatılabilir. Bu ağ blokları farklı kurumsal konumlara karşılık gelip bu özellik yönetimi ve hata ayıklamayı kolaylaştırmaktadır. Bu güvenlik duvarı kurallarını belirleme tekniği olarak Nmap kullanılmamalıdır. Ama Nmap taramalarının çıktıları önemlidir. Örneğin, bu test bazı kod çözücüler kullanılıp kullanılmayacağını gösterebilir (-D).

#### 8.1.4 UDP Sürüm Taraması

UDP ile çalışmak genellikle daha zordur. Çünkü protokol, TCP gibi açık portların onaylanmasını yapmamaktadır. Birçok UDP uygulaması beklenmedik paketleri görmezden gelmektedir. Nmap, portların açık veya filtrelenmiş olup olmadığını öğrenmek amacıyla açık portlardan yanıt alabilmek için birbirinden farklı UDP hizmetine birçok UDP probu gönderir. 

```linux
root@Stormer:~# nmap -sV -sU -p 50-59 192.168.16.128
```

### 8.2 Güvenlik Duvarı Kurallarını Atlatma Teknikleri

Güvenlik duvarı kurallarını belirlemek değerli olsa da kuralları atlamak çoğu zaman öncelikli hedeftir. Nmap bunu yapmak için birçok teknik kullanarak kötü yapılandırılmış güvenlik duvarlarını atlayabilmektedir. Saldırganın başarılı olabilmesi için oluşturulan yanlış yapılandırmalardan birini bulması gerekirken, ağ yöneticilerinin yanlış yapılandırmaların hepsini tespit edip düzeltmeleri gerekmektedir.

#### 8.2.1 Egzotik Tarama Bayrakları

Ağ portlarından hangisinin filtrelendiğini belirlemek için bir ACK taraması kullanılabilir. Ancak, erişilebilir portlardan hangisinin açık veya kapalı olduğu bilinmemektedir. Nmap, istenen port durumu bilgilerini verirken güvenlik duvarlarını gizlice atlatmak için iyi olan birkaç tarama yöntemi sunmaktadır. FIN taraması böyle bir tekniktir. –sF parametresi kullanılarak FIN taraması gerçekleştirilir.  

```linux
root@Stormer:~# nmap -sF -p1-100 192.168.16.128
```

Diğer birçok tarama türü denenmeye değer tarama türleridir. Çünkü hedef güvenlik duvarı kuralları ve hedef host türü hangi tekniklerin çalışacağını belirlemektedir. Bazı değerli tarama türleri FIN, Maimon, Window, SYN/FIN ve NULL taramalardır.

#### 8.2.2 Kaynak Port Manipülasyonu

Yaygın olarak yapılan yanlış yapılandırmalardan biri, yalnızca kaynak port numarasına dayanan trafiğe güvenmektir. Bir yönetici, uygulamaları durduran kullanıcıların şikâyetleri ile ilgilenecek şekilde yeni bir güvenlik duvarı kurarken, UDP DNS harici sunuculardan gelen yanıtlar artık ağa giremediğinden DNS çökebilir. Ayrıca FTP protokolü üzerinden dosya paylaşımlarında proxyler, sunucular görevlendirilir. Bazı sistem ve ağ yöneticilerinin FTP protokolü üzerinden saldırı yapılmayacağını varsaymaktadır. Aynı durum 53 numaralı DNS portu içinde geçerlidir. Nmap, bu zayıflıklardan yararlanmak için -g ve –source-port seçeneklerini sunmaktadır. Bir port numarası verip bu porta paketler gönderilmektedir. Nmap, belirli işletim sistemi tespit testlerinin düzgün çalışması için farklı port numaraları kullanmalıdır. SYN taraması dâhil olmak üzere çoğu TCP taraması, UDP taraması gibi seçeneği tamamen desteklemektedir. 

```linux
root@Stormer:~# nmap -sS -v -v -n -Pn -g 25 -p1-100 192.168.16.128
```
Yukarıdaki nmap komutuyla kaynak port manipülasyon işlemini gerçekleştirilmiştir. Bunun için –g parametresi kullanılmıştır. –sS parametresi ile SYN taraması, -v parametresi ile detaylı sonuç getirmesini, ikinci –v parametresi ile çıktıları detaylı bir şekilde getirmesi sağlanmıştır. Ayrıca –n parametresi ile dns çözümleme yapmaması için kullanılmıştır. –Pn parametresini kullanarak pingsiz tarama yapmasını ve –p1-100 parametresini belirterek ilk 100 portu taraması istenilmiştir.
#### 8.2.3 IPv6 Saldırıları

IPv6 tam olarak dünyanın her yerinde olmasa bile, Japonya ve diğer bazı bölgelerde oldukça popülerdir. IPv6’yı filtrelemek bazen IPv4’ten daha kritik olabilir. Çünkü genişletilmiş adres alanı, genel olarak adreslenebilir IPv6 adreslerinin genellikle RFC 1918 tarafından belirtilen özel IPv4 adreslerini kullanmak zorunda kalacak makinelere tahsis edilmesini sağlar. IPv4 varsayılanı yerine IPv6 taraması gerçekleştirmek çoğu zaman komut satırına -6 eklemek kadar kolaydır. İşletim sistemi tespiti ve UDP tarama gibi bazı özellikler bu protokol için henüz desteklememektedir. Ancak en popüler özellikler çalışmaktadır.

#### 8.2.4 IP ID Idle Scan

IP ID Idle scan, hedefinize hiçbir adres gönderilmez. Çünkü en gizli tarama türlerinden biridir. Açık portlar, seçilen bir zombi makinesinin IP ID sequencelerinden çıkarılır. Idle scan’nin daha az bilinen bir özelliği, elde edilen sonuçların, eğer zombinin doğrudan hedef host taraması durumunda elde edeceğiniz sonuçlardır. -g parametresinin güvenilir kaynak portlarından yararlanmasına izin verdiği gibi, Idle scan bazen güvenilir kaynak IP adreslerinden de yararlanabilir.

#### 8.2.5 Çoklu Ping Probları

Güvenlik duvarı ağlarını taramaya çalışırken, atılan ping probları hostlardan cevap alamayabilir. Bu sorunu azaltmak için Nmap, çok çeşitli probların paralel olarak gönderilmesine izin verir.

#### 8.2.6 Fragmentation (Parçalama)

Parçalama işlemi güvenlik duvarlarını atlatmak için kullanılan tekniklerden biridir. Bazı güvenlik duvarları paketleri boyutlarını göz önüne alarak paketin incelenmesini yapmaktadır. Paket boyutlarının parçalanması ile güvenlik duvarı paketleri tek bir paket halinde inceleyemediği için kolaylıkla güvenlik duvarı atlatılır. Paketlerin parçalanması için -f parametresi kullanılır. Nmap, paketlerin parçalanması işleminde her parçalanan paketin boyutunu 8 bayt olarak belirler. Bu nedenle tipik bir 20 veya 24 bayt TCP paketi üç küçük parça halinde gönderilir. –f parametresinin kullanımı her parçanın 8 bayt olduğunu belirtir. Yani -f -f, her bir parça içerisinde 16’ya kadar veri baytı sağlar. Alternatif olarak, –mtu parametresini belirtebilir. –mtu argümanı sekizin bir katı olmalı ve -f parametresiyle birleştirilmemelidir.

```linux
root@Stormer:~# nmap -f -f -p1-100 192.168.16.128
```

IP katmanını atlamak ve raw ethernet frameleri göndermek için –send-eth parametresi kullanılabilir. Fragmentation, Nmap’in yalnızca TCP ve UDP port taramaları (connect scan ve FTP bounce scan hariç) ve işletim sistemi tespiti içeren ham paket özellikleri için desktekler. Sürüm tespiti ve Nmap Scripting Engine gibi özellikler genellikle parçalanmayı desteklemez. Çünkü hedef hizmetlerle iletişim kurmak için makinedeki TCP stack’e güvenir. Nmap, herhangi bir çakışma olmadan sırayla parçaları gönderir. Parçalanmış bir port taraması geçerse, ana makineye saldırmak için kullanılan diğer araçları ve istismarları parçalamak için fragroute gibi bir araç kullanılabilir.

#### 8.2.7 Proxyler

Web için uygulama düzeyinde proxy’ler, algılanan güvenlik ve ağ verimliliği (önbellekleme yoluyla) yararları nedeniyle popüler hale gelir. Güvenlik duvarları ve IDS gibi, yanlış yapılandırılmış proxy’ler çözdüklerinden çok daha fazla güvenlik sorununa neden olabilir. En sık karşılaşılan sorun uygun erişim kontrollerinin ayarlanamamasıdır. İnternette yüzbinlerce geniş açık proxy sunucusu bulunur. Bu sayede herhangi birinin başka internet sitelerine isimsiz atlamalı noktalar olarak kullanılmasına izin verir. Birçok kuruluş açık proxy’leri bulmak ve IP adreslerini dağıtmak için otomatik tarayıcılar kullanır. Açık proxy’ler daha çok sitelere sızmak, kredi kartı sahtekârlığı yapmak ya da interneti spam ile doldurmak isteyen daha kötü insanlar tarafından kötüye kullanılır. İnternet kaynaklarında açık bir proxy barındırmak çok sayıda soruna neden olabilir. Ancak açık proxy’lerin korumalı ağa yeniden bağlantı kurmasına izin verilmesi daha ciddi bir durumdur. Dâhili ana makinelerin Internet kaynaklarına erişmek için bir proxy kullanması gerektiğine karar veren yöneticiler, istemeden de olsa trafiğe ters yönde de izin verir. Hacker Adrian Lamo, genellikle bu reverse-proxy tekniğinden yararlanarak Microsoft, Excite, Yahoo, WorldCom, New York Times ve diğer büyük ağlara girdiği için ünlüdür.

#### 8.2.8 MAC Address Spoofing Tekniği

Ethernet cihazları, benzersiz bir altı bayt Media Access Control (MAC) adresiyle tanımlanır. İlk üç bayt organizasyonel olarak benzersiz bir tanımlayıcı (OUI) oluşturur. Bu ön ek, bir satıcıya IEEE tarafından atanır. Satıcı daha sonra kalan üç byte’ı sattığı adaptörlere ve cihazlara benzersiz bir şekilde atamaktan sorumludur. Nmap, OUI’leri atandıkları satıcı adlarıyla eşleştiren bir veritabanı içerir. Bu, bir ağı tararken aygıtları tanımlamaya yardımcı olur. OUI veritabanı dosyası, nmap-mac-prefixes bulunur. MAC adresleri ethernet cihazlarına önceden atanmış olsalar da, mevcut donanımların çoğunda sürücü ile değiştirilebilir. Örneğin, çoğu Wireless Access Point, belirli bir MAC adresi grubuna erişimi sınırlandırmak için bir yapılandırma seçeneği sunar. Benzer şekilde, bazı ücretli veya özel ağlar, sizi bir web formu kullanarak bağlandıktan sonra doğrulamaya veya ödeme yapmaya zorlar. Ardından, MAC adresinize dayanarak ağın geri kalanına erişmenizi sağlar. MAC adreslerini spooflamanın ve MAC’in ağa yetkisiz erişim sağlaması için spoofing yapmasının kolay olduğu göz önüne alındığında, bu erişim kontrolü şekli oldukça zayıftır. Bir yönlendiriciyi geçerken son sunucunun MAC adresi değiştirilir. Erişim kontrolüne ek olarak, MAC adresleri bazen hesap verebilirlik için kullanılır. Ağ yöneticileri, DHCP lease aldıklarında veya yeni bir makine ağda iletişim kurduğunda MAC adreslerini kaydeder. Ağın kötüye kullanılması veya korsanlık şikayetleri alınırsa, IP adresini ve olay saatini temel alarak MAC adresini bulur. Sonra sorumlu makineyi ve sahibini bulmak için MAC kullanılır. Nmap, –spoof-mac parametresiyle MAC Address Spoofing işlemi yapar. Verilen argümân birkaç şekilde olabilir. Eğer sadece 0 ise, Nmap oturum için tamamen rasgele bir MAC adresi seçilir. Verilen dize çift sayıda onaltılık bir rakam ise, Nmap bunları MAC olarak kullanır. 12 hex basamaktan daha az rakam sağlanmışsa, Nmap altı baytın kalanını rasgele değerlerle doldurur. Argüman sıfır veya hexadecimal dize değilse, Nmap verilen dizeyi içeren bir satıcı adı bulmak için nmap-mac-öneklerine bakılır(büyük/küçük harf duyarsızdır). Bir eşleşme bulunursa, Nmap satıcının OUI’sini kullanır ve kalan üç baytı rastgele doldurur. Geçerli –spoof-mac argümân örnekleri Apple, 0, 01:02:03:04:05:06, deadbeefcafe, 0020F2 ve Cisco’dur. Bu parametre, Nmap’ın aslında ethernet düzeyinde paketler göndermesini sağlamak için –send-eth anlamına gelir. Bu parametre, sürüm tespiti veya Nmap Scripting Engine gibi bağlantı yönelimli özellikleri değil, yalnızca SYN taraması veya işletim sistemi algılama gibi ham paket taramalarını etkiler. MAC Address Spoofing ağ erişimi için gerekli olmasa bile, aldatma için kullanılabilir.

#### 8.2.9 Source Routing (Kaynak Yönlendirme)

Hedef ile aranızdaki bir router sorun yaşamanıza neden oluyorsa, çevresinde bir rota bulmaya çalışılır. Bu tekniğin etkinliği sınırlıdır. Çünkü paket filtreleme sorunları genellikle hedef ağ üzerinde veya yakınında meydana gelir. Bu makinelerin tüm kaynak yönlendirmeli paketlerini düşürmesi veya ağa girmesinin tek yoludur. Nmap, –ip-options parametresini kullanarak hem gevşek hem de katı kaynak yönlendirmesini desteklemektedir. Örneğin, –ip-options parametresi “L 192.168.0.7 192.168.30.9” olarak belirtilmesi, paketin, verilen iki IP yol noktasından bu serbest kaynağın yönlendirilmesini talep eder. Kesin kaynak yönlendirmesi için L yerine S belirtilir. Sıkı kaynak yönlendirmeyi seçerseniz, yol boyunca her bir sekmeyi belirtmek zorunda kalacaktır. IPv4 kaynak yönlendirmesi çok sık engellenirken, kaynak yönlendirmenin IPv6 biçimi çok daha yaygındır. Nmap ile bir hedef makineye yönlendirilmiş bir kaynak yolu keşfedilirse, exploitability port taramasıyla sınırlı değildir. Ncat, kaynak yönlendirilmiş yollar üzerinden TCP ve UDP iletişimini etkinleştirebilir (-g parametresi kullanılabilir).

#### 8.2.10 FTP Bounce Scan

FTP protokolünün bir özelliği (RFC 959) proxy FTP bağlantıları için destek sağlar. Bu, kullanıcının bir FTP sunucusuna bağlanmasını ve ardından dosyaların üçüncü taraf bir sunucuya gönderilmesini istemesini sağlar. Böyle bir özellik birçok düzeyde saldırganlar tarafından kullanılmıştır. Bu nedenle çoğu sunucu bu özelliği destekler. Özelliğin diğer bir dezavantajı, FTP sunucusunun diğer hostları taramasına izin vermesidir. FTP sunucusundan, sırayla bir hostun portuna bir dosya göndermesini istemektedir. Hata mesajı portun açık olup olmadığını açıklamaktadır. Bu, güvenlik duvarlarını atlamak için iyi bir yoldur. Çünkü kurumsal FTP sunucuları, genellikle diğer tüm ana bilgisayarlara, eski herhangi bir Internet ana bilgisayarından daha fazla erişebilecekleri bir yere yerleştirilir.Nmap, -b seçeneğiyle birlikte FTP bounce taramasını desktekler. “kullanıcıadı:parola@ftpserver:port” biçiminin bir argümanını alır. Server, güvenlik açığı bulunan bir FTP sunucusunun adı veya IP adresidir. Normal bir URL’de olduğu gibi, “kullanıcıadı:parola” öğesi atlanılabilir; bu durumda adsız oturum açma kimlik bilgileri (kullanıcı: anonymous parola: -wwwuser@) kullanılır. Port numarası ihmal edilebilir, bu durumda sunucu üzerindeki varsayılan FTP port (21) kullanılır. Bir güvenlik duvarını atlamak amacınızsa, hedef ağı bağlantı noktası 21 için tarayın (veya sürüm algılamasıyla tüm bağlantı noktalarını tararsanız herhangi bir FTP hizmeti için bile) ve ftp-bounce NSE komut dosyası kullanılabilir. Nmap hedefin savunmasız olup olmadığını söyler. İzlerinizi sadece kapatmaya çalışıyorsanız, kendinizi hedef ağdaki ana bilgisayarlarla sınırlandırmanız gerekmez. Güvenlik açığı bulunan FTP sunucuları için rasgele internet adresleri taramaya gitmeden önce, sysadmins’in sunucularını bu şekilde kötüye kullanmanıza yardımcı olmaz.

```linux
root@Stormer:~# nmap -v -b anonymous:-wwwuser@192.168.16.128:21 192.168.16.128 -Pn
```

Yukarıdaki nmap komutu FTP sunucusuna yönelik bir saldırı olarak da gösterilebilir. Böyle bir girişimde yukarıdaki gibi kullanıcıadı parola FTP sunucusu adresi sonra hedefin adresi ve pingsiz tarama için -Pn parametresini kullanmıştır.

8.2.11 Güvenlik Duvarını Atlama Örneği

```linux
root@Stormer:~# nmap -n -sn -PE -T4 kizilcinar.com
```

Yukarıdaki nmap komutunda –n parametresi ile dns çözümlemesini yapmamasını sağlanılır. –sn parametresini kullanarak port tarama işlemini devre dışı bırakılır. –PE parametresini kullanarak bir ICMP echo taraması gerçekleştirilmesi ve –T4 parametresini kullanarak zamanlamasını ve performansını ayarlama işlemi gerçkeleştirilir. Çünkü varsayılan olarak buradaki amaç ağdaki sistemlerin varlığını keşfetmektir. Tabi, örnek olması amacıyla kizilcinar.com domain verilir. Ayrıca 192.168.16.0/24 veya 104.18.41.10 makinasının subnetinin hesaplandıktan sonra /24, /32 vb. opsiyonları kullanarak verilip ağdaki sistemlerin keşfi sağlanılabilir.

```linux
root@Stormer:~# nmap -vv -n -sn -PE -T4 --packet-trace kizilcinar.com
```
**--packet-trace** parametresinin sağladığı avantajlar kullanılarak genel bir tarama gerçekleştirilebilir.

```linux
root@Stormer:~# nmap -vv -n -Pn -sI google.com:80 -p 80 kizilcinar.com
```


Idle scan işlemini kullanarak hedefin 80. portuna yönelik tarama gerçekleştirilir. Burada google.com adresinin bulunduğu makineyi zombie makina olarak belirleyip google.com makinesi üzerinden iletişime geçip tarama gerçekleştirilir.

```linux
root@Stormer:~# nmap -n -sn -PE --ip-options=L kizilcinar.com --reason google.com
```

**--ip-options** parametresinin avantajları kullanılarak hedef makineye yönelik tarama işlemleri gerçekleştirilebilir.

```linux
root@Stormer:~# nmap -n -sn -PE --ip-options=L kizilcinar.com --reason google.com -Pn
```
Ek olarak -Pn parametresi eklenerek bir tarama gerçekleştirilebilir.






















