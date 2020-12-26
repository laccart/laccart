---
title:  "Nmap ile Port Taraması Nasıl Yapılır?"
date:   2020-01-29 
layout: post
categories: 
    - network-guvenligi
thumbnail: posts/nmap.png
---

# Nmap ile Port Taraması Nasıl Yapılır?

**Bilgisayar ve bilişim sistemlerinin birbirleri arasında iletişimi sağlamaları için kullanmış oldukları bağlantı noktalarının her birine port denilmektedir. Yapılan iletişimde türüne veya iletişim çeşidine göre belirli protokoller kullanılmaktadır. Bu protokollere tahsis edilen portlar doğrultusunda iletişim sağlanılmaktadır. Portlar, bilişim sistemlerine girdi ve çıktıların geçiş noktasıdır.** 

Nmap, portları kullanan iki protokolle çalışmaktadır. Bu protokoller TCP ve UDP protokolleridir. Her protokol için bir bağlantı dört öğe tarafından gerçekleştirilmektedir. Bu öğeler: kaynak IP adresi, hedef IP adresi, kaynak port adresi ve hedef port adresidir. Protokol, IP veri bölümünde ne tür bir paketin bulunduğunu belirten 8 bitlik bir alandır. IPv4 adresleri 32 bit uzunluğunda iken, portlar ise 16 bit uzunluğundadır. IPv6 adresleri ise 128 bit uzunluğundadır. Port numarası alanı 16 bit uzunluğundadır. Bundan dolayı 65535 adet port numarası kullanılabilir. En küçük değer olan 0 değeri geçersizdir. Port numarasının 0 olarak belirtilmesi joker görevi görmektedir. Sistemin varsayılan kendince port atamasına zemin hazırlamaktadır. Kötü amaçlı dinlemelerde saldırganlar port 0 noktasını dinlemektedir. Nmap açıkça belirtildiğinde (-p0-65535) port sıfır taraması gerçekleştirilebilmektedir.

### 4.1 En Popüler TCP ve UDP Portları

- Port 80 (HTTP) : En sık kullanılan TCP portlarının arasında yer almaktadır. Varsayılan olarak web sayfaları kullanımında istemcinin bağlantı için kullanmış olduğu port numarasıdır.

- Port 23 (Telnet) : Telnet şifresiz iletişim ile internet üzerindeki bulunan bir makineye istemci olarak bağlanmasını sağlamaktadır. Şifresiz iletişim olduğundan dolayı güvenli değildir. Fakat yönlendiricilerde yönetim portu olarak çalışabilmektedir.

- Port 443 (HTTPS) : Varsayılan olarak kullanılan HTTP protokolünün SSL ile şifrelenerek iletişimin sağlanması ile güvenliği arttırılmaktadır. Web sunucularına yönelik yapılan istekler şifreli gönderilmektedir.

- Port 21 (FTP) : Dosya aktarım protokolüdür. Telnet protokolü gibi veriler şifresiz açık halde aktarılmaktadır. Güvenli bir protokol olmayıp, halen kullanılmaktadır.

- Port 22 (SSH) : Secure Shell olarak bilinip kullanıcıların sunucuları internet üzerinden kontrol etmesini ve düzenlemesini sağlayan bir yönetim protokolüdür. Uzak makine arasında şifreli iletişimi sağlamaktadır.

- Port 25 (SMTP) : Mail gönderme protokolüdür.

- Port 53 (DNS) : Domain Name Server, domain adları ve bu domainlerin sahip olduğunu IP adresleri arasında dönüşüm yapmak için kullanılmaktadır. Hem TCP hem de UDP protokolü tarafından kullanılabilen bir porttur.

- Port 67 (DHCPS) : Dynamic Host Configuration Protocol Server olarak bilinir ve ağa dâhil olacak istemci makinelere IP adresi atamaktadır. UDP protokolü tarafından kullanılmaktadır.

- Port 68 (DHCPC) : DHCP istemci portudur. UDP protokolü tarafından kullanılmaktadır.

- Port 69 (TFTP) : Özel dosya aktarım UDP protokolüdür. Trivial File Transfer Protocol olarak bilinmektedir.

- Port 110 (POP3) : Post Office Protocol version 3 olarak bilinmektedir. Yerel email istemcilerinin uzak email sunucuları ile iletişime geçmesi ile kullanılan bir protokoldür. Uzak sunuculardan email indirip bir kopyasını kendi sunucusunda bulundurma özelliği bulunmaktadır.

- Port 135 (MSRPC) : Microsoft Remote Procedure Call olarak adlandırılmaktadır. Sunucu ile istemci arasındaki iletişim için kullanılıp uzaktan kod çalıştırılmayı sağlamaktadır.

- Port 139 (NetBIOS-SSN) : MS-Windows hizmetleriyle iletişim kurmak için kullanılan ve NETBIOS Oturum Hizmeti sunan bir TCP protokolüdür.

- Port 445 (SMB) : Server Message Block Protocol tarafından kullanılmaktadır. SMB, dosya paylaşım için kullanılan bir protokoldür.

