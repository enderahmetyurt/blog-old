---
layout: post
title: Ruby 2.6 ile Gelenler
tags: [ruby]
---

Merhaba, Ruby'nin yeni versiyonu 2.6 25 Aralık 2018'de yayınlanacak. Öncesinde preview'dan yeni gelen özelliklerin bazılarına yakından bakalım.

### Array#union & Array#difference
Array class'ına gelen iki metot ile iki array arasındaki farkı görebilir ve onları birleştirebiliriz.

```
[1,2,3,4,5].difference([3])
=> # [1, 2, 4, 5]
[1,2,3,4,5].union([5,6,7])
=> # [1, 2, 3, 4, 5, 6, 7]
```

### Endless Ranges
Diğer versiyonların aksine 2.6 ile birlikte endless range'in yazımı değişti. Artık yeni endless range aşağıdaki gibi olacak.

```
(1..)
```

### Enumerable::ArithmethicSequence
Artık iki yeni ArithmethicSequence metodumuz var. Bunlar  `Range#step` ve `Numeric#step` örnekleyecek olursak;

```
(1..10).step(2) == (1..10).step(2)
=> # false - Ruby 2.5 (and older)
```

```
(1..10).step(2) == (1..10).step(2)
=> # true - Ruby 2.6
```

### Merge Hash With Multiple Arguments
Hash'leri merge etmek için artık peş peşe `merge` yazmaya gerek yok.

```
a = { a: 1 }
b = { b: 2 }
c = { c: 3 }
a.merge(b, c)
=> # {:a=>1, :b=>2, :c=>3}
```

Sevgiler.
