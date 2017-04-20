
<properties
    pageTitle="Enterprise-Integrationspaket mit Zertifikaten | Microsoft Azure"
    description="So verwenden Sie Zertifikate mit Enterprise-Integrationspaket Logik Apps"
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="msftman"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="deonhe"/>

# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>Informationen Sie zu Zertifikaten und Enterprise-Integrationspaket

## <a name="overview"></a>Übersicht
Unternehmensintegration verwendet Zertifikate, sichere B2B-Kommunikation. Zwei Arten von Zertifikaten können in Ihre Enterprise Integration apps:

- Öffentliche Zertifikate von einer Zertifizierungsstelle (CA) gekauft werden müssen.
- Private Zertifikate selbst ausstellen kann. Diese Zertifikate werden manchmal als selbstsignierte Zertifikate bezeichnet.


## <a name="what-are-certificates"></a>Was sind Zertifikate?
Zertifikate sind digitale Dokumente die Identität der Teilnehmer in die elektronische Kommunikation und die auch sichere Kommunikation.

## <a name="why-use-certificates"></a>Warum verwenden Zertifikate?
Manchmal müssen B2B-Kommunikation vertraulich. Unternehmensintegration verwendet Zertifikate dieser Kommunikation auf zwei Arten:

- Der Inhalt der Nachrichten verschlüsseln
- Durch digitales Signieren von Nachrichten  

## <a name="how-do-you-upload-certificates"></a>Wie hochladen Sie Zertifikate?

### <a name="public-certificates"></a>Öffentliche Zertifikate
Um ein *öffentliches Zertifikat* in Logik-apps mit B2B-Funktionen verwenden, müssen Sie das Zertifikat berücksichtigt Integration hochladen. Um ein *selbstsigniertes Zertifikat*verwenden, müssen auf der anderen Seite Sie zuerst es [Azure Key Vault](../key-vault/key-vault-get-started.md "erfahren Sie mehr über Schlüssel Depot")hochladen.

Nach dem Hochladen eines Zertifikats steht die B2B-Nachrichten sichern, wenn ihre Eigenschaften in den [Abkommen](./app-service-logic-enterprise-integration-agreements.md) zu definieren, die Sie erstellen können.  

Hier werden die einzelnen Schritte zum Hochladen Ihrer öffentlichen Zertifikate berücksichtigt Integration nach Azure-Portal anmelden:

1. Wählen Sie **Durchsuchen**.  
    ![Klicken Sie auf Durchsuchen](./media/app-service-logic-enterprise-integration-overview/overview-1.png)  

2. Geben Sie im Suchfeld Filter **Integration** und wählen Sie **Integration Konten** aus der Liste.     
    ![Integration Konten auswählen](./media/app-service-logic-enterprise-integration-overview/overview-2.png)

