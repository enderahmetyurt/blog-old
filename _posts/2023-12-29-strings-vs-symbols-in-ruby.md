---
layout: post
title: Strings vs Symbols in Ruby
date: 2023-12-29 19:13 +0300
summary: Ruby'de string ve symbol sınıflarının farkları
tags: [ruby, tips]
image: /public/images/symbol.jpg
---

Merhaba! Ruby programlama dilinde, diğer dillerde pek olayan adı `Symbol` olan sınıf vardır.

Ruby'e yeni başlayanlar `String` ve `Symbol`'ü genelde karıştırırlar. Her ne kadar benzer şeyler gibi görünseler de özünde ve kullanımında aslında ikisi de
birbirinden farklı iki Ruby sınıflarıdır.

`String` ve `Symbol`'ler genelde karşımıza hash tanımlamalarında çıkarlar. Ruby'de hash'leri tanımlarken key'leri string olarak verebiliriz.

```ruby
  hash = {
    "name" => "ender",
    "age" => 37
  }
```
<img src="https://enderahmetyurt.com/public/images/hash-ex.png">

Ancak bu kullanım Ruby'nin son sürümlerinde pek tavsiye edilmemektedir. Bu kullanıma ayrıca **hash rocket** da denmektedir.

```ruby
  hash = {
    name: "ender",
    age: 37
  }
```

Yukarıdaki tavsiye edilen key'lerde symbol kullanımı ile hash'lerin okunması ve yazımı daha kolay bir hale gelmiştir.

## Farkı içinde saklı

Aralarındaki temel farka gelecek olursak `Symbol` immutable iken `String` değildir. Yani `Symbol`lerin memory'deki değerleri değişmez.
Bu da Ruby kodumuzun daha hızlı olmasına yarar.

```
> foo = "hello"
> bar = "hello"
> foo.object_id == bar.object_id
  => false
> foo = :hello
> bar = :hello
> foo.object_id == bar.object_id
  => true
```

## Dönüştürmece

`String`'ler ve `Symbol`'ler birbirlerine hızlıca dönüştürebilir ve istenilen durumlarda kullanılabilir.

```
> foo = :hello
 => :hello
> foo.to_s
 => "hello"
> bar = "hello"
 => "hello"
> bar.to_sym
 => :hello
```

Ruby projelerinizde `String` ve `Symbol` istediğiniz gibi kullanabilirsiniz ancak takımınızda bir kullanıma karar verip, herkesin aynı yazım kuralına uymasını
sağlamanızı tavsiye ederim.

Sevgiler. ❤️