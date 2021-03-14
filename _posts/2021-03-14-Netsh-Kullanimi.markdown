---
title:  "Netsh Nedir?"
date:   2021-03-14
layout: post
categories: 
    - network-guvenligi
thumbnail: posts/netsh.png
---

## Netsh Nedir?

**Network Shell (netsh), çalışan bir bilgisayarın ağ yapılandırmasını görüntülemeye veya değiştirmeye olanak sağlayan bir komut satırı yardımcı programıdır. Uzak bağlantılı bilgisayarlar veya yerel bilgisayar netsh komutları kullanılarak yapılandırılabilir. Netsh ile arşivleme veya bir bilgisayarı yapılandırmaya yardımcı olmak amacıyla bir metin dosyasına yapılandırma scriptleri kaydedilebilir. Netsh ayrıca, bir grup komutunun batch modda çalıştırılmasına izin veren scripting özelliği sağlamaktadır.**

Netsh komutları, Microsoft Management Console (MMC) ek bileşenini kullanım işlevselliğini sağlamaktadır. NPS MMC bileşenini veya **netsh nps** komutu kullanılarak **Network Policy Server (NPS)** yapılandırılabilir. Ayrıca, IPv6, Network Bridge ve Remote Procedure Call (RPC) gibi ağ teknolojileri için Windows Server’da MMC bileşeni olarak bulunmayan netsh komutları bulunmaktadır.

Netsh, DLL dosyalarını kullanarak diğer işletim sistemi bileşenleriyle etkileşim kurabilir. Her Netsh Helper DLL’i, bir ağ sunucusu rolüne veya özelliğine özgü bir komut grubu olan, bağlam (context) adı verilen kapsamlı bir özellik kümesi sağlamaktadır. Bu bağlamlar, bir veya birden fazla servis, yardımcı program veya protokol için yapılandırma ve izleme desteği sağlayarak netsh işlevlerini arttırmaktadır. Örneğin, **Dhcpmon.dll dosyası**, netsh’a DHCP sunucularını yapılandırma ve yönetme için gerekli olan komutları ve içeriği sağlar.

### Netsh Komutunun Kullanımı

Netsh komutunu, Windows Powershell ortamında çalıştırarak Netsh komut satırına girilmektedir. Netsh komutunun kullanımı Resim1’de gösterilmektedir.


