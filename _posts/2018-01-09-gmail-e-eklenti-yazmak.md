---
layout: post
title: Gmail'e Eklenti Yazmak
tags: [gmail-addon]
---

Merhaba. Gmail'de işlerimizi halletmek adına birkaç tarayıcı eklentisi kullanıyoruz.

Fakat bu eklentiler kimi zaman tarayıcılar değişince iyi çalışmıyor ya da alternatifi bulunamıyor veya mobil uygulamalar üzerinde hiçbir şekilde var olmuyor. 2017 yılının sonuna doğru Google, Gmail için artık eklenti yazımını mümkün kıldı. İsteyenler artık tarayıcı eklentisi yazar gibi Gmail için eklenti yazabilecek. Peki bu Gmail eklentisi tam olarak ne anlama geliyor, ve bu iş nasıl yapılıyor?

Gmail eklentileri emailler yolu ile yapılan günlük işlerinizi aslında kolaylaştırıyor. Örneğin, Asana uygulamasının eklentisi ile size gelen bir emaili Asana'da bir iş olarak hızlı bir şekilde açabilir ve detaylandırabilirsiniz. Eskiden olsaydı Asana'nın ekranlarına girip bu işlerini halletmeniz gerekecektir. Bunun gibi örnekleri çoğaltabiliriz. Kısacası Gmail eklentileri zamanı ve işleri kısaltmamıza yarıyor.

Kendisine ait bir UI ekranı olan eklentileri direk Google servislerine bağlayabilir, her türlü istediğiniz veriyi çekip, yazabilirsiniz. Bunların dışında Google servisi olmayan dışarıdan servislere de (Asana örneği gibi) bağlanabilir, veri paylaşımı yapabilirsiniz. Ayrıca başka cihazlar üzerinden eklentileri yönetebilir ve cihazlar arası sorunsuz bir şekilde çalışma sağlayabilirsiniz.

Google bu servisi sadece Gmail için vermiyor. Bütün Google servisleri Drive, Docs, Sheets için de ayrı ayrı uygulamalar yazabilirsiniz. Bu eklentileri yazmak için [Apps Script](https://developers.google.com/apps-script/) adında Javascript'e çok benzeyen bir programlama dili kullanılıyor. Bu dilin sağladığı kütüphaneler sayesinde eklentiler geliştiriliyor ve uygulamalar arası veri akışı sağlanabiliyor.

<img style="float: left; width: 65%; margin-right: 10px;" src="https://developers.google.com/gmail/add-ons/images/top-card-graphic-01.png"> Gmail eklentileri içinde oluşan widget'lar ortaya çıkıyor. Bu widget'ler ise [Card](https://developers.google.com/gmail/add-ons/concepts/cards) adı verilen kavram üzerinde yaşıyor. Bu Card'lar eklentinin UI'ını oluşturuyor ve bütün eklenti ve eklentinin diğer sayfaları bu Card'lar üzerinde yaşıyor. Bir eklenti içinde birçok Card tanımlanabilir ve bu Card'lar üzerinde text, input alanları, button'lar gibi birçok element eklenebilir. Card'lar ile web ve mobile için ayrı ayrı uygulamaya yazmaya gerek kalmıyor. Bir eklenti cross-platfrom çalışabiliyor. Burada küçük bir not düşmek istiyorum. Bugün itibari ile bu eklentiler henüz **iOS cihazlarda çalışmıyor.**

Google bu her konuda olduğu gibi bu konu ile de alakalı müthiş bir doküman hazırlamış. Eğer konuya ilgi duyuyorsanız [buradan](https://developers.google.com/gmail/add-ons/how-tos/building) başlayabilirsiniz. Ayrıca Gmail için de hazırlanmış örnek eklentilere [buradan](https://github.com/googlesamples/gmail-add-ons-samples) erişebilirsiniz. Çok daha fazla bilgi almak istiyorsanız Gmail'in resmi eklenti [sayfasını](https://developers.google.com/gmail/add-ons/) ziyaret edebilirsiniz.

Sevgiler.

<small>*Image: https://developers.google.com/gmail/add-ons/*</small>
