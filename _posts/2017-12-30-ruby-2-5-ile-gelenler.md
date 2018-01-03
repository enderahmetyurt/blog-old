---
layout: post
title: Ruby 2.5 ile Gelenler
tags: [ruby]
---

Merhaba, Ruby en son versiyonu olan 2.5 ile yeni yıla girmeden karşımıza çıktı. Bu versiyon ile ne gibi özellikler geldi kısaca bakalım.

### Begin/End'e gerek kalmadı

Bu özellik ile artık ```begin end``` block'u yazmak zorunda kalmıyoruz.

```ruby
# 2.4
[1].each do |n|
  begin
    n / 0
  rescue
    puts "rescue"
  else
    puts "else"
  ensure
    puts "ensure"
  end
end

# 2.5
[1].each do |n|
  n / 0
rescue
  puts "rescue"
else
  puts "else"
ensure
  puts "ensure"
end
```

### Backtrace ve error message'ın sıralaması değişti
Hata masajları ve nerede hata olduğunu gösteren mesajlar artık tam tersi şekilde gelmeye başladı. Bu sanırım daha açıklayıcı olacak gibi geliyor bana. Sanırım bu özellikle 2.5 ile gelse bile daha çok deneysel bir özellik olduğu söyleniyor.

```ruby
# 2.4
> puts "hello #{name}"

# NameError: undefined local variable or method `name' for main:Object
#   from (irb):1
#   from /Users/ender/.rvm/rubies/ruby-2.4.2/bin/irb:11:in `<main>'

# 2.5
> puts "hello #{name}"

# Traceback (most recent call last):
#         2: from /Users/ender/.rvm/rubies/ruby-2.5.0/bin/irb:11:in `<main>'
#         1: from (irb):1
# NameError (undefined local variable or method `name' for main:Object)

```

### String#delete\_prefix/delete\_suffix

String sınıfı için iki yeni metot geldi. Gerekli bir metot mu bilemiyorum ama zaman zaman işe yarayabilir gibi duruyor.

```ruby
> "ender".delete_prefix('en')
# => "der"
> "minder".delete_prefix('en')
# => "minder"
> "ender".delete_suffix('er')
# => "end"
> "deriz".delete_suffix('er')
# => "deriz"
```

### Bir başka alias daha
unshift/push'un alias'ı prepend/append artık bizlerle.

```ruby
> array = [3, 4]
> array.prepend(1, 2) # => [1, 2, 3, 4]
> array               # => [1, 2, 3, 4]

> array = [1, 2]
> array.append(3, 4)  # => [1, 2, 3, 4]
> array               # => [1, 2, 3, 4]
```

### Hash#transform\_keys/transform\_keys!
Hash'e yeni ve kullanışlı metotlar geldi. Bu metotlar aşağıdaki gibi çalışıyor.

```ruby
> hash = { a: 1, b: 2 }
> hash.transform_keys { |k| k.to_s }
# => { 'a' => 1, 'b' => 2 }

> hash = { a: 1, b: 2 }
> hash.transform_keys! { |k| k.to_s }
# => { 'a' => 1, 'b' => 2 }

> hash
# => { 'a' => 1, 'b' => 2 }
```

Tabiki bütün özellikler bunlarla sınırlı değil. Bütün detaylara buradan [ulaşabilirsiniz.](https://github.com/ruby/ruby/blob/v2_5_0_preview1/NEWS)

Sevgiler.

