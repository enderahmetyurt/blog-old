---
layout: post
title: Ruby on AWS Lambda
tags: [ruby]
---

Merhaba. Yakın zamanda duyurulan Aws Lambda'dan bahsetmek istiyorum.

Aws Ruby SDK'sı Aws üzerinde işlemler yapmamıza yarıyor. Bu SDK ile Amazon'un Amazon EC2, S3, SQS, SNS gibi servislerini kullanabiliyoruz. Yeni gelişmeler ile Amazon Lambda servisini de artık kullanabilir oluyoruz. Bu geliştirme ile artık Lambda fonksiyonları Ruby gibi kullanabilir ve AWS üzerinde çalıştırabiliriz. Bu girişten sonra küçük bir örnek yapalım.

Öncelikle [Lambda Console](https://console.aws.amazon.com/lambda/home)'u açmak gerekiyor. Bu sayfada sırasıyla yapmamız gerekenler,

- `Create function`'a tıklıyoruz.
- Küçük bir örnek yapabilmek için `Author from scratch`'i seçiyoruz.
- Function'ımıza kolay bir isim verdikten sonra, Ruby versiyonunu seçiyoruz.
- `Role` için ise `Create a new role from one or more templates.` diyip, `Role` kısmına bir rol adı yazıyoruz.
- Sonrasında alttaki `Create function`'a basıp Lambda function'ımınızın oluşturulduğunu görüyoruz.

<div style="display: flex; justify-content: center;"><img src="/public/images/aws1.png"></div>

Lambda function'ı create edildikten sonraki açılan ekrandan her türlü kod düzenlemesini yapabilir, diğer servisler ile function'ı bağlayabilir ve servis ayarlarınızı yapabilirsiniz. Diğer tab yani `Monitoring` tab'ınıdan ise function'ının kullanımı ile alakalı sayıları alabilir, log servislerini kullanabilirsiniz.

Alt tarafa code editor göreceksiniz. Burada lambda function'ımızı düzeltebiliriz. Gördüğümüz kod ise basit bir JSON önden lambda_handler isimli bir function. Bu kodu çalıştırmak için yapmamız gereken `Test`'e basıp, bir test event'i set edebilmek. Şimdilik biz bu alanı `{}` şeklinde tanımlıyoruz ve bir `Event name` verip, create ediyoruz. Sonrasında tekrar `Test` dediğimizde, Ruby Lamba Function'nın çalıştığını görebiliyoruz.

<div style="display: flex; justify-content: center;"><img src="/public/images/aws2.png"></div>

Sevgiler.