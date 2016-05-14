---
layout: post
title: Debugging with Pry
tags: [ruby]
---

Merhaba, Kod yazarken bir bir şeylerin yolunda gitmediği veya kod içinde neler olduğunu anlamak istediğimiz anlar oluyor. Bu gibi durumlarda debugging yapıyoruz. Ruby'de eğer IDE kullanıyorsunuz debugging yapmak bir nebze kolay olabiliyor. Ancak benim gibi Sublime vb bir text editör kullanıcısıysanız imdadımıza PRY yetişiyor.

Pry nedir? Pry, aslında IRB yani Interactive Ruby Shell alternatif olarak çıkmış bir runtime developer console'dur. Kendisi bir RubyGem'i olup, bilgisayarınıza kurmanızla IRB'nin bazı zamanlarda yeterli olmadığı bir çok özelliği size sunar. Ancak bugünün konusu PRY'dan çok PRY ile nasıl debugging yapılır o olacak.

Her şeyden önce PRY ile debugging yapabilmek için PRY RubyGem'nin makinemizinde kurulu olması gerekiyor.

```bash
> gem install pry
```

Sonrasında ise artık kodumuzda hangi noktada durmasını istiyorsak oraya ```binding.pry``` komutu yazıyoruz. Örneğin,

```ruby
# person.rb
require 'pry'

class Person
  def say_hello(name)
    binding.pry
    puts "Hello #{name}"
  end
end

p = Person.new
puts p.say_hello("ender")
```

şeklinde bir Ruby kodu yazıyoruz ve konsoldan kodu çalıştırdığımızda ise

```bash
> ruby person.rb
    4: def say_hello(name)
 => 5:   binding.pry
    6:   puts "Hello #{name}"
    7: end
```
şeklinde bir çıktı alıyoruz. Bu anda artık ```name``` değişkenini çağırabilir ve içinde ne olduğunu görebiliriz.

```bash
[1] pry(#<Person>)> name
=> "ender"
```

Sonrasında ise ```exit``` diyerek PRY'ı kapatıyoruz.

Debugging yaparken kod içinde ileri gitmek, bir metotudun içine girmek ya da onu atlamak isteyebiliriz. PRY kendi hali ile buna cevap vermiyor. Ancak [pry-debugger](https://github.com/nixme/pry-debugger) gem'i ile bunu da halledebiliyoruz.

```bash
> gem install pry-debugger
```

Yukardaki örnek üzerinden devam edecek olursak,

```ruby
require 'pry'

class Person
  def say_hello(name)
    binding.pry
    puts "Hello #{name}" # Execution will stop here.
    puts "Age: " # Run 'step' or 'next' in the console to move here.
    ask_age(name)
  end

  def ask_age(name)
    puts "How old are you? #{name}.capitalize"
  end
end

p = Person.new
puts p.say_hello("ender")
```

```next``` komudu ile kodun durduğu yer bir alt satıra inebiliriz. Pry-debugger gem'i bize ```step```, ```finish``` ve ```continue``` gibi fonksiyonlar da sağlamaktadır. Daha fazla bilgiye Github'daki Readme'den ulaşabilirsiniz.

Sevgiler.
