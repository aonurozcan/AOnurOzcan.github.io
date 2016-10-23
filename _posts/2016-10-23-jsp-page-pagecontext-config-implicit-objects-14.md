---
layout: post
published: true
title: 'JSP – Page, PageContext, Config Implicit Objects – 14'
permalink: jsp-page-pagecontext-config-implicit-objects-14
share-img: 'http://aonurozcan.com/img/jsp-wallpaper-small.png'
date: '2016-10-23'
---
Merhaba, bir önceki yazımda Application nesnesinden bahsetmiştim. Bu yazıda ise gömülü nesnelerden geriye kalan dört nesneden üç tanesini inceleyeceğiz. Bunlar: **page**, **pageContext** ve **config**. Bu nesneler çok seyrek kullanıldığından tek bir yazıda hepsini anlatmayı uygun gördüm.

### Page Nesnesi
Page, çok fazla ihtiyaç duyulmayan, az kullanılan bir gömülü nesnedir. **JSP** sayfasının **Servlet**'e çevrilmiş halinin örneğine işaret eder. Sunucu tarafından arkaplanda

{% highlight java linenos %}
final java.lang.Object page = this;
{% endhighlight %}

şeklinde oluşturulur. Yani kısaca **this** anahtar kelimesine karşılık gelir.

### PageContext Nesnesi
**JSP**'de bulunan dört kapsamda(scope) oluşturulan değişkenlere erişip, onlarla işlem yapılabilir. Bu kapsamlar: **Page**, **Request**, **Application**, **Session**. 
Ayrıca diğer gömülü nesneleri de içerdiği metotlarla çağırabilir. 

#### Object findAttribute (String AttributeName)
Bu metodun **getAtrribute()** metodundan farkı, parametre olarak verilen isme sahip değişkeni dört kapsamda aramasıdır. Eğer bulursa nesneyi döndürür, bulamazsa **null** döner.

Aşağıda gördüğünüz metotlar diğer gömülü nesneler tarafından da kullanılabiliyordu. Bu metotları **pageContext** nesnesi üzerinden çağırdığımızda ise işin hangi kapsamda yapılmasını istiyorsak ikinci ya da üçüncü parametre olarak onu veriyoruz. 

* **Object getAttribute (String AttributeName, int Scope)**
* **void removeAttribute(String AttributeName, int Scope)**
* **void setAttribute(String AttributeName, Object AttributeValue, int Scope)**

İkinci parametre olarak ;

1.  **PageContext**.**``PAGE_SCOPE``**
2.  **PageContext**.**``REQUEST_SCOPE``**
3.  **PageContext**.**``SESSION_SCOPE``**
4.  **PageContext**.**``APPLICATION_SCOPE``**

bu değerleri verebiliriz. Sonuç olarak **PageContext** nesnesinin asıl işlevi belirli bir kapsama göre metotları çağırması diyebiliriz.

### Config Nesnesi

Config nesnesi **javax.servlet.ServletConfig** sınıfının bir örneğidir.  Belirli bir **JSP **sayfasına ait bilgileri çekmeye yarar. Bu da yukarıdaki iki nesne gibi çok seyrek kullanılır. Bir örnekle bu nesneyi geçelim. **web.xml** dosyası içerisinde **index.jsp**'yi bir servlet olarak tanımlayalım. Servlete ulaşılacak url olarak da "/index" verelim. 

#### web.xml


{% highlight xml linenos %}
<web-app>
  <servlet> 
  <servlet-name>KOD5Servlet</servlet-name> 
  <jsp-file>/index.jsp</jsp-file> 
  </servlet> 

  <servlet-mapping> 
  <servlet-name>KOD5Servlet</servlet-name> 
  <url-pattern>/index</url-pattern> 
  </servlet-mapping> 
</web-app>
{% endhighlight %}

**index.jsp** sayfasına gelindiğinde **config** nesnesi üzerinden **servlet**'in ismini alıp ekrana yazdıralım. 

#### index.jsp

{% highlight java linenos %}
<% 
String name = config.getServletName(); 
out.print("Servlet Name is: " + name); 
%>
{% endhighlight %}

### Ekran Çıktısı
[![3](http://kod5.org/wp-content/uploads/317.png)](http://kod5.org/wp-content/uploads/317.png) 

Bir sonraki yazıda **Exception** nesnesine değineceğiz. Görüşmek üzere..