---
layout: post
published: true
title: JSP – Directive Tags - 4
permalink: 2016-10-21-jsp-directive-tags-4
date: '2016-10-21'
---
Merhaba, bu yazıda emir etiketlerini göreceğiz. Emir etiketleri, JSP sayfalarının tüm işleyişini kontrol eder ve uygulama sunucusuna işleyişle ilgili bilgiler verir. Emir etiketleri JSP sayfalarının herhangi bir yerinde bulunabilir fakat genellikle en üste yazılır ve **<%@ .. %>**  arasına kodlanır. 

## Syntax (Söz dizimi)

{% highlight java linenos %}
<%@ emir attribute="değer" %>
{% endhighlight %}

Emir etiketlerinin 3 çeşidi vardır. Bunlar:

*   **Page**
*   **Include**
*   **Taglib**

## Page Etiketi

Etiketlerin attribute diye adlandırdığımız öznitelikleri vardır. Page etiketine bu öznitelikleri ekleyerek sayfa hakkında uygulama sunucusuna bilgi veririz. Fazla kafa karıştırmamak için bu özniteliklerden sadece önemli olanlarını inceleyeceğim. Bunlar:

*   contentType
*   errorPage
*   isErrorPage
*   import
*   language

  **1- contentType:** **JSP** sayfasının tipini belirler. text/html, text/xml gibi değerler alabilir. Varsayılan değeri **text/html**’dir. Kullanım:

{% highlight java linenos %}
<%@ page contentType="text/html" %>
{% endhighlight %}

  **2- errorPage:** Eğer bir **JSP** sayfasında hata oluşması bekleniyorsa, hata oluştuğunda hangi sayfaya gidileceğini bu öznitelikle belirleyebiliriz. Tabii gidilecek sayfa **isErrorPage** özniteliği kullanılarak hata sayfası olarak tanımlanmalı. Bir sonraki başlıkta bu konuya değineceğiz. Kullanım:

{% highlight java linenos %}
<%@ page errorPage="hataSayfasi.jsp" %>
{% endhighlight %}

  **3- isErrorPage:** Üstteki başlıkta, hata oluştuğunda gidilecek sayfanın hata sayfası olarak belirlenmesi gerektiğini söylemiştik. Bu özniteliğimiz yazıldığı sayfayı hata sayfası olarak tanımlar ve başka bir **JSP** sayfasından hata oluştuğunda çağrılmasına izin verir. Varsayılan değeri **false**’dir. Kullanım:

{% highlight java linenos %}
<%@ page isErrorPage="true" %>
{% endhighlight %}

  **4- import:** Çeşitli paketleri ve bu paketlerin altındaki java classlarını dahil etmek için kullanılır. Örneğin **Date** sınıfını kullanmak istiyorsak bu özniteliği kullanarak sayfaya “**java.util.Date**” i dahil etmeliyiz. Kullanım:

{% highlight java linenos %}
<%@ page import="java.util.Date" %>
{% endhighlight %}

  **5- language:** **JSP** sayfasının temelinde kullanılan programlama dilini ifade eder. Varsayılan değeri **java**’dır. Kullanım:

{% highlight java linenos %}
<%@ page language="java" %>
{% endhighlight %}

## Include Etiketi
  
  Include etiketi bir **JSP** sayfasının içeriğini olduğu gibi başka bir **JSP** sayfasına dahil eder. Bu işlemi **file** özniteliği ile yapar. Kullanım:

{% highlight java linenos %}
<%@ include file="dahilEdilecekSayfa.jsp" %>
{% endhighlight %}

## Taglib Etiketi
Bu etiket, kullanıcının özel olarak oluşturulan etiketleri(**custom tags**) kullanmasını sağlar. **JSTL** (Java Standard Tag Library) de bu sistemle oluşturulduğundan **JSTL**’i kullanmak için sayfalarımızın başında **taglib** etiketini kullanmalıyız. **JSTL**’i ileride daha detaylı inceleyeceğiz. Taglib etiketinin iki tane özniteliği vardır. Bunlar: **uri** ve **prefix**. Prefix ön ek demektir. Kullanacağımız özel etiketlerin önüne koyarız. **Örnek**: **JSTL** içindeki **c:out** etiketi. **C** burada ön ektir. **Uri** özniteliği ise kullanacağımız özel etiketlerin kaynağını belirtir. Kullanım:

{% highlight java linenos %}
<%@ taglib  prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
{% endhighlight %}

