---
title:  "Nmap ile NSE Scriptleri Nasıl Kullanılır?"
date:   2020-01-31 
layout: post
categories: 
    - network-guvenligi
thumbnail: posts/nmap.png
---
# Nmap ile NSE Scriptleri Nasıl Kullanılır?

**Nmap Scripting Engine (NSE), Nmap’in en güçlü ve esnek özelliklerinden biridir. Kullanıcıların ağ üzerinde yapmak istediği işlemleri otomatize eder. İçerisindeki komut dosyaları, Nmap ile paralel çalışmakta olup taramaya hız ve verimlilik katar. Nmap Scripting Engine, ağ keşiflerinde hedef hakkında bilgiler toplamak, açık olan portlara yönelik gelişmiş sürüm tespitini yapmak, hedef sistemde bulunan güvenlik açıklıklarının tespitini yapmak, sistem üzerinde çalışan backdoorların tespitini yapmak ve tespit edilen güvenlik açıklıklarını exploit etmek için Lua dilinde yazılmış olan bazı scriptleri kullanmak gibi işlemleri gerçekleştiren modülleri içerir.**

NSE, **--sC** parametresi ile kullanılır. Ayrıca özel script belirtilmek istendiğinde **--script** veya **--sC** parametresinden sonra kullanılacak script adının yazılması gerekir.  Elde edilen sonuçları Nmap normal ve XML çıktısına eklenir.

```linux
root@Stormer:~# nmap -sC -p22,111,139 192.168.16.128
```
Çıktı üreten servis komut dosyaları, sistemin RSA ve DSA SSH anahtarlarını sağlayan ssh-hostkey ve portmapper’ı mevcut hizmetleri numaralandırmayı sorgulayan rpcinfo’dur. Bu örnekte, çıktı üreten tek host komut dosyası, SMB sunucularından çeşitli bilgiler toplayan smb-os-discovery’dir.

### 7.1 Kullanım Şekilleri

- **--script dosyaadi|kategorisi|dizini|expression**

Virgülle ayrılmış dosya adı, script kategorileri ve dizin listesini kullanarak script taraması yapılır. Her öğe önce bir ifade, sonra bir kategori ve en sonunda bir dosya ya da dizin adı olarak yorumlanır. Script ifadesi listesindeki her öğeye, verilen script/kodları, script veya hostrule işlevlerindeki koşullardan bağımsız olarak çalışması için bir + karakteri eklenebilir. Nmap’ın mssql servisini tanıyabilmesi için kapsamlı sürüm tespiti (-sV –version-all) çalıştırarak Nmap taramasını yavaşlatmak yerine, ms-sql-config scriptini tüm hedeflenen hostlara ve portlara karşı çalıştırabilir. **--script + ms-sql-config** olarak yapılabilir.

- **--script-args argüman, --script-args-file dosya_adi**

Scriptlere argümânlar sağlanır. **--script-args-file** parametresi, argümanların bir dosyada belirtilmesinde kullanılır.

- **--script-help dosyaadi|kategorisi|dizini|expression|all**

Scriptler hakkında bilgi elde etmek için **--script-help** kullanılır. Belirtilen öğe ile eşleşen her bir script için Nmap, script adını, kategorilerini ve açıklamasını yazdırır. Özellikler **--script** tarafından kabul edilenlerle aynıdır; Örneğin, **nmap --script-help ssl-enum-ciphers** komutunu çalıştırılabilir.

- **--script-trace**

Bu parametre, **--packet-trace** öğesine benzemektedir. Ancak paket yerine uygulama düzeyinde çalışır. Bu parametre belirtilirse, scriptler tarafından gerçekleştirilen, tüm gelen ve giden iletişim yazdırılır. Görüntülenen bilgiler iletişim protokolünü, kaynak ve hedef adreslerini ve iletilen verileri içerir. İletilen verilerin %5’inden fazlası yazdırılamazsa, bunun yerine hex dökümleri yapılır. **--packet-trace** parametresinin belirlenmesi, script izlemeyi de sağlar. 

