---
layout: post
title: Jekyll'da Tag Ekleme
date: 2023-11-14 18:57 +0300
tags: [jekyll, github-pages]
---
Merhaba 🙋 Bir süredir yazı yazamıyordum. Daha doğrusu Medium tarafına yazsam bile bu siteye pek yazı yazmıyordum. Bu siteyi sadece teknik yazılar için kullanmaya karar verdiğim için buraya pek yazı yazamadım. O da benim eksikliğim olsun.

Geçenlerde düşünürken iki tarafta yazmak yerine bir yerde yazılarımı yazayım ve bu yazdığım yer de benim kontrolümde olsun dedim. Böylelikle bu site üzerinde yazılarıma ister hayata dair olsun, ister teknik olsun devam edeyim dedim. Yazıları bir yerde yayımlama kararı verdikten sonra yazılara tag ya da kategory ekleme fikri aklıma geldi. Diğer türlü yazılar ileride karışmaya başlayacak ve okuması, sonradan bulması zor bir hal alacaktı. Ancak kullandığım Jeykll teması tag özelliğini desteklemiyordu (bunu daha önceden biliyordum.) Yeni tema ile uğraşmak istemediğim için var olan temaya nasıl tag özelliği eklerim diye baktım.

Internette bir temaya nasıl tag ya da kategori özelliğinin ekleneceği ile alakalı bolca yazı var. Hatta bazı kütüphaneler de var bu işi yapan. Benim araştırmalarım sonucunda Long Qian'ın [Jekyll Tags on Github Pages](https://longqian.me/2017/02/09/github-jekyll-tag/) başlıklı yazısı hoşuma gitti. Yazıda geçen adımları kendi blog'um için uyguladım ve tag sistemi istediğim şekilde çalışıyor durumuna geldi. Siteyi deploy ettikten sonra yazının orjinalini alıp, biraz da kendi tecrübelerimi ekleyerek Türkçe bir içerik oluşturayım dedim.

Eğer herhangi bir Jeykll blog kullanıyor ve bunu Github Pages'da yayınlıyorsanız tag özelliğini blog'unuza eklemek için adım adım yapılacakları anlatayım.

## 1. Post'lara tag alanı eklemek
Eğer bir post'u `jekyll post "Hello World"` komudu ile oluşturursanız size.

```
layout: post
title: Hello World
date: 2023-11-14 18:57 +0300
```

şeklinde bir Markdown dosyası oluşturacaktır. Buraya `tags` alanını ekleyerek şu şekilde günceleyebilirsiniz.

```
layout: post
title: Hello World
date: 2023-11-14 18:57 +0300
tags: [foo, bar]
```

Tag'leri virgül ile ayırıyoruz. Eğer bir tag bir kelimeden fazlaysa, onu da `-` ile kelimeleri bağlıyoruz. Örneğin, `ruby-on-rails`

## 2. Tag'leri bir Post'ta toparlamak
Tag'leri post içerinde toparlamak için `_site` klasörü altında `tag` klasörüne ihtiyacımız var. Bunun da oluşturabilmesi için `_includes` klasörü altında `collecttags.html` adında bir dosya açıyor ve
içine alttaki kod bloğunu yapıştırıyoruz.

{% raw %}
```liquid
{% assign rawtags = "" %}
{% for post in site.posts %}
  {% assign ttags = post.tags | join:'|' | append:'|' %}
  {% assign rawtags = rawtags | append:ttags %}
{% endfor %}
{% assign rawtags = rawtags | split:'|' | sort %}

{% assign site.tags = "" %}
{% for tag in rawtags %}
  {% if tag != "" %}
    {% if tags == "" %}
      {% assign tags = tag | split:'|' %}
    {% endif %}
    {% unless tags contains tag %}
      {% assign tags = tags | join:'|' | append:'|' | append:tag | split:'|' %}
    {% endunless %}
  {% endif %}
{% endfor %}
```
{% endraw %}

