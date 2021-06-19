---
title:  "Güvenli Yazılım Geliştirme"
date:   2021-06-19
layout: post
categories: 
    - yazilim-guvenligi
thumbnail: posts/guvenli.jpeg
---

## **Güvenli Yazılım Geliştirme**

Güvenli olmayan yazılımların kuruluşlara verdiği zararların maliyeti yüksek olabilir. Bu sebeple, kullanılacak olan yazılımların güvenlik risklerini en aza indirmek için yazılım geliştirme döngüsünün her aşamasında güvenlik denetimlerinin gerçekleştirilmesi gerekir. Yazılım geliştiren birçok şirket, yazılım geliştirme yaşam döngüsü (SDLC) aşamalarında güvenlik denetimlerini yapmamaktadır. Yazılım geliştirme yaşam döngüsünün ilk aşamasında güvenlik denetimlerinin göz ardı edilmesi, birbirini izleyen diğer aşamaların kendisinden önceki aşamada meydana gelen güvenlik açıklarını devralmasına ve ortaya çıkan üründe güvenlik açıklarının artmasına neden olmaktadır. Bu durum, yazılımı geliştiren ve kullanan şirkete önemli ölçüde maddi kayıplara neden olabilir. 


Yazılımı geliştiren şirket, ilerleyen zamanlarda yazılımdaki güvenlik açıklarını kapatmak ve yazılımın güvenliğini arttırmak için ciddi ödemeler yapmak durumunda kalabilir. Bunun yanında kalitesiz ve güvensiz bir ürün ortaya koymasıyla prestij kaybı yaşabilir. Yazılımı kullanan şirket ise, daha büyük kayıplara maruz kalabilir. Örneğin, bir saldırgan bir kuruluşun kullandığı bir yazılımda bulduğu güvenlik açığı ile kuruluşa sızıp kuruluş ile ilgili hassas verilerin çalıp satışa sunabilir ya da sosyal medyada paylaşabilir. Bu durumda, hassas verilerin çalınması prestij kaybına sebep olurken, verilerin ifşa edilmesi müşterilerin kuruluşa olan güvenini kaybetmesine neden olur. Müşteri kaybının olması, kuruluş için prestij kaybına ek olarak maddi kayıpların oluşmasına neden olur.

**Güvenli yazılım geliştirmenin en iyi yolu, yazılımın proje metodolojisine bakılmaksızın, gereksinimlerin analizinden bakım aşamasına kadar yazılım geliştirme yaşam döngüsün her aşamasında güvenlik denetimlerini yapmaktır.** Geliştiriciler, yazılım geliştirme yaşam döngüsüne güvenlik denetimlerini ne kadar erken entegre ederse, güvenlik açıklarının oluşması ve açıkların kapatılması için yapılan ödemeler minimuma iner. 
Güvenli bir yazılımın geliştirilmesi beş aşamada yapılabilir. Bunlar;
-	Gereksinimlerin Analizi (Requirements Analysis)
-	Güvenli Tasarım (Secure Design)
-	Geliştirme (Development)
-	Testerin Yapılması (Testing)
-	Üretim ve Sonrası (Production & Post-Production) 
aşamalarıdır.


### **Gereksinimlerin Analizi**

Gereksinimler, yazılım geliştirme sürecini etkileyen ve yönlendiren bir kılavuz niteliği taşır. Müşterilerin gereksinimleriyle çalışırken, istenilen kullanım ve istenilmeyen (kötüye) kullanım durumlarının göz önüne alınarak belirli kombinasyonların oluşturulmasıyla güvenli yazılım geliştirme süreci başlar. Güvenlik danışmanları, yazılıma yönelik oluşabilecek tehdit unsurlarını önceden öngörmeli ve kötüye kullanım durumlarında neler olabileceği belirlenmelidir. İstenilmeyen kullanım durumlarının ifade edilmesi, aynı zamanda alınacak önlemlerin şeklini ve kapsamını belirler. Örneğin, yetkisiz bir kullanıcının bir müşteriye ait ugulamaya erişmeye çalışması istenilmeyen bir kullanım durumu iken, yetkisiz kullanıcının erişebileceği kapsamı belirlemek, sınırlandırmak ve yapılan girişimlerin kayıt altına alınması istenilmeyen durumların gerçekleşmesine karşın alınması gereken önlemleri de ortaya koyar. Geliştirilecek ürün ile ilgili bir risk değerlendirilmesi yaparak bir risk profili oluşturulabilir. Güvenlik risklerini ölçerken, uluslararası alanda kabul görülmüş **HIPAA** ve **SOX** gibi bazı yetkin kaynaklardaki güvenlik yönergeleri göz önüne alınmalıdır. Bu kaynaklarda, ürünün geliştirildiği iş alanıyla ilgili özel ek gereksinimler yer alır. 