- Port 143 (IMAP) : Internet Message Access Protocol tarafından kullanılmaktadır. E-posta iletilerinin doğrudan sunucu üzerinden yönetilmesini sağlayan bir TCP protokolüdür.

- Port 995 (POP3S) : POP3 protokolüne SSL eklenilerek yapılan iletişimin daha güvenli hale gelmesini sağlamaktadır.

- Port 993 (IMAPS) : IMAPv2 protokolünün daha güvenli iletişimi sağlaması için SSL eklenilen halidir.

- Port 5900 (VNC) : Güvenli olmayan bağlantı ile grafiksel masaüstü paylaşım sistemi için kullanılan bir TCP protokolüdür.

- Port 3389 (ms-term-server) : Remote Desktop Protocol (RDP) olarak bilinmektedir. Uzak masaüstü bağlantısı sağlayan bir TCP protokolüdür. Bağlantının güvenliği için network tabanlı port değişikliği yapılabilmektedir.

- Port 3306 (MySQL) : MySQL veritabanı ile iletişimi sağlayan bir TCP protokolüdür.

- Port 1433 (MSSQL) : Microsoft SQL Server veritabanı ile iletişimi sağlayan bir TCP protokolüdür. Ayrıca UDP 1434 numaralı MS-SQL-DS protokolü aynı işlemleri gerçekleştirebilmektedir.

- Port 8080 (HTTP-Proxy) : HTTP Proxy’leri ve web sunucuları için kullanılan alternatif bir TCP protokolüdür.

- Port 1723 (PPTP) : VPN ağlarına güvenli bir şekilde bağlanılması için altyapı sağlamaktadır.

- Port 161 (SNMP) : Simple Network Management Protocol tarafından kullanılmaktadır. Network bileşenlerinin veya network kartı olan UPS gibi cihazların yönetimini sağlamasında kullanılan bir UDP protokolüdür.

### 4.2 Port Taraması Nedir?

Hedef üzerinde bulunan portların durumlarını tespit etmek için uzaktan test etme işlemidir. Portların durumları açık ise port üzerinde gerçekleşen bağlantılar dinlenilip bağlantının güvenli olup olmadığı ve hangi servisler üzerinden işlemler yapılıp yapılmadığı tespit edilmektedir. Portların durumları, aşağıdaki durumlardan oluşmaktadır.

- **Open** : Portun açık olduğunu belirtmektedir. Genellikle açık olan portlarda servisler çalışmaktadır.

- **Closed** : Portun kapalı olduğunu belirtmektedir.

- **Filtered** : Portun açık olup olmadığı belirlenememektedir. Çünkü paket filtreleme, problarının porta ulaşmasını engellemektedir.

- **Unfiltered** : Portun erişilebilir olduğunu göstermektedir. Ancak nmap, portun açık veya kapalı olduğu belireyememektedir.

- **Open&#124;Filtered** : Portun açık veya filtreli olup olmadığının belli olmadığını belirtir.

- **Closed&#124;Filtered** : Portun kapalı veya filtreli olup olmadığının belli olmadığını belirtir.


```linux
root@Stormer:~# nmap -PU -Pn 192.168.16.160
```

Yapılan tarama sonucunda STATE(Durum) başlığı altında portların durumu açık olarak gösterilmiştir. Portların açık olan bir sistem üzerinde servislerin çalıştırılıp çalıştırılmayacağı hakkında bilgi edilebilir. Hatta açık port üzerinden güvenlik açığı var mı, yok mu diye tarama gerçekleştirilebilir. Bundan dolayı açık portların kontrol altında olması önemlidir. Sistem ve ağ yöneticileri, sistem güvenliği için kullanılan açık portları filtrelemelidir. Port kullanılmıyorsa, kapatılmalıdır.

Gerçekleştirilen bir port taramasında açık port bulunursa, portta çalışan servis tespit edilir. Tespit edilen servisin zafiyetli olup olmadığı araştırılır. Yapılan araştırma sonucunda bir güvenlik açığının olabileceği düşünülürse güvenlik taraması gerçekleştirilir. Güvenlik taraması sonucunda servisin sürümünde güvenlik açığı olduğu belirtildiğinde, doğrulamak için sızma girişiminde bulunulmaktadır. Bu işlemleri saldırgan gerçekleştirmesi halinde hedef sisteme kritik derecede zararlar verebilir.

#### 4.2.1 Varsayılan Port Taraması

Nmap veritabanında belirlenmiş olan 1000 tane portun taraması yapılır. Bu taramalar genellikle hızlı bir şekilde biter. Ayrıca taramaların kısa sürmesi açısından tarama türü üzerinde değişiklikler yapılabilir. **-n** parametresi ile DNS çözümlemesinin yapılmaması istenir. Böylelikle portlar üzerinde DNS çözümlemesi gerçekleştirilmeyip zamandan tasarruf ederek sonuçlar elde edilir.

```linux
root@Stormer:~# nmap 192.168.16.160
```

