---
title:  "Microsoft ATA Nedir ve Nasıl Kurulur?"
date:   2019-12-06 18:40:00
layout: post
categories: 
    - network-guvenligi
thumbnail: posts/ATA_mimarisi.png
---

# Microsoft ATA Nedir ve Nasıl Kurulur?

**Microsoft Advanced Threat Analytics (ATA), kurumların karşılaştığı birbirinden farklı siber saldırı tehditlerine karşı kurumu korumaya yönelik bir uygulamadır. Kurum ağı üzerindeki saldırılar hakkında uyarılar vererek güvenliği sağlamaktadır. Bir diğer deyiş ile Microsoft’un IDS sistemi denilebilir.**

>Microsoft ATA, ağ üzerinde oluşan anormal durumları tespit etmek için Kerberos, DNS, RPC, NTLM gibi protokoller üzerinde gerçekleşen ağ trafiğini izleyerek, paketleri inceleyip birbirinden ayırt eden bir ağ ayrıştırma motoru kullanmaktadır. Ayrıca, ATA bu protokoller üzerinden bilgiler de toplamaktadır. Bu bilgi toplama işlemini Domain Controller, DNS sunucuları, ATA Gateway, ATA Lightweight Gateway üzerinden gerçekleştirmektedir.

ATA, kuruluşlardaki varlıkların ve kullanıcıların davranışlarını inceleyip öğrenmek için sistem üzerindeki logları ve olayları inceleyerek veri kaynaklarından bilgi elde etmektedir. Bu doğrultuda davranışsal bir profil oluşturmaktadır. ATA loglar ve olaylar hakkındaki bilgiyi, SIEM Integration, Windows Event Forwarding(WEF) ve Windows Event Collector gibi yapılardan elde etmektedir.

Microsoft ATA, saldırganların ağ üzerinde nasıl bilgi toplayacaklarını, hangi sistemler üzerinde ne gibi zafiyet taramaları yapacağını ve yapılan taramalar sonucunda saldırganların nasıl ilerleyeceği konusunda öngörülerde bulunmaktadır. Bir saldırganın, çeşitli giriş noktalarını hangi şekilde kullanıp hedef sistemleri ele geçirmeye çalışacağı konusunda bilgiler elde etmektedir. Böylece saldırı esnasında erken uyarı verebilmektedir. Microsoft ATA, siber saldırıları

- Kötü amaçlı saldırılar,
- Anormal davranışlar,
- Güvenlik sorunları ve riskleri

olarak üçe ayırmaktadır.

Kötü amaçlı saldırılarda, ATA şüpheli durumları tespit edip, şüpheli durumu gerçekleştiren kişinin kim olduğu, olayı ne zaman gerçekleştirdiği, şüpheli işlemin ne olduğu ve nasıl yapıldığı ile ilgili bilgileri ATA web paneli üzerinde açıkça göstermektedir. Kötü amaçlı saldırılar olarak nitelendirilen teknikler aşağıdaki gibidir:

- Pass The Ticket
- Pass The Hash
- Overpass The Hash
- Forged PAC (MS14-068)
- Golden Ticket
- Kötü Amaçlı Kopyalar
- Keşif Çalışmaları
- Brute-Force Saldırısı
- Uzaktan Kod Çalıştırma (Remote Code Execution)

Anormal davranışlarda ATA, makine öğrenmesinden yaralanarak, ağ üzerindeki kullanıcıların gerçekleştirdiği şüpheli faaliyetleri, anormal davranışları algılayıp bildirmektedir. Bu anormal davranışlara örnek olarak, anormal girişler, bilinmeyen tehditler, şifre paylaşımı, hassas grupların değiştirilmesi gibi teknikler verilebilir.

Ayrıca güvenlik sorunları ve riskleri ele alındığında Trust yapılarının kırılması, zayıf protokollerin kullanılması ve bilinen protokol güvenlik açıkları bu sorunlar ve tehditler arasında yer almaktadır.

