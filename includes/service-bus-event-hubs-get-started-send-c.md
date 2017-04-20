## <a name="send-messages-to-event-hubs"></a>Senden von Nachrichten an Event Hubs

In diesem Abschnitt schreiben wir eine C Anwendung Ereignisse mit der Ereignis-Hub zu senden. Proton AMQP-Bibliothek aus dem [Projekt Apache Qpid](http://qpid.apache.org/)werden. Dies entspricht mit Servicebuswarteschlangen und Themen AMQP c als Siehe [hier](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Weitere Informationen finden Sie unter [Qpid Proton-Dokumentation](http://qpid.apache.org/proton/index.html).

1. Klicken Sie auf **Installation Qpid Proton** [Qpid AMQP Messenger Seite](http://qpid.apache.org/components/messenger/index.html)und gehen Sie je nach Ihrer Umgebung. Wir übernehmen eine Linux-Umgebung; beispielsweise eine [Azure Linux VM](../articles/virtual-machines/virtual-machines-linux-quick-create-cli.md) mit Ubuntu 14.04.

2. Die Bibliothek Proton kompilieren Installationspakete:

    ```
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```

3. Der [Qpid Proton](http://qpid.apache.org/proton/index.html) Library herunter, und extrahieren Sie es, z.B.:

    ```
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```

4. Erstellen Sie einen Build-Verzeichnis, kompilieren und installieren:

    ```
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```

5. Erstellen Sie eine neue Datei namens **sender.c** mit dem folgenden Inhalt in Ihrem Arbeitsverzeichnis. Denken Sie daran, den Wert für die Event Hub und Namespace ersetzen (Letzteres ist normalerweise `{event hub name}-ns`). Sie müssen auch eine URL-codierte Version des Schlüssels zuvor erstellten **SendRule** ersetzen. URL-Kodierung können sie [hier](http://www.w3schools.com/tags/ref_urlencode.asp).

    ```
    #include "proton/message.h"
    #include "proton/messenger.h"

    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>

    #define check(messenger)                                                     \
      {                                                                          \
        if(pn_messenger_errno(messenger))                                        \
        {                                                                        \
          printf("check\n");                                                     \
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); \
        }                                                                        \
      }  

    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  

    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }

    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://SendRule:{Send Rule key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";

        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();

        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);

        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));

        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);

        pn_message_free(message);
    }

    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");

        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);

        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }

        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);

        return 0;
    }
    ```

6. Kompilieren Sie die Datei **Gcc**:

    ```
    gcc sender.c -o sender -lqpid-proton
    ```

> [AZURE.NOTE] In diesem Codebeispiel verwenden wir eine ausgehende Fenster 1 die Nachrichten so schnell wie möglich zu erzwingen. Im Allgemeinen sollten die Anwendung Batch Nachrichten den Durchsatz erhöhen. [Qpid AMQP Messenger](http://qpid.apache.org/components/messenger/index.html) finden weitere Informationen zur Verwendung die Qpid Proton-Bibliothek und anderen Umgebungen und Plattformen für die Bindung bereitgestellt werden (momentan Perl, PHP, Python und Ruby).
