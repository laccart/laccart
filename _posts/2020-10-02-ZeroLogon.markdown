---
title:  "ZeroLogon Nedir?"
date:   2020-10-02
layout: post
categories: 
    - sistem-guvenligi
thumbnail: posts/zerologon.png
---

## ZeroLogon Nedir?

**Ağustos 2020’de, Microsoft tarafından yaması yayınlanan ve Active Directory ortamındaki Domain Controller makinesini doğrudan etkileyen CVE-2020-1472 kodlu bir güvenlik açığı açıklandı. Zerologon olarak adlandırılan güvenlik açığının CVSS skoru, 10 üzerinden 10 olarak belirtildi. Zerologon güvenlik açığından yararlanan bir saldırgan, Domain Controller makinesine kimlik doğrulama yapmadan erişerek yüksek yetkilere sahip olabilir ve kritik işlemler gerçekleştirebilir.**

**Zerologon güvenlik açığı, rastgele oluşturulan her 256 anahtardan 1’i için null baytlardan oluşan bir düz metnin (plain text) şifrelenmesi sonucunda, null baytlardan oluşan bir şifreli (cipher text) metnin oluşmasına neden olan kriptografik hatadan kaynaklanıyor. Zafiyetli şifreleme protokolü, netlogon protokolündeki kimlik doğrulama mekanizması yerine uygulanır.**

Netlogon Remote (MS-NRPC) protokolü, Active Directory ortamında iş istasyonları ve sunucuların Domain Controller makinesi ile güvenli bir kanal üzerinden iletişim kurmaları için kullanılır. Netlogon, Active Directory’e katılan her iş istasyonu veya sunucunun parolasını bildiği bir bilgisayar hesabı bulundurur. Active Directory, Kerberos ve NTLM gibi kimlik doğrulama protokollerinde kullanılabilen ve aynı paroladan türetilen birkaç anahtara sahiptir.

Zerologon saldırısı, domain ortamında bulunan Domain Controller makinesindeki bilgisayar hesabının Active Directory’deki parolasını, boş string değer olarak atanmasını ve sıfırlanan bilgisayar hesabı kimlik bilgilerinin, Domain Controller makinesi tarafından doğrulanmasını sağlamaktadır.

Domain Controller, kimlik doğrulama işlemini yüksek yetkilerle gerçekleştirir. Çünkü Domain Controller, NT hash değerleri ve Kerberos anahtarları dahil olmak üzere Active Directory verilerini senkronize etmek için DRSUAPI protokolünü kullanabilir. NT hash değerlerinin ve Kerberos anahtarlarının elde edilmesi durumunda, saldırgan domaindeki herhangi bir kullanıcının kimliğine bürünebilir veya sahte kerberos biletleri oluşturabilir.

Domain Controller bilgisayar hesabının parolası Active Directory’de sıfırlandığında Domain Controller yeniden başlatılırsa, çeşitli servisler Active Directory’den bilgi okumak istediklerinde servis başlamaz. Çünkü Domain Controller kayıt defterinde ve lsass.exe dosyasının belleğinde depolanan şifrelenmiş bilgisayar hesabı parolası değiştirilmez. Bu durumun oluşmaması için sızma testlerinde, Zerologon güvenlik açığını bulunduran Domain Controller makineleri ele geçirildikten sonra Active Directory’de parolası sıfırlanan bilgisayar hesabının parola bilgisi düzeltilmelidir.

CVE-2020-1472 kodlu Zerologon güvenlik açığı için yayınlanan exploit modülü, Privia Security ekibi tarafından optimize edilerek ADZero adında bir araç geliştirildi. ADZero aracı, Zerologon güvenlik açığı bulunan Domain Controller makinesinde NT AUTHORITY\SYSTEM yetkileriyle shell oturumunun elde edilmesini sağlamaktadır. Resim1’de, ADZero aracının kulanım talimatları gösterilmektedir.

```linux
root@Stormer:~# python3 ADZero.py -h
```

