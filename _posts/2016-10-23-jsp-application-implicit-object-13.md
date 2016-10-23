---
layout: post
published: true
title: JSP – Application Implicit Object – 13
permalink: jsp-application-implicit-object-13
share-img: 'http://aonurozcan.com/img/jsp-wallpaper-small.png'
date: '2016-10-23'
---
Merhaba, bir önceki yazıda Session nesnesinden bahsetmiştik. Bu yazıda ise **Application** nesnesine değineceğiz. 

Bu nesne **javax.servlet.ServletContext **sınıfının bir örneğidir(instance).  Genel olarak uygulama bilgilerini çekmede ve uygulamanın her yerinden erişilebilecek değişkenler oluşturmada kullanılır.

#### Application Nesnesi Metotları
*   **void setAttribute(String name, Object object)**
*   **Object getAttribute(String name)**
*   **void removeAttribute(String name)**
*   **Enumeration getAttributeNames()**
*   **String getInitParameter(String paramName)**
*   **Enumeration getInitParameterNames()**
*   **String getRealPath(String value)**
*   **void log(String message)**
*   **String getServerInfo()**

#### void setAttribute(String name, Object object)
Uygulamanın her yerinden erişilebilecek bir değişken oluşturmaya yarar. İlk parametre olarak oluşturacağımız değişkenin adını veririz. İkinci parametreye ise değişkenimizin değerini veririz. Not: Değişken Object tipinde olduğundan her türde veri saklayabilir. Örnek: İsmi "Site" değeri "KOD5" olan bir değişken oluşturduk.

{% highlight java linenos %}
<%
  application.setAttribute("Site","KOD5");
%>
{% endhighlight %}

#### Object getAttribute(String name)
setAttribute() metodu ile oluşturduğumuz değişkene bu metot ile erişebilir ve değerini çekebiliriz. Parametre olarak değişkeni oluştururken ne isim atadıysak onu vermeliyiz.

{% highlight java linenos %}
<%
  application.getAttribute("Site");
%>
{% endhighlight %}

#### void removeAttribute(String name)
Bu metot yukarıda oluşturduğumuz ve değerini çektiğimiz değişkeni yok etmeye yarar. Bu metot çağrıldıktan sonra değişkene ulaşmaya çalışırsanız null dönecektir.

{% highlight java linenos %}
<%
  application.removeAttribute("Site");
%>
{% endhighlight %}

#### Enumeration getAttributeNames()
Eğer birden fazla değişken tanımladıysak ve bu değişkenlerin isimlerine toplu ularak ulaşmak istiyorsak bu metodu kullanırız.

{% highlight java linenos %}
<%
  Enumeration attributeNames = application.getAttributeNames();
%>
{% endhighlight %}

#### String getInitParameter(String paramName)
web.xml dosyasında oluşturduğumuz başlangıç(Initialization) parametrelerinin değerine ulaşmayı sağlar. Örnek: web.xml dosyamızda başlangıç parametresi oluşturalım.

{% highlight xml linenos %}
<web-app>
    <context-param>
    <param-name>Site</param-name>
    <param-value>KOD5</param-value>
    </context-param>
</web-app>
{% endhighlight %}

Bu parametrenin değerine yani "**KOD5**" e erişelim. **deger** burada "**KOD5**" olacaktır.

{% highlight java linenos %}
<% 
    String deger = application.getInitParameter("Site");
%>
{% endhighlight %}

#### Enumeration getInitParameterNames()
getAttributeNames() metoduna benzer şekilde tüm başlangıç parametrelerinin isimlerini Enumeration tipinde döndürür.

{% highlight java linenos %}
<% 
    Enumeration names = application.getinitParameterNames();
%>
{% endhighlight %}

#### String getRealPath(String value)
Parametre olarak verilen değere göre tam dizin yolunu verir.

{% highlight java linenos %}
<%
    String path = application.getRealPath("/ApplicationObject.jsp");
    out.print(path);
%>
{% endhighlight %}

#### Ekran Çıktısı
[![1](http://kod5.org/wp-content/uploads/127.png)](http://kod5.org/wp-content/uploads/127.png) 

#### void log(String message)
Parametre olarak verilen değeri, hangi JSP sunucusu kullanılıyorsa onun varsayılan log dosyasına yazar.

{% highlight java linenos %}
<%
    application.log("File not found.");
%>
{% endhighlight %}

#### String getServerInfo()
Kullanılan sunucunun ismini ve sürüm numarasını döndürür.

{% highlight java linenos %}
<%
    String serverInfo = application.getServerInfo();
    out.print(serverInfo);
%>
{% endhighlight %}

#### Ekran Çıktısı
[![2](http://kod5.org/wp-content/uploads/219.png)](http://kod5.org/wp-content/uploads/219.png) 

Bu yazıda **application** nesnesini ve metotlarını anlatmaya çalıştım. Bir sonraki yazıda **page**, **pageContext** ve **exception** nesnelerine değineceğim. Hoşçakalın..