Gereksinimlerin analizi aşamasında, güvenlik danışmanları, proje gereksinimlerini oluşturan iş analistlerine projenin risk profilini teslim etmelidir. Risk profil belgesinde, önem derecesine göre sınıflandırılmış kötü amaçlı saldırılar ve bu saldırılara duyarlı uygulama yöntemleri yer alır.

### **Güvenli Tasarım**

Bir kuruluşa ait verinin güvenliğini sağlamak amacıyla, altyapı tasarımının oluşturulma sürecinde güvenlik konusunun dikkate alınmasını sağlayan bir siber güvenlik yaklaşımıdır. Başka bir deyişle, yazılım geliştirecek bir ekibin projeye başlamadan önce projenin güvenliğini ön plana koyması ve bu doğrultuda projenin nasıl geliştirileceğini tasarlamasıdır. Güvenli tasarım, bir şirketin bir siber güvenlik ihlalinden etkilendikten sonra sorunu çözmek yerine en baştan oluşabilecek güvenlik ihlallerini belirleyip ortadan kaldırmayı amaçlamaktadır. Güvenli tasarım aşaması, yerine getirilmesi gereken 7 güvenlik ilkesi ile yapılabilir. Bu ilkeler;
-	En Az Ayrıcalık (Least Privilege)
-	Görevlerin Ayrılması (Seperation of Duties)
-	Derinlemesine Savunma (Defence in Depth)
-	Güvenli Başarısızlık (Failing Securely)
-	Açık Tasarım (Open Design)
-	Belirsizliğe Bağlı Güvenlik (Security by Obscurity)
-	Saldırı Yüzey Alanı Daraltma (Minimizing Attack Surface Area)
ilkeleridir.

#### En Az Ayrıcalık İlkesi:
Yazılım mimarisi, yazılımın normal bir şekilde çalışması için gerekli minimum kullanıcı ayrıcalıklarıyla kullanılmasına izin vermelidir. İnsanların yalnızca kendi işlerini yapmak için ihtiyaç duydukları kadar erişime sahip olmalarını sağlamak olarak ifade edilebilir. Örneğin, müşterilerin hassas bilgilerini tutan bir yazılım tasarlandığında, normal bir kullanıcının erişmemesi gerektiği bir yer olduğu için kullanıcı yetkilerinin sınırlanırılması gerekir.

#### Görevlerin Ayrılması İlkesi:
En az ayrıcalık ilkesinden farklı olarak, hiçbir rolün çok fazla yetkiye sahip olmaması gerektiğidir. En az ayrıcalık ilkesinde bir kullanıcının kendi işini yapabilecek kadar minimum kullanıcı haklarına sahip olması gerektiği belirtilirken, görevlerin ayrılması ilkesinde bir kullanıcıya birden çok görevin verilmemesi belirtilir. Çünkü, bir kullanıcıya birden çok görev verildiği takdirde, her görev için ayrı ayrı izinler gereklidir. Dolayısıyla, böyle bir kullanıcının ele geçmesi yazılımı veya sistemi tehlikeye atmaktadır. Bütün görevlerin yönetimini ve kontrolünü ancak yönetici yetkisine sahip kullanıcı yapmalıdır. Bundan dolayı diğer kullanıcı gruplarına kritik izinler gerektiren görevler verilmemelidir.

