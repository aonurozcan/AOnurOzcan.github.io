---
layout: post
published: true
title: JSP – Action Tags – 5
date: '2016-10-22'
permalink: jsp-action-tags-5
share-img: 'http://aonurozcan.com/img/jsp-wallpaper-small.png'
---
Merhaba, bu yazıda **JSP Action Tags**'i ele alacağız. **JSP** action etiketleri yönlendirme, dahil etme, java beanleri kullanma gibi işlemlerde kullanılır. 

## Başlıca JSP Action Etiketleri

*   ``<jsp:include>``
*   ``<jsp:forward>``
*   ``<jsp:param>``
*   ``<jsp:useBean>``
*   ``<jsp:getProperty>``
*   ``<jsp:setProperty>``

## ``<jsp:include>`` Etiketi

Bir **JSP** sayfası içerisinde başka bir **JSP** sayfasını çağırmamızı sağlar. Bu sayede kodlarımızı farklı **JSP** sayfalarına yazarak bölebilir, bir sayfa içerisinde hepsini çağırıp kullanabiliriz.

**<%@ include ... %>** etiketi ile aynı işlevi görür fakat **<jsp:include>** etiketi ile dahil etme işlemi **request**(istek) geldiğinde gerçekleşir, **<%@ include ... %>** etiketi ile dahil etme işlemi ise **JSP** sayfaları **Servlet**'e çevrilirken gerçekleşir. 

#### Kullanım:

{% highlight html linenos %}
<jsp:include page="Dahil edilecek sayfa adresi"/>
{% endhighlight %}


## Örnek Uygulama 

#### index.jsp

{% highlight html linenos %}
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>

	<jsp:include page="sayfa.jsp"/>

</body>
</html>
{% endhighlight %}

sayfa.jsp'yi dahil ettik. 

#### sayfa.jsp

{% highlight html linenos %}
<%@ page import="java.util.Date, java.text.DateFormat" %>

<% 
  	String tarih = DateFormat.getDateInstance(DateFormat.LONG).format(new Date());
	out.println("Tarih :" + tarih);
%>
{% endhighlight %}

## Ekran Görüntüleri

