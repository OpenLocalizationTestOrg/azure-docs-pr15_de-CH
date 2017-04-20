<properties
    pageTitle="Einrichten von Apache Tomcat auf Linux VM | Microsoft Azure"
    description="Erlernen von Apache Tomcat7 unter Linux ein Azure Virtual Machine (VM) verwenden."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="how-to-set-up-tomcat7-on-a-linux-virtual-machine-with-microsoft-azure"></a>Tomcat7 auf einer virtuellen Linux-Maschine mit Microsoft Azure einrichten

Apache Tomcat (oder einfach Tomcat, früher auch Jakarta Tomcat) ist ein open Source-Webserver und Servlet-Container, die von der Apache Software Foundation (ASF) entwickelt. Tomcat implementiert Java Servlet und Spezifikationen JavaServer Pages (JSP) von Sun Microsystems und ermöglicht eine reine Java HTTP-Web serverumgebung zum Ausführen von Java-Code. In der einfachsten Konfiguration wird Tomcat Betriebssystem. Dieser Prozess wird eine Java Virtual Machine (JVM) ausgeführt. Jede HTTP-Anforderung von einem Browser an Tomcat wird als separater Thread im Prozess Tomcat verarbeitet.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


In diesem Handbuch tomcat7 auf einem Linux-Abbild installiert und in Microsoft Azure bereitstellen.  

Lernen Sie Folgendes:  

-   Zum Erstellen eines virtuellen Computers in Azure.
-   Der virtuelle Computer für tomcat7 vorbereiten
-   Tomcat7 installieren

