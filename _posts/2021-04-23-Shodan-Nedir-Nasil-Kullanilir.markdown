---
title:  "Shodan Nedir? Nasıl Kullanılır?"
date:   2021-04-23
layout: post
categories: 
    - siber-guvenlik
thumbnail: posts/shodan.png
---

## Shodan Nedir?

**Shodan, arama motorlarından (Google, Bing, Yahoo vb.) farklı olarak çeşitli filtreler kullanıp internete açık olan bütün sistemleri tarayan ve sistemler hakkında bilgi elde eden arama motorudur. Sunucular, yönlendiriciler ve web kameraları gibi internete açık sistemler örnek olarak verilebilir. Shodan, tespit ettiği sistemlerin port taramasını yapar, açık olan portlarda çalışan servisleri ve servislerin sürümlerini tespit eder. Tespit edilen servis sürümleri ile ilgili daha önce çıkmış herhangi bir zafiyet varsa, zafiyetin CVE kodu ile zafiyet hakkında kısa bir açıklama vermektedir. Ayrıca, Shodan web servisleri üzerinde çalışan web sayfalarına yönelik taramalar yaparken elindeki dizin bilgilerini kullanarak tarama kapsamını genişletebilir.**


| ![atgr1]({{ site.url }}/assets/img/Shodan/resim1.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim1: Shodan Ana sayfası* |

<br/>

Shodan uygulamasını kullanmak isteyen her kulllanıcı **https://shodan.io** adresi üzerinden erişim sağlayabilir. Erişim sağlandıktan ve gerekli kayıt işlemleri yapıldıktan sonra pasif keşif işlemleri yapılabilir.

| ![atgr2]({{ site.url }}/assets/img/Shodan/resim2.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim2: Shodan kayıt ve giriş sayfası* |

<br/>

**Shodan**, kullanıcıların daha kolay ve verimli pasif keşif yapması için belirli bir **cheatsheet** oluşturmuştur. Oluşturulan cheatsheet’e göre shodan tarafından tanımlanan parametreler kullanılarak pasif keşif işlemleri yapılabilir. Belirtilen cheatsheet’teki temel parametrelerin kullanımı aşağıda gösterilmektedir.


### Shodan Temel Parametreleri

**Server:** HTTP Request header’ındaki **Server header bilgisi** olarak ifade edilebilir. Bu parametre’ye herhangi bir sunucu bilgisi verilerek o sunucu hakkında pasif keşif yapılabilir. Örnek olarak zafiyet barındıran bir server bilgisi girilerek pasif keşif yapılabilir. Resim3’te, **server** parametresi kullanılarak yapılan bir pasif keşif  çalışması gösterilmektedir.

| ![atgr3]({{ site.url }}/assets/img/Shodan/resim3.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim3: server parametresinin kullanımı* |

<br/>

Resim3’te, **server: apache 2.2.3** komutu kullanılarak yapılan aramada, toplamda **935316 adet 2.2.3** sürümlü **Apache** web sunucu hizmeti kullanan sunucu tespit edilmiştir. Bu sunucuların, ülkelere göre, kullanılan servislere göre, hizmet sağlayıcılara göre ve ürünlere göre dağılımı verilmiştir. Ayrıca, tespit edilen sistemlerin **IP bilgisi, domain bilgisi, ülke ve şehir bilgisi, kullanılan web teknolojilerinin bilgisi ve elde edilen HTTP Response içeriği** gösterilmektedir. HTTP Response içeriğinde HTTP Headerları arasındaki **Server bilgisi** gösterilmektedir.


**Hostname:** Bu parametreye atanan sözcük domain veya subdomain söz dizimi içeriside arama yapmaktadır. Belirtilen sözcük domain içerisinde veya subdomain içerisinde yer alıyorsa ekrana yansılıtır. Örnek olarak, **hostname:google.com**, **hostname:ftp** ya da **hostname:login**  gibi atamalar yapılabilir. Resim4’te hostname parametresinin kullanımı gösterilmektedir.

| ![atgr4]({{ site.url }}/assets/img/Shodan/resim4.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim4: hostname parametresinin kullanımı* |

<br/>

Resim4’te, **hostname:download** komutu kullanılarak domain ya da subdomain içerisinde download geçen **4473** adet hostname’in olduğu tespit edilmiştir.

**Net:** Bir IP adresine veya **CIDR** (/24 gibi) değerine göre internete açık sistemler tespit edilmektedir. Bu parametre IP aralığını, IP adresini ve alt ağ maskesini bulmak için de kullanılabilir. Örneğin, **net:36.92.0.0/16** CIDR değerine sahip IP bloğuna yönelik pasif keşif taraması yapılabilir. Resim5’te örnek gösterilmiştir.

| ![atgr5]({{ site.url }}/assets/img/Shodan/resim5.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim5: net parametresinin kullanımı* |

<br/>

Resim5’te, **net:36.92.0.0/16** CIDR değerine göre yapılan arama sonucunda **24559** adet sistem tespit edilmiştir. Net parametresi, diğer parametreler ile kullanılarak pasif keşif çalışmasının kapsamı daraltırabilir.

**Os:** İşletim sistemlerine göre hedefi filtrelemeye yarayan bir parametredir. **os** parametresi kullanılarak internete açık olan savunmasız işletim sistemleri tespit edilebilir. Örneğin, Microsoft’un desteğini çektiği işletim sistemlerinden biri olan, Windows XP işletim sistemine yönelik bir pasif keşif yapmak için **os:Windows XP** filtresi kullanılabilir. Resim6’da örnek kullanımı gösterilmektedir.


| ![atgr6]({{ site.url }}/assets/img/Shodan/resim6.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim6: os parametresinin kullanımı* |

<br/>

Resim6’da, **os** parametresi kullanılarak internete açık Windows 7 işletim sistemini kullanan **45511** adet cihaz tespit edilmiştir. **Windows 7** işletim sisteminin savunmasız olduğu güvenlik açıkları kullanılarak hedef sistemler ele geçirilebilir. Resim6’da elde edilen sonuçlar arasında SMB servisi hakkında bilgi verilmektedir. Elde edilen SMB servisinin sürüm bilgisi doğrultusunda zafiyet araştırması yapılabilir.

**Port:** Sistemlerin açık bağlantı noktalarını tespit etmek için kullanılan bir parametredir. Spesifik olarak yapılan bir pasif keşif çalışmasında kullanılabilir. Ayrıca, bir hedefe yönelik yapılan bir pasif keşif çalışmasının kapsamını daraltmak için kullanılabilir. Örneğin, servislerin kullanmış oldukları varsayılan portlar bulunmaktadır. Bunlardan bir tanesi SMB servisinin kullandığı **445** numaralı porttur. Resim7’de örnek olarak gösterilmektedir.

| ![atgr7]({{ site.url }}/assets/img/Shodan/resim7.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim7: port parametresinin kullanımı* |

<br/>

Resim7’de yapılan pasif keşif çalışmasında, **445** numaralı portu açık olan yaklaşık **1.5 milyon** sistem tespit edilmiştir. Tespit edilen sistemler arasında, kimlik doğrulama mekanizmasının **pasif** olduğu ve **doğrudan sisteme erişim** sağlayarak dizinlerin listelendiği gösterilmektedir. Saldırganlar, bu tarz pasif keşif çalışmalarıyla sistemlere sızıp kritik bilgiler elde edebilirler.

**Org:** Spesifik olarak bir kuruluş hedef alındığında kuruluşa ait cihazların tespit edilmesi için **org** parametresine kuruluşun bilgisi atanır. Böylece, hedeflenen kuruluşa yönelik bir pasif keşif çalışması yapılabilir. Resim8’de, org parametresinin kullanımı gösterilmektedir.

| ![atgr8]({{ site.url }}/assets/img/Shodan/resim8.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim8: org parametresinin kullanımı* |

<br/>

Resim8’de, **org:Microsoft** komutunun kullanılmasıyla yapılan pasif keşif çalışmasında yaklaşık **5 milyon** sonuç tespit edilmiştir. Diğer parametrelerle kullanılarak kapsamın daraltılması sonucunda hedefe yönelik  daha spesifik bilgiler elde edilebilir. Elde edilen bilgiler doğrultusunda kuruluşu ele geçirmeye yönelik senaryolar oluşturulabilir.

**City:** Lokasyon olarak belirli bir kapsamdaki sistemlerin tespit edilmesi için kullanılan bir parametredir. Genellikle siber saldırılarda kullanılabilecek bir pasif keşif çalışması olarak gösterilebilir. Resim9’da, **city** parametresinin kullanımı gösterilmektedir.


| ![atgr9]({{ site.url }}/assets/img/Shodan/resim9.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim9: city parametresinin kullanımı* |

<br/>

Resim9’da, **city** parametresi kullanılarak **Miami** şehrinde **1.5 milyondan fazla** sistemin internete açık olduğu tespit edilmiştir. City parametresi, diğer parametreler ile kullanılarak pasif keşif çalışmasının kapsamı daraltılabilir. 

**Country:** Lokasyon olarak belirli bir ülkede internete açık olan cihazların tespit edilmesi için kullanılır. Genellikle, ülkeler arasındaki siber saldırılarda kapsamın daraltılması ve belirlenmesi için kullanılan bir filtredir. **Country** parametresinin kullanımı ile ilgili örnek Resim10’da yer almaktadır.


| ![atgr10]({{ site.url }}/assets/img/Shodan/resim10.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim10: Country parametresinin kullanımı* |

<br/>

Resim10’da, **country** parametresi kullanılarak **ÇİN**’deki yaklaşık **33.5 milyon** sistemin internete açık olduğu tespit edilmiştir. Country parametresi, diğer parametreler ile kullanılarak pasif keşif çalışmasının kapsamı daraltılabilir. 

**Geo:** Coğrafik olarak verilen lokasyon bilgisi **(enlem, boylam)** doğrultusunda, hedeflenen bölgedeki internete açık olan sistemlerin tespiti için kullanılan bir parametredir. Resim11’de, geo parametresinin kullanımı gösterilmektedir.


| ![atgr11]({{ site.url }}/assets/img/Shodan/resim11.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim11: geo parametresinin kullanımı* |

<br/>

Resim11’de, belirli bir lokasyonun koordinat bilgileri kullanılarak, **456** adet sistemin internete açık olduğu tespit edilmiştir. Resim11’de gösterilen sistemlerden bir tanesinin güvenlik kameraları yönetim paneli olduğu görülmektedir. 

**Before/after:** Belirli tarihler arasında internete açık olan sistemleri göstermek için kullanılan parametrelerdir. Böylece, spesifik olarak sistemlere yönelik yapılan pasif keşiflerde kullanılabilir. Örnek kullanımı, Resim12’de gösterilmektedir.

| ![atgr12]({{ site.url }}/assets/img/Shodan/resim12.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim12: before/after parametrelerinin kullanımı* |

<br/>

Resim12’de, **before/after** parametrelerinin sadece API üzerinden kullanıbileceği belirtilmektedir.

**Has_screenshot:** Sadece ekran görüntüsü olan sonuçları görüntülemek için kullanılır. Genellikle **uzak masaüstü erişimi** açık olan sistemlerde çalışmaktadır. Resim13’te, has_screenshot kullanımı gösterilmektedir.

| ![atgr13]({{ site.url }}/assets/img/Shodan/resim13.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim13: has_screenshot parametresinin kullanımı* |

<br/>

Resim13’te, **Miami** şehrinde screenshot alınabilen **5288** adet sistemin olduğu tespit edilmiştir. Ayrıca ekran görüntüsü alınan sistemin RDP protokolünün açık olduğu sonucuna varılabilir.

**title:** Cihazların baslıklarında yazan bilgi kullanılarak filtreleme yapılabilir. Örneğin ürün adı, modeli belirtilerek filtreleme yapılabilir. Resim14’te, **title** parametresi ile ilgili örnek verilmektedir.

| ![atgr14]({{ site.url }}/assets/img/Shodan/resim14.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim14: title parametresinin kullanımı* |

<br/>

Resim14’te, **title** parametresi kullanılarak dünyadaki internet erişimine açık **45716 tane Citrix Gateway** cihazı tespit edildi. Herhangi bir ürün ile ilgili zafiyet çıktığında, **Shodan** kullanılarak internete açık olan zafiyetli ürün tespit edilebilir. Tespit edildikten sonra iyi niyetli kişiler tarafından raporlanabilir veya kötü niyetli kişiler tarafından saldırılara maruz kalabilir.


### Shodan ile Yapılan Bazı Pasif Keşifler

**Yanlış Yapılandırılmış WordPress Siteleri:** wp-config.php dosyasına olan erişimin kısıtlanmadığı sistelerin tespit edimesi sonucunda, dosya içerisindeki **veritabanı bağlantı bilgileri** elde edilebilir. Resim15’te, örnek pasif keşif çalışması gösterilmektedir

| ![atgr15]({{ site.url }}/assets/img/Shodan/resim15.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim15: Yanlış yapılandırılmış WordPress Siteleri* |

<br/>

Resim15’te, **12 adet yanlış yapılandırılmış WordPress sitesi** tespit edilmiştir. Resim15’teki sitelerden bir tanesindeki kritik bilgilerin yetkisiz erişime açık olduğu Resim16’da gösterilmektedir.

| ![atgr16]({{ site.url }}/assets/img/Shodan/resim16.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim16: wp-config.php sayfası* |

<br/>

Resim16’da veritabanına bağlantı sağlamak için kullanılan **kullanıcı adı ve parola bilgileri** bulunmaktadır. Bu durum, WordPress sitelerinin yanlış yapılandırılmasından kaynaklanmaktadır.

**FTP Anonim Erişimine Açık Sistemler:** Shodan ile yapılan pasif keşifler sonucunda **FTP** bağlantı noktasının **anonim erişime** açık olduğu sistemler tespit edilebilir. Tespit edilen sistemlerde, anonim erişim ile FTP bağlantı noktası üzerinden dosya transferi yapılabilir. Resim17’de FTP Anonim erişimi ile ilgili örnek gösterilmektedir.

| ![atgr17]({{ site.url }}/assets/img/Shodan/resim17.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim17: FTP Anonim Erişim Sonuçları* |

<br/>

Resim17’de, yapılan spesifik pasif keşif sonucunda, dünyada **100751** adet sistemde **FTP** Anonim erişiminin olduğu tespit edilmiştir. Bu durum, FTP protokolünün varsayılan olarak kurulmasıdan ve herhangi bir konfigürasyon değişikliğinin yapılmamasından kaynaklanmaktadır.






