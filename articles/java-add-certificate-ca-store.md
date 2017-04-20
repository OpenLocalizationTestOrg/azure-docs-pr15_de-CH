<properties 
    pageTitle="Hinzufügen eines Zertifikats zur Java Zertifizierungsstellenspeicher | Microsoft Azure" 
    description="Erfahren Sie, wie der Zertifikatsspeicher Java CA (Cacerts) für Twilio Service oder Azure Service Bus ein Zertifikat der Zertifizierungsstelle (CA) hinzu." 
    services="" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="adding-a-certificate-to-the-java-ca-certificates-store"></a>Hinzufügen eines Zertifikats zum Speicher Java Zertifizierungsstelle Zertifikate
Die folgenden Schritte zeigen, wie Java CA-Zertifikatspeicher (Cacerts) ein Zertifikat der Zertifizierungsstelle (CA) hinzu. Beispiel ist für das Zertifizierungsstellenzertifikat Twilio Service erforderlich. Informationen weiter unten beschreibt das Installieren des Zertifikats für Azure Service Bus. 

Können Sie die Zertifizierungsstelle vor Ihrem JDK komprimieren und den Azure **Approot** Projektordner hinzufügen Hinzufügen Keytool oder ausführen konnte eine Azure Startaufgabe, die Keytool verwendet, um das Zertifikat hinzuzufügen. Es wird angenommen, dass ein Zertifizierungsstellenzertifikat vor JDK wird ZIP hinzufügen. Ein bestimmtes CA-Zertifikat im Beispiel verwendet auch die Schritte ein anderes CA-Zertifikat und-Speicher Cacerts importieren ist vergleichbar.

## <a name="to-add-a-certificate-to-the-cacerts-store"></a>Hinzufügen ein Zertifikats zum Speicher cacerts

1. Befehlszeile, die die JDK **Jdk\jre\lib\security** Ordner festgelegt ist, führen Sie Folgendes zu sehen, welche Zertifikate installiert sind:

    `keytool -list -keystore cacerts`

    Sie werden aufgefordert, das Kennwort speichern. Das Standardkennwort lautet **Ändern**. (Wenn Sie das Kennwort ändern möchten, finden Sie unter der Keytool-Dokumentation unter <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.) In diesem Beispiel wird vorausgesetzt, dass das Zertifikat mit dem MD5-Fingerabdruck 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 nicht aufgeführt ist, und möchten sie importieren (dieses Zertifikat ist die Twilio-API-Dienst erforderlich).
2. Beziehen Sie das Zertifikat aus der Liste der [Stammzertifikate GeoTrust](http://www.geotrust.com/resources/root-certificates/)aufgelisteten Zertifikate. Maustaste auf den Link für das Zertifikat mit der Seriennummer 35:DE:F4:CF und **Jdk\jre\lib\security** -Ordner speichern. Für dieses Beispiel in einer Datei gespeicherten **Equifax\_Secure\_Zertifikat\_Authority.cer**.
3. Importieren Sie das Zertifikat über den folgenden Befehl ein:

    `keytool -keystore cacerts -importcert -alias equifaxsecureca -file Equifax_Secure_Certificate_Authority.cer`

    Wenn aufgefordert, diesem Zertifikat zu vertrauen hat das Zertifikat MD5 Fingerabdruck 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 reagieren, indem Sie **y**eingeben.
4. Führen Sie den folgenden Befehl, um sicherzustellen, dass das CA-Zertifikat erfolgreich importiert wurde:

    `keytool -list -keystore cacerts`

5. ZIP-JDK und den Azure **Approot** Projektordner hinzugefügt.

Informationen über Keytool finden Sie unter <http://docs.oracle.com/javase/7/docs/technotes/tools/windows/keytool.html>.

## <a name="azure-root-certificates"></a>Azure Stammzertifikate

Anwendung, mit dem Azure-Dienste (z. B. Azure Service Bus) müssen Baltimore CyberTrust Root-Zertifikat vertrauen. (15. April 2013 begann Azure Migrieren von globalen GTE CyberTrust-Stamm Baltimore CyberTrust Root. Diese Migration dauerte mehrere Monate.)

Zertifikat möglicherweise bereits in Ihrem Shop Cacerts installiert Baltimore denken führen die **Keytool-Liste** Befehl nachsehen, ob er bereits vorhanden ist.

Wenn Sie Baltimore CyberTrust Root benötigen, hat Seriennummer 02:00:00:b9 und SHA1 Fingerabdruck D4:de:20:d0:5e:66:fc:53:fe:1a:50:88:2 c: 78:db:28:52:ca:e4:74. Sie können heruntergeladen, <https://cacert.omniroot.com/bc2025.crt>, in eine lokale Datei mit der Erweiterung **.cer**gespeichert und dann importiert **Keytool** verwenden (siehe oben).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Stammzertifikaten von Azure finden Sie unter [Azure Root Zertifikat Migration](http://blogs.msdn.com/b/windowsazure/archive/2013/03/15/windows-azure-root-certificate-migration.aspx).

Weitere Informationen zu Java finden Sie unter [Java Developer Center](/develop/java/).
