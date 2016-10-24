---
layout: post
published: true
title: 'Linux fork, exec ve wait işlemleri'
date: '2016-10-25'
permalink: linux-fork-exec-wait-islemleri
share-img: 'http://aonurozcan.com/img/standard-share-img-3.jpeg'
---
Merhaba, bu yazıda linux sistemlerindeki fork, exec ve wait işlemlerine değineceğiz.

## Fork
Linux sistemlerinde yeni bir proses oluşturmak için **``fork()``** fonksiyonu kullanılır. Bu fonksiyon çağrıldığı prosesin bir kopyasını oluşturur ve geriye proses id döndürür. Genel olarak oluşturulan prosese child process, oluşturana ise parent process denir.

Fork işlemi başarısız olursa -1 döndürür. Başarılı olduğunda ise parent process tarafında child process'in proses id'sini, child process tarafında ise 0 döndürür. 

Proses id'ler **``pid_t``** tipinde tutulur. 

### Örnek
{% highlight c linenos %}
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main()
{
    pid_t processId;

    //Fork fonksiyonu ile yeni bir proses oluşturuluyor.
    processId = fork();

    if(processId < 0){ //Fork işlemi başarısız
      exit(EXIT_FAILURE);
    } else if(processId > 0){ 
      //Parent proseste yapılacak işlemler
    } else if (processId == 0) {
      //Child proseste yapılacak işlemler
    }
    
    return 0;
}
{% endhighlight %}

Yukarıdaki kodu kısaca açıklamak gerekirse; fork fonksiyonu çağrıldığı anda program çatallanır yani **parent** ve **child** olmak üzere iki kola ayrılır. Parent kolunda fork fonksiyonu oluşan child prosesin id'sini döndürür. Child kolunda ise fork geriye **0** döndürür. Bu sayede if else koşulları ile geriye dönen proses id'sini kontrol ederek şu an hangi prosesin içinde olduğumuzu anlayabilir ve ona göre işlemlerimizi yapabiliriz.

## Exec
Bir prosesin içerisinde ayrı bir program çalıştırmak için kullanılır. 

Aynı işi yapan fakat aralarında çok küçük farklılıklar bulunan 6 adet exec fonksiyonu vardır. İlk parametre olarak çalıştırılabilir dosyanın yol bilgisini alırlar.

* int execl(const char *path, const char *arg0, ... , (char *) 0);

* int execv(const char *path, char *const argv[]);

* int execle(const char *path, const char *arg0, ... , (char *) 0, char *const envp[]);

* int execve(const char *path, char *const argv[], char *const envp[]);

* int execlp(const char * file, const char *arg0, ... , (char *) 0);

* int execvp(const char *file, char *const argv[]);

Exec fonksiyon isimlerinin sonundaki harflere açıklık getirelim.

* **``l``** harfi varsa, exec fonksiyonu çalıştıracağımız programa gönderilecek olan parametreleri liste şeklinde alır. 
* **``v``** harfi varsa, exec fonksiyonu çalıştıracağımız programa gönderilecek olan parametreleri dizi şeklinde alır.
* **``e``** harfi varsa, exec fonksiyonu ek olarak çevre değişkeni(envp) parametresi alır.
* **``p``** harfi varsa, exec fonksiyonu çalıştırılabilir dosyanın yerinin belirlenmesinde PATH çevre değişkenlerine bakar.

**``Not``**: Parametreleri dizi ya da liste şeklinde gönderirken, dizinin ya da listenin son elemanı **NULL** olmalı. Yalnız liste şeklinde gönderirken birkaç sebepten dolayı **NULL** yerine **NULL** ile aynı anlama gelen **``(char *) 0``** değerini kullanıyoruz. 

## execv Kullanımı

#### main.c
{% highlight c linenos %}
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(){

  //execv parametreleri dizi olarak aldığından, parametre dizimizi oluşturuyoruz.
  char *args[] = {"another", "Onur", NULL};

  //Burada exec fonksiyonu 0'dan küçük değer döndürürse başarısız demektir.
  if(execv("another", args) < 0){
    exit(EXIT_FAILURE);
  }

  return EXIT_SUCCESS;
}
{% endhighlight %}

#### another.c
{% highlight c linenos %}
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[]){

  int i;

  //Gönderdiğimiz parametreler argv dizisine gelecek. Burada tüm parametreleri yazdırıyoruz.
  for(i=0; i<argc; i++)
  {
      printf("%s\n",argv[i]);
  }

  return EXIT_SUCCESS;
}
{% endhighlight %}