#### Derinlemesine Savunma İlkesi:
En az ayrıcalık ve Görevlerin ayrılması ilkeleri bir sisteme erişimin nasıl yapılacağı üzerine senaryolar oluştururken, Derinlemesine Savunma ilkesi, sisteme olan erişimlerin engellenmesi üzerine senaryolar oluşturur. Bilgisayardaki hiçbir güvenlik sistemi atlatılamaz değildir. Derinlemesine savunma, güvevlik sisteminin atlatılması durumunda sistemi yöneten kişilere haber vermesini sağlayan bir ilkedir. Örnek olarak, geliştirilen yazılımlar en pahalı güvenlik yazılımlarıyla bir sunucu içerisinde korunabilir. Ama bu sunucuların bulunduğu bir lokasyon var ve bir saldırgan fiziksel olarak sunuculara müdahale edebilir. Sunucunun bulunduğu binanın korunması ve izlenmesi derinlenmesine savunma olarak belirtilebilir.

#### Güvenli Başarısızlık İlkesi:
Bir sistemin veya yazılımın güvenli bir şekilde hata vermesini garanti eden bir ilkedir. Güvenli başarısızlık ilkesi doğrultusunda tasarlanmış bir sistem veya yazılım, yalnızca sürecin her adımı başarıyla tamamlandığında sistemin bölümlerine erişim sağlar. Örneğin bir saldırgan, derinlemesine savunma ilkesini atlatarak sunuculara erişmek için harekete geçti. Güvenli başarısızlık ilkesinin gereğine göre elektrikler kesilebilir. Ayrıca, bir yazılımın çalışırken hata vermesi durumunda verilen hata içeriğinde kritik bilgilerin bulunmaması da örnek olarak verilebilir.

#### Açık Tasarım İlkesi:
Bu ilke, geliştirilen bir yazılımın piyasaya sürülmeden önce, yazılımı koruyacak kriptografik uygulamanın public edilmesi ve dünyadaki alanında uzman kişilerin (hackerların) kriptografik uygulamayı test etmeye teşvik edilmesi gerektiğini belirtir. Dolayısıyla, sistem ne kadar güvenli olursa olsun, ele geçirilme riski olduğu için yazılımın kaynak kodunun güvenli ve her zaman test edilen bir güçlü bir kriptografik uygulamayla korunması gerekmektedir. Güçlü kriptografik bir uygulama ile korunmaması durumunda, saldırganlar kaynak kodlara erişim sağlar ve bütün kaynak kodu çözüp kontrolü ele alabilirler.

#### Belirsizliğe Bağlı Güvenlik İlkesi:
Önemli bilgileri gizlemeye dayalı olan bir ilkedir. Sistem mühendislerinin ve yazılım geliştiricilerinin bir sistemi veya bir yazılımı güvenceye almasını belirtir. Yazılım kaynak kodlarının ele geçirilme durumunda, kaynak kodlarının arasında kullanıcı adı ve parola bilgileri gibi kimlik bilgilerinin hard-coded olarak tutulmaması gerektiği ve güçlü şifreleme algoritmalarıyla şifrelenmesi gerektiğini örnek olarak verilebilir. Klasör isimlerinin veya dizin isimlerinin varsayılan olarak değilde spesifik olarak değiştirilmesi, SSH bağlantısı için varsayılan 22. port yerine farklı bir portun kullanılması, yazılımda kullanılan Apache veya Nginx gibi servis sürümlerinin gizlenmesi örnek olarak verilebilir.

#### Saldırı Yüzey Alanı Daraltma İlkesi:
Bir sistemi veya bir yazılımı daha güvenli hale getirmek için zorunlu olmayan, olmasa da olur denilen diğer bağlantıların ortadan kaldırılmasını sağlayan bir ilkedir. Örneğin, bir sistemde birden çok servisin çalışması birden çok bağlantı noktasının çalışması anlamına gelebilir. Saldırganların bu bağlantı noktaları üzerinden sisteme veya yazılıma zarar vermelerini engellemek için bu bağlantı noktalarını kapatmak ve sadece zorunlu olan bağlantı noktalarının güvenliğini sağlamak gerekir. Böylece, yapılabilecek bir saldırının hem yüzey alanı daraltılır hem de güvenliği sağlanması gereken az bağlantı noktası olur. Dolayısıyla sistemin veya yazılımın güvenliği daha iyi bir şekilde sağlanabilir.

