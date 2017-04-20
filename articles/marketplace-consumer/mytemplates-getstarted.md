<properties
   pageTitle="Erste Schritte mit privaten Vorlagen | Microsoft Azure"
   description="Hinzufügen, verwalten und Freigeben von privaten Vorlagen mithilfe von Azure-Portal, Azure-CLI oder PowerShell."
   services="marketplace-customer"
   documentationCenter=""
   authors="VybavaRamadoss"
   manager="asimm"
   editor=""
   tags="marketplace, azure-resource-manager"
   keywords=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/18/2016"
   ms.author="vybavar"/>

# <a name="get-started-with-private-templates-on-the-azure-portal"></a>Erste Schritte mit privaten Vorlagen Azure-Portal

Eine Vorlage [Azure-Ressourcen-Manager](../resource-group-authoring-templates.md) ist eine deklarative Vorlage verwendet, um die Bereitstellung zu definieren. Sie können die Ressourcen für eine Lösung bereitstellen definieren und geben Parameter und Variablen, die Werte für verschiedene umge bungen ermöglichen. Die Vorlage umfasst JSON und Ausdrücke mit Werten für die Bereitstellung erstellen.

Können Sie neue **Vorlagen** Funktion in [Azure-Portal](https://portal.azure.com) mit dem Ressourcenanbieter **Microsoft.Gallery** als Erweiterung von [Azure Marketplace](https://azure.microsoft.com/marketplace/) Benutzer erstellen, verwalten und Bereitstellen von privaten Vorlagen aus einer persönlichen Bibliothek.

Dieses Dokument führt Sie durch Hinzufügen, verwalten und Freigeben von privaten **Vorlage** mithilfe der Azure-Portal.

## <a name="guidance"></a>Anleitung

Die folgenden Vorschläge helfen Ihnen **Vorlagen** nutzen beim Arbeiten mit Projektmappen:

- Eine **Vorlage** ist eine Kapselung Ressource, die ein Ressourcen-Manager-Vorlage und zusätzliche Metadaten enthält. Es verhält sich sehr ähnlich wie ein Element auf dem Markt. Der Hauptunterschied ist es ein privates Element öffentlichen Markt-Elemente.
- **Die Vorlagenbibliothek** eignet sich gut für Benutzer, die ihre Bereitstellung anpassen.
- **Vorlagen** eignen sich gut für Benutzer, die ein einfaches Repository in Azure.
- Beginnen Sie mit einer Vorlage Ressourcenmanager. Vorhandene Ressourcengruppe Suche Vorlagen in [Github](https://github.com/Azure/azure-quickstart-templates) oder [Vorlage exportieren](../resource-manager-export-template.md) .
- **Vorlagen** sind Veröffentlichung Benutzer gebunden. Der Name des Herausgebers ist für alle Benutzer mit Lesezugriff sichtbar.
- **Vorlagen** sind Ressourcenmanager Ressourcen und kann nicht umbenannt werden, nach der Veröffentlichung.

## <a name="add-a-template-resource"></a>Fügen Sie eine Vorlagenressource

Es gibt zwei Methoden zum Erstellen einer **Dialogfeldvorlagen** -Ressource in Azure-Portal.

### <a name="method-1--create-a-new-template-resource-from-a-running-resource-group"></a>Methode 1: Erstellen einer neuen Vorlagenressource aus einer Ressourcengruppe ausführen

1. Navigieren Sie zu einer Ressourcengruppe Azure-Portal. Wählen Sie in **Einstellungen** **Vorlage exportieren** .
2. Nach dem Exportieren der Vorlage Ressourcenmanager Schaltfläche **Vorlage speichern** zum **Vorlagen** -Repository speichern. Exportvorlage Einzelheiten finden [hier](../resource-manager-export-template.md).
<br /><br />
![Ressource Gruppe exportieren](media/rg-export-portal1.PNG)  <br />

3. Wählen Sie die Schaltfläche **als Vorlage speichern** .
<br /><br />

4. Geben Sie Folgendes ein:

    - Name – Name der Vorlage (Hinweis: Dies ist Azure-Ressourcen-Manager basiert. Alle Namen Einschränkungen und es kann nicht nach der Erstellung geändert werden).
    - Beschreibung – kurze Zusammenfassung der Vorlage.

    ![Vorlage speichern](media/save-template-portal1.PNG)  <br />

5. Klicken Sie auf **Speichern**.

    > [AZURE.NOTE] Blade Vorlage exportieren zeigt Benachrichtigungen, wenn die exportierten Vorlage Ressourcenmanager Fehler, aber dennoch Ressourcenmanager Vorlage Vorlagen gespeichert werden. Sicherstellen Sie, dass Sie und alle Ressourcen-Manager Vorlage beseitigen die exportierten Ressourcenmanager Vorlage erneut bereit.

### <a name="b-method-2--add-a-new-template-resource-from-browse"></a>B. Methode 2: Fügen Sie eine neue Vorlagenressource aus durchsuchen

Sie können auch eine neue **Vorlage** vollständig neuen die + hinzufügen Schaltfläche im **Durchsuchen > Vorlagen**. Sie müssen einen Namen, Beschreibung und Ressourcenmanager Vorlage JSON.

![Vorlage hinzufügen](media/add-template-portal1.PNG)  <br />

> [AZURE.NOTE] Microsoft.Gallery ist ein Mieter Azure Ressourcenanbieter. Die Dialogfeldvorlagen-Ressource verbunden der Benutzer erstellt hat. Es ist nicht an bestimmte Abonnements gebunden. Ein Abonnement muss nur beim Bereitstellen einer Vorlage gewählt werden.

## <a name="view-template-resources"></a>Ressourcen anzeigen

Alle **Vorlagen** verfügbar sehen Sie am **Durchsuchen > Vorlagen**. Dazu gehören **Vorlagen** sowie zu erstellen, die mit unterschiedlichen Berechtigungen freigegeben haben. Weitere Informationen im Abschnitt [Zugriffskontrolle](#access-control-for-a-tenant-resource-provider) .

![Vorlage anzeigen](media/view-template-portal1.PNG)  <br />

Durch Klicken auf ein Element in der Liste können Sie die Details einer **Vorlage** anzeigen.

![Vorlage anzeigen](media/view-template-portal2c.png)  <br />

## <a name="edit-a-template-resource"></a>Bearbeiten Sie eine Vorlagenressource

Sie können den Fluss Bearbeiten einer **Vorlage** direkt auf das Element in der Suchliste oder die Schaltfläche Bearbeiten initiieren.

![Vorlage bearbeiten](media/edit-template-portal1a.PNG)  <br />

Sie können die Beschreibung oder Ressourcenmanager Vorlagentext bearbeiten. Sie können nicht den Namen bearbeiten ist ein Ressourcen-Manager-Ressourcenname Wenn Sie die Ressourcenmanager Vorlage bearbeiten JSON wir überprüft werden, um sicherzustellen, die gültigen JSON. Wählen Sie **OK** und **Speichern** die aktualisierte Vorlage speichern.

![Vorlage bearbeiten](media/edit-template-portal2a.PNG)  <br />

Nach dem Speichern der **Vorlage** sehen Sie eine Benachrichtigung zur Bestätigung.

![Vorlage bearbeiten](media/edit-template-portal3b.png)  <br />

## <a name="deploy-a-template-resource"></a>Bereitstellen einer Dialogfeldvorlagen-Ressource

Über **Leseberechtigungen** für können jede **Vorlage** bereitgestellt werden. Fluss Bereitstellung startet das standardmäßige Azure Vorlage Bereitstellung Blade. Füllen Sie die Werte für die Ressourcenmanager Vorlagenparameter mit der Bereitstellung fortfahren.

![Vorlage bereitstellen](media/deploy-template-portal1b.png)  <br />

## <a name="share-a-template-resource"></a>Freigeben einer Vorlagenressource

**Eine Vorlagenressource** kann mit Ihren Kollegen freigegeben werden. Freigabe verhält sich ähnlich wie [Zuweisung der Sicherheitsrolle für jede Ressource in Azure](../active-directory/role-based-access-control-configure.md). Eigentümer **Vorlage** bietet Berechtigungen mit einer Vorlage anderen Benutzern interagieren können. Person oder Personengruppe Teilen Sie die **Vorlage** mit können der Ressourcen-Manager-Vorlage und seine Eigenschaften.

### <a name="access-control-for-the-microsoftgallery-resources"></a>Zugriffskontrolle für Ressourcen Microsoft.Gallery

Rolle | Berechtigungen
---|----
Besitzer | Ermöglicht Vollzugriff auf die Vorlagenressource, einschließlich
Reader | Erlaubt den Lese- und Execute(Deploy) auf die Vorlagenressource
Teilnehmer | Die Dialogfeldvorlagen-Ressource können über Berechtigungen zum Bearbeiten und löschen. Benutzer kann keine Vorlage mit anderen Teilen.

**Freigeben** des Elements durchsuchen Rechtsklick oder wählen Sie auf die Ansicht eines bestimmten Elements aus Dadurch wird ein Erfahrungsaustausch.

![Freigabe-Vorlage](media/share-template-portal1a.png)  <br />

 Sie können jetzt eine Rolle und Benutzer oder Gruppen den Zugriff auf eine bestimmte **Vorlage**. Die verfügbaren Rollen sind Besitzer und Leser Mitwirkender. Weitere Informationen im Abschnitt [Zugriffskontrolle](#access-control-for-a-tenant-resource-provider) .

![Freigabe-Vorlage](media/share-template-portal2b.png)  <br />

![Freigabe-Vorlage](media/share-template-portal3b.png)  <br />

Klicken Sie auf **auswählen** und auf **Ok**. Sie sehen die Benutzer oder Gruppen, die Sie der Ressource hinzugefügt.

![Freigabe-Vorlage](media/share-template-portal4b.png)  <br />

> [AZURE.NOTE] Eine Vorlage kann nur mit Benutzern und Gruppen in der gleichen Active Directory Azure Mieter freigegeben werden. Wenn Sie eine Vorlage für eine e-Mail-Adresse freigeben, die nicht in Ihrem Mandanten Einladung erhalten der Benutzer Mieter als Gast an.

## <a name="next-steps"></a>Nächste Schritte

- Zum Erstellen von Ressourcen-Manager-Vorlagen finden Sie unter [Erstellung Vorlagen](../resource-group-authoring-templates.md)
- Die Funktionen in einer Vorlage Ressourcenmanager verwenden finden Sie unter [Vorlagenfunktionen](../resource-group-template-functions.md)
- Anleitung zum Entwerfen von Vorlagen finden Sie unter [Vorgehensweisen zum Entwerfen von Azure-Ressourcen-Manager Vorlagen](../best-practices-resource-manager-design-templates.md)