![3](http://kod5.org/wp-content/uploads/2015/04/3.png)

## ``<jsp:forward>`` Etiketi

JSP sayfasına yapılan bir isteği başka bir sayfaya yönlendirmek için kullanılır. Bu etiketin bir özelliği de yönlendirme yapıldığında adresin değişmemesidir. Örnek uygulamada bunu görebilirsiniz. 

#### Kullanım:

{% highlight html linenos %}
<jsp:forward page="Gidilecek sayfanın adresi"/>
{% endhighlight %}

## ``<jsp:forward>`` Örnek Uygulama

#### index.jsp

{% highlight html linenos %}
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>

	<jsp:forward page="sayfa.jsp"/>

</body>
</html>
{% endhighlight %}

#### sayfa.jsp

{% highlight html linenos %}
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
   <p>
	index.jsp'den buraya geldik ama adres hala aynı :)
   </p>
</body>
</html>
{% endhighlight %}

## Ekran Görüntüleri 

![4](http://kod5.org/wp-content/uploads/2015/04/4.png)

## ``<jsp:param>`` Etiketi

Bu etiket ile diğer **JSP** etiketlerine(``<jsp:include>``,``<jsp:forward>`` vs.) parametre gönderilebilir. Bu şekilde yönlendirilen ya da dahil edilen **JSP** sayfaları bu parametrelere ulaşabilir. 

#### Kullanım:

{% highlight html linenos %}
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<jsp:param name="Parametrenin adı" value="Parametrenin değeri" />
{% endhighlight %}

## ``<jsp:param>`` Örnek Uygulama

#### index.jsp

{% highlight html linenos %}
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>

	<jsp:forward page="sayfa.jsp">
		<jsp:param value="Merhaba Dunya!" name="parametre"/>
	</jsp:forward>

</body>
</html>
{% endhighlight %}

#### sayfa.jsp

{% highlight html linenos %}
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	<p>
		<%=request.getParameter("parametre") %>
	</p>
</body>
</html>
{% endhighlight %}

## Ekran Görüntüleri

![5](http://kod5.org/wp-content/uploads/2015/04/5.png)

## ``<jsp:useBean>`` Etiketi

**JSP** sayfalarında **Java Bean**'leri çağırmak için kullanılır. **Java Beans** konusunu daha sonra detaylı olarak göreceğiz. Şimdi sadece basit bir örnek verelim. 

#### Kullanım:

{% highlight html linenos %}
<jsp:useBean id="Beani temsil edecek herhangi bir isim"  class="paketAdi.classAdi" />
{% endhighlight %}

Yukarıdaki kod ile kullanacağımız **Java Class**'ından bir nesne oluşturmuş olduk. ``<jsp:setProperty>`` ve ``<jsp:getProperty>`` etiketlerini kullanarak bu nesne üzerinden değer atama(**Set**) ve değer çekme(**Get**) işlemleri yapabileceğiz.

## ``<jsp:setProperty>`` Etiketi

Bu etiket kullanılmadan önce **jsp:useBean** etiketi tanımlanmış olmalıdır. **jsp:useBean** etiketi ile belirtilen **bean**'in değişkenlerine değer ataması yapar. Adından da anlaşılacaği gibi **set** metodunun işlevini yapar. 

#### Kullanım:

{% highlight html linenos %}
<jsp: useBean id="Beani temsil edecek herhangi bir isim"  class="paketAdi.sinifAdi" />
<jsp:setProperty name="useBean etiketinde verdiğimiz id" property="Sınıftaki değişken adı" />
{% endhighlight %}

Eğer birden fazla değişkene aynı anda değer ataması yapmak istiyorsak '*' kullanabiliriz.

{% highlight html linenos %}
<jsp:setPropery name="useBean etiketinde verdiğimiz id" property="*"/>
{% endhighlight %}

## ``<jsp:getProperty>`` Etiketi

**jsp:useBean** etiketi ile belirtlilen **bean**'in değişkenlerinin değerini çeker. Adından da anlaşılacaği gibi **get** metodunun işlevini yapar.

{% highlight html linenos %}
<jsp:getProperty name="useBean etiketinde verdiğimiz id" property="Sınıftaki değişken adı"/>
{% endhighlight %}

## useBean, getProperty ve setProperty ile İlgili Örnek Uygulama

#### index.jsp

{% highlight html linenos %}
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>

	<form action="formuAl.jsp" method="post">
		<input name="isim" type="text">
		<input name="soyisim" type="text">
		<input type="submit"> 
	</form>

</body>
</html>
{% endhighlight %}

İsim ve soyisim alan basit bir form oluşturduk. Butona basıldığında **formuAl.jsp**'ye gidecek. Şimdi bir **Bean**(Java Sınıfı) oluşturalım. 

#### Kisi.java

{% highlight java linenos %}
package deneme;

public class Kisi {

	private String isim;
	private String soyisim;

	public String getIsim() {
		return isim;
	}
	public void setIsim(String isim) {
		this.isim = isim;
	}
	public String getSoyisim() {
		return soyisim;
	}
	public void setSoyisim(String soyisim) {
		this.soyisim = soyisim;
	}

}
{% endhighlight %}

Sınıfımızın içerisinde formdan gelen verileri karşılamak için 2 adet değişken oluşturduk.Bu değişkenlerin **get** ve **set** metotlarını oluşturduk. Şimdi **formuAl.jsp** sayfamızda bu **bean**'i kullanalım.

#### formuAl.jsp

{% highlight html linenos %}
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	<jsp:useBean id="kisi" class="deneme.Kisi"/>
	<jsp:setProperty property="*" name="kisi"/>

	Hoşgeldiniz Sayın
	<jsp:getProperty name="kisi" property="isim"/>
	<jsp:getProperty name="kisi" property="soyisim"/>
</body>
</html>
{% endhighlight %}

*   Önce **jsp:useBean** ile kullanacağımız sınıfı tanımladık ve ondan bir nesne oluşturduk.
*   **jsp:setProperty** etiketini kullanarak formdan gelen verileri bean'in değişkenlerine atadık. '*' kullanarak tüm değerleri tek satırda atamış olduk. Burada önemli nokta formdaki inputların name değerleri. O name değerleri **Bean** içindeki değişkenlerin isimleriyle aynı olmalı. Eğer değilse girdiğiniz değeri değil **null** değeri atar.
*   Hemen altında **bean** değişkenlerine atadığımız değerleri çekip bir hoşgeldin mesajı oluşturduk.

## Ekran Görüntüleri
![1](http://kod5.org/wp-content/uploads/2015/04/11.png)   
![2](http://kod5.org/wp-content/uploads/2015/04/21.png)   

Bu yazıda **JSP Action** Etiketlerinden en çok kullanılanları anlatmaya çalıştım. Bir sonraki yazıda **JSP Expression Tags** konusuna değineceğiz. Görüş, öneri ve sorularınızı yorum kısmında belirtebilirsiniz. Hoşçakalın..