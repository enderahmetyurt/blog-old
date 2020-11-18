---
layout: post
title: Sorbet ile Static Typing
date: 2020-11-17 14:41 +0300
tags: [ruby, sorbet]
---

Merhaba, Ruby her ne kadar süper sevimli bir dil olsa da dinamik bir dil olduğu için yazılan kodlarda ortaya çıkan hatalar ancak runtime'da kendini gösterebiliyor.

Ancak statik programlama dillerinde kodlarda ortaya çıkan hataları compile time'da alabiliyoruz. Bu duruma bir örnek vermek gerekirse Ruby'de

```ruby
animal = 'Foo'
animal.greeting
```

Yukarıdaki kod bize ancak code çalıştığı zaman `NoMethodError` hatasını dönecektir. Ancak bu kodu Java gibi compile edilen bir dil ile yazsaydik, hatayı kod compile edildiği zaman alacaktık ve kod içindeki hatalari minimize etme şansımız olacaktı.

## Sorbet

Ruby için geliştirilmiş ve kodunuzdaki method tanımlarını, method signature kontrolleri ve değişken tiplerinin kontrolünü yapan bir Ruby Gem'i. Özünde normal Ruby gibi kodumuzu yazıyoruz ancak Sorbet kullanmak istiyor ve type check yapmak istiyorsak ile Sorbet'in `T::Sig` modulünü kullanarak kontrollerimizi yapabiliyoruz.

```ruby
# typed: true
require 'sorbet-runtime'

class A
  extend T::Sig

  sig {params(x: Integer).returns(String)}
  def bar(x)
    x.to_s
  end
end

def main
  A.new.barr(91)   # error: Typo!
  A.new.bar("91")  # error: Type mismatch!
end
```

İlk olarak Sorbet'e statik tip kontrollerini ne şekilde raporlaması gerektiğini belirtmek gerekiyor. `# typed: true` dediğimiz zaman normal type hatalarını raporla demek oluyor. Bu şekilde yapabileceğimiz tam beş adet level var. Bu tiplere ve detaylarına [buradan](https://sorbet.org/docs/static) ulaşabilirsiniz. `sorbet-runtime`'ı require ediyoruz çünku bu kod çalıştırıldığı zaman da hatayı bize döndürsün istiyoruz. Sonrasinda `A` adında bir class'ımız var ve bu class'ın instance'ının bir `bar` adında bir methodu var. Ruby'de methodlara verilen parametlerin tipi ve method'ların döndürdüğü değelerin tiplerinin pek önemi olmazken Sorbet ile bunlari `sig` methodu ile belirleyebiliyoruz. `sig {params(x: Integer).returns(String)}` ile bu method integer tipinde bir parametre alır ve String tipinde birşey döner diyoruz.

Siz de Sorbet'i hızlica denemek istiyorsaniz [buradan](https://sorbet.run/#%23%20typed%3A%20true%0Arequire%20'sorbet-runtime'%0A%0Aclass%20A%0A%20%20extend%20T%3A%3ASig%0A%0A%20%20sig%20%7Bparams(x%3A%20Integer).returns(String)%7D%0A%20%20def%20bar(x)%0A%20%20%20%20x.to_s%0A%20%20end%0Aend%0A%0Adef%20main%0A%20%20A.new.barr(91)%20%20%20%23%20error%3A%20Typo!%0A%20%20A.new.bar(%2291%22)%20%20%23%20error%3A%20Type%20mismatch!%0Aend) online Sorbet runtime aracını kullanabilirsiniz.


### Kurulum

Sorbet'i projelerimize eklemek için öncelikle `Gemfile` dosyamıza Sorbet ve Sorbet Runtime Gem'lerini eklememiz gerekiyor.

```
# -- Gemfile --

gem 'sorbet', :group => :development
gem 'sorbet-runtime'
```

`$ bundle install` ile Gem'leri indirdikten sonra artık Sorbet kullanmaya başlayabiliriz.

### Kullanım

Sorbet'i kullanacağımız projenin altinda `$ srb init` diyerek Sorbet için gerekli olan dosyaları oluşturuyoruz. Bu dosyalar Sorbet tarafından otomatik oluşuyor. `sorbet` klasörü altında `config` dosyasi ve `rbi` klasörü bulunuyor. `config` dosyası içinde Sorbet için commandline ayarlarını yapabilirsiniz. Daha fazla detaya [buradan](https://sorbet.org/docs/cli) bakabilirsiniz. `rbi` klasörü içinde ise Sorbet içinde bulunmayan sizin kendi class, module ve diğer nesne tanımlarınızı yapabilir, Sorbet'te bunları kullanabilirsiniz. Sorbet ile gelen RBI dosyalarına [buradan](buradan) ulaşabilirsiniz.

Kendi yazdığımız Ruby kodlarındaki type check'leri yapmak için ise, `$ srb tc` komudunu çalıştırarak yapabiliriz.

### Örnek

Ben daha önceden yazmış olduğum Ruby Gem'ime Sorbet'i ekledim. Bu [commit](https://github.com/enderahmetyurt/bilisim_sozlugu/commit/6854a4a607461073b2dc69569b70deeee040c0ec)'te görüldüğü gibi `search` methoduna `sig {params(word: String).void}` ekledim. Bu method konsola birşeyler bastığı için `nil` dönüyordu ve `void` ile aslında bu method'un birşey dönmediğini belirttim. Ayrıca lokal değişkenlere de tipler tanımladım. Mesela `CSS_SELECTOR` için sadece String olabilir demek istedim ve `CSS_SELECTOR = T.let("table tbody tr".freeze, String)` bu şekilde yazarak bu değişken sadece String kabul etmelidir dedim. Kodun diğer yerlerinde eğer bu değişkeni String dışında bir tipe eşitlersem Sorbet bana hata dönecek. Sizler de bu kodu düzeltmek isterseniz PR'larınızı memnuniyet ile bekliyorum.


### Sonuç

Ben Sorbet'i çok sevdim ve kendi yazdığım küçük Ruby projeleri için implemente etmeye başladım. Hepinizin merak ettiği bu iş Ruby on Rails'da nasıl oluyor sorularına ise kısa bir zamanda cevap verebilmeyi ben de istiyorum. Konu ile alakalı [şöyle](https://flexport.engineering/adding-sorbet-to-a-rails-monolith-ef72d6a18449) bir yazı var, isterseniz inceleyebilirsiniz.

Ruby her ne kadar süpersonik bir dil olsa da her güzelin bir kusuru var ve o kusurda Sorbet ile biraz olsun kapatılabiliyor.

Sevgiler.

*Referanslar:*

- [https://blog.heroku.com/static-typing-ruby-with-sorbet](https://blog.heroku.com/static-typing-ruby-with-sorbet)
- [https://medium.com/better-programming/static-typed-ruby-introducing-sorbet-from-stripe-eeb4ffd8644](https://medium.com/better-programming/static-typed-ruby-introducing-sorbet-from-stripe-eeb4ffd8644)
- [https://sorbet.org/docs/overview](https://sorbet.org/docs/overview)
