---
layout: post
title: Ruby 2.4 Hash#compact
tags: [ruby]
---

Merhaba. Ruby 2.4 yazılarına devam ediyoruz.

Ruby'de nil value'e sahip bir key ile hash oluşturduğunuzda Ruby bu değeri hash'den kaldırıyor.

```ruby
> hash = {:name=>"ender", :age=>nil}
#=> {:name=>"ender"}
```

Active Support ile value'su nil olan key'leri dönmesini Hash#compact ile sağlayabiliyorsuz. Bu özellik artık Ruby 2.4 ile birlikte geliyor.

```ruby
> hash = {:name=>"ender", :age=>nil}
> hash.compact
#=> {:name=>"ender", :age=>nil}
```

Sevgiler.

<small>*Referans*</small>

* [https://blog.bigbinary.com/2016/10/24/hash-compact-and-hash-compact-now-part-of-ruby-2-4.html](https://blog.bigbinary.com/2016/10/24/hash-compact-and-hash-compact-now-part-of-ruby-2-4.html)