Belirtilen tarama türü genel bir nmap taramasıdır. Spesifik olarak portların belirtilmesi durumunda -p parametresi kullanılır. -p0- parametresinin kullanıldığı bir taramada hedef sistemin 65535 portunun hepsi taranacaktır. Bu durum bir makine için yapıldığı zaman çok uzun sürmeyebilir. Fakat bir makine yerine bir ağın taranması saatler alabilir. Çünkü varsayılan 1000 tane portun yerine 65535 tane port taranır. 

```linux
root@Stormer:~# nmap -p0- -v -A -T4 192.168.16.160
```

Yukarıda kapsamlı bir nmap taraması için nmap komutu satırı gösterilmektedir. Bu taramanın detaylı bir şekilde çıktıları ekrana basması için –v parametresi kullanılmıştır. –A parametresi ile agresif tarama gerçekleştirilir. Agresif tarama, port taraması, servis sürüm tespiti, işletim sistemi tespiti gibi taramaları yapar. Ayrıca gerçekleştirdiği taramada NSE scriptlerini de kullanır. –T4 değeri ise taramanın zamanlamasına yönelik kullanılan bir parametredir. –T parametresinin özelliklerinden biri zaman aşımı özelliğidir. Hedef makinelerden gelen cevapların süresi belirlenen sürenin üzerinde ise zaman aşımına uğrar. Böylece, Nmap bir sonraki makineye geçer.

### 4.3 Port Tarama Tekniklerinin Seçilmesi

Port tarama tekniklerinin seçilmesi, port tarama işleminin başarılı bir şekilde gerçekleşebilmesi için büyük önem arz etmektedir. Çünkü taramanın hızlı olmasına ek olarak başarılı ve tutarlı bir tarama olması gerekmektedir.

#### 4.3.1 TCP SYN(Stealth) Scan (-sS)

TCP portlarını taramanın en hızlı yolu olduğu için en popüler tarama türüdür. Hedef sisteme bir SYN bayraklı TCP paketi gönderilerek gelen cevap doğrultusunda portun açık olup olmadığı tespit edilmektedir. Gönderilen SYN paketine, SYN/ACK paketi ile cevap gelirse hedef port açıktır. RST paketi ile cevap dönerse hedef port kapalıdır. Herhangi bir cevap gelmezse port filtreli sonucunu elde edilir. Alınan SYN/ACK paketine RST paketi gönderilip bağlantı düşürülür.

```linux
root@Stormer:~# nmap -sS 192.168.16.160
```
**–sS** parametresi kullanılarak hedef IP adresine yönelik TCP SYN taraması gerçekleştirilir.

#### 4.3.2 TCP Connect Scan (-sT)

TCP Connect Scan taraması genellikle yetkisiz Unix makinelerine ve IPv6 hedeflerine yönelik yapılmaktadır. Ayrıca TCP SYN Scan taramasını çalışmadığı veya yetersiz kaldığı durumlarda işlem görmektedir. Nmap aracı, işletim sistemi üzerinden connect system çağrısında bulunarak hedef makine ile port üzerinden bağlantı kurulmasını sağlayacaktır. Böylelikle port taramamları gerçekleştirilmektedir.

```linux
root@Stormer:~# nmap -sT -v 192.168.16.160
```

**–sT** parametresi kullanılarak TCP Connect Scan taraması gerçekleştirildi. Tarama sırasında gerçekleştirilen işlemlerin detaylı bir şekilde gösterilmesi için –v parametresi kullanıldı. Öncelikle ARP Ping Scan taraması gerçekleştirilip makinenin aktif olduğu tespit edildi. Sonrasında DNS çözümlemesi yapıldı. DNS çözümlemesinden sonra Connect Scan taraması yapılarak açık portlar elde edildi.

#### 4.3.3 UDP Scan (-sU)

Sistemlere yönelik taramalarda sadece TCP portlarına yönelik taramalar gerçekleştirmemek gerekir. Çünkü UDP portlarına yönelik güvenlik açıkları da bulunmaktadır. En popüler servisler TCP protokolü üzerinde çalışabilir. Fakat UDP üzerinde de servisler çalışmaktadır. Örneğin, DNS, SNMP ve DHCP servisleri UDP’yi kullanır. UDP taraması, TCP taramasına göre yavaş ve zor bir tarama olduğu için güvenlik uzmanları genellikle bu taramaları yapmama hatasına düşmektedir. Ayrıca sistem ve ağ yöneticileri de genellikle UDP portlarına yönelik güvenlik önlemlerini eksik almaktadır. Bu durum göz önüne alındığında UDP protokollerinde güvenlik açığı ortaya çıkma olasılığı yüksektir.

Nmap üzerinden UDP taraması gerçekleştirmek için –sU parametresi kullanılır. UDP portlarını tespit etmek için UDP paketleri gönderilmektedir. Fakat, -data, **--data-string** veya **--data-length** parametrelerini kullanmadan tarama yapılırsa UDP paketleri boş gidecektir. Cevap olarak ICMP port Unreachable hatası döndürülürse port kapalıdır. UDP paketi ile cevap dönerse port açıktır. Herhangi bir cevap alınmadığında port açık veya filtreli olabilir. Ayrıca, UDP taramalarında hızlı bir şekilde tarama yapmak, yavaş hostları atlamak ve güvenlik duvarını atlatmak için **--host-timeout** parametresi kullanılmaktadır.

