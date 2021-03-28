---
title:  "Netsh HTTP Komutları Kullanımı"
date:   2021-03-28
layout: post
categories: 
    - network-guvenligi
thumbnail: posts/netshhttp.png
---

## Netsh Nedir?

**Network Shell (netsh), çalışan bir bilgisayarın ağ yapılandırmasını görüntülemeye veya değiştirmeye olanak sağlayan bir komut satırı yardımcı programıdır. Uzak bağlantılı bilgisayarlar veya yerel bilgisayar netsh komutları kullanılarak yapılandırılabilir. Netsh ile arşivleme veya bir bilgisayarı yapılandırmaya yardımcı olmak amacıyla bir metin dosyasına yapılandırma scriptleri kaydedilebilir. Netsh ayrıca, bir grup komutunun batch modda çalıştırılmasına izin veren scripting özelliği sağlamaktadır.**

Netsh komutları, Microsoft Management Console (MMC) ek bileşenini kullanım işlevselliğini sağlamaktadır. NPS MMC bileşenini veya **netsh nps** komutu kullanılarak **Network Policy Server (NPS)** yapılandırılabilir. Ayrıca, IPv6, Network Bridge ve Remote Procedure Call (RPC) gibi ağ teknolojileri için Windows Server’da MMC bileşeni olarak bulunmayan netsh komutları bulunmaktadır.

Netsh, DLL dosyalarını kullanarak diğer işletim sistemi bileşenleriyle etkileşim kurabilir. Her Netsh Helper DLL’i, bir ağ sunucusu rolüne veya özelliğine özgü bir komut grubu olan, bağlam (context) adı verilen kapsamlı bir özellik kümesi sağlamaktadır. Bu bağlamlar, bir veya birden fazla servis, yardımcı program veya protokol için yapılandırma ve izleme desteği sağlayarak netsh işlevlerini arttırmaktadır. Örneğin, **Dhcpmon.dll dosyası**, netsh’a DHCP sunucularını yapılandırma ve yönetme için gerekli olan komutları ve içeriği sağlar.

### Netsh HTTP Komutları

HTTP.sys ayarlarını yapılandırmak ve parametrelerini sorgulamak için netsh http komutu kullanılabilir. Netsh komut satırından HTTP’ye yönelik yapılandırmaları yapmak için bazı komutlar ve parametreler kullanılmaktadır. Resim1’de netsh http komutu kullanılmıştır.