3. Wählen Sie das Integration das Zertifikat hinzugefügt werden soll.  
    ![Wählen Sie das Zertifikat hinzufügen möchten Integration-Konto](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4.  Wählen Sie das Musterelement **Zertifikate** .  
    ![Wählen Sie das Musterelement Zertifikate](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)

5. **Zertifikate** -Blatt, das geöffnet wird, klicken Sie auf **Hinzufügen** .
    ![Wählen Sie die Schaltfläche hinzufügen](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. Geben Sie einen **Namen** für das Zertifikat, und wählen Sie dann das Zertifikat. (In diesem Beispiel verwendet wir öffentlichen Zertifikattyp.) Wählen Sie das Ordnersymbol rechts neben **dem Textfeld** ein.

7. Dateiauswahl, suchen Sie und wählen Sie die Zertifikatsdatei, die Sie Ihrem Konto Integration hochladen möchten.

8. Wählen Sie das Zertifikat, und klicken Sie auf **OK** in der Dateiauswahl. Dies überprüft und lädt das Zertifikat zu Ihrem Konto Integration.

8. Schließlich auf Blade **Zertifikat hinzufügen** wählen Sie **OK** .  
    ![Klicken Sie auf OK](./media/app-service-logic-enterprise-integration-certificates/certificate-3.png)  

9. In ungefähr einer Minute sehen eine Benachrichtigung Sie die angibt, dass das Zertifikat Upload abgeschlossen ist.

10. Wählen Sie das Musterelement **Zertifikate** . Das neue Zertifikat sollte angezeigt werden.  
    ![Das neue Zertifikat](./media/app-service-logic-enterprise-integration-certificates/certificate-4.png)  

### <a name="private-certificates"></a>Private Zertifikate
Die folgenden Schritte können Sie private Zertifikate berücksichtigt Integration hochladen:  

1. [Private Schlüssel Schlüssel Tresor] (../key-vault/key-vault-get-started.md "Erfahren Sie mehr über wichtige Depot").  

    > [AZURE.TIP] Sie müssen das Logik Apps Feature von Azure App Service Operationen auf Schlüssel autorisieren. Sie können die Logik Apps Dienstprinzipalnamen Zugriff gewähren, mit dem folgenden PowerShell-Befehl:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  

2. Erstellen Sie ein privates Zertifikat.  

3. Hochladen Sie das private Zertifikat zu Ihrem Konto Integration.

Nachdem Sie die vorherigen Schritte ausgeführt haben, können Sie das private Zertifikat, um zu erstellen.

Es folgen Einzelheiten zum Hochladen Ihrer privaten Zertifikate berücksichtigt Integration nach Azure-Portal anmelden:  

1. Wählen Sie **Durchsuchen**.  
    ![Ihr Konto Integration Ihrer privaten Zertifikate hochladen](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    

2. Geben Sie im Suchfeld Filter **Integration** und wählen Sie **Integration Konten** aus der Liste.     
    ![Integration Konten auswählen](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  

3. Wählen Sie das Integration das Zertifikat hinzugefügt werden soll.  
    ![Wählen Sie das Zertifikat hinzufügen möchten Integration-Konto](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4. Wählen Sie das Musterelement **Zertifikate** .  
    ![Wählen Sie das Musterelement Zertifikate](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)  

5. **Zertifikate** -Blatt, das geöffnet wird, klicken Sie auf **Hinzufügen** .
    ![Wählen Sie die Schaltfläche hinzufügen](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. Geben Sie einen **Namen** für das Zertifikat, und wählen Sie dann das Zertifikat. (In diesem Beispiel verwendet wir öffentlichen Zertifikattyp.) Wählen Sie das Ordnersymbol rechts neben **dem Textfeld** ein.

7. Dateiauswahl, suchen Sie und wählen Sie die Zertifikatsdatei, die Sie Ihrem Konto Integration hochladen möchten.

8. Nachdem Sie das Zertifikat ausgewählt haben, wählen Sie **OK** in der Dateiauswahl. Diese Aktion das Zertifikat überprüft und Integration-Konto hochgeladen.

9. Schließlich auf Blade **Zertifikat hinzufügen** wählen Sie **OK** .  
    ![Zertifikat hinzufügen](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-1.png)  

10. In ungefähr einer Minute sehen eine Benachrichtigung Sie die angibt, dass das Zertifikat Upload abgeschlossen ist.

11. Wählen Sie das Musterelement **Zertifikate** . Das neue Zertifikat sollte angezeigt werden.
    ![Das neue Zertifikat](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-2.png)  

Nach dem Hochladen eines Zertifikats steht Ihnen bei der Sicherung der B2B-Nachrichten beim Festlegen ihrer Eigenschaften im [Abkommen](./app-service-logic-enterprise-integration-agreements.md).  

## <a name="next-steps"></a>Nächste Schritte
- [Erstellen einer Logik, die B2B-Funktionen verwendet](./app-service-logic-enterprise-integration-b2b.md)  
- [Erstellen Sie eine B2B](./app-service-logic-enterprise-integration-agreements.md)  
- [Erfahren Sie mehr über Schlüssel Depot] (../key-vault/key-vault-get-started.md "Erfahren Sie mehr über wichtige Vault")  
