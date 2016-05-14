---
layout: post
title: Ruby Enumerable
tags: [ruby]
---

Merhaba! Bugün Ruby'de hemen hemen her gün kullandığımız ```Enumerable``` metotlarından bahsetmek istiyorum. Bu yazıda kullanılan örnekleri daha iyi anlayabilmek için öncelikle Ruby'de Array ve Hash kavramlarını biraz olsun bilmeniz gerekiyor.

Ruby'de ```Enumerable``` modülüne ihtiyaç duymamızın nedeni colletion'lar üzerinde daha hızlı ve kolay çalışmalar yapabilmektir. Örneğin, bir collection içindeki bütün tek sayıları getirmek için döngüler ve if bloklarını kullanabiliriz. Ancak bunun yerine ```Enumerable``` modülünde tanımlı metotlardan yardım alabiliyoruz.

```Enumerable``` ın metotlarını aşağıdaki gibi sıralayabiliriz.

```bash
> Enumerable.instance_metots.sort
=> [:all?, :any?, :chunk, :collect, :collect_concat, :count, :cycle, :detect, :drop, :drop_while, :each_cons, :each_entry, :each_slice, :each_with_index, :each_with_object, :entries, :find, :find_all, :find_index, :first, :flat_map, :grep, :group_by, :include?, :inject, :lazy, :map, :max, :max_by, :member?, :min, :min_by, :minmax, :minmax_by, :none?, :one?, :partition, :reduce, :reject, :reverse_each, :select, :slice_before, :sort, :sort_by, :take, :take_while, :to_a, :to_h, :zip]
````

Bunların bazılarından bahsetmek gerekirse.

## #each_with_index
```each``` ile bir collection içindeki elemanları tek tek gezebiliriz.

```bash
> [1,2,3].each { |num| print "Number: #{num} " }
=> Number: 1 Number: 2 Number: 3
```
Bu elemanların tek tek index'lerine ulaşmak için ise ```each_with_index```i kullanabiliriz.

```bash
> [9,8,7].each_with_index do |e,i|
>   puts "Index: #{i} Number: #{e}"
> end
=> Index: 0 Number: 9
=> Index: 1 Number: 8
=> Index: 2 Number: 7
```

## #map
Bir collection içindeki elemanlar üzerinde çalışmaklar yapmak istiyorsak ```map``` kullanmak en iyisi olacaktır.

```bash
> [5,10,20].map{|e| puts e**2}
=> 25
=> 100
=> 400
```


## #select
Bazı durumlarda bir collection içinden belli bir duruma bağlı olarak değeri ya da değerleri almak isteyebiliriz. Bu durumda ```select``` kullanırsak kodumuzu daha anlaşılır yazmış oluruz.

```bash
> (1..20).to_a.select{|n| n.odd?}
=> [1, 3, 5, 7, 9, 11, 13, 15, 17, 19]
```

1 ile 20 arasındaki tüm tek sayıları ekrana bastık.


## #reject
Seçmenin dışında bir collection'dan çıkarmak istediğimiz elemanlarımız da olabilir. Bu gibi durumlarda ise ```reject``` kullanabiliriz.

```bash
> (1..20).to_a.reject{|n| n.odd?}
=> [2, 4, 6, 8, 10, 12, 14, 16, 18, 20]
```

Yukarıda gördüğünüz gibi 1 ile 20 arasındaki tek sayıları çıkardım ve yeni array'im artık 1 ile 20 arasındaki çift sayılar oldu.

## #any?
Bu metot bize duruma göre true ya da false bir boolean değer dönecektir. Eğer bir collection içinde bir duruma göre bir elemanın bulunup, bulumadığını kontrol etmek istiyorsak ```any?``` kullanabiliriz.

```bash
> (1..20).to_a.any?{|n| n>20}
=> false
```

## #group_by?
Sanırım kullanımı en karizmatik metot bu olabilir. Elimizdeki bir collection'ı istediğimiz bir duruma göre gruplamamıza yarıyor ve bize bir Hash dönüyor.

```bash
> ["ali", "mehmet", "osman", "ayşe", "emine"].group_by(&:length)
=> {3=>["ali"], 6=>["mehmet"], 5=>["osman", "emine"], 4=>["ayşe"]}
```

Yukarıda da görüldüğü gibi içinde String'ler olan Array'imizi String'lerin uzunluklarına göre grupladık.

## #sort
Adı üstünde :)

```bash
> ["ali", "mehmet", "osman", "ayşe", "emine"].group_by(&:length).sort
=> [[3, ["ali"]], [4, ["ayşe"]], [5, ["osman", "emine"]], [6, ["mehmet"]]]
```

Tabiki bütün kullanımlar bu kadar değil. Daha fazlası da var. Belki başka bir yazıda daha detaylı olarak diğer kullanımlardan örnekler verebilirim.

Sevgiler.
