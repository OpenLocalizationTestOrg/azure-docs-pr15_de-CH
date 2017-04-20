<properties 
    pageTitle="Registrierungseinträge für die Proxydienste App Discovery Cloud | Microsoft Azure" 
    description="Ziel dieses Themas ist bieten die Schritte müssen Sie durchführen, um die erforderlichen Ports auf den Computern des Cloud App Discovery-Agents festgelegt." 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-registry-settings-for-proxy-services"></a>Cloud App Discovery-Registrierungseinträge für Proxy-Dienste

Standardmäßig ist der Cloud App Discovery-Agent konfiguriert nur die Ports 80 und 443. Wenn Sie Cloud App Discovery in einer Umgebung mit einem Proxy-Server, die mit einem benutzerdefinierten Port (weder 80 oder 443) installieren, müssen Sie die Agents verwenden diesen Port konfigurieren. Die Konfiguration basiert auf einen Registrierungsschlüssel.


Ziel dieses Themas ist bieten die Schritte müssen Sie durchführen, um die erforderlichen Ports auf den Computern des Cloud App Discovery-Agents festgelegt.



**Ändern Sie den Port mit der Cloud App Discovery-Agent verwendet die folgenden Schritte:**


1. Starten Sie den Registrierungseditor. <br> ![Ausführen](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy01.png)

2. Navigieren Sie zu, oder erstellen Sie den folgenden Registrierungsschlüssel: <br> **HKLM_LOCAL_MACHINE\Software\Microsoft\Cloud Anwendung Discovery\Endpoint** 

3. Erstellen Sie einen neuen **mehrteiligen** Zeichenfolgenwert **Ports**bezeichnet. ![Neu](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy02.png)

4. Öffnen Sie im Dialogfeld **Mehrteilige Zeichenfolge bearbeiten** Doppelklicken Sie auf den Wert des Ports.


5. Geben Sie im Textfeld Daten Wert die folgenden Werte, und fügen Sie alle benutzerdefinierten Ports, die von Ihrer Organisation verwendet werden: <br><br>
**80** <br>
**8080** <br>
**8118** <br>
**8888** <br>
**81** <br>
**12080** <br>
**6999** <br>
**30606** <br>
**31595** <br>
**4080** <br>
**443** <br>
**1110** <br><br>
![Mehrteilige Zeichenfolge bearbeiten](./media/active-directory-cloudappdiscovery-registry-settings-for-proxy-services/proxy03.png)

6. Klicken Sie auf **OK** , um das Dialogfeld **Mehrteilige Zeichenfolge bearbeiten** zu schließen.



**Zusätzliche Ressourcen**


* [Wie kann ich Cloud-apps, mit denen im Unternehmen entdecken](active-directory-cloudappdiscovery-whatis.md) 