```linux
root@Stormer:~# nmap -sU -sV 192.168.16.160
```

Gönderilen bazı UDP paketlerine karşı herhangi bir cevap gelmediği için Nmap portları open&#124;filtered olarak belirtmiştir. Açık UDP portlarında kullanılan servislerin sürüm bilgisini elde etmek için –sV parametresi kullanılır.

#### 4.3.4 TCP NULL, FIN, Xmas Scans (-sN, -sF, -sX)

TCP NULL Scan taramasında -sN parametresi kullanılarak boş bir TCP paketi gönderilmektedir.  TCP bayrak başlık değeri 0’dır. TCP FIN Scan taraması, -sF parametresi kullanılarak TCP FIN bitinin ayarlanmasıyla gerçekleştirilen tarama türüdür. TCP Xmas Scan taraması ise –sX parametresi kullanılarak FIN, PSH ve URG bayraklarının ayarlanmasıyla gerçekleştirilen tarama türüdür.

Tarama türünün en önemli avantajı, durum bildirmeyen güvenlik duvarları ve paket filterleme yönlendiricileri üzerinden gizlice tarama yapmayan olanak vermesidir. Bu tür güvenlik duvarları, SYN biti set edilen ve ACK biti silinen TCP paketlerini engeller. NULL, FIN ve Xmas taramaları SYN bitini silerek tarama yaptığı için bu kuralı atlayabiliyor. Diğer avantajı ise, bu tarama türleri bir TCP SYN taramasına göre daha gizlidir.

```linux
root@Stormer:~# nmap -sN -v -n 192.168.16.160
```

Yukarıdaki nmap komut satırı kullanılarak TCP NULL taraması gerçekleştirilir. Ayrıca –n parametresi kullanılarak Reverse DNS çözümlemesi yapılması istenilmemiştir. -v parametresi ile detaylı bilgi göstermesi sağlanılmıştır. Ek olarak, tarama sırasında kullanılan problardan bazıları gecikmeden dolayı drop edilebilir.

```linux
root@Stormer:~# nmap -sF -v -n 192.168.16.160
```
Yukarıdaki nmap komut satırı kullanılarak TCP FIN taraması gerçekleştirilebilir. Bu tarama türünde de varsayılan 1000 porta yönelik tarama yapılır.

#### 4.3.5 TCP ACK Scan (-sA)

Durum bilgisi veren güvenlik duvarları, ağ bağlantılarının gezinimini, çalışma durumunu ve karakteristik özelliklerini izleyen güvenlik duvarlarıdır. Bu tarama türü güvenlik duvarı kural kümelerini eşleyerek durum bilgisi verip vermediğini veya hangi portunu filtereli olup olmadığını tespit etmek için kullanılır. Port taramalarında portun açık veya kapalı olduğunu tespit etmemesi bir dezavantajıdır.

Açık ve kapalı portların olup olmadığını tespit edememesinin sebebi ise tarama sırasında cevap olarak iki port durumuna da RST paketi gönderilmesidir. Nmap bu cevabı unfiltered olarak işaretlemektedir. Unfiltered işaretlenme durumu, ACK paketlerinin hedef makineye ulaştığı ve erişimin sağlandığını belirtir.

```linux
root@Stormer:~# nmap -sA -n -v 192.168.16.160
```

Ykarıdaki nmap komut satırı bir TCP ACK Scan tarama türüdür. Bu taramanın gerçekleştirilmesi için –sA parametresi kullanılmaktadır. Probların bazıları drop edilebilir. Bu durum bütün tarama türlerinde meydana gelmektedir. Çünkü probların gönderiminde herhangi bir gecikme olduğunda ağ üzerinde düşebilmektedir.

#### 4.3.6 TCP Window Scan (-sW)

Bu tarama türü TCP ACK Scan tarama türüne benzemektedir. Tarama sırasında cevap olarak alınan RST paketlerini unfiltered olarak işaretlemek yerine, RST paketlerinin TCP Window alanına bakılarak işlem yapılır. TCP Window alanında pozitif window ise portu open, sıfır window ise portu kapalı olarak işaretlemektedir. Bu tarama türü, internet üzerindeki az sayıdaki sistemlerin uygulama detaylarına dayandığı için az kullanılıp güvenilmemektedir.

```linux
root@Stormer:~# nmap -sW -n -v 192.168.16.160
```

TCP Window taraması, –sW paramretresi kullanılarak gerçekleştirilen bir tarama türüdür.

#### 4.3.7 TCP Maimon Scan (-sM)

Bu tarama türü, FIN, NULL ve Xmas tarama türleri ile benzerdir. Farklı olan tarafı ise hedef sisteme gönderilen probun FIN/ACK olmasıdır. Bu tarama, gizli bir firewall-evading tarama türüdür. TCP FIN ve ACK bayraklarının ayarlamasını ile gerçekleştirilen taramadır. Bu tarama ile paket filtreleyen güvenlik duvarları atlatılabilir.

```linux
root@Stormer:~# nmap -sM -v 192.168.16.160
```

