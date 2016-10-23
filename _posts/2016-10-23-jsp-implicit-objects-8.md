---
layout: post
published: true
title: JSP – Implicit Objects – 8
permalink: jsp-implicit-objects-8
share-img: 'http://aonurozcan.com/img/jsp-wallpaper-small.png'
date: '2016-10-23'
---
Merhaba, bir önceki yazıda **JSP - Declaration Tag** konusuna değinmiştik. Bu yazıda ise **implicit object**s yani gömülü nesneler konusuna giriş yapacağız. **Implicit Objects** **JSP** sayfalarının **Servlet**'e çevrilmesi anında **Sunucu** tarafından oluşturulurlar. Gömülü olarak adlandırılmalarının sebebi budur. Sunucu bu nesneleri **_jspService()** metodunun içerisinde oluşturur. Dolayısıyla **Scriptlet tag**'i içerisinde tekrar oluşturmaya gerek kalmadan olduğu gibi kullanabiliriz. Toplamda **9** adet **implicit object** bulunmaktadır. 

#### Bunlar:

1.  **out**
2.  **request**
3.  **response**
4.  **session**
5.  **application**
6.  **exception**
7.  **page**
8.  **pageContext**
9.  **config**

## Out 

Ekrana içerik bastırmak için kullanılır. İçerik ve buffer ile ilgili çeşitli metotları bulunur. 

## Request

Bu nesnenin en temel amacı veri almaktır. Forma girilen veriler, yönlendirilen sayfada request nesnesi ile alınır ve üzerinde çeşitli işlemler yapılır. 

## Response

Yapılan bir istek sonrası sunucu tarafından kullanıcıya gönderilen cevaplar ile ilgili işlemlerle görevlidir. 

## Session 

Oturum yönetimi için kullanılır. Oturumun açık kaldığı süre boyunca, kullanıcının bilgilerine bu nesne sayesinde her yerden erişilebilir. En çok kullanılan nesnelerden birisidir. 

## Application
 
Projemizin parametrelerine ulaşmada kullanılır. Örneğin; versiyonlar, url'ler, sunucu bilgileri. 

## Exception

Hataları yakalayıp gerekli mesajı göstermek için kullanılır. Sadece "**isErrorPage**" attribute'si "**true**" olan **JSP** sayfalarından erişilebilir. 

## Page

Yaygın kullanımı yoktur. "**this**" anahtar kelimesiyle eş anlamlıdır. 

## PageContext

Page, Request, Application ve Session seviyelerine ulaşmak için kullanılır. 

## Config

Application nesnesine benzerdir. Application nesnesi proje ile ilgili bilgileri almamızı sağlarken Config nesnesi **JSP** sayfalarına özel bilgileri almamızı sağlar. Örneğin; Servlet Name, Servlet Context gibi. 

Bu yazı kısa bir tanıtım şeklinde oldu. Her bir nesne için örneğiyle birlikte ayrı bir yazı yazmayı düşünüyorum. Bir sonraki yazıda görüşmek üzere. Hoşçakalın..