---
layout: post
title: Ruby 3 ile Gelebilecek Özellikler
date: 2020-09-29 14:03 +0300
tags: [ruby]
---

Merhaba, Ruby 3 2020 Aralık ayında yayına çıkmış olacak. Güzel özelliklerle geliyor olsa da bu özelliklerden hangilerinin hala Ruby 3 içerisinde olup olmayacağı tartışma konusu.

Deneysel özelliklerin bir kısmı Ruby 2.7 ile bizlerin kullanılımına sunuluyor ve deneme, test etme şansı buluyoruz. Bu yazıda Ruby 3'de görmemiz muhtemelen birkaçından bahsedeceğim.

Matz geçenlerde Github sponsorlarına bir anket gönderdi. Anket, hangi özelliği Ruby 3'de görmek istediğimiz ile alakalıydı. Seçenekler arasında ise şu özellikler vardı:

- Ractor
- JIT
- Type Profiler / Static Type Analysis
- Heap Compaction (GC Improvement)
- Pattern Matching
- Right Assignment

Şimdi bunlara tek tek bakalım.

## Ractor

Eski adı Guild olan bir özellikte çalışan Koichi Sasada Matz'ın da önerisi ile adını Ractor'a çevirdi. Mantığının ise `"Ractor," Ruby's Actor-like feature (not an exact Actor-model)` geldiğini söylüyor. Konu aslında MRI Ruby'nin kodu parallel çalıştırmasına izin vermemesinden geliyor. Guild yani yeni ismi ile Ractor, Ruby için yeni bir concurrency modeli öneriyor. Burada asıl amaç Ruby 3 ile bu modeli kurarken Ruby 2 ile uyumluluğu korumak ve parallel çalışabilecek parçaları ve objeleri hızlı bir şekilde paylaşımına izin vermek. Ruby'deki yaygın concurrency problemlerden biri thread'lerin race condition'lar oluşturup, oluşturmadığından emin olmaktır. Ruby'deki `Thread::Mutex` ile bunun önüne geçebilirsiniz fakat bu sefer de paralelliği ortadan kaldırmış ya da yavaşlatmış olursunuz.

Ruby'de Thread ve Fiber olmak üzere iki tane sınıf var. Thread'ler işletim sistemi seviyesinde kodun parallel çalışmasını sağlarken, Fiber ise parallel çalışacak kodların manuel olarak planlamasının yapılmasına izin verir. Guild'ı ise bu ikisinin birlikte uygulanması ile ortaya çıkar. Şöyle diyebiliriz; Bir Guild içerisinde en az bir Thread ve onun da içerisinde en az bir Fiber bulunur. Guild içerisindeki Thread'ler paralel çalışamazken, farklı Guild'dekiler çalışabilir.

Bir Guild'deki mutable bir obje başka bir Guild'den obje tarafından okunamaz ya da değiştirilemez. Bu sebeple Guild'lerin parallel çalışması sağlanır. `Guild::Channel` ile bunu yapmak da mümkündür. Bu yöntem ile bir obje diğer Guild'de kopyalanır ve ilgili Guild'e gönderilir.

