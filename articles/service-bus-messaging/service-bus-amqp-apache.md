<properties 
    pageTitle="Installation von Apache Qpid Proton-C auf Linux VM | Microsoft Azure"
    description="Erstellen einer CentOS Linux VM Azure virtuellen Maschinen und zum Erstellen und installieren Sie die Apache Qpid Proton-C-Bibliothek."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/29/2016"
    ms.author="sethm" />

# <a name="install-apache-qpid-proton-c-on-an-azure-linux-vm"></a>Installieren Sie Apache Qpid Proton-C auf Azure Linux VM

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Dieser Abschnitt zeigt, wie ein CentOS Linux VM Azure virtuelle Maschinen erstellen und herunterladen, erstellen und Apache Qpid Proton-C-Bibliothek mit sprachanbindungen Python und PHP installieren. Nach Abschluss dieser Schritte werden Sie in diesem Handbuch enthaltenen Beispiele für Python und PHP ausführen.

Der erste Schritt erfolgt mithilfe der [Azure-Verwaltungsportal][]. Der folgende Screenshot zeigt die Erstellung einer CentOS VM mit dem Namen "Scott Centos":

![Proton auf Azure Linux VM][0]

Nach der Bereitstellung wird das Portal Folgendes angezeigt:

![Proton auf Azure Linux VM][1]

Um auf dem Computer anmelden, müssen Sie der Endpunktport für SSH kennen. Dieser Wert erhalten das [klassische Azure-Portal][] Sie durch den neu erstellten virtuellen Computer auswählen und auf **aus** . Der folgende Screenshot zeigt, dass der öffentliche SSH-Port für diesen Computer 57146.

![Proton auf Azure Linux VM][2]

Sie können jetzt SSH mit dem Computer verbinden. Dieses Beispiel verwendet das Tool PuTTY wie in der folgenden Abbildung:

![Proton auf Azure Linux VM][3]

Dieses Beispiel verwendet Proton Client-Bibliotheken von Apache für apps Python und PHP. Diese Bibliotheken sind von [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html)downloaden. Die Readme-Datei im Distributionspaket erläutert die Schritte zu Abhängigkeiten Proton erstellen. Hier ist eine Zusammenfassung der Schritte:

1.  Bearbeiten die Konfigurationsdatei Yum (/ etc/yum.conf) und der Ausschluss Updates Kernel-Header (\# ausschließen Kernel =\*). Dies ist den Compiler Gcc installieren.

2.  Installieren der Pakete:

    ```
    # required dependencies 
    yum install gcc cmake libuuid-devel
    
    # dependencies needed for ssl support
    yum install openssl-devel
    
    # dependencies needed for bindings
    yum install swig python-devel ruby-devel php-devel java-1.6.0-openjdk
    
    # dependencies needed for python docs
    yum install epydoc
    ```

1.  Proton-Bibliothek herunterladen:

    ```
    [azureuser@this-user ~]$ wget http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    --2016-04-17 14:45:03--  http://apache.panu.it/qpid/proton/0.9/qpid-proton-0.9.tar.gz
    Resolving apache.panu.it (apache.panu.it)... 81.208.22.71
    Connecting to apache.panu.it (apache.panu.it)|81.208.22.71|:80... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 868226 (848K) [application/x-gzip]
    Saving to: ‘qpid-proton-0.9.tar.gz’
    
    qpid-proton-0.9.tar.gz                               
    
    100%[====================================================================================================================>] 847.88K   102KB/s    in 8.4s    
    
    2016-04-17 14:45:12 (101 KB/s) - ‘qpid-proton-0.9.tar.gz’ saved [868226/868226]
    ```

1.  Extrahieren Sie Proton-Code aus dem Archiv:

    ```
    tar xvfz qpid-proton-0.9.tar.gz
    ```

1.  Erstellen und den Code, indem die folgenden Schritte in der Readme-Datei zu installieren:

    ```
    From the directory where you found this README file:    
    
    mkdir build cd build
            
    # Set the install prefix. You may need to adjust depending on your      
    # system.       
    cmake -DCMAKE\_INSTALL\_PREFIX=/usr ..
            
    # Omit the docs target if you do not wish to build or install       
    # documentation.        
    make all docs
            
    # Note that this step will require root privileges.     
    make install
    ```

Nach diesen Schritten ist Proton auf dem Computer installiert und betriebsbereit.

## <a name="next-steps"></a>Nächste Schritte

Möchten Sie mehr erfahren? Finden Sie auf folgenden Link:

- [Service Bus AMQP Übersicht][]

[Service Bus AMQP Übersicht]: service-bus-amqp-overview.md
[0]: ./media/service-bus-amqp-apache/amqp-apache-1.png
[1]: ./media/service-bus-amqp-apache/amqp-apache-2.png
[2]: ./media/service-bus-amqp-apache/amqp-apache-3.png
[3]: ./media/service-bus-amqp-apache/amqp-apache-4.png

[Azure-Verwaltungsportal]: http://manage.windowsazure.com


