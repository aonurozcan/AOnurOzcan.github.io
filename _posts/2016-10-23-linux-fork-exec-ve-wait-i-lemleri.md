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
      //Parent process
    } else if (processId == 0) {
      //Child process
    }
    
    return 0;
  }
{% endhighlight %}

