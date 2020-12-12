---
title:  "Nessus Professional Nedir ve Nasıl Kurulur?"
date:   2018-06-26 
categories: 
    - network-guvenligi
thumbnail: posts/nessus.jpg
---

## **Nessus Professional Nedir?**

**Fiziksel taramalar ve zafiyet analizleri için kullanılan bir güvenlik aracıdır. White-Box Penetration(Sızma Testi) testinde kullanılır. Üzerinde desteklenen çoklu platform ağı ve ana güvenlik açığı tarayıcı sunucusudur(Windows, Linux, UNIX, Mac).**
- Client tarafta ise: 
    - WEB Tabanlı ve Mobil ( IOS,Android ) 

Oluşturulacak tarama profiline göre network veya belirli hostlar taranıp güvenlik açıkları keşfedilebilir. Nessus, rekabetçi çözümler, OS taramaları, ağ aygıtları, veritabanları, web sunucuları ve güvenlik açıklarını tehditler ve uyumluluk ihlalleri için kritik altyapıdan daha fazla teknolojiyi desteklemektedir. Nessus Professional, varlık bulma, yapılandırma denetimi, hedef profilleme, kötü amaçlı yazılım tespiti, hassas veri bulma gibi özellikleri barındırmaktadır.

### **Nessus Tarihçesi**

1998 yılında Renaud Deraison tarafından başlatıldı. İlk versiyon, Deraison 17 yaşında iken çıkarıldı. 2002 yılında, Tenable Network Security kuruldu. İnternet topluluğuna, ücretsiz ve uzaktan bir güvenlik tarayıcısı
sağlandı. 5 Ekim 2005'te, Tenable Network Security, Nessus 3 tescilli(kapalı kaynak) bir lisansa dönüştürüldü. Temmuz 2008'de, Tenable Network Security, Home kullanıcılarına plugin feed’lerine tam erişim veren feed lisansının bir revizyonu gönderildi.

### **Nessus Mimarisi**

Nesus bir İstemci-Sunucu (Client-Server) modeline dayanır. Nessus istemcileri, başlatılmasına izin verilmeden önce sunucuya kimlik doğrulanması yapılmalıdır. Bu mimari, Nessus kurulumları yönetmeyi
kolaylaştırır.

Nessus Sunucusu : **nessusd**
- Gerçek zafiyet testlerini yapmaktan sorumlu.
- Son kullanıcıların kullandığı Nessus istemcilerinden gelen bağlantıların dinlenmesi, belirli taramaları yapılandırabilir ve başlatabilir.

### **Neden Nessus?**

- Bugün 20.000 ’den fazla kuruluş Nessus Professional’ı kullanmakta ve dünya çapında 1.5 milyondan fazla kullanıcısı bulunmaktadır.
- Yüksek hızlı hassas tarama ve düşük false-positives.
- Düşük maliyetli güvenlik açığı taraması.
- Rekabetçi çözümlerden daha fazla teknoloji ve daha fazla güvenlik açığının kapsamı için destek.
- IP adresi taramaları için sınırsız yetkilendirme.
- Metasploit, Core Impact, Canvas, ExploitHub gibi popüler test araçlarıyla tarama verilerini ilişkilendirebilme.
- Kolay kurulumu ve bakım kolaylığı
- Yüzbinlerce sistem ölçeklenebilirliği

### **Nesus Minimum Donanım Gereksinimleri**

**Maksimum 50.000 host için :**
- İşlemci : 1 tane Dual-Core 2 GHz
- RAM Boyutu : 2 GB RAM (4 GB RAM önerilir)
- Disk Boyutu : 30 GB

**50.000 hosttan fazlası için :**
- İşlemci : 1 tane Dual-Core 2 GHz (2 tane Dual-Core önerilir)
- RAM Boyutu : 2 GB RAM (8 GB RAM önerilir)
- Disk Boyutu : (Raporlama için ek alan gerekebilir)

### **Nesus Lisans Modeli**
Nessus 2 lisans modeline sahiptir:
- **Professional Feed**
    - Ticari Kullanım
    - Destek Portalı Erişimi
- **Home Feed (Free Version)**
    - Ücretsizdir.
    - Sadece Kişisel Kullanım
    - İşlevselliğin sınırlaması vardır. Bunlar; eş zamanlı IP tarama, tarama zamanlaması yok, uygunluk ve denetim kontrolü yoktur.

