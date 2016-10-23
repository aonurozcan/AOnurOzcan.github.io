---
layout: post
published: true
title: JSP – Session Implicit Object – 12
permalink: jsp-session-implicit-object-13
share-img: 'http://aonurozcan.com/img/jsp-wallpaper-small.png'
date: '2016-10-23'
---
Merhaba, bir önceki yazıda response implicit nesnesinden bahsetmiştik. Bu yazıda ise kullanıcı giriş, çıkış işlemlerinde kullanılan **session** nesnesine değineceğiz. 

**Session** nesnesi **JSP** ile yazılım geliştirirken en çok kullanılan nesnelerden biridir. İşlevi, kullanıcı bilgilerine oturumu aktif olduğu süre boyunca uygulama içerisinde her yerden erişim sağlamaktır. Önce **Session** nesnesinin metotlarını inceleyelim, sonrasında bir kullanıcı login-logout örneği yapalım.

*   **setAttribute(String, object)**
*   **getAttribute(String name)**
*   **removeAttribute(String name)**
*   **getAttributeNames()**
*   **getCreationTime()**
*   **getLastAccessedTime()**
*   **isNew()**
*   **invalidate()**
*   **getMaxInactiveInterval()**

#### setAttribute(String, object)
Bu metot **session** nesnesinde veri saklamamızı sağlar. Metodun ikinci parametresi saklamak istediğimiz veriyi alır, ilk parametresi ise bu veriye hangi isimle ulaşacağımızı belirler. Veri **Object** tipinde olduğundan **her tipte** veri saklayabilir. 

#### getAttribute(String name)
Parametre olarak aldığı değere bağlı bir veri saklanıyorsa onu getirir. Eğer saklanan bir veri yoksa **null** döndürür. 

#### removeAttribute(String name)
Session nesnesinde saklanan bir veriyi silmeyi sağlar.  Parametre olarak veriye atanmış String tipindeki değer verilmelidir. 

#### getAttributeNames()
Session nesnesinde o anda ne kadar veri varsa o verilere karşılık gelen String tipindeki değerleri **Enumeration** tipinde döndürür. 

#### getCreationTime()
Session nesnesinin ilk oluşturulduğu tarihi verir. 

#### getLastAccessedTime()
Oturumun açık olduğu süre boyunca uygulamaya en son erişilen tarihi verir. 

#### isNew()
Oturumun yeni olup olmadığını kontrol eder. **True** ya da **false** döndürür. Aynı zamanda tarayıcının **cookie**(çerez) özelliğinin açık olup olmadığını kontrol etmek için de kullanılır. Eğer **cookie** özelliği kapalıysa **isNew()** metodu her zaman **true** dönecektir. 

#### invalidate()
Oturumu sonlandırmaya yarar. Bu metot çağrıldıktan sonra tekrar **session** nesnesine erişilmeye çalışılırsa hata alınır. 

#### getMaxInactiveInterval()
Kullanıcı hiçbir işlem yapmazsa, oturumun ne kadar sürede sonlanacağını saniye cinsinden döndürür. Varsayılan değeri **1800** saniye(**30dk**) 'dır.

## JSP Session Login - Logout Example

#### Form.jsp

{% highlight html linenos %}
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page session="false" %>
<html>
<head>
    <title>Login Form</title>
</head>
<body>
<form action="FormControl.jsp" method="post">
    <table>
        <tr>
            <td>Kullanıcı Adı :</td>
            <td><input type="text" name="username"></td>
        </tr>
        <tr>
            <td>Şifre :</td>
            <td><input type="text" name="password"></td>
        </tr>
        <tr>
            <td></td>
            <td><input type="submit" value="Giriş Yap" style="width:100%;"></td>
        </tr>
    </table>
</form>
</body>
</html>
{% endhighlight %}

#### FormControl.jsp

{% highlight java linenos %}
<%
    //Form.jsp sayfasından gelen username ve password değerlerini alıyoruz.
    String username = request.getParameter("username");
    String password = request.getParameter("password");

    //Eğer username "onur" ve password "kod5" ise session nesnesine kullanıcı adımızı atıyoruz.
    if (username.equals("onur") && password.equals("kod5")) {
        session.setAttribute("username", username);
        response.sendRedirect("UserPage.jsp");
    } else {
        //username ya da password yanlışsa Error.jsp'ye yönlendir.
        response.sendRedirect("Error.jsp");
    }
%>
{% endhighlight %}

#### UserPage.jsp

{% highlight html linenos %}
<%@ page import="java.util.Date" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title></title>
</head>
<body>

<% String username = (String) session.getAttribute("username"); %>
<h2>
    Hoşgeldiniz, Sayın <%=username%><br>
    Oturum açma tarihiniz : <%=new Date(session.getCreationTime())%><br>
    Son erişim tarihiniz : <%=new Date(session.getLastAccessedTime())%><br>
</h2>

<h3><a href="Logout.jsp">Çıkış yap</a></h3>

</body>
</html>
{% endhighlight %}

#### Error.jsp

{% highlight html linenos %}
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Error Page</title>
</head>
<body>
<h2 style="color: red">Kullanıcı adı ya da şifre yanlış!</h2>

<h2><a href="Form.jsp">Tekrar Deneyin</a></h2>
</body>
</html>
{% endhighlight %}

#### Logout.jsp

{% highlight html linenos %}
<%
    session.invalidate();
    response.sendRedirect("Form.jsp");
%>
{% endhighlight %}

#### Ekran Görüntüleri

[![1](http://kod5.org/wp-content/uploads/125.png)](http://kod5.org/wp-content/uploads/125.png) 
[![2](http://kod5.org/wp-content/uploads/218.png)](http://kod5.org/wp-content/uploads/218.png) 

Birkaç saniye sonra sayfayı yenilersek, oturum açma tarihi aynı kalır, son erişme tarihi değişir. 
[![](http://kod5.org/wp-content/uploads/316.png)](http://kod5.org/wp-content/uploads/316.png) 

Yanlış bilgi girersek, **FormControl.jsp** sayfasındaki kontrolümüzden sonra **Error.jsp**'ye yönlendirilecektir. 

[![4](http://kod5.org/wp-content/uploads/49.png)](http://kod5.org/wp-content/uploads/49.png)  
[![5](http://kod5.org/wp-content/uploads/59.png)](http://kod5.org/wp-content/uploads/59.png) 

Bu yazıda **Session** nesnesini anlatmaya çalıştım . Herhangi bir sorunuz olursa bana profil resmimin altındaki iletişim adreslerinden ulaşabilirsiniz. Bir sonraki yazı **application** nesnesi hakkında olacak. Görüşmek üzere. Hoşçakalın..