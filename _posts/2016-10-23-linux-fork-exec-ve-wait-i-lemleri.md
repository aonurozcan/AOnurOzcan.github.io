---
layout: post
published: true
title: 'Linux fork, exec ve wait işlemleri'
date: '2016-10-24'
permalink: linux-fork-exec-wait-islemleri
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

Yukarıdaki kodu kısaca açıklamak gerekirse; fork fonksiyonu çağrıldığı anda program çatallanır yani parent ve child olmak üzere iki kola ayrılır. Parent kolunda fork fonksiyonu oluşan child prosesin id'sini döndürür. Child kolunda ise fork geriye 0 döndürür. Bu sayede if else koşulları ile geriye dönen proses id'sini kontrol ederek şu an hangi prosesin içinde olduğumuzu anlayabilir ve ona göre işlemlerimizi yapabiliriz.

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

**``Not``**: Aşağıdaki örnekler için iki dosya oluşturdum. Birisi main.c diğeri ise another.c 
main.c içinde exec fonksiyonları kullanarak another.c programını çağıracağız.

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

main.c ve another.c derlenip main programı çalıştırıldığında çıktı şu şekilde olacaktır.

~~~
./another
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

main.c dosyası derlenip çalıştırıldığında Linux sistemlerinde dahili olarak bulunan ls programı -l parametresiyle çalıştırılacaktır ve çıktı şu şekilde olacaktır.

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

main.c derlenip çalıştırıldığında çıktı şu şekilde olacaktır.

~~~
total 36
-rwxrwxr-x 1 onur onur 8645 Oct 24 11:53 another
-rw-rw-r-- 1 onur onur  203 Oct 24 11:48 another.c
-rwxrwxr-x 1 onur onur 8602 Oct 24 11:53 main
-rw-rw-r-- 1 onur onur  255 Oct 24 11:50 main.c
-rw-rw-r-- 1 onur onur   61 Oct 23 14:45 makefile

~~~

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

main.c ve another.c derlenip main programı çalıştırıldığında çıktı şu şekilde olacaktır.

~~~
name=Mustafa Demir
~~~

## execle kullanımı

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

main.c ve another.c derlenip main programı çalıştırıldığında çıktı şu şekilde olacaktır.

~~~
name=Alican Akkuş
~~~

## Wait






























