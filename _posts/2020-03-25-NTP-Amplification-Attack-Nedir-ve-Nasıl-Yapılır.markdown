---
title:  "NTP Amplification Attack Nedir ve Nasıl Yapılır?"
date:   2020-03-25 
categories: 
    - network-guvenligi
thumbnail: posts/ntp.png
---

## NTP Amplification Attack Nedir?

NTP protokolü, internete bağlı olan makinelerin saatlerini senkronize etmek için kullanılır. NTP, dahili veya harici cihazlardan saat bilgisi alır ve 123 numaralı UDP portunu kullanır. Saat senkronizasyonuna ek olarak, NTP’nin eski sürümleri, yöneticilerin NTP sunucusunun gerçekleşen trafik sayısını sorgulamasına olanak veren izleme hizmetini yapmaktadır. “Monlist” adı verilen komut, istekte bulunan makineye, sorgulanan sunucuya bağlanan son 600 makinenin listesinin göndermektedir.

**NTP Amplification saldırısı, DDOS saldırı türlerinden biridir. NTP Amplification saldırısı, bir saldırganın public NTP (Network Time Protocol) sunucularını kullanarak hedef sistemin ağ bant genişliğini, alabildiği üst limitin üstündeki paket sayısına maruz bırakarak trafiğin yavaşlamasına, hatta sistemin servis dışı kalmasına neden olan saldırıdır. Bu saldırı, bir tür yansıma saldırısıdır. Yansıma saldırıları, bir sunucudan sahte bir IP adresine (hedef IP adresi) yanıt verilmesini içerir.**

DNS Amplification saldırısı, aynı teknik kullanılarak sahte DNS istekleri ile DNS sunucularına istek yapılması olarak belirtilebilir. Tipik bir DNS Amplification saldırısında istek ile yanıt arasındaki orantı 1’e 70’tir. Bu durumda, bir saldırgan 1 Gbps ile 70 Gbps trafik üreterek hedef sisteme yönlendirebilir. NTP Amplification saldırısında ise, istek ve yanıt arasındaki orantı, 1’e 20 ile 1’e 200 arasında değişmektedir. Hatta daha büyük orantı da elde edilebilir. Herhangi bir saldırgan, Metasploit Framework, Open NTP Project gibi araçlarla public NTP sunucularına sahte istekte bulunarak hedef sistemin ağ bant genişliğini doldurarak trafiğin yavaşlamasına hatta servis dışı kalmasına neden olan bir DDoS saldırısı gerçekleştirebilir.

NTP Amplification saldırısı, genellikle bir saldırganın birden çok botnet kullanmasıyla gerçekleştirilir. Saldırının gerçekleştirilmesi için hedef sistemin IP adresi yeterlidir. Saldırgan, public NTP sunucularına yapmak isteği küçük boyutlu zararlı isteğin kaynak IP adresi alanına, hedef sistemin IP adresini atamaktadır. Botnetler aracılığıyla, NTP sunucularına yapılan kısa boyutlu zararlı istek (örneğin MON_GETLIST), NTP sunucularının hedef sistemin IP adresine yanıt göndermesine neden olmaktadır. Gönderilen her yanıtın içerisinde ise sunucuya bağlanan son 600 makinenin bilgisi olacaktır.

NTP sunucularına gönderilen istek, amplifikasyonu faktörünün tetiklenmesiyle büyük boyutlarda yanıtın ortaya çıkmasına neden olmaktadır. Böylece, hedef sistem büyük boyutlu yanıt paketlerinden oluşan UDP trafiğine maruz kalmaktadır. Hedef sisteme gelen paket sayısının artması, ağ bant genişliğinin dolmasına, ağ trafiğinin yavaşlamasına ve hedef sistemin servis dışı kalmasına neden olmaktadır.