```linux
root@Stormer:~# nmap --script smb-os-discovery --script-trace 192.168.16.128
```

- **--script-updatedb**

Bu parametre, mevcut varsayılan scriptleri ve kategorileri belirlemek için Nmap tarafından kullanılan, **script/script.db** içinde bulunan script veritabanını günceller.

```linux
root@Stormer:~# nmap --script-updatedb
```

#### Script Kategorileri

NSE scriptleri, ait oldukları kategorilerin bir listesini tanımlar. Şu anda tanımlanmış kategoriler; auth, broadcast, brute, default, discovery, dos, exploit, external, fuzzer, intrusive, malware, safe, version ve vuln. Kategori adlarının büyük/küçük harf duyarlılığı yoktur.

✔ **Auth :** Hedef sisteme yönelik kimlik doğrulama işlemini yapan scriptlerin kategorisidir.

✔ **Broadcast :** Genellikle listelenmeyen hostları yerel ağda yayınlayarak keşfeden scriptlerdir.

✔ **Brute :** Uzak bir sunucunun kimlik doğrulama bilgilerini tahmin etmek için bruteforce saldırıları kullanan script kategorisidir.

✔ **Default :** Nmap’in **–A** parametresi ile kullanılan varsayılan scriptlerin kategorisidir. Bu kategori, **--script=default** kullanılarak belirtilir.

-  **Speed :** Bruteforce, kimlik doğrulama, crackleme, web dizin tespiti ve tek bir hizmeti taramak için hızlı sonuç veren default scriptleri kullanılır.

- **Usefulness :** Default kategorisindeki değerli ve işlem yapılabilir bilgiler üreten scriptleri belirtir.

- **Verbosity :** Nmap çıktısı çok çeşitli amaçlar için kullanılıp okunaklı ve özlü olması gerekir. Sık sık çıktılarla dolu sayfalar üreten bir scripti, default kategorisinden çıkartır.

- **Reliability :** Hedef host veya hizmetle ilgili sonuçlara ulaşmak için sezgisel ve bulanık imza eşleştirmeleri için kullanılan scriptleri belirtir. Örneğin, sniffer tespiti ve sql injection scriptlerini içerir.

- **Intrusiveness :** Sistemi veya hizmeti çekertebilen veya uzak yöneticilerin saldırısı olarak algılanan scriptleridir.

- **Privacy :** Üçüncü şahıslara bilgi veren scriptlerdir. Örneğin, whois betiği hedef IP adresini bölgesel whois kayıtlarına göstermelidir.

✔ **Discovery :** Ağ ve ağa bağlı bütün cihazlar hakkında bilgi elde etmek için kullanılan scriptlerin kategorisidir.

✔ **Dos :** Denial Of Service scriptlerinden oluşur. Servislerin çökeltilmesine yönelik bir zafiyet olup olmadığını test etmek için kullanılan scriptlerdir.

✔ **Exploit :** Hedef sistemde bulunan bir güvenlik açığını sömürmek için kullanılan scriptlerdir. Örnekler arasında jdwp-exec ve http-shellshock bulunur.

✔ **External :** Bir üçüncü taraf veritabanına veya başka bir ağ kaynağına veri gönderen scriptlerdir. Örneğin, whois sunucularına hedefin adresi hakkında bilgi edinmek için bir bağlantı kuran whois-ip’tir.

✔ **Fuzzer :** Hedefin cevap vermemesine yönelik rastgele hazırlanmış paketlerle istek yapılmasını sağlayan scriptlerdir. Örneğin, bir DNS sunucusunu, sunucu çökene veya kullanıcı tarafından belirlenen bir zaman sınırı doluncaya kadar yavaşça hatalı DNS istekleriyle bombalayan dns-fuzz‘dır.