| ![atgr1]({{ site.url }}/assets/img/NetshHTTP/resim1.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim1: netsh http komutunun kullanılması* |

<br/>

Resim1’de netsh http komutu kullanılarak HTTP.sys üzerindeki yapılandırmalar için kullanılabilir komutlar gösterilmektedir. 

**add iplisten :** iplisten listesine port numarası belirtilmeden sadece yeni bir IP adresi eklemeyi sağlamaktadır. Resim2’da IP adresinin iplisten listesine nasıl eklendiği gösterilmektedir.

| ![atgr2]({{ site.url }}/assets/img/NetshHTTP/resim2.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim2: iplisten listesine IP adresi eklemek* |

<br/>

Resi2’de, **netsh http add ?** komutu kullanılarak HTTP ile ilgili nelerin eklenebileceği ve nasıl kullanılacağı gösterilmektedir. Netsh üzerinden iplisten listesine IP adresi ekleneceği için, öncelikle iplisten parametresi kullanımının bilinmesi gerekir. **netsh http add iplisten** komutu kullanılarak veya komutu sonuna soru işareti koyarak kullanım şekli öğrenilebilir. Resim2’de, iplisten kullanım şekli, kullanım koşulları ve örnek kullanımı gösterilmektedir. **netsh http add iplisten ipaddress=8.8.8.8** komutu kullanılarak iplisten listesine 8.8.8.8 IP adresinin başarılı bir şekilde eklendiği gösterilmektedir.

**show iplisten :** İplisten listesinin içerisindeki IP adreslerinin görüntülenmesi için kullanılmaktadır. İplisten listesi, HTTP servisinin bağlandığı IP adreslerini listede tutmaktadır. **0.0.0.0** herhangi bir IPv4 IP adresi ve **::** herhangi bir IPv6 IP adresini anlamına gelmektedir. Resim3’te show iplisten komutunun kullanımı gösterilmektedir.


| ![atgr3]({{ site.url }}/assets/img/NetshHTTP/resim3.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim3: show iplisten komutunun kullanımı* |

<br/>

Resim3’te **netsh http show iplisten** komutu kullanılarak iplisten listesi görüntülenmiştir. Böylece, iplisten listesine eklenen 8.8.8.8 IP adresinin başarılı bir şekilde eklendiği doğrulanmaktadır.

**delete iplisten :** İplisten listesindeki belirtilen IP adresinin silinmesi için kullanılır. Resim4'te **delete iplisten** komutu kullanılmaktadır.


| ![atgr4]({{ site.url }}/assets/img/NetshHTTP/resim4.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim4: delete iplisten komutunun kullanımı* |

<br/>

Resim4'te iplisten listesine eklenen 8.8.8.8 IP adresi, **netsh http delete iplisten ipaddress=8.8.8.8** komutu kullanılarak silinmektedir. **netsh http show iplisten** komutu kullanılarak 8.8.8.8 IP adresinin silindiği doğrulanmaktadır.

**add sslcert :** Belirtilen IP adresi ve port numarası için SSL sunucu sertifikası eklemek için kullanılan netsh komutudur. Ayrıca eşleşen istemci sertifika politikalarını eklenebilir. Resim 5'te komutun nasıl çalıştırıldığı gösterilmektedir.


| ![atgr5]({{ site.url }}/assets/img/NetshHTTP/resim5.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim5: add sslcert komutunun kullanımı* |

<br/>

Resim5'te netsh komut satırında **http add sslcert** komutu çalıştırılarak komutun kullanımı hakkında detaylı bilgiler gösterilmektedir. Komut kullanılırken atanması zorunlu tutulan 3 parametre **ipport, certhash ve appid** parametreleridir. **ipport** parametresine, binding yapılacak IP adresi ve port numarası atanır. **certhash** parametresine, 20 bayt uzunluğun bir SHA hash değeri hexadecimal olarak atanır.  **appid** parametresine ise, sertifikayı kullanan uygulamanın GUID değeri atanır. Resim6'da örnek kullanımı gösterilmektedir.

| ![atgr6]({{ site.url }}/assets/img/NetshHTTP/resim6.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim6: add sslcert komutu kullanımının örnek gösterimi* |

<br/>

Resim6'da 3 farklı örnek kullanımı gösterilmektedir. Belirtilen ipport, certhash ve appid parametreleri dışındaki parametreler de isteğe bağlı olarak kullanılabilir.

**show sslcert :** IP adresi ve port numarası için eklenmiş olan SSL sertifikaları **show sslcert** komutu kullanılarak görüntülenebilir. **http show sslcert** komutu kullanılırken eklenen bütün SSL sertifikalarına ait bilgiler görüntülenmektedir. Spesifik olarak görüntülenmek istenen SSL sertifikaları için netsh komut satırında **http show sslcert ipport=IP_adresi:Port_numarası** örnek gösterimi ile gösterilebilir.

**delete sslcert :** Spesifik bir IP adresi ve port numarası için eklenmiş olan sertifikaların silinmesi için netsh komut satırında **http delete sslcert ipport=IP_adresi:Port_numarası** komutu çalıştırılabilir.

**add timeout :** HTTP servisine genel bir zaman aşımı süresi eklenmesi için netsh komut  satırında **add timeout** komutu kullanılır. Komutun kullanımında zaman aşımı türü (timeouttype) ve zaman aşımı süresi (value) belirtilmelidir. Zaman aşımı süresi, saniye cinsinden belirtilir. Resim 7’de örnek kullanımı gösterilmektedir.


| ![atgr7]({{ site.url }}/assets/img/NetshHTTP/resim7.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim7: add timeout komutu kullanımının örnek gösterimi* |

<br/>

Resim7'de **timeouttype** parametresine **idlecoonectiontimeout / headerwaittimeout** değerlerinden **idleconnectiontimeout** değeri atanmıştır. **value** parametresine ise, timeout süresi olarak **120** atanmıştır. Timeout süresi hexadecimal olarak da atanabilir. Ancak, atanan değerin başına **0x** öneki eklenmelidir.

**show timeout :** HTTP servisi için eklenen zaman aşımı süresini saniye cinsinden görüntülemek için netsh komut satırında “http show timeout” komutu kullanılabilir. Örnek gösterimi Resim 8'de gösterilmektedir.


| ![atgr8]({{ site.url }}/assets/img/NetshHTTP/resim8.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim8: show timeout komutu kullanımının örnek gösterimi* |

<br/>

Resim 8’de HTTP servisine atanan zaman aşımı süresinin 120 saniye olduğu gösterilmektedir.

**delete timeout :** HTTP servisine atanan zaman aşımı süresini silmek ve HTTP servisinin zaman aşımı süresini varsayılana çevirmek için netsh komut satırında **http delete timeout** komutu kullanılabilir. Örnek kullanımı Resim 9’da gösterilmektedir.

| ![atgr9]({{ site.url }}/assets/img/NetshHTTP/resim9.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim9: delete timeout komutu kullanımının örnek gösterimi* |

<br/>

Resim 9’da netsh komut satırında **http delete timeout timeouttype=idleconnectiontimeout** komutu kullanılarak HTTP servisi için eklenen zaman aşımı süresi silinebilir. Silme işleminden sonra zaman aşımı süresilerini görüntülemek için netsh komut satırında **http show timeout** komutu kullanılarak HTTP servisi zaman aşımı süresi görüntülenebilir. Ayrıca, HTTP servisi zaman aşımı süresinin varsayılan olarak 120 saniye olduğu bilgisine saptanmıştır.

**add urlacl :** Ayrılmış URL eklemek için kullanılır. Yetkisiz kullanıcı hesapları için ayrılmış URL ekleme işlemi yapılabilir. Ayrılmış URL eklemek için URL adresi ve kullanıcı hesabı gerekmektedir. Resim 10'da kullanım koşulları ve örnek kullanımı gösterilmektedir.

| ![atgr10]({{ site.url }}/assets/img/NetshHTTP/resim10.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim10: add urlacl komutu kullanımının örnek gösterimi* |

<br/>

Resim 10'da netsh komut satırında **http add urlacl** komutu kullanılarak komutun kullanımı hakkında bilgilendirmeler gösterilmektedir. Resim10'da yetkisiz bir kullanıcı için ayrılmış bir URL eklendiği gösterilmektedir.

**show urlacl :** Ayrılmış URL’leri ve tüm URL için DACL’leri listelemek için netsh komut satırında **http show urlacl** komutu kullanılabilir.

| ![atgr11]({{ site.url }}/assets/img/NetshHTTP/resim11.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim11: show urlacl komutu kullanımının örnek gösterimi* |

<br/>

Resim 11’de eklemiş olan ayrılmış URL gösterilmektedir. Ayrıca diğer ayrılmış URL’ler ve DACL’ler listelenir. 

**delete urlacl :** Ayrılmış URL’leri silmek için netsh komut satırında **http delete urlacl url=URL_bilgisi** komutu çalıştırılabilir. Resim 19’da eklenen ayrılmış URL silme işlemi gösterilmektedir.

| ![atgr12]({{ site.url }}/assets/img/NetshHTTP/resim12.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim12: delete urlacl komutu kullanımının örnek gösterimi* |

<br/>

Resim12'de ayrılmış URL’in başarılı bir şekilde silindiği gösterilmektedir. Ayrılmış URL’in silinip silinmediğini kontrol etmek için silme işleminden sonra netsh komut satırında **http show urlacl** komutu çalıştırılabilir.

**show cachestate :** Önbellekte tutulan URL kaynaklarını ve ilişkili özelliklerini görüntülemek için netsh komut satırında **http show cachestate url=URL_Adresi**komutu çalıştırılabilir.

**show servicestate :** HTTP servisinin snapshot’ını görüntülemek için kullanılan bir netsh http komutudur. **View** ve **verbose** olarak iki parametre içerir. **View** parametresi, **session** ve **requestq** olarak iki değer alır. Oturum bilgilerini almak için **session** değeri, HTTP istek kuyruklarını görüntülemek için ise **requestq** değeri **View** parametresine atanır. Örnek kullanımı Resim 13’te gösterilmektedir.

| ![atgr13]({{ site.url }}/assets/img/NetshHTTP/resim13.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim13: show servicestate komutu kullanımının örnek gösterimi* |

<br/>

Resim 13'te, HTTP servisindeki oturum bilgisi hakkında bilgileri görüntülemek için netsh komut satırında **http show servicestate view=”session”** komut çalıştırılmıştır.

**flush logbuffer :** Log dosyaları için iç arabellekleri temizlemede kullanılan netsh http komutudur. Resim 14'te örnek kullanımı gösterilmektedir.

| ![atgr14]({{ site.url }}/assets/img/NetshHTTP/resim14.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim14: flush logbuffer komutu kullanımının örnek gösterimi* |

<br/>

**delete cache :** HTTP servis çekirdeği URI ön belleğindeki spesifik bir girdiyi veya bütün girdileri silmek için kullanılan bir netsh http komutudur. Netsh komut satırında **http delete cache url=URL_adresi recursive=yes** komutu kullanılarak spesifik bir URL ile ilgili bilgiler HTTP servisinin çekirdek URI önbelleğinden silinebilir. Ayrıca, **http delete cache** komutu kullanılarak bütün girdiler silinebilir.







