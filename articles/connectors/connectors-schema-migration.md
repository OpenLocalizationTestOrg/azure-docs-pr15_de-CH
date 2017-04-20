<properties
    pageTitle="Migrieren von Logik-apps zu Schema Version 2015-08-01-Vorschau | Microsoft Azure App Service"
    description="Sie können die aktuelle Schemaversion leicht Logik-apps migrieren. Befolgen Sie diese Schritte."
    services="logic-apps"
    documentationCenter=""
    authors="MSFTMAN"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="deonhe"/>

# <a name="how-to-migrate-logic-apps-to-schema-version-2015-08-01-preview"></a>Migrieren von Logik-apps zu Schema Version 2015-08-01-Vorschau

Um die vorhandenen apps Logik in das neue Schema verschieben, führen Sie folgende Schritte aus:  
1. Öffnen Sie Ihre app Logik in Azure-portal  
2. Klicken Sie auf Aktualisierungsschema:

 ![API-Symbol][step1]   
Schema aktualisieren Seite angezeigt und enthält eine Verknüpfung zu einem Dokument, das Informationen zählen das neue Schema: ![API-Symbol][step2]

>[AZURE.NOTE] Bei Auswahl von **Schema aktualisieren**wir automatisch Migration ausführen und die Ausgabe für Sie. Hiermit können Sie jedoch die Definition aktualisieren, Befolgen bewährter Programmierpraktiken wie im Abschnitt **Best Practices** sicherstellen.

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a>Bewährte Logik-apps die Schema-Version migrieren:  

- Kopieren Sie migrierte Skript in eine neue Logik App - nicht überschreiben die alten bis Abschluss der Tests und bestätigt die migrierte Anwendung funktioniert wie erwartet.
- Testen der Logik app **vor der** Produktion
- Nach Abschluss der Migration starten Sie aktualisieren Ihre Logik apps [verwalteten APIs](./apis-list.md) verwenden. Beispielsweise können Sie mit Dropbox v2, wo mithilfe von DropBox v1 starten.


## <a name="whats-next"></a>Was kommt als nächstes
-  [Erfahren Sie, wie Ihre Logik apps manuell migrieren](../app-service-logic/app-service-logic-schema-2015-08-01.md)


<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png