✔ **Intrusive :** Hedef sistemi çökertecek veya hedef sistemin zararlı olarak algılayacağı scriptlerdir. Örnekler http-open-proxy (hedef sunucuyu bir HTTP proxy’si olarak kullanmaya çalışır) ve snmp-brute (genel, özel ve cisco gibi ortak değerler göndererek bir cihazın SNMP topluluk dizesini tahmin etmeye çalışır).

✔ **Malware :** Hedef platformun zararlı yazılımlara veya backdoorlara bulaşıp bulaşmadığını tespit eden scriptler malware kategorisinde yer alır. Örneğin, 25 numaralı port dışındaki farklı portlarda çalışan SMTP sunucularını izleyen smtp-strangeport ve herhangi bir istek almadan, istek almış gibi davranıp cevap veren kimlik doğrulama scripti auth-spoof’tur.

✔ **Safe :** Servisleri çökertmek, büyük miktarda ağ bant genişliği kullanmak veya güvenlik açıklarından yararlanmak için tasarlanmamış scriptlerdir. Örneğin, ssh-hostkey (bir SSH host anahtarı alır) ve html-title (başlığı bir web sayfasından alır). Sürüm kategorisindeki scriptler güvenlik açısından sınıflandırılmaz.

✔ **Version :** Sürüm tespit etme özelliğinin bir uzantısıdır. Açıkça seçilemez. Yalnızca sürüm tespiti (-sV) istendiğinde çalışacak şekilde seçilirler. Çıktıları sürüm tespit çıktısından ayırt edilemez ve hizmet ya da host script sonuçları üretmezler. Örneğin, skypev2 sürümü, pptp sürümü ve iax2 sürümü.

✔ **Vuln :** Bilinen bazı güvenlik açıklarının hedef platformda olup olmadığını tespit etmek için kullanılan scriptleridir. Örneğin, **realvnc-auth-bypass ve afp-path-vuln, smb-vuln-ms17-010** scriptleri bulunur.

### 7.3 Script Seçimi

**–-script** parametresi, virgülle ayrılmış kategorilerin, dosya adlarının ve dizin adlarının gösterimiyle kullanılır.

• **nmap --script default, safe**

Default ve Safe kategorilerindeki scriptler kullanılır. 

```linux
root@Stormer:~# nmap --script deafult,safe 192.168.16.128
```

• **nmap --script smb-os-discovery**

Yalnızca smb-os-discovery scripti kullanılır. .nse doya uzantısını eklemek zorunlu değildir.

• **nmap --script default, banner, /home/user/customscripts**

Default kategorisindeki scriptler, banner scripti ve /home/user/customscripts dizininde bulunan scriptler kullanılır.

• **nmap --script “http-*”**

Http-auth ve http-open-proxy gibi, adı http- ile başlayan tüm scriptler kullanılır. **--script** parametresi, wildcard karakterleri kabuktan korumak için tırnak içinde olmalıdır. Boolean ifadeleri oluşturmak için ve/veya operatörleri kullanılarak daha karmaşık komut dosyası seçimi yapılabilir.

• **nmap --script “not intrusive”**

intrusive kategorisi dışındaki scriptlerin kullanılmasını sağlar.

• **nmap --script “default or safe”**

İşlevsel olarak **nmap --script “default, safe”**  komutu ile eşdeğerdir. Default ya da safe kategorisindeki scriptler veya her ikisindeki scriptler kullanılır.

• **nmap --script “default and safe”**

Hem default hem de safe kategorisindeki scriptler kullanılır.

• **nmap --script “(default or safe or intrusive) and not http-*”**

http- ile başlayan scriptler hariç, default, safe ya da intrusive kategorisindeki scriptler kullanılır.

### 7.4 Script Tipleri ve Aşamaları

NSE, aldıkları hedeflerin türü ve çalıştırıldığı tarama aşaması ile ayırt edilen dört tür scripti destekler. Bireysel scriptler, çoklu işlem türlerini destekleyebilir.