## 3. Tag'leri oluşturmak
`collecttags.html` dosyasındaki kodu çalıştırabilmek için bir yerlerde çağırmamız lazım. `head.html` içinde bunu yapmak ve herhangi sayfa yüklendiğinde halletmek iyi gibi duruyor. O yüzden sizler de
kendi `_includes/head.html` dosyanız içine aşağıdaki kodu koyabilirisiniz.

{% raw %}
```liquid
{% if site.tags != "" %}
  {% include collecttags.html %}
{% endif %}
```
{% endraw %}

## 4. Bir Post'ta Tag'leri göstermek
Post içinde tag'leri görüntüleyebilmek için `_layouts/post` içine aşağıdaki kodu koyuyoruz.

{% highlight html %}{% raw %}
<span>[
  {% for tag in page.tags %}
    {% capture tag_name %}{{ tag }}{% endcapture %}
    <a href="/tag/{{ tag_name }}"><code class="highligher-rouge"><nobr>{{ tag_name }}</nobr></code>&nbsp;</a>
  {% endfor %}
]</span>
{% endraw %}{% endhighlight %}{: .highlight-left }

Burada istediğiniz tasarım değişikliklerini yapabilirsiniz.

## 5. TagPage oluşturmak
Tag'lerin kendi sayfaları olması için `_layouts/tagpage.html` oluşturuyoruz ve alttaki kodu içine yapıştırıyoruz.

{% highlight html %}{% raw %}
---
layout: default
---
<div class="post">
<h1>Tag: {{ page.tag }}</h1>
<ul>
{% for post in site.tags[page.tag] %}
  <li><a href="{{ post.url }}">{{ post.title }}</a> ({{ post.date | date_to_string }})<br>
    {{ post.description }}
  </li>
{% endfor %}
</ul>
</div>
<hr>
{% endraw %}{% endhighlight %}{: .highlight-left }

Buradaki amaç tamamen bir sayfa şeklinde Tag'leri ve ilgili tag'e ait post'ları göstermek.

Her tag'in kendi sayfası olması işini manuel de yapabiliriz ancak ben otomatik olması istediğim için [tag generator](https://github.com/qian256/qian256.github.io/blob/master/tag_generator.py) adlı script'i
ana dosya dizinine ekledim. Burada unutmamız gereken bu script'i yeni bir post ekledikten sonra deploy çıkmadan önce çalıştırmak.

## 6. Tag'leri bir arada göstermek
Bu adımı yapmak zorunda değilsiniz ancak ben bütün tag'leri bir arada göstermek istediğim için `_includes/archive.html` dosyası içine

{% highlight html %}{% raw %}
<h2>Archive</h2>
{% capture temptags %}
  {% for tag in site.tags %}
    {{ tag[1].size | plus: 1000 }}#{{ tag[0] }}#{{ tag[1].size }}
  {% endfor %}
{% endcapture %}
{% assign sortedtemptags = temptags | split:' ' | sort | reverse %}
{% for temptag in sortedtemptags %}
  {% assign tagitems = temptag | split: '#' %}
  {% capture tagname %}{{ tagitems[1] }}{% endcapture %}
  <a href="/tag/{{ tagname }}"><code class="highligher-rouge"><nobr>{{ tagname }}</nobr></code></a>
{% endfor %}
{% endraw %}{% endhighlight %}{: .highlight-left }

kodlarını ekledim ve bu dosyayı da istediğim yerde
{% raw %}
`{% include archive.html %}`
{% endraw %}
şeklinde çağırdım.

## 7. Sonuç

Evet artık sadece yapılması gerek kodu Github'a göndermek ve production ortamında görmek kalıyor.

Bu yazıya ilham olduğu için Long Qian'a teşekkür ediyorum. Thank you Long Qian 🙏

Sevgiler ❤️

<small><b>Referanslar:</b> [https://longqian.me/2017/02/09/github-jekyll-tag](https://longqian.me/2017/02/09/github-jekyll-tag)</small>