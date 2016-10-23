---
layout: post
published: true
title: JSP – Declaration Tag – 7
date: '2016-10-22'
permalink: jsp-declaration-tag-7
share-img: 'http://aonurozcan.com/img/jsp-wallpaper-small.png'
---
Merhaba. Bir önceki yazımda **JSP - Expression Tag** konusuna değinmiştim. Bu yazıda ise Declaration Tag'den bahsedeceğim. 

**Declaration Tag** değişken ve metot tanımlamaya yarar. Bu tag içerisinde tanımlanan her şey **JSP yaşam döngüsü** sırasında **_jspService()** metodunun dışına **Class** tanımının içine yazılır. Böylece **Declaration Tag** içerisinde oluşturulan değişkenler ve metotlar **Class** seviyesine sahip olur. Yani her yerden erişilebilir hale gelir. Ayrıca Declaration Tag'in içerisine yazılanlar bir kere oluşturulacağından dolayı her istek(request) geldiğinde bellek kullanılmaz 

## Söz Dizimi

{% highlight html %}
	<%! Değişken ya da metot tanımlamaları %>
{% endhighlight %}

## Metot Tanımlama

{% highlight html linenos %}
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Directive Tags</title>
	</head>
	<body>
	<%!
		public int topla(int sayi1,int sayi2){
			return sayi1+sayi2;
		}
	 %>

	 <%= "Sonuc : " + topla(3,5) %>

	</body>
</html>
{% endhighlight %}

#### Ekran Görüntüsü 
![Screenshot_3](http://kod5.org/wp-content/uploads/Screenshot_33.png)

## Değişken Tanımlama

{% highlight html linenos %}
<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Directive Tags</title>
	</head>
	<body>
	<%!
		String website = "KOD5";
		String konu = "Declaration Tag";
	 %>

	 <%= "Bu yazı " + konu + " hakkındadır." + website + "'de yazılmıştır." %>

	</body>
</html>
{% endhighlight %}

#### Ekran Görüntüsü
![Screenshot_4](http://kod5.org/wp-content/uploads/Screenshot_43.png)

## Declaration Tag ve Scriptlet Tag Arasındaki Farklar

*   **Scriptlet Tag** içerisinde sadece değişken tanımlanabilir. **Declaration Tag** içerisinde ise hem değişken hem de metot tanımı yapılabilir.
*   **Scriptlet Tag** içerisine yazılanlar **Sunucu** tarafından **_jspService()** metodunun içerisine yazılır. Dolayısıyla sadece aynı kapsam içerisinden erişilebilir. **Declaration Tag** içerisine yazılanlar ise **Class** tanımının içine **_jspService()** metodunun dışına yazılır.

Bu yazı ile birlikte sıkıcı konuları bitirmiş oluyoruz :) Bir sonraki yazıda **Implict Objects** ( Gömülü Nesneler) konusuna giriş yapacağız. Herhangi bir sorunuz olursa yorum kısmından ya da e-posta yoluyla bana ulaşabilirsiniz. Görüşmek üzere..