### **Nesus Terminolojisi**

**Policy** : Tarama için yapılandırma ayarları.

**Scan** : Bir IP veya alan adları listesini ilişkilendirir.
- Basic Scan
- Template
- Sheduled Template ( Sadece Professional Feed içindir.)

**Report** : Belirli bir tarama örneğinin sonucu.

**Plugin** : Bir güvenlik kontrolü veya tarama ayarları penceresi.

**Plugin Family** : Ortak bir şeye sahip plugingrubu.Örneğin : FTP, Web Sunucuları, Cisco.

### **Nesus Özelleştirme Seçenekleri**
- **Report Templates**
    - XSLT ’de kodlanmış raporlama şablonudur.
- **Plugins**
    - Nessus Attack Scripting Language (NASL) ile kodlanmıştır.
- **Import / Export**
    - XML ’de kodlanmıştır. Raporlar ve profiller için aynı biçimdedir.
- **Audit Files**
    - Pseudo—XML ’de kodlanmış denetim dosyaları olup sadece Professional Feed versiyonuna özgüdür.

### **NASL ( Nessus Attack Scripting Language )**

Desteklenen bir betik dili olup Nessus için güvenlik kontrollerinin yazılması için oluşturulmuştur. Güvenlik kontrolleri plugin family gruplarına göre ayrılır. Sadece local sistemlerde komutlar çalıştırılır. Paketler başka bir ana bilgisayara hedeften gönderilmez. 

Ağ ile ilgili görevleri gerçekleştirmek için optimize edilmiş yerleşik işlevler :
- Socket İşlemleri
- Portlar açık ise açık bağlantıları göstermek
- IP/TCP/ICMP paketleri

### **Neden NASL?**

Normalde Nessus, pluginlerini kendisi yazıp kullanıcılarına güncelleme göndermekteydi. Böylece kullanıcıların düzenli olarak güncelleme yapması gerekiyordu. Şimdi ise, kullanıcı bu dili öğrendikten sonra kendisine özel durumlar oluşturup kendi güvenlik kontrollerini yazabilir. NASL seçiminde kolay, anlaşılır, kod yazımı basit bir betik dili olması önemli rol alır. C programlama dili gibi dilleri bilen kullanıcılar için uyum süresi kısadır.

### **NASL Komut Dosyası Entegrasyonu**

Söz dizimi olarak C programlama diline benzemektedir. Linux editöründe ya da herhangi bir Not Defteri üzerinden komutlar yazıldıktan sonra .nasl uzantısıyla kaydedilir. Daha sonra bu dosyayı Nessus sunucusunun kurulu olduğu dizine kopyalanır. Linux için, oluşturulan .nasl dosyası **/opt/nessus/lib/nessus/plugin** dizinine kopyalanır. Kopyalamadan sonra Nessus sunucusu yeniden başlatılır. Sunucu yeniden başlatıldıktan sonra istemci arayüzü de yeniden başlatılır. Böylece oluşturulan .nasl dosyası Nessus üzerinden kullanılabilir.

### **NASL Sorgu Yazma Mantığı**

**Configuration**
- Plugin bilgileri
- ID, ismi, açıklaması, kategorisi, ailesi, bağımlılıkları, ön koşulları.

**Post Configuration—Included Header**
C programlama dilindeki kütüphane tanımlanması gibidir.
- dump.inc : Standart çıktılardaki verileri göstermek içindir.
- ftp_func.inc, http_func.inc, smb_func.inc, smtp_func.inc vb.
- misc_func.inc : Servis header’ları ve header’ların ayrıştırma işlevi.
- uddi.inc : UDDI XML sorgulamalarını biçimlendirme işlevi.

**Security Check**
- Komut dosyasının bu kısmı, dosyanın gerçek mantığını içerir. Kullanıcı önceden tanımlanmış header dosyalarından, fonksiyonlar ve yazım denetimlerini kullanabilir. Son olarak rapor oluşturmak komutları kullanılmaktadır.
    - **Security_hole** : Oluşacak hata bildirimi için kullanılır.
    - **Security_warning** : Küçük kusurları bildirmek için kullanılır.
    - **Security_note** : Misc bildirmek için kullanılır.
    - Tüm bu işlevleri iki argüman alır. Bunlar, port numarası ve raporda gösterilecek açıklama dizesi.

### **NASL Hata Ayıklama Betiği**