Microsoft ATA, tehditlere bir saldırgan bakış açısı ile bakarak savunma mekanizmasını güçlendirmiştir. Bir saldırganın hedef bir domain ağını ele geçirme aşamalarını algılamak için kendisi de aşamalı olarak hareket etmektedir. Bir saldırganın dışardan bilgi toplamasını, sistemlere yönelik keşifler ve zafiyet tespitlerinin yapılmasını, sistemlere yönelik sızma girişimlerini, sızma girişiminden sonra sistem üzerinde yerel hak ve yetki yükseltme işlemlerini, sistem üzerinde domain admin kullanıcısının kimlik bilgilerini elde etme girişimlerini, uzaktan kod çalıştırma işlemlerini, domain admin bilgileri ile keşif yapma işlemlerini, domain controller makinesini ele geçirme işlemlerini aşamalı olarak algılamaktadır. Bu işlemlerin aşamalı olarak zincir haline getirilmesi şekil 1 üzerinde gösterilmektedir. Microsoft ATA, oluşan bu tehdit zinciri üzerinde tehdit oluşturabilecek durumları, davranışları ve eylemleri önceden tespit ederek önlem alınmasına yardımcı olmaktadır.

| ![atgr1]({{ site.url }}/assets/img/MicrosoftATA/MicrosoftATA_kill-chain.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 1 : Microsoft ATA kill-chain gösterimi* |


## Microsoft ATA Mimarisi

| ![atgr2]({{ site.url }}/assets/img/MicrosoftATA/ATA_mimarisi.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 2 : Microsoft ATA Mimarisinin Gösterimi* |

Şekil 2’de gösterildiği gibi Microsoft ATA, fiziksel ve sanal anahtarlar ile bir ATA Gateway’e port mirroring yaparak Domain Controller ağ trafiğini izleyebilir. ATA Lightweight Gateway doğrudan domain controller’a eklenilirse port mirroring yapmaya gerek kalmaz. ATA, Windows loglarını herhangi bir SIEM sunucusuna veya bir Domain Controller makinesine gönderebilir. Gönderilen loglar SIEM sunucularında toplandıktan sonra saldırılar ve risk oluşturacak tehditler ile ilgi verilerin analizi yapılır. Analiz sonucunda gerekli güvenlik tedbirlerinin alınması için bilgilendirmede bulunmaktadır.

| ![atgr2]({{ site.url }}/assets/img/MicrosoftATA/ATAcenter_ATAgateway.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 3 : ATA Center ve ATA Gateway Gösterimi* |

Şekil 3 üzerinde gösterilen birimler ve bileşenler Microsoft ATA yapısı içerisindeki bileşenlerin ağ trafiği üzerinde toplanan verilerin işleyişinden ATA Center’a gönderilmesi sürecini göstermektedir.

## MICROSOFT ATA BİLEŞENLERİ

Microsoft ATA bileşenleri, ATA Center, ATA Gateway ve ATA Lightweight Gateway’den oluşmaktadır.  ATA Center, oluşturulan ATA Gateway’lerden ve ATA Lightweight Gateway’lerden Domain Controller ile ilgili ağ trafiği ve Windows logları, SIEM ile ilgili durumları almaktadır. ATA Gateway, domain controller’dan gelen trafiği port mirroring yaparak herhangi bir sunucuya yüklenmesini sağlamaktır. ATA Lightweight Gateway, doğrudan domain controller makinesi üzerinde kurulmaktadır. Böylelikle domain controller üzerinden ağ trafiğini izleyip, domain controller ile herhangi bir sunucu arasında port mirroring gibi işlemlerin yapılmasına gerek kalmaz. Son olarak, ATA Gateway’ler ve ATA Lightweight Gateway’ler tek bir ATA Center’dan oluşturulmaktadır.

Ayrıca Microsoft ATA kurulumunda isteğe bağlı olarak sadece ATA Gateway kullanma, sadece ATA Lightweight Gateway kullanma veya hem ATA Gateway hem de ATA Lightweight Gateway kullanma gibi şekillerde kullanılabilir.

### ATA Center
ATA Gateway ve ATA Lightweight Gateway’den ayrıştırılmış ağ trafiğini alıp profil oluşturma işlemi gerçekleştirir. Ağ hakkında bilgi toplama ve saldırılara karşı deterministik algılama işlemi yapmaktadır. Anormal davranış tespitleri ve şüpheli durumlar hakkında uyarmak için makine öğrenmesi ve davranışsal algoritmaları kullanmaktadır.

#### İşlevleri:

- ATA Gateway ve ATA Lightweight Gateway yapılandırma ayarlarını yönetmek
- Gateway’lerden veri almak
- Şüpheli durumları algılamak
- ATA davranışsal makine öğrenme algoritmalarını kullanarak anormal davranışları algılamak
- Gelişmiş saldırıları tespit etmek için deterministik algoritmaları uygulamak
- ATA Console çalıştırmak
- Şüpheli bir durum algılandığında e-posta ve olay gönderecek şekilde yapılandırmak

| ![atgr3]({{ site.url }}/assets/img/MicrosoftATA//tablo1.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Tablo 1 : ATA Center Ana Bileşenleri* |


Bir tane ATA Center, bir Active Directory Forest’ı izleyebilir. Birden çok Active Directory Forest olması halinden her bir forest için bir ATA Center oluşturulmalıdır. Büyük bir Active Directory kapsamını tek bir ATA Center taşıyamadığı için ATA Center sayısı arttırılmalıdır.

### ATA Gateway
Ağ trafiğini ve Windows loglarını alıp ATA Center makinesine göndermektedir. ATA Gateway ile ATA Lightweight Gateway aynı işlevlere sahiptir.

#### İşlevleri:

- Domain Controller ağ trafiğini yakalayıp incelemek. ATA Gateway’leri port mirroring yaparak, ATA Lightweight Gateway’leri ise Domain Controller üzerindeki yerel ağ trafiğini incelemektedir.
- SIEM veya Syslog sunucularındaki veya domain controller’daki Windows Event’ları Windows Event Forwarding özelliğini kullanarak aktarımını sağlar.
- Active Directory domaininden Kullanıcılar ve bilgisayarlar hakkındaki verileri alır.
- Ağ varlıklarının (kullanıcılar, gruplar, bilgisayarlar) çözünürlüğünü gerçekleştirir.
- Toplanan verileri ATA Center’a aktarır.
- Tek bir ATA Gateway ile birden çok domain controller makinesinin ağı izlenirken, bir tane ATA Lightweight Gateway ile tek bir domain controller ağı izlenebilir.

| ![atgr4]({{ site.url }}/assets/img/MicrosoftATA/tablo2.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Tablo 2 : ATA Gateway Ana Bileşenleri* |



### ATA Lightweight Gateway
ATA Gateway’e alternatif olarak yapılmıştır. Port mirroring yapılmadan yerelde Domain Contoller makinesi üzerindeki trafiği inceleyip ayrıştırdıktan sonra ATA Center’a göndermektedir.

#### İşlevleri:

- Event’ları port mirroring yapmadan yerelde inceleyebilir.
- Domain Synchronizer Candidate, belirli bir Active Directory domaininden tüm varlıkları proaktif olarak eşitlemekten sorumludur. Bir Gateway, Domain Synchronizer olarak görev yapmak üzere aday listesinden rastgele seçilir. Varsayılan olarak bütün ATA Gateway’ler birer domain synchronizer adayıdır.
- ATA, Lightweight Gateway, Domain Controller’daki kullanılabilir bilgi işlem ve bellek kapasitesini değerlendiren izleme bileşeni içerir. Her 10 saniyede bir CPU ve bellek kullanım kotasını dinamik olarak güncellemektedir. Kaynakların tükenmesi durumunda, yalnızca kısmi trafik izlenir ve **Dropped port mirrored network traffic** uyarısı oluşur.


### Ağ Bileşenleri
Microsoft ATA için Port mirroring ve Events ağ bileşenleri olarak belirtilmektedir.

#### Port Mirroring
Trafiği izlenecek olan Domain Controller makineleri için port mirroring ayarının yapılması için fiziksel/sanal anahtarları kullanarak işlemleri gerçekleştirir. Port mirroring, tüm domain controller’ların ağ trafiğini ATA Gateway’e yönlendirirken, bu trafiğin küçük bir yüzdesi ATA Center’a analiz edilmek için gönderilir.

#### Events
Microsoft ATA, Pash-the-Hash, Brute-Force, honey tokenlar ve hassas gruplarda değişiklikler yapmayı geliştirmek için, 4776, 4732, 4733, 4728, 4729, 4756, 4757 kodlu Windows eventlarına ihtiyaç duymaktadır.

### ATA Center Boyutlandırması
Microsoft ATA üzerinde davranış analizi için ATA Center’a en az 30 günlük veri gerekmektedir. 30 gün sonra davranış analizi eldeki verilerle karşılaştırma sonucunda anormali bir davranış olup olmadığı konusunda sonuçlar elde edilmektedir. ATA Center boyutlandırma koşulları aşağıdaki tablo 3’te gösterilmektedir.

| ![atgr5]({{ site.url }}/assets/img/MicrosoftATA/tablo3.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Tablo 3 : ATA Center Boyutlandırma Özellikleri* |

### ATA Lightweight Gateway Boyutlandırması
ATA Lightweight Gateway boyutlandırılması Domain Controller’ın oluşturduğu ağ trafiğini göz önüne alarak yapılmaktadır. Trafiğin miktarı ile doğru orantılı bir şekildedir. Tablo 4 üzerinde belirtilen özellikler doğrultusunda boyutlandırma işlemleri gerçekleştirilebilir.


| ![atgr6]({{ site.url }}/assets/img/MicrosoftATA/tablo4.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Tablo 4 : ATA Lightweight Gateway Boyutlandırma Özellikleri* |


Tablo 4’te Domain Controller üzerinde geçen trafiğin bir saniyedeki paket sayısının toplamı, Domain Controller’ın yüklediği çekirdek sayısı ve takılı olan belleğin toplam miktarı belirtilmektedir. Domain Controller makinesinde belirtilen özellikler yoksa veya eksik ise Domain Controller makinesinin performansında bir değişiklik olmamaktadır. Fakat ATA Lightweight Gateway verimli çalışmayabilir.

Sanal makine olarak çalıştırılırken, belleğin tamamı sanal makineye ayrılmalıdır. En iyi performans için ATA Lightweight Gateway’in Güç seçeneğini Yüksek Performans olarak ayarlamak gerekir. ATA logları ve performans logları için toplamda en az 5 GB alan gerekmektedir. Önerilen alan ise 10 GB’tır. Bu ayarlamalar aynı şekilde ATA Gateway içinde geçerlidir.


### ATA Gateway Boyutlandırması
ATA Gateway dağıtımlarında bazı özelliklere dikkat etmek gerekmektedir. ATA, Active Directory üzerinde bulunan bir forest içindeki birden çok domainin trafiğini izleyebilir. Birden çok AD Forest üzerinde bulunan Domain Controller’i izlemek için birden çok ATA dağıtımı yapılmalıdır. Ayrıca ATA Gateway’ler Domain Controller üzerinde kurulu olmadıkları için Port Mirroring gibi işlemler yapmaları gerekmektedir. Böylelikle her bir data center veya branch için birden fazla ATA Gateway bileşenini yapılandırmanız gerekmektedir.  ATA Gateway Boyutlandırması için gerekli özellikler Tablo 5 üzerinde gösterilmektedir.

| ![atgr7]({{ site.url }}/assets/img/MicrosoftATA/tablo5.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Tablo 5 : ATA Gateway Boyutlandırma Özellikleri* |

Tablo 5 üzerindeki özelikler, ATA Gateway tarafından izlenen Domain Controller’larda bir saniyedeki ortalama paket sayısının toplamı ve Domain Controller ile port mirroring trafiğinin toplam miktarıdır. Ayrıca Çekirdek kullanımı sırasında, hiper iş parçacığı devre dışı bırakılmalıdır. Gerçek çekirdeklerin sayılması gerekmektedir.

## MICROSOFT ATA GEREKSİNİMLERİ
Microsoft ATA’nın düzenli ve sorunsuz bir şekilde çalışması için sistemsel gereksinimlerinin karşılanması gerekmektedir. Microsoft ATA, ATA Center, ATA Gateway ve/veya ATA Lightweight Gateway’den oluşur. Ayrıca Microsoft ATA, Active Directory Forest yapısı üzerinde çalışmakta olup Windows 2003 ve üzerindeki sürümlerde kullanılabilir.

### ATA Center Gereksinimleri
ATA Center, Windows Server 2012 R2, Windows Server 2016 ve Windows Server 2019 sunucularda yüklenebilmektedir. ATA Center, Windows Server Core’u desteklememektedir. Bir domaine üzerinde çalışabildiği gibi bir Workgroup üyesi olarak da çalışabilmektedir. Windows 2012 R2 sunucularda yüklenmeden önce ihtiyaç duyulan KB2919355 kodlu güncelleştirmenin yapılması gerekmektedir. Powershell üzerinde güncelleştirmenin kontrolünü yapmak için **Get-HotFix –Id kb2919355** cmdlet’in çalıştırılması gerekmektedir.

ATA Center’ı sanal makine olarak çalıştırıyorsanız, yeni bir kontrol noktası oluşturmadan önce sunucuyu kapatmanız olabilecek veritabanı bozulmalarını önleyebilmektedir. Fiziksel bir sunucuda çalışırken, BIOS’ta Non-Uniform bellek erişimini (NUMA) devre dışı bırakılması gerekir. Windows Server 2008 R2 ve 2012’de Gateway, Multi Process Group modunda desteklenmemektedir. ATA Center sunucusu, ATA Gateway ve Domain Controller sunucularının beş dakika içerisinde senkronize edilmesi gerekmektedir.

En az bir ağ bağdaştırıcısı olmalıdır. ATA Center 443 numaralı portu açık olan bütün IP adresleri ile iletişime geçmektedir. ATA, kimlik bilgilerinin kontrolü için LDAP kullanmaktadır. Bundan dolayı 389 numaralı port açık olmalıdır. ATA’yı daha hızlı kurmak ve dağıtmak için imzalı sertifikalar kullanılmalıdır. ATA, mevcut bir sertifikayı güncelleme işlemini desteklememektedir. Ancak yeni bir sertifika oluşturulabilir.  ATA 1.8 sürümünden beri, ATA Gateway’ler ve Lightweight Gateway’ler kendi sertifika işlemleri yönetici etkileşimi gerektirmeksizin yönetebilir.

### ATA Gateway Gereksinimleri
Windows Server 2012 R2, Windows Server 2016 ve Windows Server 2019 gibi sunucularda desteklenmektedir. Bir domain controller veya workgroup üyesi olarak bir sunucuya yüklenebilir. Windows Server 2012 R2 kurulu bir makineye ATA Gateway yüklenmeden önce **Get-HotFix -Id kb2919355** komutunun Powershell üzerinde çalıştırılıp güncellemenin yüklü olup olmadığı kontrol edilmelidir. Güncelleştirme yüklü değilse, ATA Gateway kurulmadan güncelleştirmenin yapılması gerekmektedir. ATA’nın binary dosyaları, ATA logları ve performans loglarının depolanması ise en az 5 GB’lık alana ihtiyaç duyulur.

En iyi performansı sergilemesi için ATA Gateway üzerindeki güç seçeneği yüksek performans olarak ayarlanmalıdır. Beş dakika içerisinde senkronize işlemlerinin gerçekleştirilmesi gerekmektedir. En az bir Management adaptörü ve bir Capture adaptörü gerekmektedir. Management adaptörü, kurumsal iletişim için kullanılmaktadır. ATA Gateway domain ağında ise, otomatik olarak yapılandırılabilir. Capture adaptörü ise, domain controller ve domain ağından gelen trafiği gözlemektedir. ATA Gateway’in işlemlerini sorunsuz yapması için 135 numaralı portunun (RPC) ve 137 numaralı NetBIOS portunun inbound olarak açık olması gerekmektedir.

### ATA Lightweight Gateway Gereksinimleri
Windows Server 2008 R2 SP1 ( Server Core hariç), Windows Server 2012, Windows Server 2012 R2, Windows Server 2016 ve Windows Server 2019(Server Nano hariç) gibi Domain Controller sunucuları desteklemektedir. Domain Controller, salt okunur (readonly) olabilir. Windows Server 2012 R2 kurulu bir makineye ATA Gateway yüklenmeden önce **Get-HotFix -Id kb2919355** komutu Powershell üzerinde çalıştırılıp güncellemenin yüklü olup olmadığı kontrol edilmelidir. Windows Server 2012 R2 Server Core için **Get-HotFix –Id kb3000850** komutu ile güncelleştirmenin yapılması gerekmektedir. Güncelleştirme yüklü değilse, ATA Gateway kurulmadan önce güncelleştirmenin yapılması gerekmektedir. Güncelleştirme sırasında .NET Framework 4.6.1 yüklendiği için Domain Controller sunucusunun yeniden başlatılmasına neden olabilir. ATA’nın binary dosyaları, ATA logları ve performans loglarının depolanması ise en az 5 GB’lık alana ihtiyaç duyulur.

Domain Controller makinesinde en az 2 çekirdek ve 6 GB RAM kullanımı gerektirmektedir. En iyi performansı göstermesi için, güç seneğinin yüksek performans olarak işaretlenmesi gerekmektedir. Diğer sunucular gibi beş dakika içerisinde senkronize edilmesi gerekmektedir. ATA Lightweight Gateway, Broadcom Network Adapter Teaming aktif iken Windows Server 2008 R2 Domain Controller sunucularını desteklemez. ATA Lightweight Gateway’in işlemlerini sorunsuz yapması için 135 numaralı portunun (RPC) ve 137 numaralı NetBIOS portunun inbound olarak açık olması gerekmektedir.

## Microsoft ATA Kurulumu
Microsoft ATA’nın kurulumunu yapmadan önce gereksinimlerinin eksiksiz bir şekilde tamamlanması gerekmektedir. ATA’nın kurulacağı sunucunun güncelleştirilmeye ihtiyacı olup olmadığı kontrol edildikten sonra Microsoft ATA sitesinden indirilmelidir.

| ![atgr8]({{ site.url }}/assets/img/MicrosoftATA/sekil4.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 4 : Microsoft ATA Center Setup Dosyasının Görünümü* |

Şekil 4’te gösterilen kurulum dosyası Microsoft web sitesi üzerinden indirilerek çalıştırılmıştır. Şekil 5’te .NET kurulumu gerçekleştirilmektedir.

| ![atgr9]({{ site.url }}/assets/img/MicrosoftATA/sekil5.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 5 : .NET kurulumunun gerçekleştirilmesi* |

.NET kurulumu sonucunda makineniz otomatik olarak yeniden başlatılabilir. Şekil 6, ATA’nın dil seçim özelliğini göstermektedir.


| ![atgr10]({{ site.url }}/assets/img/MicrosoftATA/sekil6.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 6 : Kurulum sırasında ATA dil seçme ekranı* |



Şekil 6'da ATA’nın 1.9.7312 sürümünü yükleyeceğiz. Ayrıca ATA’nın hangi dilde kullanılması gerektiğini belirtebiliriz. Şekil 7'de lisans şartları gösterilmektedir.


| ![atgr11]({{ site.url }}/assets/img/MicrosoftATA/sekil7.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 7 : ATA lisans şartları* |


Şekil 7'de ATA’yı bu lisans koşulları altında kullanacağımızı belirterek **I accept the Microsoft Software License Terms** alanı işaretlenip **Next** butonuna tıklanmalıdır.


| ![atgr12]({{ site.url }}/assets/img/MicrosoftATA/sekil8.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 8 : ATA’nın Güncelleme Aşaması* |


Şekil 8'de ATA’nın otomatik güncelleme ayarlaması önerilmektedir. Bilgisayarınızda Microsoft Update otomatik olarak işaretlenmiş ise bilgisayarınız güvenli ve sorunsuz bir şekilde işleyişini sağlamak için güncelleştirmeleri yapacaktır.


| ![atgr13]({{ site.url }}/assets/img/MicrosoftATA/sekil9.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 9 : Konfigürasyon dosyalarının dizinleri* |



Şekil 9’da kurulum dosyalarının varsayılan olarak yüklendiği dizini ve veritabanı (MongoDB) dosyalarının yükleneceği dizini göstermektedir. Ayrıca kullanılan sertifikanın oluşturulması için işaretlenmesi gerekmektedir. Sertifikanın süresi bitmeden yenisini oluşturulması gerekmektedir. **Install** butonuna tıklayarak yükleme işlemini başlatılmaktadır.


| ![atgr14]({{ site.url }}/assets/img/MicrosoftATA/sekil10.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 10 : Kurulum işleminin devam etmesi* |


Şekil 10’da MongoDB ve Microsoft ATA kurulumlarının yüklenme sürecini göstermektedir. Yükleme işleminin tamamlanması şekil 8’de gösterilmektedir.


| ![atgr15]({{ site.url }}/assets/img/MicrosoftATA/sekil11.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 11 : Yüklemenin Tamamlanması* |


Şekil 11’de gösterildiği gibi Microsoft ATA’nın kurulumu başarılı bir şekilde tamamlanmıştır.


| ![atgr16]({{ site.url }}/assets/img/MicrosoftATA/sekil12.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 12 : Microsoft ATA Web Paneli* |


Şekil 12’de gösterildiği gibi kuruluma 3 aşamalı bir şekilde devam edilmektedir. Öncelikle ATA’nın var olan domain ile bağlantısınınoluşturmak için kimlik bilgileri ve domain adı bilgilerine ihtiyaç duyulmaktadır. Bu işlemler, şekil 10 gösterilmektedir.


| ![atgr17]({{ site.url }}/assets/img/MicrosoftATA/sekil13.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 13 : ATA’nın domain’e bağlanması* |


Şekil 13'te gösterildiği gibi sorunsuz bir şekilde domain ile ATA’nın test bağlantısı başarılı bir şekilde gerçekleştirilmiştir. Bu işlem **Save** butonuna tıklanarak kaydedilir.


| ![atgr18]({{ site.url }}/assets/img/MicrosoftATA/sekil14.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 14 : ATA Gateway dosyasının indirilmesi* |


Şekil 14’te gösterildiği gibi Domain Controller makinası üzerinde kurulmak üzere Gateway Setup butonuna tıklayarak ATA Gateway kurulum dosyasını indirilmektedir. İndirilen kurulum dosyası içerinde ATA Gateway ve ATA Lightweight Gateway kurulumu dosyalarını kapsamaktadır.


| ![atgr19]({{ site.url }}/assets/img/MicrosoftATA/sekil15.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 15 : ATA Lighweight Gateway Kurulumu* |



Şekil 15’te indirilen AT Gateway Setup dosyasının kurulumu başlatılmasıyla ATA Lightweight Gateway 1.9.7312 sürümü Domain Controller makinesine yüklenecektir. Gösterildiği gibi ATA Lightweight Gateway’in kullanacağı dil seçilmelidir.


| ![atgr20]({{ site.url }}/assets/img/MicrosoftATA/sekil16.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 16 : ATA Lighweight Gateway Gereksinimleri* |


Şekil 16'da ATA Lightweight Gateway’in kurulum sırasında sunucunun minimum gereksinimleri karşılayıp karşılamadığının kontrolü için bilgilendirmeler bulunmaktadır. Ayrıca Vmware üzerinde bulunan bir Domain Controller makinesi üzerinde kurulumu ile ilgili uyarı ve bilgilendirmeleri yapmaktadır. Bu gereksinimlerin karşılanması durumunda **Next** butonuna tıklanarak kurulum yapılabilmektedir.


| ![atgr21]({{ site.url }}/assets/img/MicrosoftATA/sekil17.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 17 : Lighweight konfigürasyon dosyalarının dizini* |



Şekil 17’de gösterilen Gateway kurulumu için gerekli konfigürasyon dosyalarının yükleneceği dizini göstermektedir. Bu işlemlerden sonra **Install** butonu tıklanması ile kurulum başlatılabilir. Kurulum süreci şekil 18 üzerinde gösterilmektedir.

| ![atgr22]({{ site.url }}/assets/img/MicrosoftATA/sekil18.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 18 : ATA Lighweight Gateway kurulum süreci* |



Şekil 18’de gösterildiği gibi ATA Lightweight Gateway kurulumu yapılmaktadır. Kurulum tamamlandıktan sonra Şekil 19’daki görüntü elde edilebilir.


| ![atgr23]({{ site.url }}/assets/img/MicrosoftATA/sekil19.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 19 : ATA Lighweight Gateway’in başlatılması* |

Şekil 19’da görüldüğü gibi kurulan ATA Lightweight Gateway çalıştırılmaya başlanmıştır. Herhangi bir hata vermeden kurulum başarılı bir şekilde tamamlanmıştır.

