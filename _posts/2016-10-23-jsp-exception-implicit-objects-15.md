---
layout: post
published: true
title: JSP – Exception Implicit Objects – 15
permalink: jsp-exception-objects-15
share-img: 'http://aonurozcan.com/img/jsp-wallpaper-small.png'
date: '2016-10-23'
---
Merhaba, bir önceki yazıda **JSP**'de çok fazla kullanılmayan üç nesneye değinmiştik. Bu yazıda ise gömülü nesnelerin sonuncusu olan **Exception** nesnesini anlatmaya çalışacağım. 

**Exception** nesnesi **java.lang.Throwable** sınıfından oluşturulmuştur.  Bu nesneye sadece hata sayfası(Error Page) olarak belirlenmiş sayfalardan erişilebilir. Bir sayfanın hata sayfası olarak belirtilmesi için **Directive Tag** kullanarak **isErrorPage** özniteliğine "**true**" değeri verilmelidir. Aşağıda bu duruma örnek verelim.

### Örnek Uygulama

Girilen iki sayıyı bölen bir uygulamamız olsun. Eğer kullanıcı ikinci sayıya 0 girerse, 0'a bölünme hatası ortaya çıkacaktır. Bu hatayı yakalayıp önceden belirlediğimiz bir sayfada gösterelim. 

#### index.jsp

{% highlight html linenos %}
<html>
<head>
    <title>Directive Tags</title>
</head>
<body>
<form action="bolmeYap.jsp" method="post">
    <table>
        <tr>
            <td>İlk Sayı:</td>
            <td><input type="text" name="ilkSayi"/></td>
        </tr>
        <tr>
            <td>İkinci Sayı:</td>
            <td><input type="text" name="ikinciSayi"/></td>
        </tr>
        <tr>
            <td></td>
            <td><input type="submit" value="Böl" style="width:100%;"/></td>
        </tr>
    </table>
</form>
</body>
</html>
{% endhighlight %}

İki adet inputtan oluşan bir form oluşturduk. Butona basıldığında **bolmeYap.jsp** sayfasına gidecek. Biz de o sayfada bu formdan gelen sayıları alıp bölme işlemi yapacağız. 

#### bolmeYap.jsp
{% highlight java linenos %}
<%@ page language="java" contentType="text/html; charset=UTF-8" errorPage="hataSayfasi.jsp" %>
<%
    int ilkSayi = Integer.parseInt(request.getParameter("ilkSayi"));
    int ikinciSayi = Integer.parseInt(request.getParameter("ikinciSayi"));
    int sonuc = ilkSayi / ikinciSayi;
    out.println("İŞLEMİN SONUCU : " + sonuc);
%>
{% endhighlight %}

Burada önemli bir nokta şu: en üst satırda **Directive Tag**(Emir etiketi) kullanarak, **errorPage** özniteliğine hata oluştuğunda gidilecek sayfanın ismini verdik. 

#### hataSayfasi.jsp

{% highlight html linenos %}
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" isErrorPage="true"%>

<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>Hata Sayfası</title>
</head>
<body>
<h1>Bir hata meydana geldi!!</h1>
<h3>Hata: <%=exception%></h3>
</body>
</html>
{% endhighlight %}

Bu sayfada ise exception nesnesini kullanarak hatayı ekrana yazdırdık. Bu nesneyi kullanabilmek için en üst satırda **isErrorPage**="**true**" diyerek sayfanın bir hata sayfası olduğunu belirttik.

### Ekran Görüntüleri
[](http://kod5.org/wp-content/uploads/314.png)
[![3](http://kod5.org/wp-content/uploads/318.png)](http://kod5.org/wp-content/uploads/318.png) 
[![4](http://kod5.org/wp-content/uploads/413.png)](http://kod5.org/wp-content/uploads/413.png)  

Bu yazıyla birlikte **JSP Implicit Objects** konusunu bitirmiş oluyoruz. Bir sonraki yazıda **JSP**'de **Yönlendirme** konusuna değineceğim. Görüşmek üzere.