| ![atgr1]({{ site.url }}/assets/img/ZeroLogon/resim1.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim1: ADZero aracının kullanım talimatları* |

<br/>

Resim1’de, ADZero aracının sadece Domain Controller makinesinin IP adresine ihtiyaç duyduğu gösterilmektedir. Yayınlanan Zerologon exploit modülü kullanıcıdan IP adresi, DC adı ve DC’deki bilgisayar hesabı bilgisini istemektedir. ADZero aracı, aldığı IP adresi ile Domain Controller makinesine bir SMB Login isteğinde bulunmaktadır. SMB Login isteğine dair kodlar, Resim2’de yer almaktadır.

| ![atgr2]({{ site.url }}/assets/img/ZeroLogon/resim2.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim2: SMB Login isteği yapılması* |

<br/>

Resim2’de, yapılan SMB Login isteğine gelen yanıtın içerisinden, Domain Controller makinesinin adı ve domain adı elde edilir. Domain Controller makinesinin adı, IP adresi ve DC makinesi adının sonuna **$** sembolünün eklenmesiyle oluşturulan DC bilgisayar hesabı kullanılarak exploit modülü çalıştırılır. Resim3’te, ADZero aracı 172.16.5.105 IP adresine yönelik çalıştırılmıştır.

```linux
root@Stormer:~# python3 ADZero.py 172.16.5.105
```

| ![atgr3]({{ site.url }}/assets/img/ZeroLogon/resim3.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim3: Zerologon güvenlik açığının başarılı bir şekilde sömürülmesi* |

<br/>

Resim3’te, 172.16.5.105 IP adresine yönelik yapılan Zerologon saldırısının başarılı bir şekilde gerçekleştirildiği gösterilmektedir. Böylece, Domain Controller bilgisayar hesabının parolası boş string değer olarak atanır. (NT hash = **31d6cfe0d16ae931b73c59d7e0c089c0**).

Domain Controller bilgisayar hesabının parolası boş string değer olarak atandıktan sonra, Impacket modüllerinden secretdump.py modülü kullanılarak Administrator kullanıcısının LM:NTLM hash bilgisi out adlı bir dosyaya kaydedilir. Impacket modüllerinden smbexec.py modülü kullanılarak Administrator kullanıcısının LM:NTLM hash bilgisiyle Domain Controller makinesinden shell elde edilir. LM:NTLM hash bilgisi kullanılarak hedef makinede oturum elde etme tekniğine **Pass-the-Hash** denir. Resim4’te shell oturumu gösterilmektedir.

| ![atgr4]({{ site.url }}/assets/img/ZeroLogon/resim4.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim4: ZEROLOGON-DC makinesinden shell oturumunun elde edilmesi* |

<br/>

Resim4’te shell oturumunun NT AUTHORITY\SYSTEM yetkileriyle elde edildiği gösterilmektedir. Ayrıca, ADZero aracı kullanılarak elde edilen out adlı dosya içerisindeki LM:NTLM hash değeri kullanılarak meterpreter oturumu elde edilebilir. Resim5’te out dosyasının içeriği cat komutu kullanılarak görüntülenmiştir.

| ![atgr5]({{ site.url }}/assets/img/ZeroLogon/resim5.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim5: Administrator kullanıcısının LM:NTLM hash bilgisi* |

<br/>

Resim5’te out dosyasının içerisindeki Administrator kullanıcısına ait LM:NTLM hash bilgisi gösterilmektedir. Metasploit Framework aracı içerisindeki psexec exploit modülü kullanılarak Domain Controller makinesinde meterpreter oturumu elde edilebilir. Resim6’da psexec modülünün kullanımı gösterilmektedir.

| ![atgr6]({{ site.url }}/assets/img/ZeroLogon/resim6.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim6: Psexec exploit modülünün kullanılması* |

<br/>

Resim6’da elde edilen LM:NTLM hash bilgisinin, Metasploit Framework içerisindeki **exploit/windows/smb/psexec** exploit modülünde kullanıldığı gösterilmektedir. Exploit modülünün çalıştırılmasıyla meterpreter oturumu elde edilir. Resim7’de, exploit modülünün çalıştırılması ve meterpreter oturumunun elde edilmesi gösterilmektedir.

| ![atgr7]({{ site.url }}/assets/img/ZeroLogon/resim7.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Resim7: Exploit modülünün çalıştırılması ve Meterpreter oturumunun elde edilmesi* |

<br/>

Resim7’de meterpreter oturumunun NT AUTHORITY\SYSTEM yetkileriyle elde edildiği gösterilmektedir. Geliştirilen ADZero aracı, **Privia Security github hesabı**ndan indirilebilir.