• **Prerule Scriptleri**

Herhangi bir Nmap’ın tarama aşamasından önce yayınlanır. Bu nedenle Nmap henüz hedefleri hakkında herhangi bir bilgi toplamayabilir. DHCP ve DNS SD sunucularını sorgulamak için ağ yayın istekleri yapmak gibi belirli tarama hedeflerine bağlı olmayan görevler için faydalı olabilirler. Bu scriptlerden bazıları, Nmap’ın taraması için yeni hedefler oluşturabilir. Örneğin, dns-zone-transfer, zone transfer isteğini kullanarak bir alandaki IP’lerin listesini alabilir ve bunları otomatik olarak Nmap’ın tarama hedefi listesine eklemektedir.

• **Host Scriptleri**

Bu aşamadaki scriptler, Nmap host bulma, port tarama, sürüm tespiti ve hedef hostlara karşı işletim sistemi tespiti gerçekleştirdikten sonra Nmap’ın normal tarama işlemi sırasında çalışır. Bu tür scriptler, hostrule işleviyle eşleşen her hedef hosta karşı bir kez çağrılır. Örneğin, bir hedef IP için ownership bilgilerini arayan whois-ip ve parçalanma gerektirmeden hedefe ulaşabilecek maksimum IP paket boyutunu belirlemeye çalışan yol mtu’dur.

• **Servis Scriptleri**

Bu scriptler, bir hedef hostta dinleyen belirli servislere karşı çalıştırılır. Örneğin, Nmap, web sunucularına karşı çalıştırmak için 15’ten fazla http hizmeti scripti içerir. Bir hostta birden fazla portta çalışan web sunucuları varsa, bu komut dosyaları birden çok kez çalışabilir. Bunlar en çok kullanılan Nmap komut dosyası türüdür ve bir komut dosyasının hangi algılayıcılara karşı çalıştırılacağına karar vermek için bir portrule işlevi içererek ayırt edilirler.

• **Postrule Scriptleri**

Nmap tüm hedeflerini taradıktan sonra yayınlanır. Nmap çıktısını biçimlendirmek ve sunmak için faydalı olabilirler. Örneğin, ssh-hostkey, SSH sunucularına bağlanan, genel anahtarlarını keşfeden ve bunları basan hizmet (portrule) scritpleriyle en iyi bilinir. Ancak, taranan tüm hostlar arasında yinelenen anahtarları denetleyen, ardından bulunanları yazdıran bir posta kodu da içerir. Bir postrule scripti için bir başka kullanım, Nmap çıktısının ters bir indeksini basmaktır. Hangi hostların yalnızca her bir hosttaki hizmetleri listelemek yerine belirli bir servisi çalıştırdığını gösterir. Postrule scriptleri, postrule işlevi içererek tanımlanır. Birçok script, bir ön hazırlık ya da önyükleme dosyası olarak çalıştırılabilir. Bu durumlarda, tutarlılık için bir primer kullanmanız faydalı olacaktır.

### 7.5 Scriptlerden Bağımsız Değişkenler

Argümanlar **--script-args** parametresi kullanılarak NSE scriptlerine iletilebilir. Argümanlar bir key-value çiftleri tablosunu ve muhtemelen dizi değerlerini açıklar. Argümanlar, scriptlere nmap.registry.args adlı kayıt defterinde bir tablo olarak tutulur. Ancak normalde stdnse.get_script_args işleviyle erişilebilirler. Komut satırı bağımsız değişkenlerinin sözdizimi, Lua’nın tablo yapıcısının sözdizimine benzemektedir. Bağımsız değişkenler, virgülle ayrılmış bir ad listesidir: Değer çiftleridir. Adlar ve değerler, boşluk içermeyen dizeler veya ‘{’, ‘}’, ‘=’ veya ‘,’ karakterleri olabilir. Alıntı yapılan bir dizgede ‘\’ bir alıntıdan kaçar. Bir ters eğik çizgi yalnızca bu özel durumda tırnak işaretlerinden kaçmak için kullanılır; Diğer tüm durumlarda, ters eğik çizgi tam anlamıyla yorumlanır. Komut satırındaki **--script-args** komutundaki argümânları iletmek yerine, bunları bir dosyada (virgül veya yeni satırlarla ayrılmış) saklayabilir ve **--script-args-file** ile sadece dosya adını belirleyebilirsiniz. Komut satırında **--script-args** ile belirtilen parametreler, dosyada verilenlerden önceliklidir. Dosya adı mutlak bir yol olarak veya Nmap’ın normal arama yoluna (NMAPDIR, vs.) göre görülebilir. 