Söz dizimlerini kontrol etmek için nasl betiği yazdıktan sonra ndbg binary’nin **-t** parametresi kullanılır.
- cd /opt/nessus/bin
- ndbg –t Hedef_IP /opt/nessus/lib/nessus/plugin/scriptadi.nasl

Bu söz dizimi hatasını kontrol edecek ve satır numarası ile birlikte varsa hatayı gösterecektir. Nasl Remote Debugger(ndbg).

### **PLUGINLER**
Bir taramada neler yapmasını, ne çapta bir tarama gerçekleştireceğimizi belirlemek için kullanılır. Tarama sonuçlarının güncel olması açısından pluginler güncelleştirilmelidir. Pluginler, varsayılan olarak 24 saatte bir güncelleştirilir. Ayrıca manuel güncelleştirmek için:
- **/opt/nessus/sbin/nessuscli update** komutu çalıştırılmalıdır.

Plugin otomatik güncelleştirmelerini görmek için:
- **cd /opt/nessus/etc/nessus-** dizisine girip:
    - **cat nessusd.conf** komutuyla nessus konfigürasyon dosyası okunabilir.
    - Ayrıca dosyada **auto-update-delay=24** değeri varsayılan 24 süreyle olan güncelleştirmeyi gösterir.

## **Nessus İndirmek**
https://www.tenable.com/downloads/nessus adresinden işletim sistemi bilgisine göre indirilebilir.

| ![atgr1]({{ site.url }}/assets/img/Nessus/part1/Resim1.png){: style="display: block; margin-left: auto; margin-right: auto; width: 80% "} |
|:--:|
| *Resim1 : Nesus Kurulum Dosyaları* |

### **Nessus Linux Kurulumu**

Kurulum İçin:
- **Red Hat version 6**
    - rpm -ivh Nessus-version_number-es6.x86_64.rpm 
- **Debian version 6**
    - dpkg -i Nessus-version_number-debian6_amd64.deb 
- **FreeBSD version 10**
    - pkg add Nessus-version_number-fbsd10-amd64.txz 

Çalıştırmak İçin:
- **Red Hat, CentOS, Oracle Linux, Fedora, SUSE, FreeBSD, Debian/Kali ve Ubuntu**
    - service nessusd start 

### **Nessus Kali Linux Kurulum Adımları**

**Adım1 :** Öncelikle kurulum dosyası indirilir.

| ![atgr2]({{ site.url }}/assets/img/Nessus/part1/adim1.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim2 : Kurulum dosyasının indirilmesi* |



**Adım2 :** Dosya kurulumu başlatılır. Kurulum dosyalarının sisteme yüklenmesinden sonra kurulum web arayüzünde devam eder.

```linux
root@Stormer:~# dpkg -i Nessus-7.1.0-debian6_amd64.deb
```

**Adım3 :** Web arayüzünden devam edebilmek için Nesus servisinin başlatılması gerekir.

```linux
root@Stormer:~# systemctl start nessusd.service
```


**Adım4 :** Kullanıcı hesabı oluşturmak için bir kullanıcı adı ve parola oluşturulması gerekir.

| ![atgr3]({{ site.url }}/assets/img/Nessus/part1/adim7.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim3 : Kullanıcı hesabı oluşturma ekranı* |


**Adım5 :** Kullanıcı hesabının oluşturulmasından sonra Lisans anahtarının girilmesi gerekir.

| ![atgr4]({{ site.url }}/assets/img/Nessus/part1/adim9.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim4 : Lisans anahtarının girilmesi* |

**Adım6 :** Giriş yapıldıktan sonra Nesus tarama paneli görüntülenir.

| ![atgr5]({{ site.url }}/assets/img/Nessus/part1/adim11.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim5 : Nessus Tarama paneli* |

### **Nesus Windows Kurulumu**

İndirme işlemi gerçekleştirildikten sonra yanda gösterildiği gibi **next** işlemlerini gerçekleştirerek kurulum yapılır.

| ![atgr6]({{ site.url }}/assets/img/Nessus/part1/Resim2.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim6 : Nessus Windows Kurulumu* |

### **Nesus Mac OSX Kurulumu**

İndirilmiş olan dosya extract edildikten sonra Windows ortamında yapıldığı gibi çift tıklanarak kurulum işlemini başlatılır.

| ![atgr7]({{ site.url }}/assets/img/Nessus/part1/Resim3.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim7 : Nessus Mac OSX Kurulumu* |