---
layout: post
title: Ruby State Design Pattern
tags: [ruby]
---


Merhaba. Bugün **State Design Pattern**'ın Ruby'deki kullanımından bahsetmek istiyorum.


State design pattern, behavior pattern altında tanımlanır. Bu pattern'da bir nesnenin farklı durumlarda farklı şekillerde çalışmasını düzenlenir. Yani bir nesnenin durumu değiştiğinde o nesnenin çalışma şekli değişir. Bu duruma en güzel örnek trafik ışıklarını verebiliriz. Görme engelliler için trafik ışıklarını düşünün, kırmızı, sarı ve yeşil yandığında, durun, bekleyin ve geçin diyor. Işıklar arası geçişlerde ise birkaç saniye boşluk vererek, diğer ışığa geçiyor. Bu durumda ışıkların state'leri değişiyor ve yapacakları eylemler de state'lere göre belli oluyor.

State design pattern'ın 3 ana component'dan oluşuyor.

- Context Class: Mevcut durumu bilen class.
- State Class: tanımlanmış state'lerin yapacağı işleri bilen class.
- State class'ından türetilmiş bütün durumlar için olan class'lar.

Peki bu durumun yararı nedir? Bütün state'ler kendi durumlarını yapacakları işi ve sonraki state'i biliyor. Mevcut state'in ne olduğuyla ilgilenmiyor. Aslında bu design yerine if/else de kullanıbilir ancak hem mimari olarak hem de kodu okumak anlamında if/else kullanmak daha zor olacaktır.

Şimdi bu tanımları Ruby'de trafik ışıkları örneği ile nasıl yazabiliriz, ona bakalım.

Öncelikle bir context class'ına ihtiyacımız var. TrafficLight class'ını context olarak belirliyoruz.

```ruby
class TrafficLight
  def initialize
    @state = nil
  end

  def next_state(klass = Green)
    @state = klass.new(self)
    @state.beep
    @state.start_timer
  end
end
```

Sonrasında ise bir State class'ı tanımlıyoruz.

```ruby
class State
  def initialize(light)
    @light = light
  end

  def beep
  end

  def next_state
  end

  def start_timer
  end
end
```

Green state ve diğerleri için şu şekilde bir class tanımlayabiliriz.

```ruby
class Green < State
  def beep
    puts "Color is now green"
  end

  def next_state
    @light.next_state(Yellow)
  end

  def start_timer
    sleep 5; next_state
  end
end
```

```ruby
class Yellow < State
  def beep
    puts "Color is now yellow"
  end

  def next_state
    @light.next_state(Red)
  end

  def start_timer
    sleep 5; next_state
  end
end
```

```ruby
class Red < State
  def beep
    puts "Color is now red"
  end

  def next_state
    @light.next_state(green)
  end

  def start_timer
    sleep 5; next_state
  end
end
```

Artık her state ne yapacağı ve kendisinden sonra hangi state'in geleceğini biliyor.

Daha gerçekçi ve çalışan örnek için [RubyWarrior](https://github.com/ryanb/ruby-warrior)'ı inceleyebilirsiniz.

Eğer Ruby projelerinizde state machine kullanmak istiyorsanız [AASM](https://github.com/aasm/aasm) deneyebilirsiniz. Bu gem ile state'lerin durumlarını takip edebilir, callback'ler ile event'leri yönetebilirsiniz.

Sevgiler.



<small>**Referanslar:**</small>

<small>*https://www.rubyguides.com/2018/12/state-machines/?tl_inbound=1&tl_target_all=1&tl_form_type=1&tl_period_type=1*</small>