Saldırgan, botnetleri kullanarak ve isteği UDP üzerinden gerçekleştirerek kendisini güvene almaktadır. Çünkü UDP, TCP gibi Üçlü El Sıkışma yapmadan veri akışını sağlamaktadır. UDP’de, kimlik kontrolü ve veri filtrelenme olmadığı için paketler güvensiz ve hızlı bir şekilde iletilir. Botnetlerin kullanılması, aynı şekilde saldırganın kendisini gizlemek amacıyla yapılır. Önceden farklı güvenlik açıklarının kullanılmasıyla ele geçirilen, zombi diye adlandırılan kurban makineler botnet olarak ifade edilir. Saldırgan, UDP üzerinden NTP sunucularına gönderilmesini isteği zararlı isteği botnetler aracılığıyla göndererek kendisini gizleyebilir.

Bütün amplifikasyon saldırıları, saldırgan ve hedeflenen sistem arasındaki ağ bant genişliği eşitsizliğinden faydalanmaktadır. Ağ bant genişliğindeki eşitsizlik, istekleri gönderen saldırganın ağ bant genişliği üst limitinin üstünde trafik oluşturması denilebilir. Böylece, hedef sisteme büyük yanıtlar verdirtecek isteklerin gönderilmesiyle ağ bant genişliği sınırı aşılabilir ve ağ alt yapısı bozulabilir. Gönderilen zararlı istekler, birden çok botnet kullanılarak gerçekleştirilebilir. Botnetlerin kullanılmasıyla hem saldırının şiddeti arttırılabilir hem de saldırgan kendisini gizleyebilir.

NTP Amplification saldırısı beş adımda gerçekleştirilebilir:

- Saldırgan, saldırının miktarını arttırmak ve kendisini gizlemek için botnet kullanır.

- Botnet, sahte IP adresi içeren UDP paketlerini monlist komutunun aktif olduğu public NTP sunucularına gönderir. Her paketin sahte IP adresi, hedef sistemin IP adresi olmalıdır.

- Public NTP sunucularına gönderilen her UDP paketi, monlist komutunu kullanarak NTP sunucusuna istekte bulunur ve büyük bir yanıt alınır.

- NTP sunucusu, yanıtları sahte IP adresi olarak belirtilen hedef sistemine gönderilir.

- Hedef sistemin yanıt almasıyla ağ bant genişliği dolup ağ trafiği yavaşlar ve servis kesinti ile sonuçlanır.

Belirtilen beş adım şekil1’de gösterilmektedir.