### **Geliştirme Aşaması**

Güvenli bir yazılımın geliştirme aşamasında, yazılımın uluslararası alandan bilinen en kritik güvenlik açıklarına karşı korunacak şekilde kodlanması gerekmektedir. Yazılım geliştiricileri, yazılım güvenliği konusunda yetkin kuruluşlardan biri olan **OWASP**’ın Top10 güvenlik açıkları listesinden faydalanabilirler. Yazılım geliştirme yaşam döngüsünde bu tür güvenlik açıklarına karşı önemlerin alınması daha sonra düzeltmeye gerek kalmamasını, iyileştirme maliyetlerinin azalmasını ve müşteri güvenliğini sağlar. 

Ayrıca, yazılım geliştirildikten sonra kaynak kod inceleme işleminin yapılması gerekmektedir. Uygulamanın yazıldığı programlama dili kurallarını ve **güvenli kod yazımıyla** ilgili kuralları referans alarak kaynak kod incelenmesi yapılır. Bu sırada, diğer güvenlik açıkları tespit edilir, kaynak kodun kalitesi arttırılır, kompleks kod yazımları ve kritik bilgi ifşasına neden olacak program hataları kod düzeyinde tespit edilir ve düzeltilir. Güvenli kod incelemesiyle ilgili OWASP gibi kuruluşların dokümantasyonları fayda sağlayabilir. 

### **Test Aşaması**

Test aşaması, yazılımın müşteri gereksinimlerine göre çalışmasına izin vermeyen hataları bulmaya odaklanır. Test mühendisleri, yazılımın çalışıp çalışmadığına yönelik fonksiyonel ve fonksiyonel olmayan testleri yaparlar. E2E testi, performans testi, güvenlik testi vb. birçok test yapılır. Yazılımın müşteri gereksinimlerini hatasız bir şekilde karşıladığı tespit edildikten sonra güvenlik açıklarının olup olmamasını test etmek için sızma testi çalışmaları yapılır. Sızma testi çalışmalarında yazılımı ele geçirmeye yönelik bütün senaryolar gerçekleştirilir.  

Ayrıca, yazılım release aşamasına girdiğinde, güvenli yazılım geliştirme yaşam döngüsünün her aşamasında yapıldığı gibi keşif amaçlı sızma testi gerçekleştirilmelidir. Keşif amaçlı sızma testinde, sızma testi uzmanları, spesifik güvenlik açıklarını aramak yerine, kendi deneyimlerine ve sezgilerine dayanarak sistemi olası güvenlik kusurlarına karşı kontrol ederler. Testi yapacak mühendislerin, yazılım saldırı yöntemleri konusunda eğitimli olması ve geliştirilen yazılım hakkında bilgiye sahip olması gerekir.

### **Üretim ve Sonrası**

Yazılım, production ortamına kurulduktan sonra oluşabilecek yeni tehditleri ele almak için bir olay müdahale planı oluşturulmalıdır. Güvenlik anlamında oluşabilecek herhangi bir acil durumda iletişime geçilecek kişilerin belirlenmesi gerekmektedir. Ayrıca, önceki kontroller sırasında gözden kaçmış güvenlik açıkları olabilir. Bu sebepten dolayı son bir kez yazılımın incelenmesi gerekir. Gereksinimlerin analizi aşamasında tanımlanan tüm yanlış kullanım durumlarının ve güvenlik risklerinin kontrol edilmesi gerekir. Tüm gereksinimlerin karşılandığı doğrulandıktan sonra yazılım onaylanır. Bir sonraki bakım çalışmaları için arşivlenir. Bu kontroller, Microsoft’un yazılım ürününü poduction ortamına kurduktan sonra uyguladığı temel kontrollerdir.
