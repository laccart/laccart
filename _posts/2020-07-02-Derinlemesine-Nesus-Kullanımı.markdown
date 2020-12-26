---
title:  "Derinlemesine Nessus Özellikleri ve Kullanımı"
date:   2018-07-02 
categories: 
    - network-guvenligi
thumbnail: posts/nessus.jpg
---

## **Derinlemesine Nessus Özellikleri ve Kullanımı**

**Varsayılan olarak https://localhost:8834/#/ adresi üzerinden giriş paneline geldikten sonra kullanıcı adı ve parola bilgileriyle giriş yapılır.**

| ![atgr1]({{ site.url }}/assets/img/Nessus/part2/resim1.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim1 : Nessus Login Sayfası* |
<br/><br/>
Giriş panelinde kullanıcı bilgileri doğru girildiği takdirde ana panele yönlendirilir.

| ![atgr2]({{ site.url }}/assets/img/Nessus/part2/resim2.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim2 : Nesus Ana Paneli* |
<br/><br/>

### **Nessus Sekmelerinde Temel Gezinti**

Nesus'un Settings sekmesinde kullanıcı ve sunucu taraflı ayarlamalar yapılır.

| ![atgr3]({{ site.url }}/assets/img/Nessus/part2/resim3.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim3 : Nesus Settings Ekranı* |
<br/><br/>

- Settings
    - Settings
        - About
        - Advanced Settings
        - Proxy Server
        - Remote Link
        - SMTP Server
        - Custom CA
        - Upgrade Assistant
        - Password Management
- Accounts
- My Account
<br/><br/>

**Settings -> Advanced Settings** : Global ayarları manuel olarak yapılandırmamıza izin verir. Nessus servisi veya sunucunun yeniden başlatılması gerekebilir.

| ![atgr4]({{ site.url }}/assets/img/Nessus/part2/resim4.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim4 : Nesus Settings -> Advanced Settings* |
<br/><br/>

**Settings -> Proxy Server** : HTTP isteklerini iletmek için kullanılır. Nessus eklenti güncelleştirmelerini gerçekleştirmek için uzak tarayıcılarla veya aracılarla iletişim kurmak için kullanır. Ana makine ve bağlantı portu zorunlu olup diğer bilgiler isteğe bağlı olabilir. 

| ![atgr5]({{ site.url }}/assets/img/Nessus/part2/resim5.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim5 : Nesus Settings -> Proxy Server* |
<br/><br/>

**Settings -> Remote Link** : Remote link ayarı etkinleştirilerek tarayıcı Tenable.io veya Nessus Manager ’a bağlanabilir. Taramalar yapılandırılırken veya başlatılırken tamamen yönetilebilir ve seçilebilir. 

| ![atgr6]({{ site.url }}/assets/img/Nessus/part2/resim6.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim6 : Nesus Settings -> Remote Link* |
<br/><br/>

**Settings -> SMTP Server** : E-posta almak veya göndermek için bir endüstri standardıdır. SMTP yapılandırıldıktan sonra tarama sonuçları bir taramanın *e-posta bildirimleri* yapılandırılmasında belirtilen alıcılar listesine gönderilir. Bu sonuçlar filtrelerle özel olarak özelleştirilebilir ve HTML uyumlu e-posta istemcisi gerektirir.

| ![atgr7]({{ site.url }}/assets/img/Nessus/part2/resim7.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim7 : Nesus Settings -> SMTP Server* |
<br/><br/>

**Settings -> Custom CA** : Taramalar sırasında bu eklenti ( SSL Sertifikası ) yanlış bulguları azaltmaya yardımcı olur. 

| ![atgr8]({{ site.url }}/assets/img/Nessus/part2/resim8.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim8 : Nesus Settings -> Custom CA* |
<br/><br/>

**Settings -> Upgrade Assistant** : Nessus ’u güncelleştirme ve Tenable.io yükseltme işlemi gerçekleştirilir.

| ![atgr9]({{ site.url }}/assets/img/Nessus/part2/resim9.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim9 : Nesus Settings -> Upgrade Assistant* |
<br/><br/>

**Settings -> Password Management** : Parolalar için parametreleri ayarlamanıza, oturum açma bildirimlerini açmanıza ve oturum zaman aşımını ayarlamanıza olanak tanır. Giriş bildirimleri, kullanıcının son başarılı giriş - başarısız girişlerinin tarih, saat, IP gibi bilgileri tutar. Başarısız deneme kaydını da tutar.

| ![atgr10]({{ site.url }}/assets/img/Nessus/part2/resim10.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim10 : Nesus Settings -> Password Management* |
<br/><br/>

**Accounts -> My Account** : Hesap ayarları ve API Keys bulunmaktadır.

| ![atgr11]({{ site.url }}/assets/img/Nessus/part2/resim11.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim11 : Nesus Accounts -> My Account* |
<br/><br/>

Nessus'un taramalar sekmesinde yapılan taramaların bulunduğu klasörler ve tarama yapmak için kullanılan kaynaklar bulunmaktadır. Kaynaklar arasında politikalar, eklentiler, raporların özelleştirilmesi ve taramalar ile ilgili işlemler mevcuttur.

| ![atgr12]({{ site.url }}/assets/img/Nessus/part2/resim12.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim12 : Nesus Tarama işlemleri sekmesi* |
<br/><br/>

- Scans
    - Folders
        - My Scans
        - All Scans
        - Trash
    - Resources
        - Policies
        - Plugin Rules
        - Customized Reports
        - Scanners
<br/><br/>

**Folders -> My Scans** : Kullanıcı tarafından yapılan taramalar yer alır. Ayrıca Import, New Folder, New Scan gibi sekmeleri vardır. 

| ![atgr13]({{ site.url }}/assets/img/Nessus/part2/resim13.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim13 : Nesus Folders -> My Scans* |
<br/><br/>

**Folders -> All Scans** : Bütün taramaları içerisinde barındırır.

| ![atgr14]({{ site.url }}/assets/img/Nessus/part2/resim14.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim14 : Nesus Folders -> All Scans* |
<br/><br/>

**Folders -> Trash** : Silinen taramaların gönderildiği çöp kutusu denilebilir.

| ![atgr15]({{ site.url }}/assets/img/Nessus/part2/resim15.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim15 : Nesus Folders -> Trash* |
<br/><br/>

**Resources -> Policies** : Tarama sırasında hangi işlemlerin  gerçekleştirildiğini tanımlayan özel şablonlar oluşturmaya olanak tanır. Bir kez oluşturulduktan şablon, tarama şablonları listesinden seçilebilir. Politikalar görüntülenebilir, oluşturulabilir, içe aktarılabilir, indirilebilir, düzenlenebilir ve silinebilir.

| ![atgr16]({{ site.url }}/assets/img/Nessus/part2/resim16.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim16 : Nesus Resources -> Policies* |
<br/><br/>

**Resources -> Plugin Rules** : Eklenti kuralları, verilen herhangi bir eklentinin önemini gizlemenize veya değiştirmenize izin verir. Ek olarak, kurallar belirli bir ana bilgisayar veya belirli bir zaman dilimi ile sınırlandırılabilir. Kurallar görüntülenebilir, oluşturulabilir, düzenlenebilir ve silinebilir.

| ![atgr17]({{ site.url }}/assets/img/Nessus/part2/resim17.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim17 : Nesus Resources -> Plugin Rules* |
<br/><br/>

**Resources -> Customized Reports** : Tarama sonuçlarından HTML veya PDF dosyalarını dışarı aktarırken kullanmak için özel bir ad ve logo eklenebilir. Resimler maksimum 10 MB boyutunda olan JPEG, GIF veya PNG formatında olmalı ve saydamlık içermemelidir.

| ![atgr18]({{ site.url }}/assets/img/Nessus/part2/resim18.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim18 : Nesus Resources -> Customized Reports* |
<br/><br/>

**Resources -> Scanners** : Uzak tarayıcılara upgrade linkine tıklayarak bağlanılabilir. Bağlantı kuruldutan sonra taramaları yapılandırırken yerel olarak yönetilebilir ve seçilebilir. Tarayıcının mevcut durumu ve tüm çalışan taramalar kontrol edilebilir.

| ![atgr19]({{ site.url }}/assets/img/Nessus/part2/resim19.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim19 : Nesus Resources -> Scanners* |
<br/><br/>

### **Nessus Tarama Politikaları**

Nessus taramalarında, hedef varlıklar kategorize edilerek tarama politikaları oluşturulmuştur. Taramanın yapılacağı hedef sisteme göre belirli politikalar oluşturulur. Bu politikalar, varlığın kapsamına, işlemine ve amacına göre belirlenir. Oluşturulacak yeni politikalarda, hedefe yönelik bilgi toplama işlemleri, network taramaları, listeleme (enumeration) işlemleri ve hedef ile ilgili zafiyet tespit işlemlerini yapacak eklentiler (plugins) kullanıcı tarafından seçilip yapılandırılabilir. Ayrıca, Nessus tarafından varsayılan olarak gelen politikalar üzerinde düzenlemeler yapılarak yeni politikalar oluşturulabilir. Oluşturulan yeni politikalar kullanılarak hedefe yönelik Nessus taraması yapılabilir.

| ![atgr19]({{ site.url }}/assets/img/Nessus/part2/resim19.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim19 : Nesus Varsayılan Tarama Politikaları* |
<br/><br/>

**Resources -> Policies -> New Policy** : Tarama yapılacak varlıklara yönelik yeni tarama politikaları oluşturulabilir. Ayrıca varsayılan politikalar yapılandırılarak yeni politikalar oluşturulabilir.
<br/><br/>

**Resources -> Policies -> New Policy -> Advanced Scan** : Herhangi bir öneri kullanmadan yapılacak tarama yapılandırılabilir. Bu taramada kullanıcı hedef sisteme yönelik network taramaları ve plugin kullanarak zafiyet taramaları gerçekleştirebilir.

| ![atgr20]({{ site.url }}/assets/img/Nessus/part2/resim20.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim20 : Nesus Resources -> Policies -> New Policy -> Advanced Scan* |
<br/><br/>

**Resources -> Policies -> New Policy -> Audit Cloud Infrastructure** : Third-party bulut hizmetlerinin denetlenmesi için oluşturulan bir politikadır.

| ![atgr21]({{ site.url }}/assets/img/Nessus/part2/resim21.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim21 : Nesus Resources -> Policies -> New Policy -> Audit Cloud Infrastructure* |
<br/><br/>

**Resources -> Policies -> New Policy -> BadLock Detection** : CVE-2016-2118 (samba) ve CVE-2016-0128(Microsoft) kodlu güvenlik açıklarının sistemlerde olup olmadığını kontrol eden bır tarama politikasıdır. BadLock zafiyeti, Wiındows 2003, Windows 2000, Windows XP ve samba sunucularını hedef alır.

| ![atgr22]({{ site.url }}/assets/img/Nessus/part2/resim22.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim22 : MS08-067 ve BADLOCK zafiyetlerinin karşılaştırılması* |
<br/><br/>

**Resources -> Policies -> New Policy -> Bash Shellshock Detection** : CVE-2014-6271 ve CVE-2014-7169 kodlu güvenlik açıklarının sistemlerde olup olmadığını kontrol eden bır tarama politikasıdır. Bu politika, Shellshock güvenlik açığı için HTTP, FTP, SMTP, Telnet ve SIP aracılığıyla uzaktan denetleme yapmak için kullanılır. SSH bilgileri isteğe bağlı olarak CVE-2014-6271 kodlu zafiyetin test edilmesi için kullanılır. Bash Shellshock, birçok Linux/UNIX işletim sisteminde ve Apple’ın Mac OS X’inde kullanılan ortak komut satırı kabuğu olan GNU Bourne-Again Shell(Bash)’da bulunan bir güvenlik açığıdır. Hata, bir saldırganın zararlı kodu ekleyerek işletim sistemi tarafından kullanılan ortam değişkenlerinde kabuk komutlarını uzaktan çalıştırmasına izin verir.

<br/><br/>
**Resources -> Policies -> New Policy -> Basic Network Scan** : Herhangi bir hedef için genel sistem taramasıdır. TCP, ARP, ICMP paketlerini kullanır. Port taraması gerçekleştirilir.

| ![atgr23]({{ site.url }}/assets/img/Nessus/part2/resim23.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim23 : Resources -> Policies -> New Policy -> Basic Network Scan* |
<br/><br/>

**Resources -> Policies -> New Policy -> Credentialed Patch Audit** : Ana makineler için kimlik doğrulaması yapıp eksik güncellemeleri numaralandırır. Brute-Force, Windows, Malware değerlendirmesi yapılabilir.

| ![atgr24]({{ site.url }}/assets/img/Nessus/part2/resim24.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim24 : Resources -> Policies -> New Policy -> Credentialed Patch Audit* |
<br/><br/>

**Resources -> Policies -> New Policy -> DROWN Detection** : CVE-2016-0800 kodlu güvenlik açığının sistemlerde olup olmadığını kontrol eden bır tarama politikasıdır. Bu zafiyet, SSLv2 protokolündeki bir hatadan kaynaklanmaktadır. RSA şifreleme metnini daha yeni bir SSL/TLS protokolü sürümü kullanarak bir bağlantıdan şifrelemek ya da şifreyi çözmek olarak DROWN tekniği kullanılır.

| ![atgr25]({{ site.url }}/assets/img/Nessus/part2/resim25.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim25 : Resources -> Policies -> New Policy -> DROWN Detection* |
<br/><br/>

**Resources -> Policies -> New Policy -> Host Discovery** : Basit bir keşif taraması sonucu açık hostların ve açık hostlara ait açık portların keşfi yapılır.

| ![atgr26]({{ site.url }}/assets/img/Nessus/part2/resim26.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim26 : Resources -> Policies -> New Policy -> Host Discovery* |
<br/><br/>

**Resources -> Policies -> New Policy -> Intel AMT Security Bypass** : CVE-2017-5689 kodlu güvenlik açığının sistemlerde olup olmadığını kontrol eden bır tarama politikasıdır. Özellikle kurumsal bilgisayarlar ve ağlar için daha büyük risk oluşturabilecek bir ayrıcalık yükseltme zafiyetidir. Belirli faktörler ve tetikleyiciler olsa bile, saldırganlara yönetim erişimi sağlayabilir, uzaktan kontrol edilip sıfırlama yada kapatma yapılabilir.

| ![atgr27]({{ site.url }}/assets/img/Nessus/part2/resim27.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim27 : Resources -> Policies -> New Policy -> Intel AMT Security Bypass* |
<br/><br/>

**Resources -> Policies -> New Policy -> Internal PCI Network Scan** : Dahili bir PCI DSS güvenlik açığı taraması gerçekleştirir. Bir kurumun kart sahibi veri ortamına giden veya bu içeriğe giden iç taraftaki tüm ana bilgisayarlardaki mantıksal ağ çerçevesinde çalıştırılan güvenlik açığını tarar. Kimlik bilgileri, eksik yama gibi kontroller yapılır. En az 3 ayda biri tarama yapılması önerilir.

| ![atgr28]({{ site.url }}/assets/img/Nessus/part2/resim28.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim28 : Resources -> Policies -> New Policy -> Internal PCI Network Scan* |
<br/><br/>

**Resources -> Policies -> New Policy -> Malware Scan** : Windows ve UNIX sistemler için malware yani zararlı yazılım analizi yapılır.

| ![atgr29]({{ site.url }}/assets/img/Nessus/part2/resim29.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim29 : Resources -> Policies -> New Policy -> Malware Scan* |
<br/><br/>

**Resources -> Policies -> New Policy -> MDM Config Audit** : Mobil Cihazlarını yapılandırılmasını denetler. Kullanımı için Upgrade edilmesi gerekmektedir.
<br/><br/>

**Resources -> Policies -> New Policy -> Mobile Device Scan** : Microsoft Exchange ve mobil cihazların denetiminde kullanılır. Kullanılması için Upgrade edilmesi gerekmektedir.
<br/><br/>

**Resources -> Policies -> New Policy -> Offline Config Audit** : Offline olarak ağ cihazlarının yapılandırılmasını denetlemektedir.

| ![atgr30]({{ site.url }}/assets/img/Nessus/part2/resim30.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim30 : Resources -> Policies -> New Policy -> Offline Config Audit* |
<br/><br/>

**Resources -> Policies -> New Policy -> PCI Quarterly External Scan** : PCI tarafından gerektiği gibi harici tarama için onaylanmıştır. (Unofficial)

| ![atgr31]({{ site.url }}/assets/img/Nessus/part2/resim31.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim31 : Resources -> Policies -> New Policy -> PCI Quarterly External Scan* |
<br/><br/>

**Resources -> Policies -> New Policy -> Policy Compliance Auditing** : Bilinen bir temel çizgiye karşı denetim sistemi yapılandırmalarıdır.

| ![atgr32]({{ site.url }}/assets/img/Nessus/part2/resim32.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim32 : Resources -> Policies -> New Policy -> Policy Compliance Auditing* |
<br/><br/>

**Resources -> Policies -> New Policy -> SCAP and OVAL Auditing** : SCAP ve OVAL tanımlarını kullanan denetim sistemlerine yönelik taramadır. The Security Content Automation Protocol (SCAP), bir kuruluşta dağıtılan sistemlerin otomatik güvenlik açığı yönetimi, ölçümü ve politika uyumluluğu değerlendirmesini etkinleştirmek için belirli standartların kullanılması için bir yöntemdir.

| ![atgr33]({{ site.url }}/assets/img/Nessus/part2/resim33.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim33 : Resources -> Policies -> New Policy -> SCAP and OVAL Auditing* |
<br/><br/>

**Resources -> Policies -> New Policy -> Shadow Brokers Scan** : Shadow Brokers tarafından bulunan zafiyetlerin taranmasını sağlayan bir tarama politikasıdır.

| ![atgr34]({{ site.url }}/assets/img/Nessus/part2/resim34.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim34 : Resources -> Policies -> New Policy -> Shadow Brokers Scan* |
<br/><br/>

**Resources -> Policies -> New Policy -> Spectre and Meltdown** : CVE-2017-5753, CVE-2017-5715 ve CVE-2017-5754  kodlu güvenlik açıklarının sistemlerde olup olmadığını kontrol eden bır tarama politikasıdır. Modern işlemcilerdeki kritik güvenlik açıklarıdır. Bu donanım güvenlik açıkları, programların bilgisayarda şu anda işlenen verileri çalmalarına izin verir. Programların genellikle veri okumalarına izin verilmemesine karşın, kötü amaçlı bir program diğer çalışan programların belleğinde saklanan bilgileri saklamak için kullanılır. Bu, bir şifre yöneticisi ve tarayıcısında, e-postalarınızdan, kişisel fotoğraflarınızdan, anlık iletilerinizden ve hatta kritik iş belgelerinizde saklanan şifrelerinizi içerebilir.
<br/><br/>

**Resources -> Policies -> New Policy -> WannaCry Ransomware** : MS17-010 güvenlik açığının sistemlerde olup olmadığını kontrol eden bır tarama politikasıdır. SMB protokolü ve 445 nolu port üzerinden uzaktan kontrol sağlanmaktadır. Windows kimlik bilgilerini isteğe bağlı olarak WMI ile test etmek ve eksiksiz yazılım güncellemelerini numaralandırmak için sağlanabilir. 

| ![atgr35]({{ site.url }}/assets/img/Nessus/part2/resim35.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim35 : Resources -> Policies -> New Policy -> WannaCry Ransomware* |
<br/><br/>

Eternalblue & Doublepulsar olarak adlandırılan zafiyet, smb üzerinden dll injection yaparak hedefe sızmayı sağlıyor. EternalBlue zafiyetinden yararlanarak WannaCry ve Petya zararlı yazılımları, dünyada birçok sistemi olumsuz etkilemiştir. Wannacry, hiçbir kullanıcı etkileşimine gerek kalmadan Microsoft Windows’un SMBv1 sisteminde bulunan MS17-010 zafiyetini kullanıyor.
<br/><br/>

**Resources -> Policies -> New Policy -> Web Application Tests** : Web tabanlı uygulamalarda bulunabilecek güvenlik açıklarına yönelik oluşturulmuş bir tarama politikasıdır.

| ![atgr36]({{ site.url }}/assets/img/Nessus/part2/resim36.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim36 : Resources -> Policies -> New Policy -> Web Application Tests* |
<br/><br/>

### **Nessus Kullanımı**

- **Adım1:**
![atgr37]({{ site.url }}/assets/img/Nessus/part2/resim37.png){: style="display: block; margin-left: auto; margin-right: auto; width: 70% "}
<br/><br/>
- **Adım2:**

![atgr38]({{ site.url }}/assets/img/Nessus/part2/resim38.png){: style="display: block; margin-left: auto; margin-right: auto; width: 70% "}
<br/><br/>
- **Adım3:**

![atgr39]({{ site.url }}/assets/img/Nessus/part2/resim39.png){: style="display: block; margin-left: auto; margin-right: auto; width: 70% "}
<br/><br/>
- **Adım4:**

![atgr40]({{ site.url }}/assets/img/Nessus/part2/resim40.png){: style="display: block; margin-left: auto; margin-right: auto; width: 70% "}
<br/><br/>
- **Adım5:**

![atgr41]({{ site.url }}/assets/img/Nessus/part2/resim41.png){: style="display: block; margin-left: auto; margin-right: auto; width: 70% "}
<br/><br/>
- **Adım6:**

![atgr42]({{ site.url }}/assets/img/Nessus/part2/resim42.png){: style="display: block; margin-left: auto; margin-right: auto; width: 70% "}
<br/><br/>
- **Adım7:**

![atgr43]({{ site.url }}/assets/img/Nessus/part2/resim43.png){: style="display: block; margin-left: auto; margin-right: auto; width: 70% "}
<br/><br/>
- **Adım8:**

![atgr44]({{ site.url }}/assets/img/Nessus/part2/resim44.png){: style="display: block; margin-left: auto; margin-right: auto; width: 70% "}
<br/><br/>
- **Adım9:**

![atgr45]({{ site.url }}/assets/img/Nessus/part2/resim45.png){: style="display: block; margin-left: auto; margin-right: auto; width: 70% "}
<br/><br/>
- **Adım10:**

![atgr46]({{ site.url }}/assets/img/Nessus/part2/resim46.png){: style="display: block; margin-left: auto; margin-right: auto; width: 70% "}
<br/><br/>
- **Adım11:**

![atgr47]({{ site.url }}/assets/img/Nessus/part2/resim47.png){: style="display: block; margin-left: auto; margin-right: auto; width: 70% "}
<br/><br/>
- **Adım12:**

![atgr48]({{ site.url }}/assets/img/Nessus/part2/resim48.png){: style="display: block; margin-left: auto; margin-right: auto; width: 70% "}
<br/><br/>
- **Adım13:**

![atgr49]({{ site.url }}/assets/img/Nessus/part2/resim49.png){: style="display: block; margin-left: auto; margin-right: auto; width: 70% "}
<br/><br/>
- **Adım14:**

![atgr50]({{ site.url }}/assets/img/Nessus/part2/resim50.png){: style="display: block; margin-left: auto; margin-right: auto; width: 70% "}
<br/><br/>
- **Adım15:**

![atgr51]({{ site.url }}/assets/img/Nessus/part2/resim51.png){: style="display: block; margin-left: auto; margin-right: auto; width: 70% "}
<br/><br/>
- **Adım16:**

![atgr52]({{ site.url }}/assets/img/Nessus/part2/resim52.png){: style="display: block; margin-left: auto; margin-right: auto; width: 70% "}
<br/><br/>
- **Adım17:**

![atgr53]({{ site.url }}/assets/img/Nessus/part2/resim53.png){: style="display: block; margin-left: auto; margin-right: auto; width: 70% "}
