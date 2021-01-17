---
title:  "Derinlemesine Nmap Kullanımı"
date:   2020-02-12 
layout: post
categories: 
    - network-guvenligi
thumbnail: posts/DNmap.png
---

## Derinlemesine Nmap Kullanımı

Nmap, günümüzdeki en gelişmiş ağ tarama araçlarının başında gelir. Nmap, ağda bulunan cihazların IP adreslerini, cihaz bilgisini, açık portlarını, işletim sistemini, açık portlarda çalışan servisleri ve cihazlarda bulunan güvenlik açıklarını tespit etmek için kullanılır.

**Herhangi bir kurum veya kuruluşa yönelik yapılan güvenlik testlerinde, sızma testi aşamalarından bilgi toplama, tarama ve listeleme aşamaları Nmap aracı ile yapılabilir. Öncelikle hedef ağ üzerinde bilgi toplama aşamasını gerçekleştirmek için hedef ağ taranır ve hedef ağ üzerinde çalışan cihazlar hakkında bilgi elde edilir. Çalışan cihazların IP adresleri, Hostname bilgileri bu bilgiler arasında yer alır.**

Örnek 192.168.30.0/24 **CIDR** bilgisine sahip bir ağ üzerinde, Nmap aracı ile sızma testinin yaşam döngüsünde bulunan bilgi toplama, tarama ve listeleme aşamaları gerçekleştirilebilir.

```linux
root@Stormer:~# nmap -sn 192.168.30.0/24
```

