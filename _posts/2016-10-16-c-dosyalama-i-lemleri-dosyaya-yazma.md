---
layout: post
published: true
title: C++ Dosyalama İşlemleri (Dosyaya Yazma)
permalink: cpp-dosyalama-islemleri-dosyaya-yazma
---
C++ da dosyalama işlemleri C den oldukça farklı. Şimdi C++ da dosyalama işlemleri için başlıca hangi komutları kullanıyoruz bir bakalım.

  * fstream (Okuma ve Yazma)
  * ifstream (Okuma)
  * ofstream (Yazma)

Peki dosyamızı hangi modda açacağız? (Sadece temel modları yazıyorum)

  * ios::in (Veri okuma modu)
  * ios::out (Veri yazma modu)
  * ios::app (Veri ekleme modu)

İzlememiz gereken adımlar;

  1. Önce seçeceğimiz sınıftan bir nesne yaratıyoruz.
  2. Bu nesneden **`open()`** fonksiyonu kullanarak bir dosya oluşturuyoruz ve modunu belirliyoruz.
  3. Dosya açma kalıbımız şu şekilde; Nesneİsmi.open(Dosyaİsmi, Mod)
  4. İşlemlerimizi yapıyoruz.
  5. **`close()`** fonksiyonunu kullanarak dosyamızı kapatıyoruz.

Bir örnek yapalım. Deneme.txt isimli bir dosya oluşturup içine veri girelim

{% highlight cpp linenos %}
#include <fstream> //Dosyalama işlemleri için gerekli olan kütüphane.
using namespace std;

int main() {
  ofstream dosya; //ofstream sınıfından bir nesne oluşturduk.Herhangi bir isim verebilirsiniz.
  dosya.open("deneme.txt"); //deneme.txt isimli bir dosya açtık.
  dosya << "Dosyaya yazi yazdim."; //Dikkat edin burada oluşturduğumuz nesneyi cout gibi kullanıyoruz.
  dosya.close(); //dosyayı kapattık.
  return 0;
{% endhighlight %}

Programımızın çıktısı şu şekilde olacaktır.

![ProgramCiktisi]({{site.baseurl}}/img/denmee.png)
