<properties 
    pageTitle="Ausstellername und Aussteller Schlüssel in BizTalk | Microsoft Azure" 
    description="Informationen Sie zum Ausstellername und Aussteller Schlüssel Service Bus oder Access Control (ACS) BizTalk-Dienste abrufen. MAK, WABS" 
    services="biztalk-services" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="biztalk-services" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="mandia"/>




# <a name="biztalk-services-issuer-name-and-issuer-key"></a>BizTalk-Dienste: Ausstellername und Aussteller Schlüssel

Azure BizTalk Services verwendet den Aussteller Service Bus und Aussteller Schlüssel und Access Control Ausstellername und Aussteller Schlüssel. Insbesondere:

Aufgabe | Welche Ausstellername und Aussteller Schlüssel
--- | ---
Bereitstellen der Anwendung aus Visual Studio | Access Control Ausstellername und Aussteller Schlüssel
Konfiguration von Azure BizTalk Services-Portal | Access Control Ausstellername und Aussteller Schlüssel
LOB-Relays erstellen mit der BizTalk-Adapterdienste in Visual Studio | Service Bus Ausstellername und Aussteller Schlüssel

In diesem Thema werden die Schritte Ausstellername und Aussteller Schlüssel abzurufen. 

## <a name="access-control-issuer-name-and-issuer-key"></a>Access Control Ausstellername und Aussteller Schlüssel
Access Control Ausstellername und Aussteller Schlüssel werden wie folgt verwendet:

- Die Azure BizTalk-Dienst-Anwendung in Visual Studio erstellt: zur erfolgreichen Bereitstellung die Anwendung BizTalk Service in Visual Studio in Azure Access Control Ausstellername und Aussteller Schlüssel eingeben. 
- Das Azure BizTalk-Portal: BizTalk Service erstellen und öffnen Sie das BizTalk-Portal werden die Access Control Ausstellername und Aussteller Schlüssel automatisch für die Bereitstellung mit den gleichen Werten Zugriffskontrolle registriert.

### <a name="to-copy-and-paste-the-access-control-issuer-name-and-issuer-key"></a>Kopieren und Einfügen der Access Control Ausstellername und Aussteller Schlüssel

1. Melden Sie sich bei [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/p/?LinkID=213885)an.
2. Wählen Sie im linken Navigationsbereich **Der BizTalk-Dienste**.
3. Wählen Sie den BizTalk-Dienst. 
4. Wählen Sie **Verbindungsinformationen** in der Taskleiste. Access Control Namespace Standard Aussteller (Ausstellername) und Standard (Taste Aussteller) aufgelisteten kopiert und eingefügt.  

Zusammenfassung:  
Ausstellername = Standard Aussteller  
Aussteller Key = Standardschlüssel


Sie können auch **Öffnen ACS-Verwaltungsportal** zu Access Control-Werte:

1. Melden Sie sich bei [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/p/?LinkID=213885)an.
2. Wählen Sie im linken Navigationsbereich **Der BizTalk-Dienste**.
3. Wählen Sie den BizTalk-Dienst.
4. Wählen Sie die Schaltfläche Verbindung und **Öffnen ACS-Verwaltungsportal**.
5. Wählen Sie im Portal unter **Einstellungen** **Dienstidentitäten**. Die Dienstidentität, die Access Control Ausstellername Wert angezeigt. Klicken Sie die Dienstidentität zu Ihrem Aussteller Schlüsselwert ist das Kennwort. Ihre Werte können kopiert werden.<br/><br/>
**Dienstidentitäten**sehen Sie z. B. "Owner". "Besitzer" ist der Access Control Ausstellername. Beim Klicken auf den Link "Besitzer" das **Kennwort**angezeigt. Beim Klicken auf den Link "Kennwort" sehen Sie den Wert. Dieser Wert Kennwort ist Ihr Steuerelement Aussteller Zugriffstaste.  

Zusammenfassung:  
Ausstellername = Name der Dienstidentität  
Aussteller Schlüssel = Wert

Klicken Sie im linken Navigationsbereich können Sie auch **Active Directory** Access Control Werte abrufen auswählen. 

> [AZURE.IMPORTANT]Beim Erstellen einer Access Control Namespace wird mithilfe von **Active Directory**, einer Dienstidentität **nicht** automatisch erstellt. Bei der Bereitstellung einer BizTalk Service eine Access Control Namespace Dienstidentität mit dem Namen "Besitzer" (Ausstellername) Kennwort (Aussteller Schlüssel) und symmetrischen Schlüssel automatisch erstellt.<br /> 
[Wie: mit ACS-Verwaltungsdienst Dienstidentitäten konfigurieren](http://go.microsoft.com/fwlink/p/?LinkID=303942) enthält weitere Informationen zu Access Control Service Identitäten.


## <a name="service-bus-issuer-name-and-issuer-key"></a>Service Bus Ausstellername und Aussteller Schlüssel
Service Bus Ausstellername und Aussteller Schlüssel werden von BizTalk-Adapter verwendet. In Ihrem Projekt BizTalk-Dienste in Visual Studio wird der BizTalk-Adapterdienste auf einem lokalen Line-of-Business (LOB) verwenden. Anschließen, LOB-Relay erstellen und Ihr LOB-System eingeben. Dabei geben Sie auch den Service Bus Ausstellername und Aussteller Schlüssel.

### <a name="to-retrieve-the-service-bus-issuer-name-and-issuer-key"></a>Service Bus Ausstellername und Aussteller Schlüssel abrufen

1. Melden Sie sich bei [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/p/?LinkID=213885)an.
2. Wählen Sie im linken Navigationsbereich **Service Bus**.
3. Wählen Sie den Namespace. Wählen Sie in der Taskleiste **Verbindungsinformationen**. Zeigt **Standardmäßig Aussteller** (Ausstellername) und **Standardschlüssel** (Aussteller Schlüssel). Ihre Werte können kopiert werden.  

Zusammenfassung:  
Ausstellername = Standard Aussteller  
Aussteller Key = Standardschlüssel

## <a name="next"></a>Weiter
Weitere Themen Azure BizTalk-Dienste:

-  [Azure BizTalk-Dienste SDK installieren](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
-  [Lernprogramme: Azure BizTalk-Dienste](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
-  [Wie kann ich starten mithilfe von Azure BizTalk-Dienste SDK](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
-  [Azure BizTalk-Dienste](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>


## <a name="see-also"></a>Siehe auch
-  [Gewusst wie: Verwenden Sie ACS-Verwaltungsdienst Dienstidentitäten konfigurieren](http://go.microsoft.com/fwlink/p/?LinkID=303942)<br/>
- [BizTalk-Dienste: Entwickler, Basic, Standard und Premium Editions Diagramm](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
- [BizTalk-Dienste: Bereitstellung mithilfe von Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
- [BizTalk-Dienste: Bereitstellung Statusdiagramm](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
- [Der BizTalk-Dienste: Registerkarten Dashboard, Monitor und skalieren](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
- [Der BizTalk-Dienste: Sicherung und Wiederherstellung](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
- [BizTalk-Dienste: Drosselung](http://go.microsoft.com/fwlink/p/?LinkID=302282)<br/>
 