Ractor (Guild) ile alakalı daha fazla bilgiye ve örneğe buradan ulaşabilirsiniz. [https://github.com/ko1/ruby/blob/ractor_parallel/doc/ractor.md](https://github.com/ko1/ruby/blob/ractor_parallel/doc/ractor.md)

Sonuçta bu özellik ile artık Ruby'de thread-safe konularına kafayı takmadan paralel işler yapabileceğiz gibi duruyor.

## JIT

Uzun adi Just In Compile olan JIT Ruby programlarının makine kodlarına dönüştürür. Diğer derleyicilerden farklı olarak bütün kod derlemez sadece kodun değiştirilen yerlerini derler. JIT ne kadar hızlanma getirir ve neler götürür bu konuda bazı çalışmalar [https://eregon.me/blog/2016/11/28/optcarrot.html](https://eregon.me/blog/2016/11/28/optcarrot.html) olsa da yorumlanan bir Ruby'den daha hızlı olacağı kesin.

JIT'in avantajı kadar yanında getirdiği ve muhtemelen geliştirilecek bir takım dezavantajları da var. Bunlardan biri memory kullanımı diğeri ise warmup süresinin uzunluğu.

Bildiğim kadarı ile JIT şuan Ruby 2.6'da var ve `-jit` flag'i ile kullanılabiliyor. Ruby on Rails gibi büyük gem'lerde henüz kullanılmıyor diye biliyorum ancak daha küçük işler için tercih edilebilir.

JIT konusu hakkında çalışan Takashi Kokubun, Ruby Rouges podcast'ine konuk olmuş. Daha fazla detayı [buradan](https://devchat.tv/ruby-rogues/rr-470-performance-improvement-of-ruby-3-0-jit-with-takashi-kokubun/) dinleyebilirsiniz.

## Type Profiler / Static Type Analysis

Tip kontrolü ve statik tipli bir dil olmaya doğru giden Ruby, bu özelliği ile beni de heyecanlandırıyor. Dinamik bir dil olan Ruby'nin statik bir dil olma yolunda attığı adımlardan biri RBS [https://github.com/ruby/rbs](https://github.com/ruby/rbs) dili diyebiliriz. RBS, Ruby 3 için bir tip imzalama dili. Type'ları olan kodlar artik RBS dosyaları içine yazılmaya başlıyor ki bu da mevcut Ruby kodunun karışmasını engelliyor.

Aslında şuan Sorbet projesi ile type check yapılabiliyor. Sorbet ve Ruby geliştiricileri RBS için birlikte işler çıkarmaya çalışıyorlar.

Herkes RBS kullanmak zorunda değil tabiki ama tiplerin ve tip kontrolünün gelmesi Ruby'e daha fazla güç ve çeşitlilik katacaktır diye düşünüyorum. Yazılan kodun kalitesi ve hata oranı ne kadar azalsa da dinamik bir dilde program yazmak, statik bir dile göre her zaman daha hızlı olacağı da aşikar.

## Heap Compaction (GC Improvement)

Ruby'de garbage collection gibi bellek yönetim işleri ile pek uğraşmaya gerek yok. Zaten en güzel yanı da bu değil mi? :) Ancak işler karmaşıklaştıkca ve programlar büyüdükce programlama dillerinin de kendi garbage collector'larında geliştirmeler yapması gerekir.

Ruby 2.7 ile aslında şuan kullanılabilen heap compaction özelliği ile Ruby'de garbage collector daha verimli bir hale geldi. Bu özellik sayesinde program için memory'de kullanılmak istenen alana heap denir. Bu heap objeler için kullanıldikca veya heap içindeki objeler serbest kaldıkca memory içerisinde boşluklar oluşur ve memory dağınık bir hale gelir. Ayrılan bütün hafızayı birlikte kullanmaktansa heap içindeki kullanılan memory sıkıştırılır boşluklar da değerlendirilmiş olur.


## Pattern Matching

Ruby 2.7 ile hayatımıza giren bu özelliğe pek ısındım diyemem. Ama seveni de vardır elbet. Elixir kullananlar zaten bu özelliğe aşinadırlar. Küçük bir örnekle anlatacak olursak

```ruby
[name, surname, 20] = [:foo, "bar", 20]
name #=> :foo
surname #=> "bar"
```

ancak eşitliğin iki tarafındaki pattern ayni olmadığı vakit sorun çıkacaktır. Örneğin `[name, surname, 30] = [:foo, "bar", 20]` bu şekilde yaparsak hata alacağız.

Pattern Matching işimize daha çok if veya when statement'larda işimize yarayacak gibi duruyor. Daha fazlası Hash'lerde ve Array'lerde de bu özellik çokça kullanılabilir. Ancak tehlikeli olan şu ki yeni syntax biraz kafa karıştırıcı ve alışması zor olabilir. Bazı karışık condition'lar için yararlı gibi duruyor ancak kullanımı o kadar kolay olduğunu söyleyemem.

### Right Assignment

Neden diye sordurttan bir özellik daha. Soldan sağa kod okumaya alıştığımız için bu özellik bizlere biraz zor gelecektir.

```ruby
  'hello' => foo # This is equivalent to foo = 'hello'
```

Burada çok anlamlı değil ama şu örnek için güzel duruyor.

```ruby
def full_name
  ['foo', 'bar']
end

full_name => name, surname
```

aslında burada yapılmak istenden `name, surname = full_name`

### Sonuç

Tüm bunlar tabiki bu kadar az değil. Çok fazla detayın olduğu ve dikkatli bakıldığında hepsi Ruby'e bir şekilde birşeyler katacak özellikler. Benim şahsi fikrim Ruby zaten rahat yazılabilen ve okunabilen bir dil. Bazı performans problemleri de halledildikten sonra daha iyi bir dil olacağı kesin. Ruby 3 ile umarim performansa yönelik geliştirmelere daha fazla önem verirler.

Sevgiler.

*Referanslar:*

- [https://bugs.ruby-lang.org/issues/17100](https://bugs.ruby-lang.org/issues/17100)
- [https://olivierlacan.com/posts/concurrency-in-ruby-3-with-guilds/](https://olivierlacan.com/posts/concurrency-in-ruby-3-with-guilds/)
- [https://engineering.appfolio.com/appfolio-engineering/2017/12/26/ruby-3-and-jit-where-when-and-how-fast](https://engineering.appfolio.com/appfolio-engineering/2017/12/26/ruby-3-and-jit-where-when-and-how-fast)
- [https://developer.squareup.com/blog/the-state-of-ruby-3-typing/](https://developer.squareup.com/blog/the-state-of-ruby-3-typing/)
- [https://stackify.com/how-does-ruby-garbage-collection-work-a-simple-tutorial/](https://stackify.com/how-does-ruby-garbage-collection-work-a-simple-tutorial/)
- [https://www.toptal.com/ruby/ruby-pattern-matching-tutorial](https://www.toptal.com/ruby/ruby-pattern-matching-tutorial)
- [https://bugs.ruby-lang.org/issues/15921](https://bugs.ruby-lang.org/issues/15921)
- [https://blog.saeloun.com/2020/08/31/ruby-adds-experimental-rightward-assignment.html](https://blog.saeloun.com/2020/08/31/ruby-adds-experimental-rightward-assignment.html)