---
layout: post
title: Gmail Eklenti Örneği
date: 2018-01-23 11:52 +0300
---

Merhaba. Daha önce [Gmail add-on](https://enderahmetyurt.com/2018/01/09/gmail-e-eklenti-yazmak.html) nedir? Bu konudan biraz bahsetmiştim. Şimdi ise küçük bir uygulama ile giriş yapıp daha pratik birşeyler göstermek istiyorum.

Öncelikle ihtiyacımız olan add-on'u yazmak için gerekli olan bir ortam. Google bize bu ortamı [https://script.google.com/](https://script.google.com/) ile veriyor. Sonrasında ise uygulamanın ayarlarının ve kütüphanelerinin bağımlılıklarının bulunduğu ```manifest``` dosyasını tanımlamak gerekiyor. Son olarak ise kodu yani ```CardService```'i yazmaya başlıyoruz.

Manifest dosyasını ```appscript.json``` içine koymak gerekiyor. Default olarak bu dosya kod yazdığımız yerde görünür değil o yüzden ```View > Show manifest file``` diyoruz ve aşağıdaki kodu dosyaya ekliyoruz.

```json
  "gmail": {
    "name": "Quickstart Toolbar Label",
    "logoUrl": "https://www.example.com/hosted/images/2x/my-icon.png",
    "primaryColor": "#4285F4",
    "secondaryColor": "#4285F4",
    "contextualTriggers": [{
      "unconditional": {},
      "onTriggerFunction": "getContextualAddOn"
    }],
    "version": "TRUSTED_TESTER_V2"
  }
```

Gördüğünüz gibi bu dosya içinde basit bir şekilde config ayarları yapılmış durumda. Uygulamanın adı, logosu, renkler ve versiyon gibi basit özelliklerin yanında asıl önemli olan uygulama ilk başladığında hangi metodun çalışması gerektiğini söyleyen ```onTriggerFunction``` parametresi. Bu parametre uygulama ayağa kalkınca ilk çalışması gereken metodu belirtiyor ve artık yapmamız gerekenler bu metot içinden çalışmaya başlıyor.

Google bizim için ```Code.gs``` adında bir google script dosyası açıyor ve aslında uygulama için ilk çalışan dosya burası oluyor. Bu dosya içine uygulamamız ile alakalı kodları yazıyoruz. Manifest dosyasında belirttiğimiz ```getContextualAddOn``` metodunu bu dosya içinde yazmaya başlıyoruz.

```js
function getContextualAddOn(e) {
  // Activate temporary Gmail add-on scopes, in this case to allow
  // message metadata to be read.
  var accessToken = e.messageMetadata.accessToken;
  GmailApp.setCurrentMessageAccessToken(accessToken);

  var messageId = e.messageMetadata.messageId;
  var message = GmailApp.getMessageById(messageId);
  var subject = message.getSubject();
  var sender = message.getFrom();

  // Create a card with a single card section and two widgets.
  // Be sure to execute build() to finalize the card construction.
  var exampleCard = CardService.newCardBuilder()
  .setHeader(CardService.newCardHeader()
             .setTitle('Example card'))
  .addSection(CardService.newCardSection()
              .addWidget(CardService.newKeyValue()
                         .setTopLabel('Subject')
                         .setContent(subject))
              .addWidget(CardService.newKeyValue()
                         .setTopLabel('From')
                         .setContent(sender)))
  .build();   // Don't forget to build the Card!
  return [exampleCard];
}
```

Yukarıdaki kodda amacımız bir email içeriğine ulaşabilmek. Bunun için Gmail'in servislerini kullanabilmemiz lazım. O yüzden Gmail'e authenticate olmamız gerekiyor. Zaten uygulama çalışmaya başladığı zaman bu bize soruluyor ve onay verildikten sonra kod çalışmaya başlıyor. Sonrasında ise Card servisi devreye giriyor. Daha önce de anlattığım gibi Gmail add-on için UI desteği Card ile geliyor ve gelen veriyi kullanıcıya göstermek için öncellikle bir Card tanımlıyoruz. Sonrasında ise gerekli component'ları koyup son olarak Card'ı ```build()``` methodu ile oluşturuyoruz. Burası çok önemli. Eğer Card'ı hazırlayıp, build metodunu çağırmazsak isek istediğimiz sonucu alamayız.

Biraz Card'lardan bahsetmek gerekirse. Card'lar onlar için tanımlanmış özel **widget**'lardan oluşuyor. Her action için yeni bir Card tanımlayabilir ve özel widget'lar atayabiliriz içerlerine. CardBuilder class'ı yeni bir Card tanımlamamıza yarıyor. Her Card **header** ve **section**'lardan oluşuyor. Bu section'lar içine kullanıcı ile kurulacak action'ları tanımlama adına widget'ları ekleyebiliyoruz. Kısacası bir Card oluşturmak için sırasıyla,

* Widget'ı oluşturmak
* Section'a widget'ı eklemek
* Section'a tüm widget'ları tanımlayana kadar devam etmek,
* Son olarak ise section'ı Card'a eklemek gerekiyor.

Gelelim test ve deployment konusuna. Farklı [deployment yöntemleri](https://developers.google.com/apps-script/concepts/deployments) mevcut fakat development tarafında hızlı test edebilmemiz için **Head Deployments**'dan bahsedeceğim.

Eğer bütün kodu düzgün bir şekilde yazdıysak script ekranından ```Publish > Deploy from manifest``` diyerek açılan pop-up'dan ```Lastest Version (HEAD)``` satırından **Get ID** linkine tıklayarak açılan pencereden **Deployment ID**'yi kopyalayıp, alıyoruz ve ayrı bir pencerede Gmail hesabımı açıyoruz.

Gmail için ayarlarımızın yapıldığı **Settings** sayfasına geliyoruz. Buradan **Add-on** sekmesinde yer alan **Developer add-ons** alanına biraz önce kopyaladığımız Deployment ID'yi yapıştırıyoruz ve sonrasında Install diyoruz. Sonrasında Inbox'a gidip sayfası yeniliyoruz.

Inbox içinden herhangi bir email'e tıkladığımız zaman sağ tafta uygulama görülecektir. Daha önce de belirttiğim gibi Gmail servislerini kullanmak için Gmail'e izin vermemiz gerekiyor. O yüzden ```AUTHORIZE ACCESS``` buttonuna tıklayıp, uygulama için yetkilendirme izinini veriyoruz. Burada https ile bağlanmadığımız ve uygulamamız development ortamında olduğu için bir güvenlik sorunu çıkıyor olabilir. Bu uyuglama size ait olduğu için kullanmaya devam et diyerek Gmail servislerinin bilgilerimize ulaşmasına izin verebiliriz. Sonrasında uygulama Card'ı yeniden yükleniyor ve uygulamanın çalıştığını görebiliriz.

<div style="display: flex; justify-content: center;"><img src="/public/images/gmail-add-on-ss.png"></div>


Sevgiler.

<small>
*Referanslar:*
*https://developers.google.com/gmail/add-ons/how-tos/building*
</small>