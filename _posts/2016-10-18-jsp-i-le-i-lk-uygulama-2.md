---
layout: post
published: true
title: JSP İle İlk Uygulama - 2
permalink: 2016-10-18-jsp-ilk-uygulama-2
---
Merhaba, [önceki](http://aonurozcan.com/2016-10-18-java-server-pages-jsp-giris-1) yazımda **JSP**’nin ne olduğundan, **JSP** ile neler yapılabildiğinden, gerekli **IDE** ve araçların kurulumundan bahsetmiştim. Bu yazıda teorik bilgilerden sonra artık uygulamaya geçebiliriz. Bu aşamada size tavsiyem bir klasör oluşturun ve bu klasörün içine **Eclipse** ve **Tomcat**’i atın. Bu şekilde projenizi başka bir bilgisayarda çalıştırmak isterseniz klasörü olduğu gibi kopyalamak yeterli olacaktır. Hazırsanız başlayalım..   

Size söylediğim gibi **“JSP_Uygulama”** isimli bir klasör oluşturdum ve içerisine **Eclipse** ve **Tomcat’i** attım. 

![k2](http://kod5.org/wp-content/uploads/2015/03/k2.png) 

Daha sonra **Eclipse**’yi çalıştıralım. **Eclipse** ilk açıldığında bizden workspace yani çalışma alanı ister. Bu yüzden projelerimizin düzenini sağlamak için **Eclipse** klasörü içerisinde **“JSP_Egitim”** isimli bir klasör oluşturdum. Projelerimiz bu klasörün içerisinde bulunacak. 

![k3](http://kod5.org/wp-content/uploads/2015/03/k3.png) 

**Eclipse’ye** bu klasörü göstereceğim. **Eclipse** IDE’yi açalım. **Browse** butonuna basarak oluşturduğumuz klasörü workspace olarak belirleyelim. Bunu bize her defasında sormaması için sol alttaki kutucuğa tik atalım **OK** diyelim. 

![k4](http://kod5.org/wp-content/uploads/2015/03/k4.png)

Şimdi projelerimizde kullanacağımız sunucumuzu ekleyelim. Alt kısımdan **servers**’a daha sonra **create a new server**’a tıklayalım. Açılan pencerede **Apache** klasörünün içinden **Tomcat v8.0 Server**’ı seçelim ve **next** diyelim. 

![k6](http://kod5.org/wp-content/uploads/2015/03/k6-1024x545.png)

Sonraki sayfada **Browse** diyelim. **JSP_Uygulama** isimli klasörün içine attığımız **Tomcat** klasörünü seçelim. Tamam diyelim ve **Finish**’e basarak bitirelim. 

![k7](http://kod5.org/wp-content/uploads/2015/03/k7-1024x545.png)

Şimdi projemizi oluşturalım. Sol üstten **File -> New -> Dynamic Web Project** diyelim. 

![k5](http://kod5.org/wp-content/uploads/2015/03/k5-1024x545.png) 

**Project name** kısmında projemize bir isim verelim. **Target runtime** kısmı bu şekilde gelmiş olması gerekiyor. Gelmezse biraz önce oluşturduğumuz **server**’ımızı bulup seçebilirsiniz. **Next** diyelim. 

![k8](http://kod5.org/wp-content/uploads/2015/03/k8.png) 

Bu bölümde herhangi bir değişiklik yapmamıza gerek yok. **Next** diyelim. 

![k9](http://kod5.org/wp-content/uploads/2015/03/k9.png)

Bu sayfada işaretliğim yere bir tik atalım. Bu, projelerimiz için gerekli olan **web.xml** dosyasının otomatik olarak oluşmasını sağlayacak. Belki bu projemizde işe yaramayacak ama ileri seviye projelerde buraya tik atmayı ihmal etmeyin. **Finish** deyip bitirelim. 

![k10](http://kod5.org/wp-content/uploads/2015/03/k10.png){:target="_blank"}

Projemizi oluşturduk. Şimdi projemize bir **JSP** dosyası ekleyelim ve ilk uygulamamızı çalıştıralım. **Web Content**’in üzerine gelip sağ tıklıyoruz. **New -> JSP File** diyoruz. 

![k11](http://kod5.org/wp-content/uploads/2015/03/k11-1024x546.png)

Açılan pencerede dosyamıza bir isim veriyoruz. Herhangi bir isim verebilirsiniz fakat projeyi çalıştırdığımızda ilk bu sayfanın çalışmasını istiyorsanız **index.jsp** diyebilirsiniz. **Finish** deyip bitirelim. 

![k12](http://kod5.org/wp-content/uploads/2015/03/k12.png)

**index.jsp** isimli dosyamızın içeriğini şu kodlarla dolduralım.

{% highlight html linenos %}
<%@ page language="java" contentType="text/html; charset=ISO-8859-1" pageEncoding="ISO-8859-1"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">

<html>
<head>
   <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
   <title>JSP Ilk Uygulama - 2</title>
</head>
<body>
	<%
	     out.println("Hello World!");
	%>
</body>
</html>
{% endhighlight %}

Daha sonra dosyamıza sağ tıklayalım. **Run as -> Run on Server** diyelim. 

![k13](http://kod5.org/wp-content/uploads/2015/03/k13-1024x545.png) 

Bu kısımda daha önceden oluşturmuş olduğumuz sunucumuzu seçelim. Sol alt köşede işaretlediğim kısma tik atalım ki bir daha bize bunu sormasın. **Finish** deyip sayfamızı çalıştıralım. 

![k14](http://kod5.org/wp-content/uploads/2015/03/k14.png) 

Evet ilk projemizi oluşturduk ve çalıştırdık. 

![k15](http://kod5.org/wp-content/uploads/2015/03/k15-1024x545.png)

Ekrana sadece “Hello World” yazdırmış olsak da bu yazıda bir proje nasıl oluşturulur, bir JSP dosyası nasıl eklenir gibi sorulara cevap vermeye çalıştık. Umarım faydalı olmuştur. Takıldığınız bir yer olursa yorum kısmından sorularınızı bana iletebilirsiniz. Bir sonraki yazıda [JSP Yaşam Döngüsü](http://kod5.org/jsp-yasam-dongusu-lifecycle-3/)nden bahsedeceğiz. Hoşçakalın..