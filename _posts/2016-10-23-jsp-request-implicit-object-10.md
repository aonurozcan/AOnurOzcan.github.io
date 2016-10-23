---
layout: post
published: true
title: JSP - Request Implicit Object - 10
permalink: jsp-request-implicit-object-10
share-img: 'http://aonurozcan.com/img/jsp-wallpaper-small.png'
date: '2016-10-23'
---
Merhaba, bir önceki yazımda JSP - Out Implicit Object' den bahsetmiştim. Bu yazıda ise **JSP** ile yazılım geliştirirken en çok kullanacağımız nesnelerden birisi olan **request** nesnesini inceleyeceğiz. Request nesnesinin görevi bir HTTP isteği anında JSP sayfasındaki verileri Sunucuya iletmektir. Bu veriler; cookieler, başlıklar ya da kullanıcı tarafından girilmiş parametreler olabilir. Request nesnesinin birçok metodu vardır. Biz bunlardan en çok kullanılan 15 tanesine değineceğiz.

*   getParameter(String name)
*   getParameterNames()
*   getParameterValues(String name)
*   getAttribute(String), setAttribute(String,Object)
*   getAttributeNames()
*   removeAttribute(String)
*   getCookies()
*   getHeader(String name), getHeaderNames()
*   getRequestURI()
*   getRequestURL()
*   getMethod()
*   getQueryString()
*   getServletPath()

#### getParameter(String degiskenAdi)
Bu metot, yapılan request ile birlikte gönderilen parametreleri almayı sağlar. Metodun aldığı String tipindeki değişken gönderilen parametrenin "name" değeridir. 

#### Örnek 
Aşağıdaki **textbox'**tan girilen değeri almak istiyorsak,

{% highlight html linenos %}
<input type="text" name="sifre">
{% endhighlight %}

**getParameter()** metodunu çağırmalı ve parametre olarak **name** değerini vermeliyiz.

{% highlight java linenos %}
<%

   String sifre = request.getParameter("sifre");

%>
{% endhighlight %}

**``Not:``** getParameter() metodunun dönüş tipi String'dir. Dolayısıyla bir sayı gönderseniz bile önce String olarak alıp sonradan integer'a dönüştürmelisiniz.

#### getParameterNames()
request ile birlikte gönderilen tüm parametrelerin **name** değerlerini **Enumeration** tipinde döndürür.

#### Örnek
Bu iki **textbox**'ın **name** değerlerini almak istiyorsak,

{% highlight html linenos %}
<input type="text" name="isim">
<input type="text" name="soyisim">
{% endhighlight %}

**getParameterNames()** metodunu çağırırız.

{% highlight java linenos %}
<% 

   Enumeration e = request.getParameterNames();

   while (e.hasMoreElements()) {
        String element = e.nextElement().toString();
        out.println(element);
        out.println("<br>");
    }

%>
{% endhighlight %}

