---
layout: post
published: true
title: JSP – Response Implicit Object – 11
permalink: jsp-response-implicit-object-11
share-img: 'http://aonurozcan.com/img/jsp-wallpaper-small.png'
date: '2016-10-23'
---
Merhaba, bir önceki yazıda request nesnesinden ve metotlarından bahsetmiştik. Bu yazıda ise response nesnesine değineceğiz. 

#### Başlıca response nesnesi metotları:

*   void setContentType(String type)
*   void sendRedirect(String address)
*   void addHeader(String name, String value)
*   void setHeader(String name, String value)
*   boolean containsHeader(String name)
*   void addCookie(Cookie value)
*   void sendError(int status_code, String message)
*   void setStatus(int statuscode)

#### void setContentType(String type)
Bu metot, aldığı parametre ile sayfa içeriğinin tipini belirler.

{% highlight java linenos %}
<%
  response.setContentType("text/html");
  response.setContentType("image/gif");
  response.setContentType("image/png");
  response.setContentType("application/pdf");
%>
{% endhighlight %}

#### void sendRedirect(String address)
Parametre olarak aldığı sayfaya yönlendirme yapar. Yönlendirme sırasında URL değişir.

{% highlight java linenos %}
<%
   response.sendRedirect("gidilecekSayfa.jsp");
%>
{% endhighlight %}

#### void addHeader(String name, String value)
Bu metot başlık eklemeye yarar. Başlık bir değişken ve bir değerden oluşur.

{% highlight java linenos %}
<%
   response.addHeader("site","KOD5");
%>
{% endhighlight %}

#### void setHeader(String name, String value)
Varolan bir başlığın değerini değiştirmeye yarar.  Yukarıda addHeader metodunda değerini "KOD5" olarak belirlediğimiz "site" değişkenine yeni bir değer atayalım.

{% highlight java linenos %}
<%
   response.setHeader("site","Onur");
%>
{% endhighlight %}

#### boolean containsHeader(String name)
Parametre olarak verilen değişkene karşılık bir değer atanıp atanmadığını kontrol eder.

{% highlight java linenos %}
<%
    response.addHeader("degisken","deger");
%>

<%=response.containsHeader("degisken")%>
{% endhighlight %}

#### Ekran Görüntüsü
[![Screenshot_1](http://kod5.org/wp-content/uploads/Screenshot_113.png)](http://kod5.org/wp-content/uploads/Screenshot_113.png)   

#### void addCookie(Cookie value)
Oluşturduğumuz cookie'yi bu metot aracılığıyla response nesnesine ekleyebiliriz.

{% highlight java linenos %}
<%
    Cookie person = new Cookie("name","Onur");
    response.addCookie(person);
%>
{% endhighlight %}

#### void sendError(int status_code, String message)
Bu metot ile bir sayfada hata mesajı yazdırabiliriz. **Örnek:**

{% highlight java linenos %}
<%response.sendError(404, "Page not found error");%>
{% endhighlight %}

##### Ekran Görüntüsü
[![Screenshot_3](http://kod5.org/wp-content/uploads/Screenshot_34.png)](http://kod5.org/wp-content/uploads/Screenshot_34.png)     

#### int setStatus(int status_code)
Parametre olarak aldığı değer ile Http durumuna bir kod ataması yapar.

{% highlight java linenos %}
<%response.setStatus(404);%>
{% endhighlight %}

#### Ekran Görüntüsü
[![Screenshot_4](http://kod5.org/wp-content/uploads/Screenshot_44.png)](http://kod5.org/wp-content/uploads/Screenshot_44.png) 

Response nesnesini elimden geldiğince anlatmaya çalıştım. Bir sonraki yazıda en çok kullanılan nesnelerden biri olan session nesnesine değineceğiz. Okuduğunuz için teşekkürler. Görüşmek üzere..