Yukarıda **main.c** dosyasında **execv** kullanarak aynı dizinde bulunan **another** isimli programı çalıştırdık ve parametre olarak "another" ve "Onur" değerlerini gönderdik.(Dizi şeklinde gönderim)

main.c ve another.c derlenip main programı çalıştırıldığında çıktı şu şekilde olacaktır.

~~~
another
Onur
~~~

## execl Kullanımı

#### main.c
{% highlight c linenos %}
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(){

  if(execl("another", "İlkay", "Günel", (char *) 0) < 0){
    exit(EXIT_FAILURE);
  }

  return EXIT_SUCCESS;
}
{% endhighlight %}

#### another.c
{% highlight c linenos %}
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[]){

  int i;

  //Gönderdiğimiz parametreler argv dizisine gelecek. Burada tüm parametreleri yazdırıyoruz.
  for(i=0; i<argc; i++)
  {
      printf("%s\n",argv[i]);
  }

  return EXIT_SUCCESS;
}
{% endhighlight %}

Yukarıda **main.c** dosyasında **execl** kullanarak aynı dizinde bulunan **another** isimli programı çalıştırdık ve parametre olarak "İlkay" ve "Günel" değerlerini gönderdik.(Liste şeklinde gönderim)

main.c ve another.c derlenip main programı çalıştırıldığında çıktı şu şekilde olacaktır.

~~~
İlkay
Günel
~~~

## execlp Kullanımı

#### main.c
{% highlight c linenos %}
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(){

  if(execlp("ls", "-l", (char *) 0) < 0){
    exit(EXIT_FAILURE);
  }

  return EXIT_SUCCESS;
}
{% endhighlight %}

Burada sonunda **p** harfi olan exec fonksiyonu arkaplanda ne yapıyor bir bakalım; **execlp** fonksiyonu ilk parametre olarak girilen programı sistemin **PATH** değişkeninde yazılı olan yerlerde arar. **ls** programı sistemde **/bin** konumunda bulunuyor. **PATH** değişkeni içerisinde de bin klasörü tanımlı olduğundan programımızı aynı dizinde olmamasına rağmen çalıştırabildik. 

main.c dosyası derlenip çalıştırıldığında Linux sistemlerinde dahili olarak bulunan **ls** programı **-l** parametresiyle çalıştırılacaktır ve çıktı şu şekilde olacaktır.

~~~
total 36
-rwxrwxr-x 1 onur onur 8645 Oct 24 11:53 another
-rw-rw-r-- 1 onur onur  203 Oct 24 11:48 another.c
-rwxrwxr-x 1 onur onur 8602 Oct 24 11:53 main
-rw-rw-r-- 1 onur onur  255 Oct 24 11:50 main.c
-rw-rw-r-- 1 onur onur   61 Oct 23 14:45 makefile
~~~

## execvp Kullanımı

#### main.c

{% highlight c linenos %}
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(){

    char *args[] = {"ls", "-l", NULL};

    if (execvp("ls", args) < 0) {
        perror("execvp");
        exit(EXIT_FAILURE);
    }

  return EXIT_SUCCESS;
}
{% endhighlight %}

Bir önceki örnekte yapılan işlemler burada da yapıldı. Tek fark şu: sonunda **v** olan exec fonksiyonu kullandığımız için parametreleri dizi olarak verdik.

main.c derlenip çalıştırıldığında çıktı şu şekilde olacaktır.

~~~
total 36
-rwxrwxr-x 1 onur onur 8645 Oct 24 11:53 another
-rw-rw-r-- 1 onur onur  203 Oct 24 11:48 another.c
-rwxrwxr-x 1 onur onur 8602 Oct 24 11:53 main
-rw-rw-r-- 1 onur onur  255 Oct 24 11:50 main.c
-rw-rw-r-- 1 onur onur   61 Oct 23 14:45 makefile

~~~