| ![atgr1]({{ site.url }}/assets/img/DerinlemesineNmap/resim1.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim1 : sn parametresi kullanılarak Nmap taraması* |

<br/>

Resim1’de, 192.168.30.0/24 IP aralığında hangi makinelerin açık olduğu tespit edildi. –sn parametresi, hedef makinelere herhangi bir port taraması yapmadan, makinelerin açık olup olmadığını kontrol etmek için kullanılır. Yapılan taramada 192.168.30.1 IP adresli fiziksel makine dışında 5 tane IP adresine sahip sistemin açık olduğu tespit edildi. 192.168.30.2 Gateway IP adresi, 192.168.30.254 Broadcast IP adresi ve 192.168.30.137 ise taramayı gerçekleştiren IP adresidir. 192.168.30.180 ve 192.168.30.184 IP adresleri açık olarak tespit edilen hedef makinelerdir. Makineler ARP Ping Scan Taraması sonucunda tespit edilmiştir. Bazı güvenlik duvarları, Ping taramasını engeller. Ping taramasını engelleyen güvenlik duvarlarını atlatmak için –Pn parametresi kullanılabilir.

```linux
root@Stormer:~# nmap -A -p- 192.168.30.180 192.168.30.184
```

| ![atgr2]({{ site.url }}/assets/img/DerinlemesineNmap/resim2.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim2 : Agresif modda full port Nmap taraması* |

<br/>

Resim2’de 192.168.30.180 ve 192.168.30.184 IP adreslerine yönelik agresif modda (-A) ve full port taraması (-p-) yapılmıştır. 192.168.30.180 IP adresli makinede 80, 2121, 5985 numaralı portlar ve  bu portlar üzerinde çalışan servisler tespit edilmiştir. 2121. portta çalışan FTP servisine yönelik ftp-anon NSE script’i kullanılarak Anonymous kullanıcısının sistemde aktif olduğu tespit edilmiştir. Makinenin İşletim sistemi Microsoft Windows Server 2016 olarak işaretlenmiştir. 192.168.30.184 IP adresli makine Resim3’te gösterilmektedir.


| ![atgr3]({{ site.url }}/assets/img/DerinlemesineNmap/resim3.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim3 : Nmap taraması sonucu elde edilen çıktının bir parçası* |

<br/>

Resim3’te 192.168.30.184 IP adresine yapılan tarama sonucunda 445 ve 5985 numaraları portların açık olduğu tespit edilmiştir. 445. portta çalışan microsoft-ds servisinin versiyon bilgisinde işletim sistemi bilgisi **Microsoft Server 2012 R2 Evaluation 9600** olarak tespit edilmiştir. Ayrıca agresif modda tarama yapıldığı için 445 numaralı portta çalışan SMB servisine yönelik bazı NSE script’leri kullanılmıştır. smb-os-discovery script’inin ürettiği çıktı içerisinde işletim sistemi bilgisi, bilgisayar adı, sistem zamanı bilgisi tespit edilmiştir. **smb-security-mode** NSE script’i kullanılarak guest kullanıcısının hedef sistemde aktif olduğu belirtilmiştir. Sonuç olarak sisteme yönelik tarama ve bilgi toplama işlemleri Nmap aracı ile gerçekleştirilmiştir.

Bilgi toplama ve tarama işlemleri yapıldıktan sonra açık portlarda zafiyet olup olmadığını kontrol etmek için Nmap içerisinde bulunan script’ler kullanılabilir. Hedef sistemde zafiyet olup olmadığını kontrol eden bu script’ler vuln kategorisinde yer almaktadır. Resim4’te hedef sistemlere yönelik güvenlik açığı tespiti yapılmıştır.

```linux
root@Stormer:~# nmap -p 80,445,2121,5985 --script=vuln,auth 192.168.30.180 192.168.30.184
```

| ![atgr4]({{ site.url }}/assets/img/DerinlemesineNmap/resim4.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim4 : NSE vulnerability ve authentication kategorisindeki scriptlerinin kullanılması* |

<br/>

Resim4’te 192.168.30.180 ve 192.168.30.184 IP adresine sahip makinelerin 80, 445, 2121 ve 5985 numaralı portlarına yönelik tarama yapıldı. Yapılan taramada vuln ve auth kategorisindeki script’ler kullanıldı. Vuln script’leri zafiyet tespiti için, auth script’leri ise kimlik doğrulama işlemleri için kullanıldı.

Resim4’te 192.168.30.180 IP adresinin 80. portunda HTTP servisi çalıştığı için **http-csrf, http-dombased-xss, http-stored-xss** scriptleri kullanıldı. 192.168.30.184 IP adresli makinenin 445. portuna, **smb-vuln-ms10-054, smb-vuln-ms10-061, smb-vuln-ms17-010 ve smb-vuln-regsvc-dos** scriptleri kullanıldı. Vuln script kategorisinde olan **smb-vuln-ms17-010** scripti, hedef sistemde **MS17-010** olarak adlandırılan uzaktan kod çalıştırma zafiyetini tespit eder. Oluşan script çıktısı içerisinde **VULNERABLE** sözcüğünün olması makinenin zafiyetli olduğunu gösterir.

Zafiyet tespitinde, –script parametresine, sadece script’lerin kategorileri değil, script’lerin isimleri ve uzantıları da atanabilir. Resim5’te, 192.168.30.184 IP adresli makinenin 445. portuna yönelik zafiyet tespiti yapılmıştır.

```linux
root@Stormer:~# nmap -p 445 --script=smb-vuln-* 192.168.30.184
```

| ![atgr5]({{ site.url }}/assets/img/DerinlemesineNmap/resim5.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim5 : SMB protokolüne yönelik NSE scriptlerinin kullanılması* |

<br/>

Resim5’te smb-vuln-* değerindeki yıldız (*) işareti, smb-vuln- sözcüğü ile başlayıp farklı sözcükle biten bütün script’lerin kullanılmasını sağlar. Aynı şekilde farklı portlara yönelik taramalarda script’lerin kullanımı özelleştirilebilir. Örneğin, **nmap -p80 –script=http-* 192.168.30.180** komutu kullanılabilir.

Ek olarak, hedef sistemde Windows güvenlik duvarının aktif veya pasif olduğu durumlarda, Nmap aracının üretmiş olduğu çıktıların nasıl olduğunu bilmek gerekir. Resim6’da, Nmap aracı ile gerçekleştirilen bir taramada güvenlik duvarının aktif ve pasif olduğu durumlardaki çıktısı gösterilmektedir.

```linux
root@Stormer:~# nmap 192.168.30.184
```

| ![atgr6]({{ site.url }}/assets/img/DerinlemesineNmap/resim6.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim6 : Firewall durumlarına göre Nmap Çıktısı* |

<br/>

Resim6’da **nmap 192.168.30.184** komutu kullanılarak yapılan varsayılan taramada, Windows güvenlik duvarı aktif iken, 445. port açık olarak tespit edilmiştir. 445. portun açık olarak tespit edilmesinin sebebi, güvenlik duvarında bulunan Inbound kurallarında 445. port üzerindeki trafiğe izin veren bir kuralın aktif edilmesidir. İkinci Nmap taramasında ise 445, 139, ve 135 numaralı portların açık olduğu tespit edilmiştir. Bu iki örnekle Windows güvenlik duvarının aktif ve pasif olduğu durumlarındaki farklılıklar görülebilir. Resim7’de ise, güvenlik duvarlarını atlatma tekniklerinden biri olan TCP ACK Scan yapılmıştır.Resim6’da **nmap 192.168.30.184** komutu kullanılarak yapılan varsayılan taramada, Windows güvenlik duvarı aktif iken, 445. port açık olarak tespit edilmiştir. 445. portun açık olarak tespit edilmesinin sebebi, güvenlik duvarında bulunan Inbound kurallarında 445. port üzerindeki trafiğe izin veren bir kuralın aktif edilmesidir. İkinci Nmap taramasında ise 445, 139, ve 135 numaralı portların açık olduğu tespit edilmiştir. Bu iki örnekle Windows güvenlik duvarının aktif ve pasif olduğu durumlarındaki farklılıklar görülebilir. Resim7’de ise, güvenlik duvarlarını atlatma tekniklerinden biri olan TCP ACK Scan yapılmıştır.

```linux
root@Stormer:~# nmap -sA 192.168.30.184
```

| ![atgr7]({{ site.url }}/assets/img/DerinlemesineNmap/resim7.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim7 : Firewall durumlarına göre Nmap Çıktısı* |

<br/>

Resim7’de –sA parametresi kullanılarak TCP ACK Scan yapılmıştır. Resim7’de güvenlik duvarının pasif olduğu durumda varsayılan olarak belirtilen 1000 port unfiltered olarak işaretlenir. Güvenlik duvarının aktif olduğu durumda ise, belirtilen 1000 port filtered olarak işaretlenir.

TCP ACK Scan dışında, güvenlik duvarlarını atlatmak için kullanılan tekniklerden bir diğeri ise TCP Window Scan’dir. Resim8’de TCP Window Scan gösterilmektedir.

```linux
root@Stormer:~# nmap -sW 192.168.30.184
```

| ![atgr8]({{ site.url }}/assets/img/DerinlemesineNmap/resim8.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim8 : Firewall durumlarına göre Nmap Çıktısı* |

<br/>

Resim8’de **nmap –sW 192.168.30.184** komutuyla TCP Window Scan yapılmıştır. Windows güvenlik duvarının aktif olduğu durumda 1000 port filtered olarak işaretlemiştir. Windows güvenlik duvarının pasif olduğu durumda ise, 1000 port closed olarak işaretlemiştir.

Windows güvenlik duvarının pasif olduğu durumda, TCP ACK Scan taramasında oluşan çıktıda 1000 port unfiltered olarak işaretlenirken, TCP Window Scan taramasında 1000 port closed olarak işaretlenir. Bu şekilde iki tarama tekniği karşılaştırılabilir. Son olarak Resim9’da fragmentation tekniği kullanılarak güvenlik duvarının aktif ve pasif olduğu durumlarda tarama yapılmıştır.

```linux
root@Stormer:~# nmap -f 192.168.30.184
```

| ![atgr9]({{ site.url }}/assets/img/DerinlemesineNmap/resim9.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim9 : Firewall durumlarına göre Nmap Çıktısı* |

<br/>

Fragmentation tekniği, gönderilen paketlerin parçalanarak gönderilmesini sağlayarak güvenlik duvarının paket içeriğini algılamasını zorlaştırır. Resim9’da Windows Güvenlik duvarının aktif olduğu durumda fragmentation tekniği ile yapılan taramada 445. portun açık olduğu tespit edilmiştir. Yukarıda bahsedilen örnekteki gibi burada da 445. portun açık olarak tespit edilmesinin sebebi, güvenlik duvarında bulunan Inbound kurallarında 445. port üzerindeki trafiğe izin veren bir kuralın aktif edilmesidir. Windows güvenlik duvarının pasif edilmesi durumunda ise, 445,139,135 numaralı portların açık olduğu tespit edilmiştir. Bu şekilde Windows güvenlik duvarının aktif ve pasif olduğu durumlarda Nmap aracının üretmiş olduğu çıktılardaki farklılıklar görülmektedir.





