<properties
    pageTitle="Übersicht über Schemas und Enterprise-Integrationspaket | Microsoft Azure"
    description="Enthält Informationen zum Verwenden von Schemas Integrationspaket Enterprise und Logik Apps"
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
    ms.date="07/29/2016"
    ms.author="deonhe"/>

# <a name="learn-about-schemas-and-the-enterprise-integration-pack"></a>Informationen Sie zu Schemas und Enterprise-Integrationspaket  

## <a name="why-use-a-schema"></a>Wozu dient ein Schema?
Mithilfe von Schemas um zu bestätigen, dass XML-Dokumente erhalten Sie mit den erwarteten Daten in einem vordefinierten Format gültig sind. Schemas dienen Nachrichten überprüft, die in einem B2B-Szenario ausgetauscht werden.

## <a name="add-a-schema"></a>Fügen Sie ein schema
Von Azure-Portal:  

1. Wählen Sie **Weitere Dienste**.  
![Screenshot der Azure-Portal mit "Weitere Dienste" markiert](./media/app-service-logic-enterprise-integration-overview/overview-11.png)    

2. Geben Sie im Feld Search Filter **Integration**und wählen Sie **Integration Konten** aus der Liste aus.     
![Screenshot des Filter-Suchfeld](./media/app-service-logic-enterprise-integration-overview/overview-21.png)  
3. Wählen Sie das **Konto Integration** , das Schema hinzufügen.    
![Screenshot der Kontenliste integration](./media/app-service-logic-enterprise-integration-overview/overview-31.png)  

4. Wählen Sie das Musterelement **Schemas** .  
![Screenshot des IEP Integration Konto "Schemas" markiert](./media/app-service-logic-enterprise-integration-schemas/schema-11.png)  

### <a name="add-a-schema-file-less-than-2-mb"></a>Hinzufügen einer Datei weniger als 2 MB  

1. Wählen Sie (aus den vorherigen Schritten) Öffnet Blatt **Schemas** **Hinzufügen**.  
![Screenshot des Schemas Blade mit "hinzufügen" markiert](./media/app-service-logic-enterprise-integration-schemas/schema-21.png)  

2. Geben Sie einen Namen für das Schema ein. Upload die Schemadatei wählen Sie das Ordnersymbol neben dem Textfeld **Schema** . Nachdem der Upload abgeschlossen ist, wählen Sie **OK**.    
![Screenshot von "Schema hinzufügen", "Kleine Datei" markiert](./media/app-service-logic-enterprise-integration-schemas/schema-31.png)  

### <a name="add-a-schema-file-larger-than-2-mb-up-to-a-maximum-of-8-mb"></a>Hinzufügen einer Schemadatei größer als 2 MB (maximal 8 MB)  

Dieser Prozess hängt von BLOB-Container zugreifen: **Public** oder **keinen anonymen Zugriff**. Um diese Zugriffsstufe im **Speicher-Explorer Azure** **Blob-Container**wählen Sie gewünschte Blob-Container aus Wählen Sie **Sicherheit**und **Zugriff auf** die Registerkarte.

1. Die BLOB-Zugriff auf **Öffentliche**ist, gehen Sie folgendermaßen vor.  
  ![Screenshot von Azure Storage Explorer "Blob-Container", "Sicherheit" und "Public" markiert](./media/app-service-logic-enterprise-integration-schemas/blob-public.png)  

    ein. Laden Sie Schema in den Speicher, und kopieren Sie den URI.  
    ![Screenshot des Speicherkonto URI hervorgehoben](./media/app-service-logic-enterprise-integration-schemas/schema-blob.png)  

    b. Im **Schema hinzufügen** **große Datei**auswählen und im Textfeld **URI Content** bereitstellen.  
    ![Screenshot des Schemas mit "hinzufügen" und "Groß" markiert](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

2. Die Zugriff auf Blob **keinen anonymen Zugriff**ist, gehen Sie folgendermaßen vor.  
  ![Screenshot von Azure Storage Explorer mit "BLOB-Container", "Sicherheit" und "Keine anonymen Zugriff" markiert](./media/app-service-logic-enterprise-integration-schemas/blob-1.png)  

    ein. Laden Sie das Schema in den Speicher.  
    ![Screenshot des Speicher-Konto](./media/app-service-logic-enterprise-integration-schemas/blob-3.png)

    b. SAS für das Schema zu generieren.  
    ![Screenshot des Speicher-Benutzerkontos mit gemeinsamen Zugriff Registerkarte "Signaturen" markiert](./media/app-service-logic-enterprise-integration-schemas/blob-2.png)

    c. Wählen Sie im **Schema hinzufügen** **große Datei**und geben Sie SAS-URI in das Textfeld **Content URI an** .  
    ![Screenshot des Schemas mit "hinzufügen" und "Groß" markiert](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

3. Blatt **Schemas** EIP Integration Konto sollte jetzt neu hinzugefügte Schema angezeigt werden.  
![Screenshot der EIP Integration Konto "Schemas" mit dem neuen Schema markiert](./media/app-service-logic-enterprise-integration-schemas/schema-41.png)
  

## <a name="edit-schemas"></a>Bearbeiten von schemas
1. Wählen Sie das Musterelement **Schemas** .  
2. Wählen Sie das Schema, **Schemas** Blatt bearbeiten, die geöffnet werden soll.
3. Wählen Sie auf dem Blatt **Schemas** **Bearbeiten**.  
![Screenshot des Schemas blade](./media/app-service-logic-enterprise-integration-schemas/edit-12.png)    
4. Wählen Sie die Schemadatei zu bearbeiten, indem Sie die Farbauswahl-Dialogfeld, das geöffnet wird.
5. Wählen Sie in der Dateiauswahl **geöffnet** .  
![Screenshot der Dateiauswahl](./media/app-service-logic-enterprise-integration-schemas/edit-31.png)  
6. Sie erhalten eine Benachrichtigung, die angibt, dass der Upload erfolgreich war.  

## <a name="delete-schemas"></a>Löschen von Schemata
1. Wählen Sie das Musterelement **Schemas** .  
2. Wählen Sie das Schema, **Schemas** Blatt löschen, die geöffnet werden soll.  
3. Das Blade **Schemas** wählen Sie **Löschen**.
![Screenshot des Schemas blade](./media/app-service-logic-enterprise-integration-schemas/delete-12.png)  

4. Wählen Sie **Ja**, um Ihre Auswahl zu bestätigen.  
![Screenshot der Bestätigungsnachricht "Schema löschen"](./media/app-service-logic-enterprise-integration-schemas/delete-21.png)  
5. Beachten Sie, dass die Liste der Schemas in **Schemas** Blade aktualisiert und gelöschten Schema nicht mehr aufgeführt.  
![Screenshot der EIP Integration Konto "Schemas" markiert](./media/app-service-logic-enterprise-integration-schemas/delete-31.png)    

## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr über Enterprise-Integrationspaket] (./app-service-logic-enterprise-integration-overview.md "Erfahren Sie mehr über Enterprise Integration Packs").  