| ![atgr1]({{ site.url }}/assets/img/NTPAmplification/sekil1.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil1 : NTP Amplification saldırısının gösterimi* |

<br/>

Şekil1’de saldırgan, hedef sistemin IP adresini oluşturulan zararlı UDP istekleri içerisindeki kaynak IP adresi olarak belirtmektedir. UDP isteklerini botnet kullanarak NTP sunucularına gönderir. UDP paketleri herhangi bir el sıkışma (handshake) gerektirmediği için, NTP sunucusu, isteğin gerçek olup olmadığını doğrulamadan, büyük boyutlu yanıt verir. Verilen yanıt, gönderilen isteğin içerisindeki kaynak IP adresine gönderilir. Böylece, hedef sistem göndermemiş olduğu UDP isteklerin doğurduğu büyük yanıtlara maruz kalır. NTP sunucuları, DDoS amplifikasyon saldırıları için büyük bir kaynak olarak kullanılabilir.

| ![atgr2]({{ site.url }}/assets/img/NTPAmplification/resim1.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim1: Shodan Kullanılarak 4.2.7’den düşük sürümlü NTP sunucularının gösterimi* |

<br/>

Resim1’de, Shodan kullanılarak 4.2.7 sürümünden önceki NTP sürümleri gösterilmektedir. “port:123 protocolversion:3” komutu kullanılarak, 123 numaralı port üzerinde ve protokol sürümü 3 olan 6.096.865 tane NTP sunucusu listelenmektedir.

## NTP Amplification Senaryosu

DDoS saldırıları hedef sistemlerin devre dışı bırakılması için yapılmaktadır. NTP Amplification saldırısı, DDoS saldırılarının en etkili tekniklerinden biridir. NTP Amplification saldırısının gerçekleştirilmesi için;

- NTP sunucularına istek göndermek için otomatize bir araca,

- Saldırganın kendisini gizlemesi ve saldırı şiddetini arttırması için botnetlere,

- Zafiyetli NTP sunucu IP’lerinin olduğu bir listeye

ihtiyaç duyulmaktadır. NTP sunucularına yapılacak istekler için NTPDoser adında bir NTP Amplification saldırısı gerçekleştiren DDoS aracı bulunmaktadır. Resim2’de NTPDoser aracı gösterilmektedir.

| ![atgr3]({{ site.url }}/assets/img/NTPAmplification/resim2.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim2: NTPDoser aracının gösterimi* |

<br/>

NTPDoser aracı, “Clone or Download” butonuna tıkladıktan sonra gelen “Download ZIP” butonuna tıklanarak indirilebilir. Ayrıca, Resim3’te NTPDoser aracının klonu indirilmiştir.

```linux
root@Stormer:~# git clone https://github.com/DrizzleRisk/NTPDoser.git
```
NTPDoser aracı, ZIP olarak indirmek dışında klonlanabilir. Aşağıda, indirilen C++ kaynak kodu derlenmektedir.

```linux
root@Stormer:~/NTPDoser# gcc NTPDoser.cpp  -o NTPDoser  -lstdc++  -pthread
```
Yukarıdaki komutunun çalıştırılması ile NTPDoser.cpp dosyası derlenerek NTPDoser çalıştırılabilir dosyası oluşturulur.

```linux
root@Stormer:~/NTPDoser# ./NTPDoser 104.28.28.246 3 10
```
Yukarıda, NTPDoser aracı ile NTP Amplification saldırısı yapılmıştır. 104.28.28.246 IP adresine yönelik NTP Amplification saldırısını 3 farklı thread’den 10 saniye içerisinde yapmaktadır.

## NTP Amplification Saldırılarına Karşı Koruma

ISP’ler, hedeflenen sistemin IP adresine gelen tüm trafiği blackhole filtreleme/yönlendirme ile düşürebilir. Böylece, kendisini koruyabilir ve hedef sistemi devre dışı bırakabilir. En yaygın blackhole şekli, çalışmayan bir sisteme ait IP adresine veya hiçbir sisteme atanmayan IP adresine yönlendirmenin yapılması olarak gösterilebilir.  

Monlist komutunu destekleyen NTP sunucuların sayısı azaltılabilir veya monlist komutu devre dışı bırakılabilir. Monlist güvenlik açığından etkilenmemek için 4.2.7 sürümünden önceki NTP yazılımları kullanılmamalıdır. Monlist güvenlik açığından etkilenmemek için 4.2.7 sürümden önceki yazılımların güncelleştirilmesi gerekir. Sürüm güncellenmesi yapılmıyorsa, US-CERT talimatları göz önüne alınarak sunucu üzerinde konfigürasyon yapılabilir.

Kaynak IP doğrulaması yapılarak, ağdan çıkan sahte paketler durdurulabilir. Ağın içinden, ağın dışındaymış gibi görünen bir kaynak IP adresiyle bir paket gönderiliyorsa, sahte bir paket olduğu belirlenir ve paket düşürülür. ISP’ler, UDP tabanlı amplifikasyon saldırıları engellemek için sahte IP adresleriyle olan trafiği sonlandırır.

NTP sunucularında monlist komutunun devre dışı bırakılması ve sahte IP’lere izin veren ağlara giriş filtrelemesinin uygulanması ile amplifikasyon saldırılarının engellenmesi amaçlanmaktadır.