Davon ist, dass der Reader bereits Azure-Abonnement.  Wenn nicht für eine kostenlose Testversion an [http://azure.microsoft.com](https://azure.microsoft.com/)anmelden können. Wenn Sie ein MSDN-Abonnement verfügen, finden Sie unter [Microsoft Azure Sonderpreise: MSDN MPN und Bizspark Vorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). Weitere Informationen zu Azure finden Sie unter [Neuigkeiten von Azure?](https://azure.microsoft.com/overview/what-is-azure/).

In diesem Thema wird vorausgesetzt, dass Sie grundlegende Kenntnisse Tomcat und Linux.  

##<a name="phase-1-create-an-image"></a>Phase 1: Erstellen eines Abbilds
In dieser Phase erstellen Sie einen virtuellen Computer mit einem Linux-Image in Azure.  

###<a name="step-1-generate-an-ssh-authentication-key"></a>Schritt 1: Erstellen Sie einen SSH Authentifizierungsschlüssel
SSH ist ein wichtiges Tool für Systemadministratoren. Konfigurieren von Sicherheit basierend auf einem Kennwort Menschen bestimmt ist jedoch nicht empfohlen. Böswillige Benutzer können in Ihrem System einen Benutzernamen und ein unsicheres Kennwort aufteilen.

Die gute Nachricht ist, dass es zu RAS offen haben keine Kennwörter. Authentifizierung mit asymmetrische Kryptografie besteht. Privaten Schlüssel des Benutzers wird die Authentifizierung gewährt. Benutzerkonto um Kennwortauthentifizierung vollständig unterbinden können auch gesperrt werden.

Ein weiterer Vorteil dieser Methode ist, dass Sie sich auf verschiedenen Servern unterschiedliche Kennwörter nicht benötigen. Sie können mit dem persönlichen privaten Schlüssel auf allen Servern verhindert, dass mehrere Kennwörter merken authentifizieren.

Kann auch mit einem Kennwort mit dieser Methode anmelden.

Gehen Sie SSH Authentifizierungsschlüssel generieren.

1.  Herunterladen und Installieren von Puttygen von folgender Adresse: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2.  Führen Sie PUTTYGEN. EXE.
3.  Klicken Sie auf **generieren** , um die Schlüssel zu generieren. Dabei können Sie die Maus über den leeren Bereich im Fenster Zufälligkeit erhöhen.  
![][1]
4.  Nachdem der Prozess generieren zeigen Puttygen.exe generierten Schlüssel. Zum Beispiel:  
![][2]
5.  Auswählen und kopieren Sie den öffentlichen Schlüssel im **Schlüssel** in einer Datei namens publicKey.pem gespeichert. Klicken Sie nicht auf **öffentliche Schlüssel speichern**ist Dateiformat der gespeicherten öffentlichen Schlüssel mit dem öffentlichen Schlüssel, wir möchten.
6.  Klicken Sie auf **privaten Schlüssel speichern** und speichern in einer Datei namens privateKey.ppk.

###<a name="step-2-create-the-image-in-the-azure-portal"></a>Schritt 2: Erstellen Sie das Bild im Azure-Portal.
Klicken Sie im [Azure-Portal](https://portal.azure.com/) **neu** in der Taskleiste ein Bild auswählen Linux Bild basierend auf Ihre Bedürfnisse erstellen. Im folgende Beispiel wird Bild Ubuntu 14.04 verwendet.
![][3]

**Hostnamen** angeben den Namen für die URL ein, zu Internet-Clients auf diesem virtuellen Computer. Den letzten Teil der DNS-Name beispielsweise Tomcatdemo definieren und Azure wird die URL als tomcatdemo.cloudapp.net erstellen.  

**SSH-Authentifizierungsschlüssel**kopieren Sie den Schlüsselwert aus der **publicKey.pem** -Datei mit dem öffentlichen Schlüssel von Puttygen generiert.  
![][4]

Konfigurieren Sie andere Einstellungen und dann auf erstellen.  

##<a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>Phase 2: Vorbereiten der virtuellen Computer für Tomcat7
In dieser Phase Sie einen Endpunkt für den Tomcat-Datenverkehr konfigurieren und Verbinden mit den neuen virtuellen Computer.
###<a name="step-1-open-the-http-port-to-allow-web-access"></a>Schritt 1: Öffnen des HTTP-Ports um Webzugriff zulassen
Endpunkte in Azure bestehen aus einem Protokoll (TCP oder UDP) mit öffentlichen und privaten Port. Private Port ist der Port, den der Dienst auf dem virtuellen Computer überwacht. Öffentliche Port ist der Port, den Azure-Clouddienst extern für eingehende Internet-basierten Datenverkehr abhört.  

TCP-Anschluss 8080 ist die standardmäßige Anschlussnummer für die Tomcat überwacht. Öffnen diesen Anschluss mit Azure Endpunkt ermöglicht Sie und andere Clients Internetzugriff tomcat Seiten.  

1.  Azure-Portal klicken Sie auf **Durchsuchen,** -> **virtuellen Computer**, und klicken Sie auf den erstellten virtuellen Computer.  
![][5]
2.  Um einen Endpunkt zum virtuellen Computer hinzufügen, klicken Sie auf Feld **Endpunkte** .
![][6]
3.  Klicken Sie auf **Hinzufügen**.  
    1.  Für den **Endpunkt**Endpunkt geben Sie einen Namen für den Endpunkt, und geben Sie in **Öffentlichen Port**80.  

        Wenn Sie auf 80 festlegen, müssen Sie die Portnummer im URL enthalten, die Sie Tomcat zugreifen können. Beispielsweise http://tomcatdemo.cloudapp.net.    

        Beim Festlegen auf einen anderen Wert, wie 81, müssen Sie die Anschlussnummer der URL auf Tomcat hinzufügen. Z. B. Http://tomcatdemo.cloudapp.net:81 /.
    2.  Geben Sie in privater Port 8080. Tomcat hört standardmäßig TCP-Port 8080. Geändert standardmäßig Port Tomcat überwachen, aktualisieren Sie Private Port, den Tomcat identisch Port abhören.  
    ![][7]

4.  Klicken Sie auf **OK** , um den Endpunkt mit dem virtuellen Computer hinzuzufügen.



###<a name="step-2-connect-to-the-image-you-created"></a>Schritt 2: Verbinden Sie auf das Bild, das Sie erstellt haben.
Sie können SSH-Tool für die Verbindung mit dem virtuellen Computer. In diesem Beispiel verwenden wir kitten.  

Azure-Portal, erhalten Sie den DNS-Namen des virtuellen Computers. **Klicken Sie** -> **virtuellen Computer** den Namen des virtuellen Computers -> -> **Eigenschaften**, und suchen Sie im Feld **Domänenname** die Kachel **Eigenschaften** .  

Erhalten Sie die Port-Nummer für SSH-Verbindung von **SSH** -Feld. Hier ist ein Beispiel.  
![][8]

Kitten [hier](http://www.putty.org/) herunterladen  

Klicken Sie nach dem Download auf die ausführbare Datei KITTEN. EXE. Konfigurieren der Standardoptionen mit dem Hostnamen und der Anschlussnummer aus den Eigenschaften des virtuellen Computers abgerufen. Hier ist ein Beispiel:  
![][9]

Klicken Sie im linken Bereich auf **Verbindung** -> **SSH** -> **Authentifizierung** und klicken an den Speicherort der Datei **privateKey.ppk** enthält den privaten Schlüssel **Suchen** von Puttygen in Phase 1 generiert: Erstellen eines Abbilds. Hier ist ein Beispiel:  
![][10]

Klicken Sie auf **Öffnen**. Sie können durch ein Meldungsfeld benachrichtigt. Konfiguriert den DNS-Namen und port-Nummer korrekt klicken Sie auf **Ja**.
![][11]  


Sie sollten Folgendes angezeigt:  
![][12]

Geben Sie den Benutzernamen angegeben, wenn Sie den virtuellen Computer in Phase 1 erstellt: Erstellen eines Abbilds. Folgendes wird angezeigt:  
![][13]





##<a name="phase-3-install-software"></a>Phase 3: Installieren der software
In dieser Phase installieren Sie das Java Runtime Environment, Tomcat und andere Tomcat.  

###<a name="java-runtime-environment"></a>Java Runtime-Umgebung
Tomcat ist in Java geschrieben. Es gibt zwei Arten von Java Development Kits (JDKs) (OpenJDK und Oracle JDK) und wählen Sie die gewünschte.  

>AZURE. Hinweis: Beide JDKs fast denselben Code für Klassen in Java API, jedoch der Code für den virtuellen Computer tatsächlich. OpenJDK tendenziell geht um Bibliotheken, geöffnete Bibliotheken verwenden, wohingegen Oracle geschlossenen verwenden. Aber Oracle JDK-Klassen und einige Fehler behoben und Oracle JDK ist stabiler als OpenJDK.

Die folgenden Befehle herunterladen unterschiedliche JDKs  

Jdk öffnen   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  

Oracle jdk  

-   JDK von Oracle-Website herunterladen:  

        wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  

-   So erstellen Sie ein Verzeichnis für die JDK-Dateien  

        sudo mkdir /usr/lib/jvm  

-   Die JDK-Dateien in das Verzeichnis/Usr/Lib/Jvm extrahieren:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  

-   Oracle JDK standardmäßig JVM festlegen:  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  
        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

####<a name="test"></a>Test:
Können Sie einen Befehl wie den folgenden um zu testen, ob die Java Runtime-Umgebung korrekt installiert ist:  

    java -version  

Wenn Sie Open Jdk installiert, erhalten Sie eine Meldung wie die folgende:![][14]

Wenn Sie Oracle Jdk installiert haben, erhalten Sie eine Meldung wie die folgende:![][15]

###<a name="tomcat7"></a>Tomcat7
Mithilfe des folgenden Befehls tomcat7 installieren:  

    sudo apt-get install tomcat7  

Wenn Sie tomcat7 nicht verwenden, verwenden Sie die entsprechende Variante dieses Befehls.  

####<a name="test"></a>Test:

Prüfen, ob tomcat7 installiert ist, wechseln Sie zu der Tomcat Server DNS-Namen (http://tomcatexample.cloudapp.net/ ist der URL wird in diesem Artikel). Siehe Seite wie den folgenden haben Sie installiert tomcat7 korrekt.
![][16]


###<a name="install-other-tomcat-components"></a>Andere Tomcat-Komponenten installieren
Es gibt andere optionale Tomcat auch installieren können.  

Befehl **Sudo apt-Cache suchen tomcat7** verfügbaren Komponenten angezeigt. Die folgenden Befehle sind Beispiele für einige nützlichen Komponenten installieren.  

    sudo apt-get install tomcat7-admin      #admin web applications
    sudo apt-get install tomcat7-user         #tools to create user instances  

##<a name="phase-4-configure-tomcat"></a>Phase 4: Konfigurieren Sie Tomcat
In dieser Phase verwalten Sie Tomcat.
###<a name="start-and-stop-tomcat7"></a>Starten und Beenden von tomcat7
Tomcat7 Server wird automatisch gestartet, wenn Sie es installieren. Sie können auch sie sich mit dem folgenden Befehl starten:   

    sudo /etc/init.d/tomcat7 start

Tomcat7 beenden:  

    sudo /etc/init.d/tomcat7 stop

Den Status des tomcat7 anzeigen:  

    sudo /etc/init.d/tomcat7 status

Tomcat-Dienste neu starten:  

    sudo /etc/init.d/tomcat7 restart

###<a name="tomcat-administration"></a>Tomcat-Verwaltung
Sie können der Tomcat Benutzerkonfigurationsdatei einrichten die Administratoranmeldeinformationen mit dem folgenden Befehl:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Hier ist ein Beispiel:  
![][17]  

>AZURE. Hinweis: Erstellen Sie ein sicheres Kennwort für den Benutzernamen Admin.  

Nach der Bearbeitung dieser Datei sollten Sie tomcat7 Services mit dem folgenden Befehl, um sicherzustellen, dass die Änderungen wirksam werden neu starten:  

    sudo /etc/init.d/tomcat7 restart  

Öffnen Sie Ihren Browser, und geben Sie den URL **http://<your tomcat server DNS name>/Manager/HTML-**. Das Beispiel in diesem Artikel wird die URL http://tomcatexample.cloudapp.net/manager/html.  

Nach dem anschließen, sollte ähnlich der folgenden angezeigt werden:  
![][18]

##<a name="common-issues"></a>Allgemeine Probleme

###<a name="cant-access-the-virtual-machine-with-tomcat-and-moodle-from-the-internet"></a>Die virtuelle Maschine mit Tomcat und Moodle kann nicht aus dem Internet zugegriffen werden.

-   **Symptom**  
Tomcat ausgeführt, aber nicht die Tomcat-Standardseite in Ihrem Browser angezeigt.
-   **Mögliche Stamm Fall**   
    1.  Tomcat-Listenanschluss entspricht nicht Private Port der virtuellen Maschine Endpunkt für Tomcat-Datenverkehr.  

        Überprüfen Sie Ihre öffentlichen Port und Private Port Endpunkt und sicherstellen Sie, dass Private Port ist identisch mit dem Tomcat Port abhören. Siehe Phase 1: Erstellen eines Bildes für eine Anleitung zum Konfigurieren von Endpunkten für den virtuellen Computer.  

        Ermitteln Sie den Tomcat-Listenanschluss öffnen Sie /etc/httpd/conf/httpd.conf (Red Hat-Version) oder /etc/tomcat7/server.xml (Debian Release). Standardmäßig ist der Tomcat hören Port 8080. Hier ist ein Beispiel:  

            <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"  URIEncoding="UTF-8"            redirectPort="8443" />  

        Bei einem virtuellen Computer wie Debian oder Ubuntu und Sie die standardmäßige Port Tomcat hören (zum Beispiel 8081 möchten) sollten Sie auch den Port für das Betriebssystem öffnen. Öffnen Sie zunächst das Profil:  

            sudo vi /etc/default/tomcat7  

        Kommentieren Sie die letzte Zeile und ändern Sie "no" auf "yes".  

            AUTHBIND=yes

    2.  Die Firewall hat den Port Abhören Tomcat deaktiviert.

        Wenn Sie Tomcat-Standardseite vom lokalen Host nur sehen, ist das Problem wahrscheinlich, Tomcat lauscht Port durch die Firewall blockiert wird. W3m-Tool können Sie die Webseite. Die folgenden Befehle w3m installieren und der Tomcat-Standardseite durchsuchen:  

            sudo yum  install w3m w3m-img
            w3m http://localhost:8080  

-   **Lösung**
    1. Wenn der Tomcat überwacht Port nicht wie Private Port des Endpunkts auf den Datenverkehr an den virtuellen Computer, müssen Private Port, den Tomcat identisch Port Abhören ändern.   

    2.  Wenn das Problem durch die Firewall/Iptables verursacht wird, fügen Sie folgende Zeilen zu /etc/sysconfig/iptables:  

            -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
            -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT  

        Beachten Sie, dass die zweite Zeile nur für Https-Datenverkehr erforderlich ist.  

        Sicherstellen Sie, dass diese Zeilen überschreitet, die Global Access wie folgt einschränken würde:  

            -A INPUT -j REJECT --reject-with icmp-host-prohibited  

        Um die Iptables zu laden, führen Sie den folgenden Befehl ein:  

            service iptables restart  

        Dies wurde auf CentOS 6.3 getestet.

###<a name="permission-denied-when-upload-you-project-files-to-varlibtomcat7webapps"></a>Zugriff verweigert beim Upload Sie Projektdateien, /var/lib/tomcat7/webapps /  

-   **Symptom**  
Wenn Sie Verbindung mit dem virtuellen Computer, und navigieren Sie zu /var/lib/tomcat7/webapps und Ihre Website veröffentlichen SFTP-Client (wie FileZilla) verwenden, erhalten Sie eine Fehlermeldung ähnlich der folgenden:  

        status: Listing directory /var/lib/tomcat7/webapps
        Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
        Error:  /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
        Error:  File transfer failed

-   **Mögliche Stamm Fall** Sie haben keine Zugriffsberechtigungen für den Ordner /var/lib/tomcat7/webapps.  
-   **Lösung**  
Sie müssen Berechtigungen aus dem Root-Konto erhalten. Sie können den Besitz des Ordners vom Stamm zum Benutzernamen ändern verwendet, wenn den Computer bereitstellen. Hier ist ein Beispiel mit dem Kontonamen Azureuser:  

        sudo chown azureuser -R /var/lib/tomcat7/webapps

    Verwenden Sie die Option -R, um Berechtigungen für alle Dateien in einem Verzeichnis zu anzuwenden.  

    Beachten Sie, dass dieser Befehl auch für Verzeichnisse. Die Option-R ändert die Berechtigungen für alle Dateien und Verzeichnisse innerhalb des Verzeichnisses. Hier ist ein Beispiel:  

        sudo chown -R username:group directory  

    Dieser Befehl ändert den Besitz (Benutzer und Gruppen) für alle Dateien und Verzeichnisse innerhalb und Verzeichnis selbst.  

    Der folgende Befehl ändert die Berechtigung des Verzeichnisses Ordner nur Dateien und Ordner im Verzeichnis allein lässt.  

        sudo chown username:group directory



[1]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png