#### 4.3.8 TCP Idle Scan (-sI)

Bu tarama yöntemi, hedefe yönelik kör bir TCP tarama yapılmasını sağlamaktadır. Kör bir TCP taraması denenmesinin sebebi ise hedef makineye gönderilen paketlerin hiçbirinin taramayı gerçekleştiren makinenin IP adresi ile gönderilmemesidir. Side-channel saldırısı ile ağ üzerindeki zombi makineler tahmin edilir ve IP Fragmentation ID dizisi oluşturularak kullanılır. Ayrıca TCP Idle Scan, en gizli tarama türüdür. Aynı zamanda bu tarama türü yavaş ve karmaşıktır.

| ![atgr1]({{ site.url }}/assets/img/Nmap/part3/sekil438a.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil4.3.8.a – TCP IDLE SCAN Port Durumlarının Gösterimi* |

<br/>

Şekil 4.3.8.a ‘da gösterildiği gibi TCP Idle Scan taramasında port durumları IP ID fragmentation işlemine göre belirlenir.

**Açık port durumunun belirlenmesi:**

- Saldırgan makine, zombi makinesine SYN/ACK bayrağı gönderir.

- Zombi makinesi, saldırgan makineye IP ID değeri 31337 olan bir RST bayrağı gönderir.

- Saldırgan makine, zombie makinesinin paket bilgileri ile hedef makineye SYN bayrağı gönderir.

- Hedef makine, zombi makinesine SYN/ACK bayrağı gönderir.

- Zombi makinesi, hedef makineye IP ID değeri 31338 olan bir RST bayrağı gönderir.

- Saldırgan makine tekrar zombie makinesine SYN/ACK bayrağı gönderir.

- Zombi makinesi, saldırgan makineye IP ID değeri 31339 olan bir RST bayrağı gönderir.

- Saldırgan makine, zombi makinesindeki IP ID değerinin 2 sayısı kadar arttığını tespit ederek hedef makine ile iletişime geçtiğini doğrulayıp hedef makine portunu açık olarak işaretler.

**Kapalı port durumunun belirlenmesi:**

- Saldırgan makine, zombi makinesine SYN/ACK bayrağı gönderir.

- Zombi makinesi, saldırgan makineye IP ID değeri 31337 olan bir RST bayrağı gönderir.

- Saldırgan makine, zombie makinesinin paket bilgileri ile hedef makineye SYN bayrağı gönderir.

- Hedef makine, zombi makinesine RST bayrağı gönderir.

- Saldırgan makine tekrar zombie makinesine SYN/ACK bayrağı gönderir.

- Zombi makinesi, saldırgan makineye IP ID değeri 31338 olan bir RST bayrağı gönderir.

- Saldırgan makine, zombi makinesindeki IP ID değerinin 1 sayısı kadar arttığını tespit ederek hedef makine ile iletişime geçmediğini doğrulayıp hedef makine portunu kapalı olarak işaretler.

**Filtrelenmiş port durumunun belirlenmesi:**

- Saldırgan makine, zombi makinesine SYN/ACK bayrağı gönderir.

- Zombi makinesi, saldırgan makineye IP ID değeri 31337 olan bir RST bayrağı gönderir.

- Saldırgan makine, zombie makinesinin paket bilgileri ile hedef makineye SYN bayrağı gönderir.

- Hedef makine, zombi makinesine hiçbir dönüş yapmaz.

- Saldırgan makine tekrar zombie makinesine SYN/ACK bayrağı gönderir.

- Zombi makinesi, saldırgan makineye IP ID değeri 31338 olan bir RST bayrağı gönderir.

- Saldırgan makine, zombi makinesindeki IP ID değerinin 1 sayısı kadar arttığını tespit ederek hedef makine ile iletişime geçmediğini doğrular. Saldırgan bakış açısıyla bakıldığında filtrelenmiş port ve kapalı port ayırt edilmez.

```linux
root@Stormer:~# nmap -sI 192.168.10.12 192.168.16.160 -Pn
```

TCP Idle Scan taramasını başlatmak için –sI parametresinin kullanılması yeterli olacaktır. Parametreden sonra gelen IP adresi zombi IP adresidir. Varsayılan olarak 80. Portu kullanmaktadır. İsteğimiz dahilinde zombi bilgisayardaki açık olan portlardan biri [IP:Port] formatında girilebilir. Zombi makina IP adresinden sonra hedef makinanın IP adresi girilmelidir. Daha sonra taramada istenilen özelliğe göre parametreler girilebilir. Örnek olarak pingsiz tarama için –Pn parametresi kullanılmıştır. Ayrıca wireshark aracı üzerinden ağ dinlemeye alındığında zombi makina kullanılarak tarama işlemi yapıldığı görülmektedir.

#### 4.3.9 IP Protocol Scan (-sO)

Bu tarama türü teknik olarak port taraması değildir. Hedef sistem üzerinde hangi protokollerin çalıştığını tespit etmek için kullanılır. –p parametresi kullanılarak port numarası yerine protocol numarası yazılmaktadır. –p parametresinin protokol veya port taraması olup olmadığının ayırt edilebilmesi için –sO parametresi kullanılır. –p  parametresine atanan protokol numarası ile IP Protocol taraması gerçekleştirilir. Çıktı formatı normal formata benzemektedir. Fakat numaralarının yazıldığı yerde protokol yazılmıştır.

```linux
root@Stormer:~# nmap -sO -p135 n -v 192.168.16.160
```

Yukarıdaki nmap komutunda iki farklı özelliğe göre tarama yapılmıştır. İlk tarama komutu, –sO parametresi ile protokol taraması gerçekleştirileceği belirtilir. –p parametresine protokol numarası atanır. Detaylı çıktı olması için –v parametresi, DNS çözümlemesinin yapılmaması için –n parametresi kullanılmıştır.  İkinci tarama komutunda da port taraması yapılmıştır. İlk taramada 135 numaralı protokole yönelik tarama yapılırken, TCP olup olmadığına bakılmayıp port durumu open&#124;filtered olarak işaretlenmiştir. Ayrıca ilk taramada ekstra servis tespiti için parameter girilmesine ihtiyaç duyulmayıp varsayılan olarak protokolün kullanmış olduğu servis hakkında bilgi verir. İkinci taramadaki çıktıda Protokol başlığı yerine varsayılan Port başlığı adı altında 135/TCP belirtilip durumu open olarak işaretlenmiştir. Port üzerinde çalışan servis sürümünün tespiti için –sV parametresi kullanılır.

#### 4.3.10 TCP FTP Bounce Scan (-b)

FTP protokolünün Proxy FTP bağlantısı özelliği vardır. Bu özellik, kullanıcının bir FTP sunucusuna bağlanmasını ve ardından dosyaları üçüncü taraf bir sunucuya göndermesini sağlar. Bu özellikler saldırganlar tarafından kullanıldığı için az sunucu bu özelliği kullanır. Bir diğer dezavantaj ise FTP sunucusunun diğer hostları taramasına izin verilmesidir. Bu tarama ise FTP sunucu tespit ettiği zaman sırasıyla diğer hostlara dosya gönderir. Alınan hata mesajından portun açık olup olmadığı anlaşılmaktadır. Bu durum güvenlik duvarlarını atlatmak için kullanılır. Çünkü kurumsal FTP sunucuları, genellikle bütün ana bilgisayarların erişebilecekleri bir konuma koyulur. Bu tarama –b parametresi ile kullanılmaktadır. **kullanıcıadı:parola@ftpserver_IP:port** şeklinde argüman almaktadır.Port numarası girilmezse varsayılan 21. port üzerinden işlem yapmaktadır. Bir güvenlik duvarını atlatmak için sadece 21. port numarası taranıp ftp-bounce NSE dosyası kullanılabilir.

```linux
root@Stormer:~# nmap -v -b anonymous:-wwwuser@192.168.16.128:21  192.168.16.128 -Pn
```
FTP Bounce Scan taramasının yapılması aynı zamanda bir saldırı olarak görülür. Çünkü FTP Bounce sunucuları üzerinde başka makinelere yönelik port taraması ve dosya gönderilmesi kolaylıkla yapılabilir.

#### 4.3.11 SCTP INIT Scan (-sY)

Bu tarama türü, TCP SYN taramasının SCTP eşdeğeridir. Engellenmeyen bir ağda saniyede binlerce port taranabilir. TCP SYN taraması gibi INIT taraması da SCTR ilişkilerini tamamlamadığı için göze çarpmayan gizli bir taramadır.

```linux
root@Stormer:~# nmap -sY -v -n 192.168.16.160
```
**-sY** parametresi kullanılarak tarama başlatılabilir. Bu tarama tekniği half-open scanning olarak adlandırılır. Çünkü tam bir SCTP ilişkilendirmesi açılmamaktadır. Bir INIT chuck gönderilip, gerçek bir ilişkilendirme yapılıyormuş gibi hedefe gösterilerek hedef makineden bir cevap beklenilir. Dönen cevap bir INIT-ACK chuck ise portun açık olduğunu, bir ABORT chuck ise portun kapalı olduğunu belirtmektedir. Birkaç INIT chuck isteğinden sonra herhangi bir cevap alınmadığında portun filtreli olduğu belirtilmektedir.

#### 4.3.12 SCTP COOKIE ECHO Scan (-sZ)

Bu tarama türü gelişmiş bir SCTP taramasıdır. Bu tarama türünün avantajı, INIT taramasına göre bir port taraması olarak görülmemesidir. Dezavatajı ise, tarama sırasında açık ve filtreli portları birbirinden ayırt edememesidir. Bu durum dışında portun kapalı olması durumunda SCTP INIT taramasındaki gibi bir ABORT cevabı aldığında portun kapalı olduğu belirtilir.

#### 4.3.13 Özel TCP Taraması (–scanflags)

TCP bayraklarının özel olarak seçilip kullanılması durumunda **--scanflags** parametresi kullanılır. **--scanflags** parametresi, güvenlik duvarlarının atlatılmasında kullanılabilir. **--scanflags** parametresine TCP bayraklarının isimleri atandığı gibi TCP bayraklarını ifade eden sayısal değerlerde atanabilir. Örnek olarak, 9 sayısı PSH ve FIN bayraklarının bir arada kullanılacağı anlamına gelmektedir. Ayrıca herhangi bir tarama türü belirtilmezse varsayılan olarak SYN taraması gerçekleştirilir.

```linux
root@Stormer:~# nmap --scanflags PSH -v -n 192.168.16.160
```

**--scanflags** parametresi kullanılarak özel bir tarama gerçekleştirilmiştir. Bu taramada ek olarak bir tarama türü seçilmediği için varsayılan olarak SYN taraması gerçekleştirilmiştir.

### 4.4 Port Seçerek Tarama

Nmap varsayılan olarak port taraması yaptığında popüler 1000 portu taramaktadır. -F parametresi kullanılarak hızlı tarama yapılması istenildiğinde popüler 100 port taranır. Ayrıca **--top-ports** parametresi kullanılarak popüler portlar taranır. Popüler port taramaları dışında -p parametresi kullanılarak port numarası, taranılacak port aralığı veya protokol numarası belirtilerek tarama yapılabilir. -p parametresinin kullanım şekilleri aşağıdaki gibidir:

- -p 445 : Yalnızca SMB portuna yönelik bir tarama gerçekleştirilmesini ifade eder.

- -p ssh : SSH servisinin çalıştığı 22 numaralı porta yönelik bir tarama belirtir.

- -p 22,25,80 : Birden çok porta yönelik tarama gerçekleştirilir. Bu parametreden sonra –sS parametresi kullanılırsa sadece TCP portları, -sU parametresi kullanılırsa UDP portları taranmaktadır.

- -p 22-89,110 : Böylelikle 22 ve 89 numaralı portlar arasındaki portları ve 110 numaralı portu taramaktadır.

- -p- : Bu parametrenin kullanımı ise 65535 tane portun taranmasını sağlar. (-p0-)

- -pT:22,U:53 : TCP 22. port ve UDP 53. port taraması yapılır.

- -p http* : HTTP ile başlayan bütün servislerin olduğu portlar taranır.

Yukarıda gösterilen parametreler –p parametresi ile kullanılmakta olup, port taraması için ek olarak kullanılan parametreler de aşağıdaki gibidir:

- **–-exclude-ports 20-30** : Bu parametre ile 20 ile 30 numaralı portlar arasındaki port taramaları yapılmayacağını göstermektedir.

- **-F** : Bu parametre ile hızlı port taraması için kullanılır. Varsayılan olarak taranan popüler 1000 port yerine popüler 100 port taraması yapılmaktadır.

- **-r** : Nmap, port taramalarını varsayılan olarak rasgele yapmaktadır. Bu port taramalarını belirli bir sıraya görmek için bu parametre kullanılmaktadır.

- **–-port-ration** : Port taramalarının sıklık derecesini belirtmek için kullanılır. Atanan değer 0 ile 1 sayısı arasında olmalıdır.

- **--top-ports** : Taranacak popüler port sayısı verilir.

#### 4.4.1 Port Taramada Zamanlama Önemi

Nmap aracı, hedef sistemlere yönelik port taramalarını gerçekleştirirken hız ve zaman önemli bir yer tutmaktadır. Özellikle büyük ağlara yönelik gerçekleştirilen taramalarda önemlidir. Ayrıca verimlilik ve performansında iyi olması gerekir. Port taramalarında zamanlama ile ilgili kullanılan bazı parametreler aşağıdaki gibidir:

- **–-min-rtt-timeout, –-max-rtt-timeout, –-initial-rtt-timeout** : Port taramalarında kullanılan probların cevap verebilecekleri minimum ve maximum sürelerin ayarlanması için kullanılır.

```linux
root@Stormer:~# nmap -T4 -Pn -p135,139,445 -v -n --max-rtt-timeout 200ms --initial-rtt-timeout 150ms 192.168.16.160
```

- **–-host-timeout** : Host başına belirtilen tarama süresidir. Zaman aşımı durumunda tarama iptal edilir.

- **--min-rate, –-max-rate** : Taramada kullanılacak probların saniyede kaç adet gönderileceğini belirler.

- **–-max-retries** : Tek bir porta verilen maksimum iletim sayısını belirtir.

- **–-min-hostgroup, –-max-hostgroup** : Paralel olarak taranacak minimum ve maksimum host sayısını belirtir.

- **-–min-parallelism, –-max-parallelism** : Paralel olarak taranacak minimum ve maksimum prob sayısını belirtir.

- **–-scan-delay, –-max-scan-delay** : Tarama sırasında gönderilen probun vereceği cevabın beklemesi için bir sınırlamadır. Çünkü büyük bir taramada gecikme olması durumunda tarama süresi uzayabilir.

- **–-defeat-rst-ratelimit** : Yalnızca açık portlara önem verildiğinde kullanışlıdır. Bu parametre kullanılarak hız sınırlamaları göz ardı edilir. Taramalarda RST cevabı için uzun bir süre beklenmediğinde, bazı portların yanıt vermeyeceği için doğruluğu azaltabilir.

- **–-defeat-icmp-ratelimit** :  Hız için doğruluk sunan bir parametre olup ICMP hata mesajlarını hızlandıran hostlara karşı UDP tarama hızını arttırır. Yanıt vermeyen portları varsayılan olarak open&#124;filtered yerine close&#124;filtered olarak işaretler.

- **–-nsock-engine epoll&#124;kqueue&#124;poll&#124;select** : Bir nsock IO multiplexing motorunun kullanımını sağlamaktadır. **nmap –V** parametresi ile hangi motorların desteklediğini görülebilir.

### 4.5 Nmap Performansının Optimize Edilmesi

Nmap aracı ile büyük ağlarda tarama yapılırken performansın optimize edilmesi gerekmektedir. Optimize işlemi iyi yapıldığı sürece kısa sürede sonuçlar elde edilebilir. Bunun en önemli yollarından biri, –sn parametresi kullanılarak açık hostların önceden tespit edilmesidir. Böylece ağdaki kapalı hostlara yönelik gereksiz port taramaları yapılmayıp, taramanın hızlı ve kısa sürede sonuçlanması sağlanır. Nmap aracı varsayılan olarak en yaygın 1000 portu taramaktadır. Gereksiz port taramalarının önüne geçmek için –p, -F ve **--top-ports** parametreleri kullanılabilir. –A parametresi ile yapılacak agresif taramalarda işletim sistemi tespiti, servis versiyon tespiti, traceroute, port taraması gibi işlemler yapılmaktadır. Bu taramalarda **--osscan-limit** ve **--max-os-tries** gibi parametreler kullanılarak işletim sistemi tespitinin defalarca kez tekrarlanmasının önüne geçilebilir. Gerekmediği sürece DNS çözümlemesi işleminin yapılmaması için –n parametresi kullanılabilir. Ayrıca ping atılmadan tarama yapılması istenildiğinde –Pn parametresi kullanılabilir.

Taramaların zamanında bitmesi ve sonuç üretmesi için –T parametresi ile belirtilen zamanlama şablonları kullanılabilir. UDP taraması yapılması durumunda TCP taraması ile beraber yapılmaması daha uygundur. Çünkü TCP taramasında ICMP hata oranı sınırlaması ile karşılaşabilir. Önemli noktalardan biri de Nmap aracının güncel tutulması gerekmektedir. Çünkü, Nmap geliştiricileri aracı her geçen gün geliştirip optimize çalışmaları yapmaktadır. Son olarak band genişliği ve CPU’nun arttırılması, nmap aracı üzerindeki iş yükünü azaltmak için kullanılabilir.

### 4.6 Zamanlama Şablonları (-T)

Nmap aracı ile taramalardaki zamanlama ayarları -T parametresi kullanılarak ayarlanabilmektedir. Hedef ağa yönelik nasıl bir tarama gerçekleştirmek istersek ona göre –T parametresine değer veriyoruz. Bu tür işlemler şablonlar haline getirilmiştir. Toplamda 6 şablon bulunmaktadır. Bunlar, paranoid(0), sneaky(1), polite(2), normal(3), aggressive(4) ve insane(5) olarak adlandırılır. Bu şablonlar –T parametresi ile kullanılmaktadır.

```linux
root@Stormer:~# nmap -sV -T4 -n 192.168.16.160
```

**-T** parametresinin agresif modu kullanıldı. Bu mod ile –sV parametresi kullanılarak sürüm tespit yapılır. –n parametresinin kullanımı ile DNS çözümlenmesi yapılmamıştır. Böylece taramanın sonuçlanması için gereken zaman kısalır.

Polite(2) mod, daha az bant genişliği kullanmak ve makine kaynaklarını hedeflemek için taramayı yavaşlatmaktadır. Normal(3) mod, -T3 olarak gösterilip zamanlama ayarlaması için herhangi bir özelliği tetiklemez. Aggressive(4) mod, oldukça hızlı olup güvenilir bir ağda tarama yapıyormuş gibi taramaları hızlandırır. Insane(5) mod ise, olağanüstü hızlı bir ağda olduğunuzu veya hız için kesin bir hassasiyetten ödün vermeye istekli olduğunuzu varsaymaktadır. Bu şablonlar kullanıcının Nmap’in tam zamanlama değerlerini seçmesini sağlarken ne kadar agresif olmak istediklerini belirlemesini sağlar. Şablonlar ayrıca hassas kontrol seçeneklerinin bulunmadığı bazı küçük hız ayarlamaları da yapar. Örneğin, -T4, TCP taramaları için dinamik tarama gecikmesinin 10 ms’yi geçmesini ve bu değeri 5 ms’de -T5 büyüklüğü ile yasaklamaktadır. Şablonlar, ince ayarlı kontrollerle birlikte kullanılabilir ve ayrıntılı parametreler bu belirli değerler için genel zamanlama şablonlarını geçersiz kılmaktadır. Oldukça modern ve güvenilir ağlar taranırken -T4 kullanılabilir.

| ![atgr2]({{ site.url }}/assets/img/Nmap/part3/sekil46b.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 4.6.b -T parametresine yönelik Şablonları Gösterimi* |






















