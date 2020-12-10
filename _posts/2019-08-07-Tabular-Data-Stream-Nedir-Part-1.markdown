---
title:  "Tabular Data Stream Nedir?"
date:   2019-08-07
layout: post
categories: 
    - veritabani-guvenligi
thumbnail: posts/TDS-sekil1.png
---

# Tabular Data Stream Nedir?

**Bir veritabanı sunucusu ve bir istemci arasında veri aktarımını sağlamak için oluşturulan bir uygulama katmanı protokolüdür. İlk olarak Sybase Inc, tarafından Sybase SQL Server veritabanı motoru için 1980’li yılların başında tasarlandı. Daha sonra Microsoft SQL Server’da kullanılmak için geliştirildi. Bağlantı odaklı bir taşıma servisidir. TCP/IP böyle bir taşıma sağlamaktadır. Saklı bir prosedürün veya kullanıcı tanımlı fonksiyonun(uzak prosedür çağrısı) çağrılması, veri ve işlem yöneticisi isteklerinin iadesi işlemlerini yapmaktadır.**

#### Teknik Özellikler:
-	TDS’nin kimlik doğrulama ve tanımlama
-	Kanal şifreleme anlaşması
-	SQL gruplarının yayınlanması
-	Saklı yordam çağrıları
-	Geri dönen veriler ve işlem yöneticisi talepleri

Veritabanı üzerindeki işlemlerin istemci ile akışının sağlanması için genellikle 1433 portunu kullanan bir protokoldür. Bu protokolü varsayılan olarak MSSQL Server bağlantı portu olarak bilinmektedir. Microsoft’un MS-TDS açık spesifikasyonlarında, TDS protokolü, bir veritabanı sunucusuyla iletişim kurmaya, kimlik doğrulama ve kanal şifreleme işlemine izin veren bir uygulama katmanı request/response protokolü olarak tanımlanmaktadır. Bir ağ katmanının üzerine yerleştirilmiş bir veritabanı iletişim katmanıdır. Bu durumda Transmission Control Protocol(TCP), Internet Protocol(IP) üzerine kurulmuştur. Güvenilmeyen ağlar üzerinde iletişimi korumak için Microsoft Sql Server, uygulanan Tabular Data Stream protokolünün şifreleme özelliklerine dayanarak kurabildikleri bir uygulamayı karşıladı. Bir Sybase istemcisinin, bir Sybase sunucusuna bir Tabular Data Stream paketi göndermek için bir IP adresi ve bir TCP bağlantısı noktası belirtmesi gerekir.Tabular Data Stream paketi, TCP paketinin data bölümünde bulunmaktadır.

#### Desteklenen İstemciler:
-	Açık İstemci/Sunucu
-	ASE Veri Erişim Ürünleri:
    -	ODBC, OLE DB, ADO.NET, JConnect.

#### Proxy Sunucuları ve Ağ Geçitleri:
-	ASE/CIS, Direct Connect, Çoğaltma Sunucusu, IQ.



## BASİT BİR TABULAR DATA STREAM OTURUMU
İstemci sunucuya(ASE’ye) bağlanacağı zaman, önce ASE sunucusuna bir aktarım bağlantısı (TCP/IP) talep eder. İstemci daha sonra diyaloğu başlatmak için giriş kaydını gönderir. ASE, bir tamamlama tokenı (TDS_DONE) ile birlikte bir alındı işareti (TDS_LOGINACK) ile yanıt verir. Bu işlem olduğunda, ASE bu istemci ile diyaloğu kabul ettiği anlamına gelir. Kurulan iletişim ile istemci artık ASE’ye SQL ifadeleri göndermeye hazırdır.
-   SELECT ifadesini istemciye göndermek için, bunu TDS_LANGUAGE tokenı göndererek yapmaktadır.
-	ASE sorgu çalıştırır ve aşağıdakileri gönderir:
    -	Sonuç kümesinin sütun açıklaması
    -	Veri satırları bunu takip eder.
    -	TDS_DONE tamamlama paketi, satır sayısı ile gönderilen son şeydir.


