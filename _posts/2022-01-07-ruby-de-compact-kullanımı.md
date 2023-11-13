---
layout: post
title: Ruby'de Array#compact Kullanımı
date: 2022-01-07 14:02 +0300
tags: [ruby]
---

Merhaba 👋 Ruby'de Array'ler ile çalışırken test yazmadığımız zaman sonuçları bize pahallıya patlayabilecek hatalar yapabiliyoruz. Bu hatalardan en çok yapılan bir tanesine değinmek ve ne yapabiliriz diye biraz anlatmak istiyorum.

Senaryomuz şu şekilde olsun. Elimizde içinde Hash'ler olan bir Array var ve biz bu Array içinden Hash'lerin belirli key'lerini alıp yeni bir Array yapmak istiyoruz.

```ruby
people = [
    { fullname: 'Rees Mcloughlin', age: 11 },
    { fullname: 'Kylo Hale', age: 15 },
    { fullname: 'Aadam Bass', age: 22 },
    { fullname: 'Mikhail Healy', age: 30 },
]
```

Elimizdeki bu veriden yetişkin olanların adlarını almak istiyoruz. Aklımıza hızlıca şu yöntem gelebilir.

```ruby
ADULT_AGE = 18

adult_people = []

people.each do |person|
  next if person[:age] < ADULT_AGE

  adult_people <<  person[:fullname]
end

adult_people
# => ["Aadam Bass", "Mikhail Healy"]
```

Bu yöntem yanlış değil ancak daha `map` ile daha sade yazılabilir.

```ruby
ADULT_AGE = 18

adult_people = people.map do |person|
  next if person[:age] < ADULT_AGE

   person[:fullname]
end

adult_people
# => [nil, nil, "Aadam Bass", "Mikhail Healy"]
```

Kodu `map` ile yazmak kodumuzu ne kadar okunaklı yapsa da `each` gibi belli bir duruma uymayan değerleri array dışında bırakmıyor, `nil` objesi olarak array'e ekliyor. Bu pek istediğimiz bir durum değil. Çözüm için Ruby'deki `Array#compact` metotunu kullanabiliriz. `map` ile bir dizi içersinde dönüyorsak ve dizideki `nil`'leri kaldırmak istiyorsak `compact` ile bunu yapabiliriz.

```ruby
ADULT_AGE = 18

adult_people = people.map do |person|
  next if person[:age] < ADULT_AGE

   person[:fullname]
end.compact

adult_people
# => ["Aadam Bass", "Mikhail Healy"]
```

Sevgiler 🙋‍♂️

<small><b>Referanslar:</b> https://kaiwern.com/posts/2018/03/28/using-ruby-next-in-map/</small>
