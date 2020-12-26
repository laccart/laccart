---
title:  "Nmap Nedir ve Kurulumu Nasıl Yapılır?"
date:   2020-01-27 
layout: post
categories: 
    - network-guvenligi
thumbnail: posts/nmap.png
---
# Nmap Nedir ve Kurulumu Nasıl Yapılır?

**Nmap, ağ tarama ve zafiyet tespiti için kullanılan açık kaynaklı bir araçtır. Nmap, birçok sisteme yönelik taramaları gerçekleştirerek esnek, hızlı ve anlamlı bir şekilde sonuç üretmektedir. Sistemlerin açık olup olmadığını, açık olan sistemlerin port durumları, hangi servislerin çalıştığı ve kullanılan işletim sistemi gibi birçok bilgiyi verebilmektedir. Ayrıca içerisinde barındırmış olduğu scriptler ile hedef sisteme yönelik tarama gerçekleştirildiğinde hedef sistem hakkında detaylı bilgi verir ve güvenlik açığı olup olmadığına yönelik sonuç üretir. Nmap aracı, alanının en iyi araçları arasında yer alır.**

### 1.1 NMAP TARAMA AŞAMALARI

Nmap uygulaması kullanılarak taramanın başarılı bir şekilde sonuçlanması için belirli aşamalar takip edilerek tarama işlemleri yapılmalıdır.

#### 1.1.1 Tarama Öncesi Scriptlerin Kullanılması

Nmap aracı, taranacak ağ hakkında bilgi toplamak için scriptler barındırır. Örneğin, ağ servislerinden bilgi almak için broadcast sorgularını kullanan dhcp-discover ve broadcast-dns-service-discover gibi scriptler kullanılmaktadır.

#### 1.1.2 Hedef Numaralandırma

Nmap, hedef numaralandırma işlemlerinde DNS, IP adresleri, CIDR değerleri gibi host belirteçlerini tespit etmektedir. Hedef hostları belirlemek için –iR parametresi kullanılabilir.

#### 1.1.3 Host Keşif İşlemleri

Nmap’te host keşif işlemleri genellikle bir makinenin aktif olup olmadığını tespit etmek için yapılmaktadır. Nmap varsayılan olarak önce host keşfi yapar ve sonra port taramasına başlar. Eğer port taraması yapmadan sadece host keşif işlemi yapılmak isteniyorsa –sn parametresi kullanılır. Host keşif işleminin yapılması istenmiyorsa –Pn parametresi kullanılabilir. –Pn parametresinin kullanılması ile hostlara ping atılmaz. Böylelikle host keşfi yapılmaz.

#### 1.1.4 Reverse-DNS Resolution

Nmap, ping taraması ile tarayıp belirlediği aktif makinelere yönelik Reverse-DNS çözümleme işlemi gerçekleştirilmektedir. Reverse-DNS çözümlemesi, –R parametresi ile yapılır. Normal şartlarda yalnızca açık makinelere yapılır.

#### 1.1.5 Port Taraması

Port taraması, Nmap aracının ana işlevlerinden biridir. Aktif olan bir sistemin portlarına istek atarak, portların açık veya kapalı olma durumunu tespit eder.

#### 1.1.6 Versiyon Tespiti

Tespit edilen açık portlarda hangi servisin çalıştığının tespiti için kullanılır. Nmap aracı içerisinde barındırmış olduğu problar ve 6500’den fazla servis imzası ile portlarda bulunan servisleri karşılaştırıp tespit etmektedir. Bu işlem –sV parametresi kullanılarak gerçekleştirilmektedir.

#### 1.1.7 İşletim Sistemi Tespiti

Nmap ile açık olan makinelere yönelik işletim sistemi tespiti yapılmaktadır. Nmap içerisinde bulunan bir veritabanında işletim sistemlerinin yanıtları bulunmaktadır. Nmap oluşturmuş olduğu probları makinelere göndererek makinelerden gelen yanıtları veritabanında bulunan yanıtlarla karşılaştırılmaktadır. Böylelikle kullanılan işletim sistemi bilgisi elde edilmektedir. Nmap aracı üzerinde bu işlem –O parametresi kullanılarak gerçekleştirilmektedir.

