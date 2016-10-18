---
layout: post
published: true
title: Java Server Pages - JSP Giriş - 1
permalink: 2016-10-18-java-server-pages-jsp-giris-1
---
Herkese merhaba. Bu yazı dizisinde sizlere JSP ve JSP içerisinde kullandığımız diğer teknolojileri(Servlet, JSTL, EL) anlatmaya çalışacağım. Halen öğrenmeye devam eden birisi olarak hatalarım olabilir. Dolayısıyla istek, öneri ve şikayetlerinizi benimle paylaşmaktan çekinmeyin. 

## JSP nedir? 

JSP, programlama dili olarak JAVA’yı kullanan ve dinamik websiteleri oluşturmayı sağlayan bir JAVA teknolojisidir. 

## JSP ile Neler Yapılabilir?

*   Web tabanlı İçerik Yönetim Sistemleri (CMS).
*   Bloglar, Sosyal Medya uygulamaları ve forumlar.
*   Veritabanı bağlantılı uygulamalar.
*   Ziyaretçi istatistik uygulamaları.
*   Web üzerinden doküman gönderme ve indirme yönetimi.
*   Cookie ve Session uygulamaları.
*   Üyelik sistemleri.

## JSP’nin Avantajları

*   JSP’de sunum ve program mantığı birbirinden ayrılır.
*   JSP Java’nın gücünden faydalanır.
*   JSP farklı platformlarda ve farklı sunucularda çalışabilir.
*   JSP esnektir.
*   JSP özgün takıları destekler.

## JSP ile basit bir “Hello World” örneği
  
{% highlight html linenos %}
<html>

<head>

   <title>JSP Örnek</title>

</head>

<body>

   <% out.print("Hello World"); %>

</body>

</html>
{% endhighlight %}

  Yukarıdaki örnekte dikkat ederseniz **HTML** kodlarının içinde **JAVA** kodu kullandık. İşte **JSP** tam olarak budur. **JSP** sayfalarında **JAVA** kodları **<%  %>** etiketleri arasına yazılır ve sonlarına noktalı virgül konur.  
  
  **JSP Mimarisi** ![ada](http://kod5.org/wp-content/uploads/2015/03/ada-1024x274.jpg){:target="_blank"}

1.  HTML formundan gelen bilgi JSP veya Servlet’e gider.
2.  JSP sayfası iş bileşenlerine bağlanır.
3.  İş bileşenleri Database işlemlerini yapar.
4.  Yapılan işlemler sonucunda JSP sayfasına bilgi döner.
5.  JSP dönen sonucu client’e yani kullanıcıya gönderir.

 Dilerseniz bu kısa tanıtımdan sonra uygulamaya geçelim. Ben bu eğitimde sunucu olarak **Apache Tomcat**, IDE olarak da **Eclipse** kullanacağım. Tabii hepsinden önce sistemimize **JDK**’yı kurmamız gerekiyor. Başlayalım!   
 
## JDK Kurulumu

1.  **[Buradan](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html){:target="_blank"}** JDK indirme sayfasına gidiyoruz. Şartları kabul edip hangi işletim sistemini kullanıyorsak ona uygun olan sürümünü indiriyoruz. Ben 64 bit Windows kullandığım için onu işaretledim.

![1](http://kod5.org/wp-content/uploads/2015/03/11-1024x570.png){:target="_blank"}

2.  İndirdiğimiz dosyayı açıp kuralım. Kurulum aşamasında herhangi bir ayar yapmamıza gerek yok.

3.  Eğer windows kullanıyorsanız, kurulumdan sonra birkaç ayar yapmanız gerekecek. Bilgisayarım simgesine sağ tıklayıp özellikler diyelim. Gelişmiş sistem ayarlarına tıklayalım.

![2.1](http://kod5.org/wp-content/uploads/2015/03/2.1-1024x545.png){:target="_blank"}

4.  Açılan pencerede **`Ortam Değişkenleri`**’ne tıklayalım.

![3](http://kod5.org/wp-content/uploads/2015/03/3.png){:target="_blank"}

5.  Alt kısımdan **`Path`** değişkenini bulup düzenle diyoruz.

![4](http://kod5.org/wp-content/uploads/2015/03/4.png){:target="_blank"}

6.  Değişken değeri yazan bölümde en sona kadar gidip bir noktalı virgül koyuyoruz ve JDK’mızın kurulu olduğu dizinin adresini yazıyoruz. **“C:\Program Files\Java\jdk1.8.0_20\bin” **(Sizde farklı olabilir)

![5](http://kod5.org/wp-content/uploads/2015/03/5.png){:target="_blank"}

7.  İşlem tamam. Artık bu eğitimde kullanacağımız Eclipse IDE’nin kurulumuna geçebiliriz.

## Eclipse IDE Kurulumu

1.  [**Buraya**](https://eclipse.org/downloads/){:target="_blank"} tıklayarak Eclipse indirme sayfasına gidiyoruz. İşaretlediğim kısımdan işletim sistemimize uygun olanına tıklıyoruz.

![1](http://kod5.org/wp-content/uploads/2015/03/12-1024x547.png)

1.  Sonraki sayfada yeşil butona tıklayıp indirmemizi başlatıyoruz.

![2](http://kod5.org/wp-content/uploads/2015/03/21-1024x546.png){:target="_blank"} Eclipse IDE’nin bildiğimiz anlamda kurulumu yoktur. İndirmeniz bittiğinde .zip uzantılı bir dosya göreceksiniz. O dosyayı istediğiniz yere çıkarıp içindeki eclipse.exe’yi tıklayarak **Eclipse**’yi kullanmaya başlayabilirsiniz.   **Apache Tomcat Kurulumu**

1.  **[Buradan](http://tomcat.apache.org/){:target="_blank"}** önce Tomcat’in ana sayfasına sonra işaretli olan yere tıklayarak indirme sayfasına gidelim.

![1](http://kod5.org/wp-content/uploads/2015/03/1-1024x546.png){:target="_blank"}

1.  İndirme sayfamızda yine işletim sistemimize uygun olanı seçip indiriyoruz.

![2](http://kod5.org/wp-content/uploads/2015/03/2-1024x546.png){:target="_blank"}

1.  Tomcat tıpkı Eclipse gibi taşınabilir bir program. İndirdiğiniz .zip uzantılı dosyayı istediğiniz yere çıkartabilirsiniz. Eclipse klasörünün içine çıkarmak taşınabilirlik açısından daha uygundur.

JSP projesi geliştirmemiz için ihtiyacımız olan tüm programları indirdik ve kurduk. Bir sonraki [yazımızda](http://kod5.org/jsp-ile-ilk-uygulama-2/){:target="_blank"} ilk JSP projemizi Eclipse üzerinde oluşturacağız. Yazıda anlatılanlarla ilgili aklınıza takılan soruları yorum kısmında sorabilirsiniz. Görüşmek üzere.. **Kaynaklar:**

1.  _Java Server Pages - Numan Pekgöz_
2.  _Java Server Faces Slaytı - Ahmet Demirelli_
