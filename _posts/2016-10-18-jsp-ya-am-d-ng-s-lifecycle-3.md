---
layout: post
published: true
title: JSP Yaşam Döngüsü (Lifecycle) - 3
permalink: 2016-10-18-jsp-yasam-dongusu-lifecycle-3
---
Merhaba, [önceki](http://aonurozcan.com/2016-10-18-jsp-ilk-uygulama-2) yazımda **JSP** ile ilk uygulamamızı yapmıştık. Aslında bu yazıda **JSP** Emir etiketlerini anlatacaktım fakat aldığım tavsiyeler doğrultusunda **JSP**'nin yaşam döngüsünden bahsetmeye karar verdim. Şimdi bir **JSP** sayfası çalıştırıldığı andan itibaren arkaplanda hangi olaylar gerçekleşiyor buna bakacağız. Bir nevi işin derinine iniyoruz :) 

## Servlet Nedir? 

Servletler en basit anlamda bildiğimiz java sınıflarıdır. **HTTP** isteklerine cevap verebilmeleriyle standart **java** sınıflarından ayrılırlar. Bir **Servlet** örneği verelim.

{% highlight java linenos %}
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/ServletOrnek")
public class ServletOrnek extends HttpServlet {

    protected void doGet(HttpServletRequest request, HttpServletResponse response) 
throws ServletException, IOException {

		System.out.println("GET ISTEGI GELDIGINDE YAPILACAKLAR");

	}

    protected void doPost(HttpServletRequest request, HttpServletResponse response) 
throws ServletException, IOException {

		System.out.println("POST ISTEGI GELDIGINDE YAPILACAKLAR");

	}

}
{% endhighlight %}

Peki konumuz ile bağlantısı ne? **JSP**'den önce **Servlet**'ler vardı. Şimdinin aksine dinamik bir web sayfası oluşturmak için **HTML** kodları servletlerin içine yazılıyordu. Bu beraberinde birçok sorun getirdi. Sunum ve iş katmanlarının iç içe geçmesi, kod karmaşıklığı vs. **JSP**'de bu şekilde ortaya çıkmış oldu. 

## JSP Yaşam Döngüsü 

JSP sayfaları çalıştırıldığında önce Servlet'lere çevrilirler. Bu süreç 4 aşamada gerçekleşir: 

![JSP-Lifecycle](http://kod5.org/wp-content/uploads/2015/03/JSP-Lifecycle.jpg)

*   **index.jsp** sayfası **java** class'ına dönüştürülür.
*   Bu **class** derlenerek **.class** uzantılı dosya oluşur.
*   Derlenmiş dosya hafızaya yüklenir.
*   Hafızaya yüklenen **class**'tan bir **nesne** oluşturulur.

Bu aşamadan sonra Servlet'e dönüştürülen JSP sayfası Servlet'e benzer bir yaşam döngüsüne dahil olur.   

![Untitled Diagram](http://kod5.org/wp-content/uploads/2015/03/Untitled-Diagram.png)  

*   Oluşturulan nesnenin önce **jspInit()** metodu çağrılır ve **Servlet** hizmet vermeye hazır hale gelir.
*   Hazır halde bekleyen **Servlet**'a her istek geldiğinde **_jspService()** metodu çalışır.
*   Oluşturulan nesne yok edilmek istenirse **Server** tarafından **jspDestroy()** metodu çağrılır.

#### Önemli Noktalar:

*   **jspInit()**  metodu yaşam döngüsünde sadece bir kez çalışır.
*   **JSP** sayfamız ayağa kalkmadan önce yapmak istediklerimiz varsa **jspInit()** metodunu **override** edip içine yazabiliriz.
*   **_jspService()** metodu **HTTP** isteğinin türüne göre(GET, POST vs.) ilgili **do** metodunu çağırır(doGet, doPost vs.)
*   **_jspService()** metodu **override** edilemez.
*   **jspDestroy()** metodu da **jspInit()** gibi sadece bir kez çalışır. Uygulama kapanmadan önce yapılması gereken işlemler **override** edilerek içerisine yazılabilir.

#### JSP Yaşam Döngüsünde Server Nasıl Davranır?

*   Bir **JSP** sayfasına **request**(istek) geldiğinde **Server** bu **JSP** sayfasının derlenmiş olup olmadığına bakar. Eğer derlenmemişse önce derler. **.java** ve **.class** uzantılı dosyalar oluşur.
*   Eğer **JSP** sayfasının kodları değiştirilip tekrar çalıştırılırsa **Server** bu sayfanın daha önce derlenmediğini düşünür ve tekrar derleme işlemi yapar.
*   **jspDestroy()** metodu 3 durumda çağrılır: Server kapatıldığında, Server bellek sıkıntısı olduğuna karar verdiğinde, sayfa uzun süre **request** almadığında.

Şimdi yaşam döngüsündeki **jspInit()** ve **jspDestroy()** metotlarının ne zaman çağrıldığını uygulamayla test edelim. Bir JSP projesi oluşturdum ve içerisine **index.jsp** isimli bir dosya ekledim. **jspInit()** ve **jspDestroy()** metotlarını **override** ettim.

{% highlight html linenos %}
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>JSP Lifecycle</title>
</head>
<body>

	<%!
		public void jspInit(){

			System.out.println("######## INIT METODU CALISTI ########");

		}

		public void jspDestroy(){

			System.out.println("######## DESTROY METODU CALISTI ########");

		}
	%>

</body>
</html>
{% endhighlight %}

**Server**'ımızı başlatıp **index.jsp** dosyasını çalıştırıp konsola bir göz atalım.

![1](http://kod5.org/wp-content/uploads/2015/03/13.png)

Gördüğünüz gibi **index.jsp** dosyasını ilk kez çalıştırdığımız için önce servlete çevrildi ve ilk olarak **jspInit()** metodu çağrıldı. Şimdi **server**ı kapatıp tekrar konsola bakalım. 

![2](http://kod5.org/wp-content/uploads/2015/03/22.png) 

Server oluşturduğu servletin yaşam döngüsünü **jspDestroy()** metodunu çağırarak bitirdi ve kullandığı kaynakları serbest bıraktı. 

Bu yazıda **JSP** yaşam döngüsünü elimden geldiğince açıklamaya çalıştım. Umarım faydalı olmuştur. Takıldığınız bir yer olursa yorum kısmından sorularınızı bana iletebilirsiniz. Bir sonraki yazıda [JSP’de **Directive Tags**](http://kod5.org/jsp-directive-tags-emir-etiketleri-4/) yani **Emir Etiketlerini **göreceğiz. Hoşçakalın..