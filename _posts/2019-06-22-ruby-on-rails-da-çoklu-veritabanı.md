---
layout: post
title: Ruby on Rails'da çoklu veritabanı
date: 2019-06-22 20:01 +0300
thumbnail: octocat.png
tags: [ruby_on_rails]
---

Merhaba. Ruby on Rails'ın son versiyonu olan 6. versiyonu ile gelen [**multiple database**](https://edgeguides.rubyonrails.org/active_record_multiple_databases.html) özelliği ile artık çoklu veritabanı yönetimi yapılabiliyor.

Bu durumun birçok anlamı var;

- Bir veritabanının replikasını oluşturup, onlar üzerinde çalışmalar yapabilmek.
- Birincil ve replika veritabanları arasında HTTP metotları ile otomatik geçişler yapabilmek.
- Çoklu veritabanlarında Rails'ın task'larını çalıştırabilmek.

Bunların yanında ise henüz desteklenmeyen özellikler arasında ise **sharding** ve replikalar arası **load balancing** başta geliyor.

Çoklu veritabanı için ayarları yapmak Rails 6'da çok kolay. Daha önce Ruby on Rails kullananlar bilecektir ki veritabanı ayar dosyası `config/database.yml`'da tutulur. Bu dosya üzerinde yeni bir takım eklemeler ile uygulamanız için çoklu veritabanlarını tanımlayabilirsiniz.

```yml
production:
  primary:
    database: my_primary_database
    user: root
    adapter: mysql
  primary_replica:
    database: my_primary_database
    user: root_readonly
    adapter: mysql
    replica: true
  cities:
    database: cities_database
    user: cities_root
    adapter: mysql
    migrations_paths: db/cities_migrate
  cities_replica:
    database: my_cities_database
    user: cities_readonly
    adapter: mysql
    replica: true
```

Bu noktada birkaç konuya değinmek ve dikkat etmek gerekiyor. Öncelikle bir veritabanı eğer replika bir veritabanı ise `replica: true` yazmak gerekiyor. Bunun yanında replika veritabanlarının adlarının aynı olması şart değil ancak aynı veri üzerinden çalışmalar olacağı için adlarının aynı olmasının faydası var.

`migration_paths` Rails'da veritabanı **migration**'larının yerini söylemek için belirtmemiz gerekiyor. `cities ` veritabanı bir
**migration** çalıştırmak istersek ``--database`` opsiyonu ile hangi dosyada çalışmaların olacağını belirtebiliyoruz.

`
$ rails g migration CreateTowns name:string --database cities
`

**Model** seviyesinde ise birkaç ayara ihtiyac duyuyoruz. Burada önemli olan hangi veritabanlarının ne görevler yapacağı.

```ruby
class CitiesBase < ApplicationRecord
  self.abstract_class = true

  connects_to database: { writing: :cities, reading: :cities_replica }
end
```


**ApplicationRecord**'u da güncellememiz gerekiyor.

```ruby
class ApplicationRecord < ActiveRecord::Base
  self.abstract_class = true

  connects_to database: { writing: :primary, reading: :primary_replica }
end
```

`rails -T` komutu ile de bütün **task**'ları görebilirsiniz. Hangi veritabanında çalışma yapmak istiyorsanız, adını sona ekleyerek yapabilirsiniz. `rails db:migrate:status:cities`

Ruby on Rails ile çoklu veritabanı kullanımı henüz RC versiyonda olasa da projelerde kullanılabilir durumda. Github ekibi tarafından geliştirilen ve şu an kullanılan bu özellik Ruby on Rails'ın gücünü daha da artıracak gibi duruyor.


Sevgiler.


<small>**Referanslar:**</small>
<br/>
<small>*https://edgeguides.rubyonrails.org/active_record_multiple_databases.html*</small>
<small>*https://www.citusdata.com/blog/2019/05/23/rails-6-multiple-databases/*</small>