#### Ekran Çıktısı
[![1](http://kod5.org/wp-content/uploads/120.png)](http://kod5.org/wp-content/uploads/120.png)

#### getParameterValues()
Name değeri aynı olan birden fazla parametreyi almaya yarar. Genelde checkbox'lar için kullanılır. 

#### Örnek 
Aşağıdakine benzer bir durumda seçilen renkleri almak istiyorsak,

{% highlight html linenos %}
Hangi renkleri seviyorsunuz? :
<input type="checkbox" name="renk" value="Mavi">Mavi
<input type="checkbox" name="renk" value="Sarı">Sarı
<input type="checkbox" name="renk" value="Yeşil">Yeşil
<input type="checkbox" name="renk" value="Kırmızı">Kırmızı
{% endhighlight %}

Seçilen renkler alınıp ekrana yazdırılıyor.

{% highlight java linenos %}
String[] renkler = request.getParameterValues("renk");

out.println("Seçilen renkler :" );

for (String renk : renkler){
    out.println(renk);
}
{% endhighlight %}

#### Ekran Çıktısı
[![2](http://kod5.org/wp-content/uploads/216.png)](http://kod5.org/wp-content/uploads/216.png)   
[![3](http://kod5.org/wp-content/uploads/39.png)](http://kod5.org/wp-content/uploads/39.png)

#### getAttribute() ve setAttribute()
Bu metotlar sayesinderequest yapılırken beraberinde bir değişken gönderilebilir ve bu degişkenin değeri gidilen yerde alınıp kullanılabilir. 

#### Örnek
index.jsp sayfasında "isim" değişkenine  "KOD5" değerini set ettik,

{% highlight java linenos %}
<%
    request.setAttribute("isim","KOD5");
    //sonuc.jsp sayfasına yönlendirme yapılıyor.
    request.getRequestDispatcher("sonuc.jsp").forward(request,response);
%>
{% endhighlight %}

sonuc.jsp'ye yönlendirme yapıldıktan sonra da orada bu değeri alalım.

{% highlight java linenos %}
<%
  String deger = (String) request.getAttribute("isim");
  out.println(deger);
%>
{% endhighlight %}

#### Ekran Çıktısı
[![1](http://kod5.org/wp-content/uploads/123.png)](http://kod5.org/wp-content/uploads/123.png) 

**``Not:``** setAttribute() metodunun ilk parametresi String ikinci parametresi Object tipindedir. Dolayısıyla ikinci parametre olarak her tipte veriyi kabul eder fakat getAttribute() ile bu değerler alınırken ilgili veri tipine dönüşüm yapılmalıdır.

#### getAttributeNames()
Bu metot hangi nesne üzerinden çağrılırsa, o nesneye setAttribute() metoduyla atanmış tüm değişkenlerin ismini Enumeration tipinde döndürür. Biz request üzerinden çağıracağız.

#### Örnek
request nesnesine bir değişken ve ona karşılık gelen bir değer atayalım.(isim -> KOD5)

{% highlight java linenos %}
<%
    request.setAttribute("isim","KOD5");
    //sonuc.jsp sayfasına yönlendirme yapılıyor.
    request.getRequestDispatcher("sonuc.jsp").forward(request,response);
%>
{% endhighlight %}

Şimdi requet nesnesi üzerinde tanımlanmış tüm değişken isimlerini getAttributeNames() metoduyla alalım

{% highlight java linenos %}
<%
  Enumeration e = request.getAttributeNames();
  while (e.hasMoreElements()) {
    String s = e.nextElement().toString();
    out.println("Değişken Adı : " + s + " -> Değeri : " + request.getAttribute(s));
    out.println("<br>");
  }
%>
{% endhighlight %}

#### Ekran Çıktısı 
Burada varsayılan olarak request nesine atanan değişkenler de gelecektir. En altta ise bizim atadığımız değişken görünmektedir. 
[![1](http://kod5.org/wp-content/uploads/124.png)](http://kod5.org/wp-content/uploads/124.png)

#### removeAttribute()
Bir nesneye atanmış değişkenlerin yok edilmesinde kullanılır. Bu metot kullanılarak bir değişken silindikten sonra tekrar ulaşılmaya çalışılırsa null döner. 

#### Örnek
{% highlight java linenos %}
<%
    request.setAttribute("isim","KOD5");
    //sonuc.jsp sayfasına yönlendirme yapılıyor.
    request.getRequestDispatcher("sonuc.jsp").forward(request,response);
%>
{% endhighlight %}

removeAttribute() metodu kullanarak değişken siliniyor.

{% highlight java linenos %}
<%
  request.removeAttribute("isim");
  String deger = (String) request.getAttribute("isim");
  out.println(deger);
%>
{% endhighlight %}

#### Ekran Çıktısı 
Değişken yok olduğu için null dönüyor. 
[![2](http://kod5.org/wp-content/uploads/217.png)](http://kod5.org/wp-content/uploads/217.png)

#### getCookies()
Var olan cookieleri bir dizi halinde döndürür. 

#### Örnek 
index.jsp'de bir cookie oluşturalım ve "siteAdi" değikenine "KOD5" değerini atayalım. Cookie'nin gidilen sayfada kullanılabilmesi için response.addCookie() metodu çağrılıp oluşturulan cookie parametre olarak verilmelidir.

{% highlight java linenos %}
<%
    Cookie cookie = new Cookie("siteAdi","KOD5");
    response.addCookie(cookie);
    request.getRequestDispatcher("sonuc.jsp").forward(request,response);
%>
{% endhighlight %}

Bir Cookie dizisi oluşturup getCookies() metodu ile var olan cookieleri döndürdük. Bir forEach döngüsü ile cookieleri gezip değişken isimlerini ve değerlerini ekrana yazdırdık.

{% highlight java linenos %}
<%
  Cookie[] cookieler = request.getCookies();
  for(Cookie cookie : cookieler){
    out.println(cookie.getValue() + "<br>" + cookie.getName() + "<br>");
  }
%>
{% endhighlight %}

#### Ekran Çıktısı
siteAdi ve KOD5 değerlerimiz ekrana geldi. Burada ilk iki satırda sunucunun otomatik olarak oluşturduğu ikinci bir cookienin bilgilerini görebilirsiniz. 
[![3](http://kod5.org/wp-content/uploads/310.png)](http://kod5.org/wp-content/uploads/310.png)

#### getHeader(String) ve getHeaderNames()
Browser bir sunucuya istek yaptığında beraberinde birçok bilgi gönderir. Bu bilgileri sunucu tarafında görmek ve kullanmak istersek bu metotları kullanırız. 

#### Örnek 
sonuc.jsp sayfasına gitmemizi sağlayacak bir link oluşturuyoruz.

{% highlight html linenos %}
<a href="sonuc.jsp">sonuc.jsp'ye gitmek için tıklayın</a>
{% endhighlight %}

Burada getHeaderNames() metodu header başlıklarını Enumeration tipinde döndürür. Aldığımız başlıkları getHeader() metoduna parametre olarak verdiğimizde o başlıklara ait değerleri döndürmüş oluruz.

{% highlight java linenos %}
<%
  Enumeration<String> headers = request.getHeaderNames();
  while(headers.hasMoreElements()){
    String headerName = headers.nextElement();
    out.println(headerName + " : " + request.getHeader(headerName) + "<br>");
  }
%>
{% endhighlight %}

#### Ekran Çıktısı
[![4](http://kod5.org/wp-content/uploads/48.png)](http://kod5.org/wp-content/uploads/48.png) 
[![5](http://kod5.org/wp-content/uploads/58.png)](http://kod5.org/wp-content/uploads/58.png)

  Son 5 örnek benzer olduğundan hepsini tek örnek içinde ele alalım. 
  
#### getRequestURL()
İsteğin yapıldığı adresin tamamını verir.

#### getRequestURI()
İsteğin yapıldığı adresin ana dizininden sonraki kısmını verir. 

#### getMethod()
Yapılan HTTP isteğinin türünü verir. 

#### getQueryString()
İstek yapılırken URL üzerinden gönderilen parametreyi döndürür. 

#### getServletPath()
İstek yapılan JSP sayfasının adını döndürür. 

#### Örnek
Burada URL üzerinden "?site=KOD5" parametresi gönderdiğimize dikkat edelim.

{% highlight html linenos %}
<a href="sonuc.jsp?site=KOD5">sonuc.jsp'ye gitmek için tıklayın</a>
{% endhighlight %}

sonuc.jsp sayfasında yapılan istekle ilgili bilgileri çekelim.

{% highlight java linenos %}
<%="Request URI : " + request.getRequestURI() + "<br>"%>
<%="Request URL : " + request.getRequestURL() + "<br>"%>
<%="Query String : " + request.getQueryString() + "<br>"%>
<%="Method : " + request.getMethod() + "<br>"%>
<%="Servlet Path : " + request.getServletPath() + "<br>"%>
{% endhighlight %}

#### Ekran Çıktısı
[![6](http://kod5.org/wp-content/uploads/65.png)](http://kod5.org/wp-content/uploads/65.png) 
[![7](http://kod5.org/wp-content/uploads/74.png)](http://kod5.org/wp-content/uploads/74.png)

  Evet, request nesnesi ile ilgili anlatacaklarım bu kadar. Bir sonraki yazıda response nesnesine değineceğiz. Aklınıza takılan kısımları bana yorum kısmından ya da facebook üzerinden sorabilirsiniz. Görüşmek üzere..