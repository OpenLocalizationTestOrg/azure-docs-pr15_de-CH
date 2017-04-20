<properties 
    pageTitle="Protokollierung für maschinelles lernen Webdienste | Microsoft Azure" 
    description="Aktivieren der Protokollierung für maschinelles lernen WebServices erfahren. Protokollierung bietet weitere Informationen zur Problembehandlung bei den APIs." 
    services="machine-learning" 
    documentationCenter="" 
    authors="raymondlaghaeian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data" 
    ms.date="10/05/2016"
    ms.author="raymondl;garye"/>

# <a name="enable-logging-for-machine-learning-web-services"></a>Aktivieren der Protokollierung für maschinelles lernen Webdienste  

Dieses Dokument enthält Informationen, auf die Protokollfunktion klassische Webdienste. Aktivieren der Protokollierung in Webdiensten enthält zusätzliche Informationen über nur eine Nummer und eine Meldung, die die Aufrufe von APIs lernen Computer beheben helfen.  

Die Protokollierung in Webdiensten im klassischen Azure-Portal:   

1.  [Klassische Azure-Portal](https://manage.windowsazure.com/) anmelden
2.  Klicken Sie in der Spalte links Funktionen auf **COMPUTERLERNEN**.
3.  Klicken Sie auf den Arbeitsbereich dann **Webdienste**.
4.  Klicken Sie auf den Namen des Webdienstes in Liste der Dienste.
5.  Klicken Sie in der Liste Endpunkte Endpunkt.
6.  Klicken Sie auf **Konfigurieren**.
7.  **ABLAUFVERFOLGUNGSEBENE Diagnose** auf *Fehler* oder *Alle*festgelegt, dann klicken Sie auf **Speichern**.

Die Protokollierung im Azure Machine Learning Web-Portal.

1. Das [Azure Machine Learning Web](https://services.azureml.net) Portal anmelden.
2. Klicken Sie auf klassische Webdienste.
3.  Klicken Sie auf den Namen des Webdienstes in Liste der Dienste.
4.  Klicken Sie in der Liste Endpunkte Endpunkt.
5.  Klicken Sie auf **Konfigurieren**.
6.  **Protokollierung** auf *Fehler* oder *Alle*festgelegt, dann klicken Sie auf **Speichern**.

## <a name="the-effects-of-enabling-logging"></a>Die Effekte der Protokollierung aktivieren

Bei aktivierter Protokollierung verbunden, Diagnose und Fehler vom ausgewählten Endpunkt Azure Storage-Konto angemeldet sind, mit dem Workspace des Benutzers. Sie sehen diese Speicherkonto im klassischen Azure-Portal Dashboardansicht (Unterer Abschnitt kurzer Blick) mit ihren Arbeitsbereich.  

Die Protokolle können mit ' ein Speicherkonto Azure erkunden ' mehrere Werkzeuge angezeigt werden. Am einfachsten kann zu navigieren Sie einfach zu Speicherkonto in Azure-Verwaltungsportal **Container**sein. Dann wird einen Container namens **ml-Diagnose**angezeigt werden. Dieser Container enthält alle Diagnoseinformationen für alle Webdienstendpunkte für alle Arbeitsbereiche, die diesem Storage-Konto zugeordnet. 
 
## <a name="log-blob-detail-information"></a>BLOB-detaillierte Informationen

Jedes Blob im Container enthält Diagnoseinformationen für genau eine der folgenden:

-   Ausführung der Batch Execution-Methode  
-   Ausführung der Anforderung / Antwort-Methode  
-   Initialisierung der Anforderung / Antwort-container
  
Der Name jedes BLOB hat ein Präfix der folgenden Form: 

{Arbeitsbereich Id}-{Webdienst Id}-{Endpunkt Id} / {Protokolltyp}  

Protokolltyp der folgenden Werte ist:  

- Stapelverarbeitung  
- Bewertung-Anfragen  
- Bewertung-Initialisierung  