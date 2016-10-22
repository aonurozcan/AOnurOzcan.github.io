---
layout: post
published: true
title: JSP – Expression Tags – 6
date: '2016-10-22'
permalink: jsp-expression-tags-6
share-img: 'http://aonurozcan.com/img/jsp-wallpaper-small.png'
---
Merhaba. Bu yazıda **JSP - Expression Tags** konusuna değineceğiz. **Expression Tag** en basit anlamda içine yazılan kodun sonucunu döndürür, String'e çevirir ve ekrana yazdırır. 

## Söz Dizimi (Syntax)

{% highlight html %}
	<%= sonucu ekrana basılacak olan ifade %>
{% endhighlight %}

**``Not``**: Expression Tag içindeki ifadelerin sonuna noktalı virgül konulmaz. Birkaç örnekle devam edelim. **Expression tag** kullanarak neler yapabiliriz bir görelim. 

**-> Matematiksel işlemler yapıp sonucunu döndürebiliriz.**

{% highlight html %}
	<html>
   <head>
      <title>Matematiksel İşlem</title>
   </head>
   <body>
      <p> Sonuc : <%= 3+5 %> </p>
   </body>
</html>
{% endhighlight %}

#### Ekran Görüntüsü

![Screenshot_1](http://kod5.org/wp-content/uploads/Screenshot_110.png)         

**-> Değişkenleri kullanarak işlem yapabiliriz.**

{% highlight html linenos %}
<html>
   <head>
      <title>Matematiksel İşlem</title>
   </head>
   <body>
      <%
         int sayi1 = 5;
   	 int sayi2 = 10;
      %>
      <p> Sonuc : <%= sayi1 + sayi2 %> </p>
   </body>
</html>
{% endhighlight %}

#### Ekran Görüntüsü
![Screensho1t_3](http://kod5.org/wp-content/uploads/Screensho1t_3.png)         

Expression Tag bu kadar. Bir sonraki yazıda **JSP - Declaration Tag** konusunu işleyeceğiz. Görüşmek üzere.