| ![atgr1]({{ site.url }}/assets/img/TDS/TDS-sekil1.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil1 : Basit Bir TDS Oturumu (Login Request)* |


| ![atgr2]({{ site.url }}/assets/img/TDS/TDS-sekil2.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil2 : TDS Request Gönderimi* |


## PROTOCOL DATA UNITS(PDUs)
PDU, istemci(request) ve sunucu(response) içeriği için kullanılıp request/response birkaç PDU içerebilir. İletişim kurma işlemi, PDU’nun boyutu oturum açma süresi boyunca istemci ve sunucu arasında kararlaştırılır. PDU, header ve genellikle data içermektedir.

### PDU HEADER
PDU header, PDU’nun boyutu ve içeriği ile birlikte bir request/response sonucunda son PDU ise bir gösterge içerir.
-	TDS yarı çift yönlüdür.
    -  	Client, request tamamlıyor, yazar.
    -	Sonra sunucudan tam response okur.
    -	Hiçbiri karıştırılmamış ve birden çok istek beklenemez.
-	PDU’ları okunabilir formatta biçimlendirmek için daha sonra söz edilecek bazı araçlar vardır:
    -	RAW formatı, sniffer trace’da bulunanlara benzer verileri gösterir.
        -	ASE traceflag’larını komut satırında kullanılır:
            -	4001, 4013, 4034, 4035, 4036.
            -	4013 girişli kayıt içeriklerini yazdırır.
        -  	Bilgi stdout’a gider, bu yüzden tee kullanmak isteyebilir.
    -  	RIBO adlı araç ile oluşturulan çıktı
    -	Java tabanlı TDS paket ağ geçidi aracı


| ![atgr3]({{ site.url }}/assets/img/TDS/TDS-sekil3.png){: style="display: block; margin-left: auto; margin-right: auto; width: 100% "} |
|:--:|
| *Şekil3 : Mesaj Tampon Başlığının gösterimi* |


### PDU DATA
PDU’lar genellikle bazı verileri içerir.
-	Kontrol PDU’ları veri içermemektedir.
    -	Sadece Header
-	Request ve Response PDU’ları TDS tokenları içerir.
    -	Token request/response tanımlar.
    -	Data değiş/tokuş yapılır. Böylece client ve server (request/response) döngüsünü tanımlayabilir.

### CLİENT PDUs REQUEST
-	Dialog Establishment Information (İletişim Kurma Bilgileri)
-	Language Command(Dil Komutları)
-	Cursor Command(İmleç Komutları)
-	Database Remote Procedure Call (Veritabanı Uzak Yordam Çağrısı)
-	Attentions(Dikkat Edilmesi Gerekenler)
-	Dynamic SQL Command(Dinamik SQL Komutları)
-	Message Command(Mesaj Komutları)

#### DIALOG ESTABLISHMENT INFORMATION(İLETİŞİM KURMA BİLGİLERİ)
Öncelikle bir transport connection oluşturulmalıdır.
-	Bu düşük seviyeli bir ağ işlevidir.
    -	TCP/IP ve Socket kurulumu
Login bilgileri girilmelidir.
-	Kimlik bilgileri ve özellikleri
Data Stream Capability(yetenek) gönderilir.
-	Cilent’ın ne yapabilip ne yapamadığını öğrenilir.
Authentication Handshaking(Kimlik Doğrulama El Sıkışması)
-	Parola Şifreleme(Password Encryption)
Okuma ve Giriş Onaylama(Read And Login Acknowlodgement)

#### LANGUAGE COMMAND(DİL KOMUTLARI)
ASE’ye bir SQL komutu gönderilir.
-	TDS_LANGUAGE tokenı komutunu göndermek için kullanılır.
-	Komut çoklu PDU’ları kapsayabilir.
    -	TDS_LANGUAGE tokendaki uzunluk alanına göre sınırlandırılır.


#### CURSOR COMMAND(İMLEÇ KOMUTLARI)
Cursor komutlarını göndermenin üç yolu vardır:
-	Dil Komutları
    -	TDS_LANGUAGE
    -	Cursor, T-SQL komutlarını kullanarak ASE’ye gönderir.
    -	Diğer sunucular dil ayrıştırmak ve cursor uygulayabilmek için gereklidir.
-	Cursor TDS Tokenları
    -	ANSI belirtilen cursor işlemleri
    -	Yerel token desteği
    -	Ayrıştırma dizinini ortadan kaldırarak verimli olabilir.
-	TDS_CUR* tokenları

#### DATABASE REMOTE PROCEDURE CALL
Client TDS_DBRPC token’ı gönderir.
-	Binary Stream içeriği:
    -	RPC Adı
    -	Seçenekler
    -	Parametreler
-	SQL komutları veya diğer RPC komutlarıyla karıştırılmamalıdır.
-	Parametreleri tanımlamak ve parametre değerleri göndermek için ek tokenlar kullanılır.

#### ATTENTIONS(DİKKAT EDİLMESİ GEREKENLER)
Client, mevcut isteğini iptal etmek için sunucuya bir attention isteği göndermektedir.
-	İstemci sunucuya attention gönderir.
-	İstemci okumaya devam eder.
-	İstemci attentions gönderdikten sonra okuma verileri artar.
-	Sunucular attentions kabul ediyor ve böylece devam ediyor.

#### DYNAMIC SQL COMMAND(DİNAMİK SQL KOMUTLARI)
İstemci, sunucu üzerinden dinamik SQL yürütmek için TDS_DYNAMIC veya TDS_DYNAMIC2 data stream gönderir.
-	Stream çeşitli işlevleri gösterir:
    -	Hazırlama(Prepare)
    -	Çalıştırma(Execute)
    -	Ayırma(Deallocate)
    -   SQL bildirimleri ve tanımlama

### SERVER PDUs RESPONSE
-	Dialog Establishment Acknowledgement
    -	İletişim kuruldu alındı belgesi
        -	Giriş talebi alındı.
-	Satır Sonuçları
-	Procedures Dönüş Durumu
-	Dönüş Parametreleri
-	Yanıt Tamamlama
-	Hata Bilgisi
-	Attention Onaylama
-	Cursor Durumu
-	Mesaja Yanıt
