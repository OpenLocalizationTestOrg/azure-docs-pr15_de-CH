<properties 
    pageTitle="Logik-App Funktionen | Microsoft Azure" 
    description="Erfahren Sie, wie die erweiterten Funktionen von Logik-apps." 
    authors="stepsic-microsoft-com" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/28/2016"
    ms.author="stepsic"/> 
    
# <a name="use-logic-apps-features"></a>Logik-Apps Funktionen

Im [vorherigen Thema](app-service-logic-create-a-logic-app.md)haben Sie Ihre erste Anwendung Logik erstellt. Jetzt zeigen wir Ihnen wie einen vollständigen Prozess App Services Logik Apps. Dieses Thema stellt die folgenden neuen Logik Apps Konzepte:

- Bedingte Logik, die eine Aktion ausgeführt wird, nur, wenn eine bestimmte Bedingung erfüllt ist.
- So bearbeiten Sie eine vorhandene Anwendung Logik Codeansicht.
- Optionen zum Starten eines Workflows.

Bevor Sie dieses Thema abschließen, sollten Sie [erstellen eine neue Logik app](app-service-logic-create-a-logic-app.md)Schritte. [Azure-Portal]wechseln zu Ihrer Anwendung Logik und **Auslöser und Aktionen** in der Zusammenfassung der Logik app bearbeiten klicken.

## <a name="reference-material"></a>Referenzmaterial

Sie können die folgenden Dokumente hilfreich:

- [Management und Runtime REST-APIs](https://msdn.microsoft.com/library/azure/mt643787.aspx) - wie Logik apps direkt aufrufen
- [Referenzhandbuch](https://msdn.microsoft.com/library/azure/mt643789.aspx) - eine umfassende Liste der unterstützten Funktionen/Ausdrücke
- [Trigger und Aktionstypen](https://msdn.microsoft.com/library/azure/mt643939.aspx) - Arten von Aktionen und nehmen sie Eingaben
- [Übersicht der App Service](../app-service/app-service-value-prop-what-is.md) - Beschreibung der Komponenten beim Erstellen einer Lösung auswählen

## <a name="adding-conditional-logic"></a>Bedingten Logik hinzufügen

Der ursprüngliche Fluss funktioniert sind einige Bereiche, die verbessert werden könnte. 


### <a name="conditional"></a>Bedingte
Diese Logik app führen Sie viele e-Mail-Nachrichten. Die folgenden Schritte hinzufügen Logik, um sicherzustellen, dass Sie nur eine e-Mail erhalten, wenn jemand mit einer bestimmten Anzahl von Anhängern die Tweet stammen. 

1. Klicken Sie auf das Pluszeichen und Twitter suchen Sie Aktion *Erhalten Benutzer* .

2. Im Feld **Tweeted durch** den Trigger Twitter Benutzer erfahren übergeben.

    ![Benutzer](./media/app-service-logic-use-logic-app-features/getuser.png)

3. Klicken Sie erneut auf das Pluszeichen, aber diesmal wählen Sie **Bedingung hinzufügen**

4. Klicken Sie im ersten Feld auf **...** unter **Benutzer erhalten** das Feld **Nachfolger** .

5. Wählen Sie in der Dropdownliste **größer als**

6. Geben Sie im zweiten Feld die Anzahl der Anhänger, die Sie Benutzern zuweisen möchten.

    ![Bedingte](./media/app-service-logic-use-logic-app-features/conditional.png)

7.  Ziehen und Ablegen e-Mail im Feld **Wenn Ja** schließlich Feld. Dies bedeutet, dass Sie nur e-Mails erhalten Anhänger zählen erfüllt ist.

## <a name="repeating-over-a-list-with-foreach"></a>Eine Liste mit ForEach wiederholen

Die ForEach-Schleife gibt ein Array über eine Aktion zu wiederholen. Der Datenfluss schlägt fehl, wenn kein Array ist. Beispielsweise wenn Sie Aktion1, das ein Array von Nachrichten ausgibt, und jede Nachricht senden möchten Sie zählen ForEach-Anweisung in den Eigenschaften der Aktion: ForEach:"@action('action1').outputs.messages"
 

## <a name="using-the-code-view-to-edit-a-logic-app"></a>Verwenden der Codeansicht Logik App bearbeiten

Neben dem Designer können Sie direkt den Code bearbeiten, der eine Anwendung Logik wie folgt definiert. 

1. Klicken Sie auf die Schaltfläche **Code anzeigen** in der Befehlsleiste. 

    Eine vollständige Editor, die die Definition wird gerade bearbeiteten wird geöffnet.

    ![Code anzeigen](./media/app-service-logic-use-logic-app-features/codeview.png)

    Mithilfe des Text-Editors können Sie kopieren und fügen eine beliebige Anzahl von Aktionen innerhalb der gleichen Logik app oder Logik-apps. Sie können auch hinzufügen oder Entfernen von ganze Abschnitte aus der Definition und können Sie auch Definitionen für andere freigeben.

2. Nachdem Sie die in der Codeansicht ändern, klicken Sie einfach auf **Speichern**. 

### <a name="parameters"></a>Parameter
Es gibt einige Funktionen von Logik-Apps, die nur in der Codeansicht verwendet werden kann. Ein Beispiel dafür ist Parameter. Parameter erleichtern die Wiederverwendung Werte Logik-app. Wenn Sie eine e-Mail-Adresse Sie mehrere Aktionen möchten haben, sollten Sie es beispielsweise als Parameter definieren.

Die folgenden aktualisiert Ihre vorhandene Logik app für Abfrage Parameter verwenden.

1. Suchen Sie in der Codeansicht die `parameters : {}` Objekt, und fügen Sie das folgende Thema-Objekt:

        "topic" : {
            "type" : "string",
            "defaultValue" : "MicrosoftAzure"
        }
    
2. Wählen Sie die `twitterconnector` Aktion den Wert für das Suchen und Ersetzen `#@{parameters('topic')}`.
    Sie können die Funktion **Concat** verbinden zwei oder mehr Zeichenfolgen, z. B.: `@concat('#',parameters('topic'))` ist identisch mit der obigen. 
 
Parameter sind eine gute Möglichkeit, Werte abrufen, die Sie wahrscheinlich viel geändert. Sie sind besonders nützlich, wenn Sie Parameter in unterschiedlichen Umgebungen überschreiben möchten. Weitere Informationen zum Überschreiben Parameter-Umgebung finden Sie in unserer [REST API-Dokumentation](https://msdn.microsoft.com/library/mt643787.aspx).

Wenn Sie auf **Speichern**klicken, jetzt stündlich Sie alle neuen Tweets, die mehr als 5 Retweets Ordner **Tweets** in Ihrer Dropbox übermittelt.

Über Logik App Definitionen finden Sie unter [Logik App Definitionen erstellen](app-service-logic-author-definitions.md).

## <a name="starting-a-logic-app-workflow"></a>Starten eines Workflows Logik app
Es gibt mehrere verschiedene Optionen zum Starten des Workflows Logik app definiert. Ein Workflow immer gestartet werden kann bei Bedarf in [Azure-Portal].

### <a name="recurrence-triggers"></a>Serie Trigger
Serie Trigger führt in Zeitabständen, die Sie angeben. Wenn der Trigger bedingten Logik ist, bestimmt der Trigger oder ob der Workflow ausgeführt werden muss. Ein Trigger gibt führen durch Rückgabe eine `200` Statuscode. Wenn es keine ausführen, wird ein `202` -Statuscode.

### <a name="callback-using-rest-apis"></a>Rückruf mit REST-APIs
Dienste können einen Logik app Endpunkt zum Starten eines Workflows aufrufen. Weitere Informationen finden Sie in der [Logik apps als aufrufbare Endpunkte](app-service-logic-connector-http.md) . Um diese Art von Logik app bei Bedarf zu starten, klicken Sie **jetzt** auf der Befehlsleiste. 

<!-- Shared links -->
[Azure-portal]: https://portal.azure.com 