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

Aynı işi yapan fakat aralarında çok küçük farklılıklar bulunan 6 adet exec fonksiyonu vardır.

* int execl(const char *path, const char *arg0, ... , (char *) 0);

* int execv(const char *path, char *const argv[]);

* int execle(const char *path, const char *arg0, ... , (char *) 0, char *const envp[]);

* int execve(const char *path, char *const argv[], char *const envp[]);

* int execlp(const char * file, const char *arg0, ... , (char *) 0);

* int execvp(const char *file, char *const argv[]);

Exec fonksiyon isimlerinin sonundaki harflere açıklık getirelim.
**``l``** harfi çalıştıracağımız programa gönderilecek parametreleri liste şeklinde alır. 






