```linux
root@Stormer:~# nmap -sC --script-args 'user=foo,pass=",{}=bar=",paths={/admin,/cgi-bin},xmpp-info.server name=localhost'
```

### 7.6 Script Dili

Nmap Scripting Engine çekirdeği gömülebilir bir Lua yorumlayıcısıdır. Lua, genişletilebilirlik için tasarlanmış hafif bir dildir. Nmap gibi diğer yazılımlarla arayüz oluşturmak için güçlü ve iyi belgelenmiş bir API sunmaktadır. Nmap Scripting Enginenin ikinci kısmı, Lua ile Nmap’ı birbirine bağlayan NSE kütüphanesidir. Bu katman, Lua yorumlayıcısının başlatılması, paralel komut dosyası çalıştırma zamanlaması, komut dosyası alımı ve daha fazlası gibi sorunları ele alır. Aynı zamanda NSE ağ I/O framework ve yönetim mekanizmasının kalbidir. Ayrıca, komut dosyalarını daha güçlü ve kullanışlı hale getirmek için yardımcı program kitaplıkları da içerir.

Nmap script dili, Nmap ile arabirim oluşturmak için kütüphanelerle genişletilen gömülü bir Lua yorumlayıcısıdır. Nmap API, Lua namespace’inde nmap adıyla bulunur. Bu, Nmap tarafından sağlanan tüm kaynaklara yapılan çağrıların bir nmap ön ekine sahip olduğu anlamına gelmektedir. nmap.new socket(), örneğin, yeni bir soket sarmalayıcı nesnesi döndürür. Nmap kütüphane katmanı aynı zamanda Lua içeriğini başlatmayı, paralel kodları planlamayı ve tamamlanmış kodlar tarafından üretilen çıktıyı toplamayı da önemsemektedir. Planlama aşamalarında, Nmap script için temel olarak birkaç programlama dili düşünüldü. Diğer bir seçenek ise tamamen yeni bir programlama dili uygulamaktı. NSE’nin kullanımı kolay, küçük boyutlu, Nmap lisansı ile uyumlu, ölçeklenebilir, hızlı ve paralelleştirilebilir olması gerekiyordu. Perl, Python ve Ruby dillerde daha kapsamlı interpreterlar vardır. Ancak, Nmap içerisine verimli bir şekilde gömülmesi zordur. Ama, Lua Nmap içerisinde gömülü geldiği gibi, Wireshark sniffer ve Snort IDS gibi diğer popüler açık kaynaklı güvenlik araçlarında bile gömülü geliyor.

Lua’nın önemli yerleşik özelliklerine ek olarak, senaryo yazmayı daha güçlü ve kullanışlı hale getiren birçok uzantı kütüphanesi yazılıp entegre edilmiştir. Gerekirse bu kütüphaneler (bazen modüller olarak da adlandırılır) derlenir ve Nmap ile birlikte kurulur. Yapılandırılmış Nmap veri dizinine yüklenen kendi dizinleri nselib vardır. NSE Kütüphaneleri Şekil 7.6’da gösterilmiştir.

| ![atgr1]({{ site.url }}/assets/img/Nmap/part5/sekil76.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 7.6 : NSE Kütüphaneleri* |