Not: **JSTL** kullanabilmeniz için ilgili jar dosyasını projenizin **WebContent\WEB-INF\lib** dizinine atmanız gerekmektedir. Gerekli Jar dosyasını şu linkten indirebilirsiniz : [JSTL 1.2](http://central.maven.org/maven2/javax/servlet/jstl/1.2/jstl-1.2.jar) Evet, **Directive Tags**(Emir etiketleri) konusunu tamamlamış olduk. Şimdi öğrendiklerimizle bir uygulama yapalım. Hazır mıyız? 

**index.jsp**

{% highlight html linenos %}
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ page import="java.util.Date,java.text.DateFormat"%>

<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Directive Tags</title>
	</head>
	<body>
		<%@ include file="tarihYazdir.jsp" %>
		<form action="bolmeYap.jsp" method="post">
			<table>
				<tr>
					<td><c:out value="İlk Sayı:"/></td>
					<td><input type="text" name="ilkSayi"/></td>
				</tr>
				<tr>
					<td><c:out value="İkinci Sayı:"/></td>
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

*   **JSP** sayfamızla ilgili ayarları yaptık. **language="java"** ve **contentType="text/html"** dedik.
*   2.satırda **JSTL**’nin özel etiketlerini kullanmak için **taglib** emir etiketini kullandık.
*   13.satırda **<%@ include >** etiketi ile **tarihYazdir.jsp** dosyasını çağırdık. **tarihYazdir.jsp** dosyasında java kodları ile tarih ve saat bilgisi alınıp ekrana yazdırılıyor. Dikkat ederseniz kodlar **tarihYazdir.jsp**’de fakat çalıştığı yer **index.jsp**. Çünkü biz o kodları dahil(include) ettik.
*   14.satırda butona basıldığında **bolmeYap.jsp**'ye gidecek bir form oluşturduk.

  **tarihYazdir.jsp**

{% highlight java linenos %}
<%@ page import="java.util.Date"%>
<%@ page import="java.text.DateFormat"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
"http://www.w3.org/TR/html4/loose.dtd">

<html>
<head>
   <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
   <title>Tarih Saat</title>
</head>
<body>
<%
String tarih = DateFormat.getDateTimeInstance(DateFormat.FULL,DateFormat.SHORT)
                         .format(new Date());
out.println(tarih);
%>
</body>
</html>
{% endhighlight %}

*   İlk iki satırda sayfa içerisinde tarihlerle ilgili işlem yapacağımız için gerekli paketleri **import** ettik.
*   **JSP** etiketleri arasında **java** kodu kullanarak tarih ve saati alıp ekrana yazdırdık.

  **bolmeYap.jsp**

{% highlight java linenos %}
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" errorPage="hataSayfasi.jsp"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
"http://www.w3.org/TR/html4/loose.dtd">

<html>
<head>
   <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
   <title>Bölme Sayfası</title>
</head>
<body>
	<%
		int ilkSayi = Integer.parseInt(request.getParameter("ilkSayi"));
	 	int ikinciSayi = Integer.parseInt(request.getParameter("ikinciSayi"));
	 	int sonuc = ilkSayi / ikinciSayi;
	 	out.println("İŞLEMİN SONUCU : " + sonuc);
	%>
</body>
</html>
{% endhighlight %}

*   **Page** emir etiketinin içerisinde **errorPage** belirledik. Yani bu sayfada bir hata olursa **hataSayfasi.jsp**’ye git dedik.
*   **Body** içerisinde formdan gelen verileri aldık ve bölme işlemini yapıp ekrana yazdırdık.(**request** objesini ve fonksiyonlarını daha sonra detaylı inceleyeceğiz. **getParameter()** fonksiyonu formdan gelen verileri almak için kullanılır. Bunu bilmeniz şimdilik yeterli.)

  **hataSayfasi.jsp**

{% highlight java linenos %}
<%@ page language="java" contentType="text/html; charset=UTF-8"
    	 pageEncoding="UTF-8" isErrorPage="true"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" 
"http://www.w3.org/TR/html4/loose.dtd">

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

*   **Page** emir etiketinin içerisinde **isErrorPage** özniteliğine true değeri verdik. **bolmeYap.jsp**’de hata olduğunda buraya yönlendirilmesini söylemiştik ama bu sayfanın hata sayfası olduğunu doğrulamadık. **isErrorPage** özniteliğiyle bunu sağladık.
*   **<%=exception%>** kod parçası ile oluşan hatayı ekrana yazdırdık.

  Özetleyecek olursak, index.jsp’de öncelikle tarihYazdir.jsp isimli sayfa dahil edilmiş. Ardından iki adet sayı alan bir form var. Girilen iki sayı bolmeYap.jsp’ye gönderiliyor ve orada işlem yapılıp sonuç ekrana yazdırılıyor. Eğer bir hata meydana gelirse önceden belirlenmiş olan hataSayfasi.jsp’ye yönlendiriliyor.  
  
## Ekran Görüntüleri

  ![1](http://kod5.org/wp-content/uploads/2015/03/14.png) 
  
  ![2](http://kod5.org/wp-content/uploads/2015/03/23.png)
  
  Eğer ikinci sayıya “0” girersek, sıfıra bölme hatası oluşacağından bizi hata sayfasına yönlendirecek. 
  
  ![3](http://kod5.org/wp-content/uploads/2015/03/31.png) 
  
  ![4](http://kod5.org/wp-content/uploads/2015/03/41.png) 
  
  Bir sonraki yazıda **JSP Action Tags**’leri işleyeceğiz. Görüşmek üzere.