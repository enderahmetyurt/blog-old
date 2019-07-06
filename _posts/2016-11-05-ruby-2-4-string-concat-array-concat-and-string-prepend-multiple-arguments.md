---
layout: post
title: Ruby 2.4 String#concat, Array#concat and String#prepend Multiple Arguments
tags: [ruby]
---

Merhaba. Ruby 2.4'den önceki sürümler String ve Array için tanımlı #concat metotu birleştirme işlemini parametre almadan yapıyor.

```ruby
> my_string = "Hello"
> my_string.concat(" World")
#=> "Hello World"

> my_array = ['a', 's', 'l']
> my_array.concat(['e'])
#=> ["a", "s", "l", "e"]
```

Aynı şeklide String için tanımlı #prepend metotu da başa ekleme işlemi yapıyor.

```ruby
> my_string = "World"
> my_string.prepend("Hello ")
#=> "Hello World"
```

Bu sürümler için bu metotlara parametre vermeyi denersek,

```ruby
> my_string = "Hello"
> my_string.concat(" World", "!")
#=> ArgumentError: wrong number of arguments (2 for 1)
```

Argüman hatası alıyoruz.

Ruby 2.4 ile artık bu metotlara çoklu argüman geçebiliriz.

```ruby
> my_string = "Hello"
> my_string.concat(" World", "!")
#=> "Hello World!"

> my_array = ['a', 's', 'l']
> my_array.concat(['e'], ['f'])
#=> ["a", "s", "l", "e", "f"]

> my_string = "World!"
> my_string.prepend("Ruby,"," Hello ")
#=> "Ruby, Hello World!"
```

Sevgiler.

<small>*Referans*</small>

* [https://blog.bigbinary.com/2016/10/28/string-array-concat-and-string-prepend-take-multiple-arguments-in-ruby-2-4.html](https://blog.bigbinary.com/2016/10/28/string-array-concat-and-string-prepend-take-multiple-arguments-in-ruby-2-4.html)