#### 1.1.8 Traceroute İşlemi

Nmap, **--traceroute** parametresi ile her hedef için paketlerin hangi yoldan geçtiğini tespit edebilir.

#### 1.1.9 Script Taraması

Nmap içerisinde, Nmap Script Engine(NSE) adı verilen bir yapı bulunmaktadır. Bu yapı içerisinde birçok script bulunur. Bu scriptler kullanılarak hedefe yönelik bilgi toplama ve güvenlik açığı tespit etme gibi birçok işlem gerçekleştirilebilmektedir. NSE, lua programlama dili ve ağ üzerinde bilgi toplama için tasarlanmış standart bir kütüphane tarafından desteklenmektedir. Bu scriptler genellikle tespit edilen her host üzerinde bulunan her bir port için bir kere çalıştırılmaktadır. –script veya -sC parametreleri kullanarak Nmap üzerindeki script’ler çalıştırılabilir.

#### 1.1.10 Çıktı Alma

Nmap, tarama işlemleri sonucunda elde ettiği bilgileri ekrana basar. Bu sonuçlar farklı dosya formatlarında kaydedilebilir.

## 2. Nmap Kurulumu
Nmap, Windows ve Linux işletim sistemleri gibi birçok işletim sistemini destekler. Kurulumu kolaydır.

### 2.1 Linux (Debian/Ubuntu) Ortamı

Terminal’de **sudo apt-get install nmap** komutu çalıştırıldığında Nmap yüklenmeye başlayacaktır. Ayrıca nmap, sitesinden .rpm veya .deb uzantılı setup dosyaları indirilerek kurulabilir.

```linux
root@Stormer:~# sudo apt-get install nmap
sudo: unable to resolve host Stormer: Name or service not known
Reading package lists... Done
Building dependency tree       
Reading state information... Done
nmap is already the newest version (7.91+dfsg1-1kali1).
The following packages were automatically installed and are no longer required:
  golang-1.14 golang-1.14-doc golang-1.14-go golang-1.14-src libre2-6 libx265-179 oracle-instantclient-basic
Use 'sudo apt autoremove' to remove them.
0 upgraded, 0 newly installed, 0 to remove and 1233 not upgraded.

```

### 2.2 Windows Ortamı

Nmap aracının Windows ortamındaki kurulumunu sağlamak için http://nmap.org/download.html uzantılı sayfasından aşağıda gösterileceği gibi indirme işlemi gerçekleştirilecektir.

| ![atgr1]({{ site.url }}/assets/img/Nmap/part1/sekil221.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 2.2.1 : Windows Ortamı için İndirme İşlemi* |

<br/>

Daha sonra indirilen .exe uzantılı nmap setup’ı yönetici olarak çalıştırılmaktadır.

| ![atgr2]({{ site.url }}/assets/img/Nmap/part1/sekil222.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 2.2.2 : Windows Kurulum Gösterimi* |

<br/>

Kurulum işlemi bittikten sonra Program Files(x86)\Nmap dizinin altındaki nmap.exe uygulaması çalıştırılabilir. Ayrıca bu kurulumla birlikte Nmap’in grafik kullanıcı arayüzlü hali olan zenmap uygulaması da yüklenecektir.

| ![atgr13]({{ site.url }}/assets/img/Nmap/part1/sekil223.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 2.2.3 : Command Prompt Ekranında Nmap* |

<br/>

Şekil 2.2.3’te Command Prompt ekranı üzerinde nmap.exe çalıştırılmıştır.

| ![atgr4]({{ site.url }}/assets/img/Nmap/part1/sekil224.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil 2.2.4 – Windows Ortamında ZenMap Uygulamasının Çalıştırılması* |

<br/>

Şekil 2.2.3 ve Şekil 2.2.4’te gösterildiği gibi Nmap ve Zenmap araçları Linux ortamında da mevcuttur. Terminal üzerinden nmap ve zenmap komutu çalıştırılarak görüntülenebilir.
