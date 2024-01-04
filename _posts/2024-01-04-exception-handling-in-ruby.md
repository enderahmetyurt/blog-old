---
layout: post
title: Exception Handling in Ruby
date: 2024-01-04 17:31 +0300
summary: Ruby'de exception handling
tags: [ruby]
---

Merhaba. Biz yazılım geliştiricileri her ne kadar programları genelde kötü senaryoları düşünerek yazsak da bazen işler hiç beklemediğimiz gibi gidebiliyor.

Bu gibi durumlarda tabiki yazdığımız kodlar da istediğimiz şekilde çalışamayabiliyor ya da hiç çalışamıyor ve kodun asıl yapması gereken, vermesi gereken çıktı
ortaya çıkamayabiliyor. Bu durumların üstesinden gelebilmek için programlamada genel bir konsept olan ve çokça kullanılan
**Exception Handling** kavramının Ruby'de nasıl olduğundan biraz bahsedeyim.

Ruby'de herşey birer class olduğu için exception'lar için ayrılmış bir class da var. `Exception` class'ı bize Ruby'de hataları yakalamamız ve bu durumlarda
neler yapmak istiyorsak ona yardım ediyor.

```ruby
begin
  # ...
rescue => e
  # ...
end
```

Yukarıda görülen basit Ruby kod bloğuna `begin-rescue` hata yakalama bloğu deniyor. `begin` ile başlayan kısıma çalışmasını istediğiniz kodları yazıyorsunuz,
eğer bir hata olursa `rescue` kısmı hatayı yakalayıp kendi bloğundaki kısımdaki kodu çalıştırıyor.

```ruby
begin
  3 / 0
rescue ZeroDivisionError => e
  puts "#{e.class}: #{e.message}"
end
```

Normal şartlarda bir integer, sıfıra bölünmeyeceği için program hata fırlatıcak ve `rescue` ile yakalanan hata ile `ZeroDivisionError: divided by 0`
mesajı ekrana basılacak.

Bunu biraz daha gerçek örnek senaryosuna çevirirsek, mesela kullanıcıdan dışardan iki sayı alıyoruz ve ilk sayıyı, ikinci sayıya bölüyoruz.

```ruby
puts "Enter first number"
number1 = gets.chomp.to_i
puts "Enter second number"
number2 = gets.chomp.to_i

result = number1 / number2

puts "#{number1}/#{number2}=#{result}"
```

<img src="https://enderahmetyurt.com/public/images/error-1.png">

Normal şartlar bu kod güzel güzel çalışır ancak ya ikinci sayı yerine sıfır girilirse?

<img src="https://enderahmetyurt.com/public/images/error-2.png">

Kodumu hata fırlatır ve program kapanır. Bunu aşabilmek için kodu güncelemek istersek

```ruby
puts "Enter first number"
number1 = gets.chomp.to_i
puts "Enter second number"
number2 = gets.chomp.to_i

begin
  result = number1 / number2
  puts "#{number1}/#{number2}=#{result}"
rescue ZeroDivisionError => e
  puts "Please don't enter 0"
end
```

<img src="https://enderahmetyurt.com/public/images/error-3.png">

Sıfır girilse bile kodumuz çalışır ama ekrana bizim istediğimiz hata mesajı yazılır.

Basit olarak Ruby'de hata yakalamayı bu şekilde yapabiliriz. Tabiki bu iş, bu kadar ile kalmıyor. Daha farklı yöntemler ile de Ruby'de hata yakalama
genişletilebilir.

Sevgiler ❤️