| ![atgr1]({{ site.url }}/assets/img/Netsh/resim1.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim1: Netsh komutunun kullanımı* |

<br/>

Resim1’de, **netsh** komut satırında **/?** komutu kullanılarak netsh içerisindeki bağlamlar gösterilir. Netsh bağlamları, hem komutları hem de alt bağlamlar adı verilen ek bağlamları içermektedir. Bağlamların hakkında bilgi ve detayların görüntülenmesi Resim2’de gösterilmektedir.

| ![atgr2]({{ site.url }}/assets/img/Netsh/resim2.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim2: Bir bağlamın kullanım talimatları* |

<br/>

Resim2’de, Netsh komut satırında bulunan bir bağlamın nasıl kullanılacağı ve onun altındaki alt bağlamların ne olduğu **?, /?** veya **help** komutu kullanılarak öğrenilebilir. Kullanılabilir bağlamlar, yüklenilen ağ bileşenlerine bağlıdır. 

| ![atgr3]({{ site.url }}/assets/img/Netsh/resim3.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim3: Routing bağlamının yüklü olmaması* |

<br/>

Netsh komut satırında, Resim3’teki görünüm oluşuyorsa, routing bağlamını yapacak olan ağ bileşeninin bilgisayarda tanımlı olmamasında kaynaklanmaktadır.

Netsh komut satırında, toplu netsh komut dosyasında veya bir script dosyasında netsh komutunun doğru yorumlanması için sözdizimi kurallarının bilinmesi gerekir. Bir satır komutun içerisinde italik olarak yazılan değer, kullanıcıdan girilmesi gereken değeri ifade etmektedir. Örneğin, **-u UserName** olarak ifade edilirken, “UserName” yerine bir kullanıcı adının girileceği anlamına gelmektedir. 
Kalın olarak belirtilen değer, olduğu gibi yazılması istenen değerdir. Metinden ve ardından gelen üç nokta (…), komut satırında birkaç kez tekrarlanabilen bir parametre olduğunu gösterir. Köşeli ayraçlar [] arasındaki metin isteğe bağlı bir öğedir. Süslü parantez {} arasındaki metinler yalnızca **{ enable | disable }** birinin seçilmesi gerektiğini göstermektedir. Courier yazı tipi ile biçimlendirilmiş metin kod veya program çıktısı anlamına gelmektedir.

Command Prompt ve Windows Powershell ekranında netsh komutunun parametre kullanılmadan çalıştırılması **Netsh.exe** dosyasının çalıştırılması anlamına gelmektedir. Resim4’te netsh komutunun kullanım biçimi verilmiştir.

| ![atgr4]({{ site.url }}/assets/img/Netsh/resim4.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim4: Netsh Komutunun kullanım biçimi* |

<br/>

Resim4’te netsh komutunun doğru kullanımı için söz dizimi gösterilmektedir. **netsh -h** komutu kullanılarak, netsh komutu hakkında bilgi ve kullanımı hakkında bilgi alınabilir.

**-a** parametresi isteğe bağlı bir parametredir. Birden fazla netsh komutunun içerisinde bulunduğu scriptlerin çalıştırılmasından sonra netsh komut satırına döndürüleceğini belirtir. AliasFile, birden fazla netsh komutunu içeren scriptler olarak ifade edilmektedir. **–c** parametresi, netsh bağlamlarının belirtilmesi için kullanılır. Kullanılacak bağlam **–c** parametresine atanır.

**-r** parametresi yapılandırılmak istenen uzak makineyi belirtmektedir. Netsh **–r** parametresi ile uzak bir makinede netsh komutlarının kullanması durumunda uzak makinede **Remote Registry Service** servisinin çalışması gerekir. Remote Registry Service çalışmıyorsa, Windows ***Network Path Not Found** hatası verir.

**-u** parametresi, netsh komutunun hangi kullanıcı adı altında çalıştırılacağını belirtir. Etki alanında kullanılan kullanıcı adı **Etki_Alan_Adı\Kullanıcı_Adı** olarak belirtilmelidir. Etki alanı belirtilmediği takdirde, yerel etki alanı varsayılan olarak kabul edilir. **-p** parametresi ise, kullanılmak istenen hesabın parolasını belirtir. **-f** parametresi, ScriptFile olarak belirtilen komut dosyasını ve komut dosyası çalıştırıldıktan sonra netsh komut satırından çıkacağını belirtir. **/?** ise, netsh komut satırında yardım talimatlarını görüntülemek için kullanılır.

**-r** parametresi başka bir komut ile birlikte belirtildiğinde, netsh komutu uzak makinede çalıştırılır ve cmd.exe komut satırına döndürülür. **-r** parametresinin başka bir komut ile belirtilmemesi durumunda ise, netsh uzak modda çalışır. Yapılan işlem, Netsh komut satırında makine oluşturmaya benzemektedir. **-r** parametresi kullanıldığında, hedef makine yalnızca geçerli netsh örneği için ayarlanır. Netsh’tan çıkıp yeniden girdikten sonra, hedef makine yerel makine olarak sıfırlanır. **WINS**’de depolanan bir makine adı, bir **UNC adı**, DNS sunucusu tarafından çözümlenen bir internet adresi ya da bir IP adresi belirtilerek uzak bir makinede Netsh komutları çalıştırılabilir. 

Netsh komutları için kullanılan parametrelere bir string değer atanması belirli bir kurala göre uygulanmalıdır. Bir string değerin, karakterler arasında, birden çok kelimeden oluşan string değerleri gibi boşluk içermesi durumunda, string değeri tırnak içerisinde belirtilerek atanmalıdır. Örneğin, Wireless Network Connection olarak adlandırılan string değeri, interface adlı bir parametreye atanması **interface=Wireless Network Connection** olarak yapılır.

Netsh komutları kullanılarak batch dosyası oluşturulabilir. Örneğin, bilgisayar IP adresinin ve DNS bilgilerinin statik olarak değiştirilmesi üzerinde bir batch dosyası oluşturulabilir. Resim5’te bir bilgisayara statik IP adresi ve DNS ayarının tanımlanması için batch dosyayı oluşturulmuştur.

| ![atgr5]({{ site.url }}/assets/img/Netsh/resim5.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim5: IP adresini ve DNS bilgilerini statik oluşturan batch dosyası* |

<br/>

Resim5’te, **static.bat** adlı batch dosyası oluşturulmuştur. **Batch** dosyasının içeriğinde, öncelikle IP adresinin tanımlanacağı arabirimin belirtilmesi gerekmektedir. Bundan dolayı, interfaceName parametresine Ethernet0 arabiriminin adı atanmıştır. ``netsh interface ipv4 set address name=”%interfaceName%” source=static address=192.168.40.128 mask=255.255.255.0 gateway=192.168.40.1`` netsh komutu, bilgisayara statik olarak IP adresini atamaktadır. Atanan IP adresinin arabirimi, ipv4 standardı, statik olarak atandığı, IP adresi, subnetmask ve gateway değerleri belirtilmiştir. ``netsh interfaceipv4 set dnsservers name=”%interfaceName%” source=static address=8.8.8.8 registry=primary validate=no`` netsh komutu, bilgisayara DNS sunucusunun bilgileri statik olarak tanımlanmıştır.

Batch dosyası oluşturulduktan sonra çalıştırılacak sistemin Ethernet0 arabirimindeki IP bilgileri Resim6’da gösterilmektedir.

| ![atgr6]({{ site.url }}/assets/img/Netsh/resim6.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim6: Bir bilgisayardaki dinamik IP ve DNS bilgilerinin gösterilmesi* |

<br/>

Resim6’da, **ipconfig /all** komutu kullanılarak, Ethernet0 arabirimindeki IP ve DNS bilgilerinin dinamik olarak atandığı gösterilmektedir. **DHCP Enabled** parametresi değerinin **Yes** olması IP ve DNS bilgilerinin dinamik olarak atandığı anlamına gelmektedir. DHCP sunucusunun Default Gateway, DNS Servers ve **Primary WINS Server** IP adresini 192.168.40.2 olarak atadığı gösterilmektedir. Resim7’de ise oluşturulan batch dosyasının çalıştırılmasıyla IP ve DNS sunucusunun bilgileri statik olarak atanmaktadır.

| ![atgr7]({{ site.url }}/assets/img/Netsh/resim7.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim7: Dinamik IP ve DNS bilgilerinin statik olarak değiştirilmesi* |

<br/>

Resim7’de **static.bat** adlı batch dosyası çalıştırılarak bilgisayardaki IP ve DNS bilgileri statik olarak değiştirilmiştir. Bilgilerin değiştirildiğini doğrulamak için **ipconfig /all** komutu çalıştırılır. Böylece IP adresinin statik olarak değiştirildiği **DHCP Enabled** parametre değerinin **No** olmasından anlaşılmaktadır. Ayrıca, **static.bat** dosyasında atanan IP ve DNS bilgilerinin başarılı bir şekilde atandığı gösterilmektedir.










