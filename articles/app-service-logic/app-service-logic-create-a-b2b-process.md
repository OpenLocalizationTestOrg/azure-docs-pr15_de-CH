<properties 
   pageTitle="Erstellen einen B2B-Prozess in Azure App Service | Microsoft Azure" 
   description="Übersicht über Business-to-Business-Prozess erstellen" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="04/20/2016"
   ms.author="rajram"/>

# <a name="creating-a-b2b-process"></a>Erstellen eines B2B-Prozess

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]


## <a name="business-scenario"></a>Business-Szenario 
Contoso und Northwind sind zwei. Contoso (Händler) sendet Aufträge an Northwind (Lieferanten) über eine Branche auf Transport wie AS2. Northwind speichert alle eingehenden Aufträge in die Cloud-Speicher. Die Aufträge sind XML-Nachrichten zwischen diesen beiden Partnern. Sobald die Nachricht in Northwinds-Cloud-Speicher gespeichert behandeln Northwind interne Prozesse auf die Reihenfolge von diesem Punkt.
 
Das Ziel dieser praktischen Einführung soll wie Northwind ein Geschäftsprozesses herstellen kann, über dem sie Nachrichten (Bestellungen in XML) vom Partner Contoso über AS2 und dann in die Cloud-Speicher beibehalten.


## <a name="capabilities-demonstrated"></a>Funktionen gezeigt 
In diesem Lernprogramm unterstützt die folgenden Funktionen zu präsentieren: 

- **Nachricht Transport**: der Einzelhändler und auf unterschiedlichen Plattformen kann aber sie können noch Kommunikation zwischen den beiden. In diesem Lernprogramm kommunizieren über AS2 (Anwendbarkeit Anweisung 2). AS2 ist auf die Übertragung von Daten zwischen Handelspartnern im Business-to-Business-Kommunikation.
- **Dauerhaftigkeit der Benutzerdaten**: Nachricht über AS2 eingegangen und Northwind vor der weiteren Verarbeitung beibehalten möchte. Sie können einen Connector verwenden, um Nachrichten in die Cloud-Speicher beizubehalten. In diesem Lernprogramm wird Azure-Blobs für Northwind als Cloud-Speicher genutzt werden.
- **Erstellen eines Geschäftsprozesses**: In einem Datenfluss mehrere API-apps können werden zusammengefügt um geschäftlichen Ergebnis zu erzielen, wie hier gezeigt.


## <a name="before-you-begin"></a>Bevor Sie beginnen
In diesem Lernprogramm wird davon ausgegangen, dass ein grundlegendes Verständnis von Azure App Services kennen und API-apps erstellen und setzen einen Fluss zusammen.


## <a name="steps-to-achieve-the-business-scenario"></a>Schritte zum Erreichen des Geschäftsszenarios
**Erstellen Sie und konfigurieren Sie die erforderlichen API-apps**

1. Erstellen Sie eine Instanz von **Azure Storage BLOB-Connector**. Die Anmeldeinformationen Azure Storage-Konto erforderlich. Stellen Sie sicher, dass er bereit ist, bevor Sie erstellen diese.
2. Erstellen Sie eine Instanz der **BizTalk Trading Partner-Management**. Hierfür wird eine leere SQL-Datenbank Funktion. Stellen Sie sicher, dass Sie vor dem Erstellen dieser bereit ist.
3. Erstellen Sie eine Instanz von **AS2-Connector**. Dies ist erforderlich, eine leere SQL-Datenbank Funktion. Stellen Sie sicher, dass Sie vor dem Erstellen dieser bereit ist. Darüber hinaus sollten Bestandteil AS2 verarbeitet Nachrichten archivieren können Sie Anmeldeinformationen eine Azure BLOB während seiner Erstellung angeben.
4. Konfigurieren des TPM (Trading Partner Management)-Dienstes, der erstellt wird:  
    1. Durchsuchen der Instanz der TPM-Dienst als Teil der Schritte.
    2. Erforderliche AS2-Identität verwenden unter *Komponenten* **Hinzufügen** neuer Partner namens **Contoso** und seine **Partner** -Option hinzufügen
    3. Verwenden Sie die Option **Partner** unter *Komponenten* **Hinzufügen** neuer Partner namens **Northwind** in seinem Profil erforderliche AS2-Identität hinzufügen
    4. Die Option **Abkommen** unter *Komponenten* zum **Hinzufügen** einer neuen AS2 zwischen Northwind und Contoso. Nordwind wird der gehosteten Partner und Contoso werden gastpartner. Gegebenenfalls signieren, Verschlüsselung, Komprimierung und Empfangsbestätigungen während dieser Vereinbarung Erstellung konfigurieren. Bei Zertifikaten verwendet werden müssen, können sie über die Option **Zertifikate** hochgeladen werden beim TPM-Service, der erstellt wird.


## <a name="create-a-flow--business-process"></a>Erstellen einer / Geschäftsprozess
1. Erstellen einer neuen in dem erste Schritt AS2 ist. Ziehen Sie den **AS2-Connector** und wählen Sie die Instanz bereits erstellt. Wählen Sie Trigger als Funktionen:  
    ![][1]  
2. Als Nächstes ziehen Sie **Azure Storage BLOB-Connector** und wählen Sie die Instanz bereits erstellt. Wählen Sie Aktion als die Funktionen und, **Blob hochladen** als die gewünschte Funktionalität. Führen Sie die entsprechenden.
3. Nun erstellen/Bereitstellen des Fluss.


## <a name="message-processing--troubleshooting"></a>Verarbeitung und Fehlerbehebung
1. Es ist Zeit, den Fluss testen wir bereitgestellt. Senden Sie XML im umschlossenen AS2 (nach oben erstellten AS2-Vereinbarung) Nachrichten die erstellte AS2Connector-Instanz angegeben AS2-Endpunkt. Möglicherweise müssen die Authentifizierung für den Endpunkt so konfigurieren, dass es öffentlich ist.
2. Ausführungsinformationen über die durch den Fluss suchen und dann in der Flow-Instanz ausgeführt wurde angegeben
3. AS2 Verarbeitung Informationen nach die Instanz AS2Connector beteiligt und Tracking Teil schrittweise folgen. Sie können die Filter an Informationen beschränken, die gewünscht wird.

![][2]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-b2b-process/Flow.png
[2]: ./media/app-service-logic-create-a-b2b-process/Tracking.png
 
