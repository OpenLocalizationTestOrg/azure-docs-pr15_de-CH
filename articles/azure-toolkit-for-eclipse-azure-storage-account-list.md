<properties
    pageTitle="Liste Azure-Speicher"
    description="Verwalten der Storage-Konto mit dem Azure-Toolkit für Eclipse"
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->

# <a name="azure-storage-account-list"></a>Liste Azure-Speicher #

Azure-Speicherkonten aktivieren Downloadadressen für JDK, Anwendungsserver und beliebige Komponenten sowie zum Speichern von Status bei Verwendung zwischenspeichern. Eclipse verwaltet eine Liste der bekannten Speicherkonten Projekte im Arbeitsbereich Eclipse zur Verfügung stehen. Zur Verwaltung dieser Liste in Eclipse **Speicherkonten** Dialogfeld Öffnen klicken Sie auf **Fenster**, klicken Sie auf **Voreinstellungen** **Azure**erweitern und **Speicherkonten**klicken.

Im folgenden wird das Dialogfeld **Speicherkonten** .

![][ic719496]

Dieses Dialogfeld kann auch über einen Link **Konten** Dialogfelder geöffnet werden, die Speicherkonten wie den folgenden verwenden:

* Die **JDK** -Registerkarte im Dialogfeld **Serverkonfiguration** .
* Die Registerkarte **Server** der **Server-Konfiguration** .
* Das Dialogfeld **Komponente hinzufügen** .
* Eigenschaftendialogfeld **Zwischenspeichern** .

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a>So importieren Sie eine Datei veröffentlichen verwenden Speicherkonten ##

1. Klicken Sie im Dialogfeld **Speicherkonten** **aus veröffentlichen Einstellungsdatei importieren**.
2. (Überspringen Sie diesen Schritt, wenn Sie bereits eine Datei veröffentlichen auf Ihrem Computer gespeichert haben.) Klicken Sie im Dialogfeld **Import Abonnementinformationen** auf **Veröffentlichen-Einstellungsdatei herunterladen**. Wenn Sie Ihre Azure-Konto noch nicht angemeldet sind, werden Sie aufgefordert, anmelden. Anschließend werden Sie aufgefordert zum Speichern einer Azure Datei veröffentlichen. (Die resultierenden Anweisungen Anmeldeseiten - ignorieren sie Azure-Portal bereitgestellt und sollen für Visual Studio-Benutzer) Auf Ihrem Computer speichern.
3. Klicken Sie im Dialogfeld **Import Abonnementinformationen** klicken Sie auf die Schaltfläche **Durchsuchen** , wählen Sie die Datei veröffentlichen, die lokal gespeicherte und klicken Sie auf **Öffnen**.
4. Klicken Sie auf **OK** , um das Dialogfeld **Import Abonnementinformationen** zu schließen.

## <a name="to-create-a-new-storage-account"></a>Neue Speicher-Konto erstellen ##

1. Klicken Sie im Dialogfeld **Speicherkonten** **Hinzufügen**.
2. Klicken Sie im Dialogfeld **Speicherkonto hinzufügen** auf **neu**.
3. Geben Sie im Dialogfeld **Neue Speicherkonto** Werte für die folgenden:
    * Speicher-Kontoname.
    * Speicherort des Speicherkontos.
    * Beschreibung des Speicherkontos.
    * Das Abonnement das Speicherkonto gehört.
4. Klicken Sie auf **OK** , um das Dialogfeld **Neue Speicher-Konto** zu schließen.

Es kann mehrere Minuten für das Speicherkonto erstellt werden. Nachdem es erstellt wurde, klicken Sie auf **OK** , um das Dialogfeld **Speicherkonto hinzufügen** zu schließen und das neue Speicherkonto zur Liste der verfügbaren Speicherkonten hinzugefügt.

## <a name="to-add-an-existing-storage-account-to-the-list"></a>Ein Speicherkonto zu der Liste hinzufügen ##

1. Wenn Sie noch nicht über ein Azure Storage-Konto erstellen, indem Sie die Schritte im **Abschnitt einen neuen Speicher erstellen** oben aufgeführten verfügen. (Alternativ können Sie ein neues Speicherkonto [Azure-Verwaltungsportal][]erstellen.)
2. Klicken Sie im Dialogfeld **Speicherkonten** **Hinzufügen**.
3. Geben Sie im Dialogfeld **Speicherkonto hinzufügen** **Namen** und **Zugriffstaste**. Der Konto-Namen und Schlüssel muss für ein vorhandenes Konto der Azure-Speicher. Verwenden Sie **den Lagerbereich [Azure-Verwaltungsportal][] ** Ihren Kontonamen Speicher Schlüssel anzeigen und. **Speicherkonto hinzufügen** Dialog sieht etwa wie folgt.

    ![][ic719497]

4. Klicken Sie auf **OK** , um das Dialogfeld **Speicherkonto hinzufügen** zu schließen.

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a>So ändern Sie ein Speicherkonto, um eine neue Tastenkombination verwenden ##

1. Klicken Sie im Dialogfeld **Speicherkonten** Speicherkonto, das Sie bearbeiten möchten und klicken Sie dann auf **Bearbeiten**.
2. Ändern Sie im Dialogfeld **Storage Konto Zugriff auf Schlüssel bearbeiten** den **Schlüssel** -Wert.
3. Klicken Sie auf **OK** , um das Dialogfeld **Storage Konto Zugriff auf Schlüssel bearbeiten** zu schließen.

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a>Ein Speicherkonto aus der Liste in Eclipse entfernen ##

1. Klicken Sie im Dialogfeld **Speicherkonten** Speicherkonto, das Sie bearbeiten möchten und klicken Sie auf **Entfernen**.
2. Klicken Sie auf **OK** , wenn Sie dazu aufgefordert, das Speicherkonto entfernen.

>[AZURE.NOTE] Entfernen das Speicherkonto Dialog **Speicherkonten** entfernt nur es aus der Liste der Speicherkonten in Eclipse angezeigt. Es entfernt nicht das Speicherkonto aus dem Azure-Abonnement. Darüber hinaus konnte das Speicherkonto wieder in der Liste angezeigt, nachdem Eclipse die Details Ihres Abonnements lädt.

## <a name="see-also"></a>Siehe auch ##

[Azure-Toolkit für Eclipse][]

[Azure Toolkit installieren für Eclipse][] 

[Erstellen eine Hello World-Anwendung für Azure in Eclipse][]

Weitere Informationen zum Verwenden von Azure mit Java finden Sie unter [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure-Toolkit für Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure-Verwaltungsportal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Erstellen eine Hello World-Anwendung für Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure Toolkit installieren für Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png
