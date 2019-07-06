---
layout: post
title: Ruby 2.4 Enumerable#sum
tags: [ruby]
---

Merhaba! [Ruby 2.4 previewleri](https://www.ruby-lang.org/en/news/2016/09/08/ruby-2-4-0-preview2-released/) bundan yakın bir zaman önce çıktı ve bir takım yenilikler ile birlikte geldiler. Kısa kısa Ruby 2.4 ile gelen yeniliklerden ve değişikliklerden bahsedelim.

Bir Array'de ya da Hash'de elementleri toplamak yapmak için Active Support'da tanımlı olan [Enumerable#sum](https://github.com/rails/rails/blob/3d716b9e66e334c113c98fb3fc4bcf8a945b93a1/activesupport/lib/active_support/core_ext/enumerable.rb#L2-L27)'ı kullanıyoruz.

```bash
> [1,2,3].sum
#=> 6

> ['e', 'a', 'y'].sum
#=> "eay"
```

Ancak bundan sonra bu metotu Ruby 2.4 birlikte Active Support'a ihtiyacımız kalmadan kullanabileceğiniz. Fakat sadece integer için geçerli olan bu metot Active Support'daki gibi string ya da karakteler için geçerli değil. Üzülmeyelim bu metota parametre geçilebildiği için eğer işimiz sayılar ile değilse,

```bash
> ['e', 'a', 'y'].sum('')
#=> "eay"
```

yaparak işimizi görebiliriz.

Sevgiler.


<small>*Referans*</small>

* [https://blog.bigbinary.com/2016/11/02/ruby-2-4-introduces-enumerable-sum.html](https://blog.bigbinary.com/2016/11/02/ruby-2-4-introduces-enumerable-sum.html)