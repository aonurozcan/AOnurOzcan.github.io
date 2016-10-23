---
layout: post
published: true
title: JSP - Out Implicit Object - 9
permalink: jsp-out-implicit-object-9
share-img: 'http://aonurozcan.com/img/jsp-wallpaper-small.png'
date: '2016-10-23'
---
Merhaba, bir önceki yazıda JSP Implicit Objects'e giriş yapmıştık. Bu yazıda Out objesini birkaç örnekle anlatacağım. Bu nesne **javax.servlet.jsp.JspWriter** sınıfından oluşturulmuştur.  Genel anlamda ekrana veri basmak için kullanılır. Toplam 9 adet metodu vardır. 

## Out Nesnesinin Metotları

*   void print()
*   void println()
*   void newLine()
*   boolean isAutoFlush()
*   void clear()
*   void clearBuffer()
*   void flush()
*   int getBufferSize()
*   int getRemaining()

#### void print()
Parametre olarak gönderilen veriyi ekrana basar.
{% highlight java linenos %}
<% out.print("KOD5'e Hoşgeldiniz."); %>
{% endhighlight %}

[![1](http://kod5.org/wp-content/uploads/119.png)](http://kod5.org/wp-content/uploads/119.png) 

#### void println()
print metodundan tek farkı veriyi ekrana bastıktan sonra bir alt satıra geçmesidir.
#### void newLine()
Yeni bir satır açar. println() metodu ile aynı işlevi görür. 

Diğer metotları açıklamadan önce buffer kavramına değinmek istiyorum. Buffer'ın kelime anlamı tampon'dur. Jsp sayfalarında ekrana basılan verinin boyutunu sınırlar. Varsayılan olarak 8kb'dir. Yine varsayılan olarak 8kb yetmediğinde otomatik olarak buffer tazelenir. Buna da **``flush``** denir. 

#### boolean isAutoFlush()
Buffer dolduğunda otomatik olarak arttırılıp arttırılmayacağını "true" ya da "false" olarak tutar. Varsayılan olarak "true" dir. 
#### void clear()
Ekrana o ana kadar ne yazdırılmışsa temizler. Yani buffer'ı sıfırlar. 
#### void clearBuffer()
clear() metoduyla aynı işlevi görür. Bu metodun farkı eğer önceden flush() metodu çağrıldıysa exception(hata) fırlatır. 
#### flush()
Bufferı tazeler. Sınırın aşılmasını sağlar. Eğer flush() metodu çağrılmaz ve sınır aşılırsa  "java.io.IOException: Error: JSP Buffer overflow" hatası fırlatılır. 
#### getBufferSize()
Varsayılan buffer değerini(8kb) döndürür.  1024 * 8 = 8192 değeri ekranda görünür. 
#### getRemaining()
Buffer'da ne kadar boş alan kaldığını anlamamızı sağlar. 

## Buffer, Flush Kavramlarıyla İlgili Örnek

{% highlight html linenos %}
<html>
<body>

	<%out.print("Burasi ekrana basilmayacak. Cunku clearBuffer() metodu cagrilacak."); %>
	<%out.clearBuffer(); %>
	<%="Buffer Miktari : " + out.getBufferSize() %><br>
	<%out.print("KOD5'e Hosgeldiniz!"); %><br>
	<%="Ekrana yazi basildiktan sonra kalan miktar : " + out.getRemaining()%>
	<%out.flush();%><br>
	<%="Flush yapildiktan sonra kalan miktar : " + out.getRemaining()%><br>

	<%-- <%out.clear();%> Bu satir hata firlatacak. Cunku daha onceden flush() metodu cagrildi! --%>

</body>
</html>
{% endhighlight %}

## Ekran Görüntüsü 
[![2](http://kod5.org/wp-content/uploads/210.png)](http://kod5.org/wp-content/uploads/210.png) 

Bu yazıda out nesnesini ve metotlarını elimden geldiğince anlatmaya çalıştım. Yazılarımın gidişatı hakkında her türlü görüş ve öneriye açığım. Okuduğunuz için teşekkürler. Bir sonraki yazıda JSP - Request Implicit Object konusuna değineceğiz. Hoşçakalın..
