---
layout: post
title: Block, Proc, Lambda
tags: [ruby]
---

Merhaba! Ruby'nin en güçlü özelliklerinden biri bloklardır. Diğer dillerin aksine Ruby'de blokları anlamak ve kullanmak kolay ve basittir.

Ruby'de bloklarını kullanmak için iki yöntemimiz var,

* ```do``` ve ```end``` arasına kodlarımızı yazmak.
* ```{}``` bu iki parantez arasına kodlarımızı yazmak.

Örnek vericek olursak,

```bash
> my_array = [2,4,6]

> my_array.map! do |m|
>  m ** 3
> end
=> [8, 64, 216]
```

Bu işlemi tek satırda parantezler içinde de yapmak mümkündür.

```bash
> my_array = [2,4,6]
> my_array.map! { |m| m ** 3 }
=> [8, 64, 216]
```

Ruby'de hazır block'ların dışında kendi blocklarımızı da yazabiliriz.

```ruby
def change(&block)
 self.each_with_index do |e, i|
   self[i] == block.call(e)
 end
end

[1,2,3,4].change do |e|
  e ** 2
end

=> [1,4,9,16]
```
*Örnek, Sıktı Bağdat'ın Ruby kitabından alınmıştır.*

Yukarıdaki örnekte ```change``` metotuna block'u parametre olarak gönderiyoruz. Ruby'de bir metota block'un parametre olarak geçtiğini belirtmek için ```&``` kullanıyoruz. Metot içinde ise gönderilen block'u kullanmak için ```call``` metotunu çağırıyoruz.

```yield``` Ruby'de bir anahtar kelimedir ve kullanımı ```block``` gibidir. Ancak aralarında ufak farklılıklar vardır. Bunlardan biri yield'i çağırırken call metotunu çağırmaya ihtiyacımız yoktur. Ayrıca yield, block'a göre performans olarak daha iyidir.

```ruby
def name
  yield
end

puts name { puts "ender" }

=> ender
```

Parametre geçirmek yield için de geçerlidir. Aynı block'da olduğu gibi yield da parametre alabilir.

```ruby
def sum(n)
  sum = 0
  n.times {|i| sum += yield(i+1)}
  sum
end

puts sum(10) {|i| i}

=> 55
```
*Örnek, Sıktı Bağdat'ın Ruby kitabından alınmıştır.*

Ruby'de block'ları birer nesne gibi kullanmak için ```Proc``` nesnesinden yararlanıyoruz. ```proc = Proc.new {}``` şeklinde çağırarak işlemlerimize başlıyoruz.

Proc nesnesini çağırmak için block'larda olduğu gibi call metotununu kullanabiliriz.

```bash
> proc = Proc.new { puts "Hello from Proc." }
> proc.call
=> Hello from Proc.
```

```Lambda``` Proc sınıfından türeyen bir nesnedir. Lambda'yı oluşturmak için Ruby'de genellikle ```my_lambda = -> {}``` kullanılır. Bu kullanım Lambda nesnesi oluşturmamıza yarar.

```bash
> mult  = lambda {|x,y| x * y }
> mult.call(2,3)
=> 6
```

```bash
> wtp = -> { puts "What's up?" }
> wtp.call
=> What's up?
```

Ruby'de Block, Yield, Proc ve Lambda kullanımları oldukça yaygın olduğunu unutmayalım. Özellikle kodları daha okunur ve kendini tekrar etmeyecek şekilde yazmak istediğimizde bu özellikleri kullanabiliriz.

Sevgiler.