**``Not``**: Sonunda **p** olan exec fonksiyonları ilk parametre olarak verilen programı aynı dizinde aramazlar. Fakat eğer biz program ismimizin başına **./** koyarak parametre olarak verirsek aynı dizinde aramasını sağlayabiliriz. Kendi yazdığımız bir programı komut satırından çalıştırmak istediğimizde başına **./** koymamızın sebebi de budur. Çünkü Linux sistemi yazdığımız program ismini arka planda sonu **p** ile biten exec fonksiyonlarıyla çağırır.

## execve Kullanımı

#### main.c

{% highlight c linenos %}
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(){

  char *args[] = {"another", NULL};
  char *env[] = {"name=Mustafa Demir", NULL};

  if (execve("another", args, env) < 0)  {
      exit(EXIT_FAILURE);
  }

  return EXIT_SUCCESS;
}
{% endhighlight %}

#### another.c

{% highlight c linenos %}
#include <stdio.h>
#include <stdlib.h>

extern char **environ;

int main(int argc, char *argv[]){

  int i;

    for (i = 0; environ[i] != NULL; ++i)
        puts(environ[i]);


  return EXIT_SUCCESS;
}
{% endhighlight %}

Yukarıda main.c dosyasının içinde **execve** kullanarak aynı dizinde bulunan **another** isimli programı çağırdık. Parametre olarak "another" değerini, çevre değişkeni olarak da "name=Mustafa Demir" değerini yolladık. Sonunda **e** olan exec fonksiyonu kullanmamız bize çalıştıracağımız programa **çevre değişkeni** gönderebilmemizi sağladı.

main.c ve another.c derlenip main programı çalıştırıldığında çıktı şu şekilde olacaktır.

~~~
name=Mustafa Demir
~~~

## execle Kullanımı

#### main.c

{% highlight c linenos %}
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(){

  char *env[] = {"name=Alican Akkuş", NULL};

  if (execle("another", "another", (char *) 0, env) < 0) {
      exit(EXIT_FAILURE);
  }

  return EXIT_SUCCESS;
}
{% endhighlight %}

#### another.c

{% highlight c linenos %}
#include <stdio.h>
#include <stdlib.h>

extern char **environ;

int main(int argc, char *argv[]){

  int i;

    for (i = 0; environ[i] != NULL; ++i)
        puts(environ[i]);


  return EXIT_SUCCESS;
}
{% endhighlight %}

Bir önceki örnekten farklı bir şey yapmadık. Sadece sonunda **l** olan bir exec fonksiyonu kullandığımız için liste şeklinde yolladık.

main.c ve another.c derlenip main programı çalıştırıldığında çıktı şu şekilde olacaktır.

~~~
name=Alican Akkuş
~~~

## Wait

Bir prosesin başka bir prosesi beklemesi için kullanılır. **Wait** fonksiyonu alt proseslerden herhangi birisi sonlanıncaya kadar bekler. Yani birden fazla alt proses varsa hepsini beklemez. 

Fonksiyonunun tanımı şu şekildedir;

* pid_t wait(int *status);

**Wait** fonksiyonu geriye sonlanan prosesin **proses id**'sini döndürür. 
Parametre olarak **int** tipinde bir değişkenin adresini alır. Bu değişkene sonlanan prosesin çıkış kodu yazılır.
**``Not``**: Parametre olarak **NULL** verilirse sonlanan prosesin çıkış kodunu vermez.

## Örnek

#### main.c
{% highlight c linenos %}

#include <stdio.h>
#include <stdlib.h>

int main(){

  pid_t processId;
  int status;

  processId = fork();

  if(processId < 0){
    exit(EXIT_FAILURE);
  } else if(processId > 0){ //Parent process
    wait(NULL);
    //wait(&status); şeklinde çağırsaydım çıkış kodu status değişkenine yazılacaktı
    printf("Child Process işini tamamlayıncaya kadar beklendi\n");
  } else if(processId == 0){ //Child process
    sleep(5);
  }

  return EXIT_SUCCESS;
}

{% endhighlight %}

Yukarıdaki kodda önce fork() fonksiyonu ile child process oluşturduk. Parent process içerisinde wait() fonksiyonu ile alt proses sonlanıncaya kadar beklemesini söyledik. Child process içerisinde ise programı 5 saniye duraklattık.

main.c derlenip çalıştırıldığında 5 saniye boyunca program bekleyecek ve bu sürenin sonunda **Child Process işini tamamlayıncaya kadar beklendi** yazacaktır. Yani parent process, child processi beklemiş olacaktır.

> Kaynaklar:

[www.kaanaslan.com - Exec](http://www.kaanaslan.com/resource/article/display_article.php?key=exec&id=88){:target="_blank"}
[www.kaanaslan.com - Fork](http://www.kaanaslan.com/resource/article/display_article.php?key=fork&id=87){:target="_blank"}
