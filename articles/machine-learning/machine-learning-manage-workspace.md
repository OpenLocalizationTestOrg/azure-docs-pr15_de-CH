<properties
    pageTitle="Einen Arbeitsbereich Computerlernen verwalten | Microsoft Azure"
    description="Zugriff auf Azure Machine Learning Arbeitsbereiche verwalten und bereitstellen und Verwalten von ML API-Webdiensten"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="garye"/>


# <a name="manage-an-azure-machine-learning-workspace"></a>Verwalten von Azure Machine Learning-Arbeitsbereich

>[AZURE.NOTE] Die Verfahren in diesem Artikel sind für Azure Machine Learning klassische Webdienste. Informationen zum Verwalten von Webdiensten im Computer lernen Web-Portal finden Sie unter [Verwalten eines Webdienstes mit Azure Machine Learning Web Services-Portal](machine-learning-manage-new-webservice.md).

Klassische Azure-Portal können Sie Ihre Computerlernen Arbeitsbereiche verwalten:

- Überwachen Sie, wie der Arbeitsbereich verwendet wird
- Konfigurieren Sie den Arbeitsbereich, um den Zugriff gewähren oder verweigern
- Verwalten Sie im Arbeitsbereich erstellte Webdienste
- Arbeitsbereich löschen

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Darüber hinaus die Dashboard-Registerkarte Überblick über Ihr Workspace-Verwendung und eine kurze Übersicht über die Informationen des Arbeitsbereichs.  

> [AZURE.TIP] In Azure Machine Learning Studio auf der Registerkarte **WEB SERVICES** können Sie hinzufügen, aktualisieren und Löschen einen Machine Learning-Webdienst.

So verwalten Sie einen Arbeitsbereich

1.  Melden Sie sich mit Ihrem Microsoft Azure-Konto [Azure-Verwaltungsportal](https://manage.windowsazure.com/) - verwenden Sie das Konto der Azure-Abonnement zugeordnet ist.
2.  Klicken Sie im Bereich Dienste Microsoft Azure **MASCHINELLES lernen**.
3.  Klicken Sie auf den Arbeitsbereich, den Sie verwalten möchten.

Die Workspace-Seite enthält drei Registerkarten:

- **DASHBOARD** - ermöglicht Ansicht Workspace-Verwendung und Informationen
- **Konfigurieren** - ermöglicht den Zugriff auf den Arbeitsbereich verwalten
- **Webdienste** - können Sie WebServices verwalten, die aus diesem Arbeitsbereich veröffentlicht wurden

## <a name="to-monitor-how-the-workspace-is-being-used"></a>Überwachen, wie der Arbeitsbereich verwendet wird

Klicken Sie auf die Registerkarte **DASHBOARD** .

Dashboard können Gesamtauslastung des Arbeitsbereichs anzeigen und Blick Workspace-Informationen abrufen.

- Die **COMPUTE** Diagramm verwendeten Arbeitsbereich Serverressourcen. Sie können die Anzeige von relativen oder absoluten Werten, und den im Diagramm angezeigten Zeitrahmen ändern.
- **Übersicht über die Verwendung** zeigt Azure-Speicher von Arbeitsbereich verwendet wird.
- **Blick** bietet eine Zusammenfassung der Informationen des Arbeitsbereichs und nützliche Links.

> [AZURE.NOTE] **Zeichen in ML Studio** Link öffnet Machine Learning Studio mit Microsoft Account Sie derzeit angemeldet sind. Microsoft Account klassischen Azure-Portal anmelden, um einen Arbeitsbereich erstellen verwendeten muss nicht automatisch über die Berechtigung zum Öffnen dieses Arbeitsbereichs. Um einen Arbeitsbereich zu öffnen, müssen Sie angemeldet sein Microsoft Account, der als Besitzer des Arbeitsbereichs definiert wurde, oder vom Besitzer an den Arbeitsbereich eingeladen werden sollen.


## <a name="to-grant-or-suspend-access-for-users"></a>Gewähren oder Sperren für Benutzer ##

Klicken Sie auf die Registerkarte **Konfigurieren** .

In der Konfigurationsregisterkarte können Sie folgende Aktionen ausführen:

- Den Zugang zum Arbeitsbereich Computerlernen durch Klicken auf verweigern. Benutzer werden nicht mehr in Machine Learning Studio Arbeitsbereich öffnen. Klicken Sie auf zulassen, um den Zugriff wiederherzustellen.

Um weitere Konten verwalten, die Zugriff auf den Arbeitsbereich in Machine Learning Studio, klicken Sie auf **Sign in ML Studio** Registerkarte **DASHBOARD** (Siehe vorausgehenden Hinweis zu **ML Studio anmelden**). Den Arbeitsbereich im Machine Learning Studio geöffnet. Klicken Sie hier auf die **Registerkarte** und dann **Benutzer**. Sie können **Weitere Benutzer EINLADEN** , um Benutzern Zugriff auf den Arbeitsbereich oder einen Benutzer auswählen und auf **Entfernen**klicken.


## <a name="to-manage-web-services-in-this-workspace"></a>Webdienste in diesem Arbeitsbereich verwalten

Klicken Sie auf die Registerkarte **WEB SERVICES** .

Eine Liste von Webdiensten aus diesem Arbeitsbereich veröffentlicht.
Um einen Webdienst zu verwalten, klicken Sie in der Liste Dienst Webseite geöffnet.

Ein Webdienst möglicherweise eine oder mehrere Endpunkte definiert.

- Sie können mehrere Endpunkte neben den Endpunkt "Default" definieren. Klicken Sie den Endpunkt **Endpunkte verwalten** unten Dashboard das Azure Machine Learning Web Portal öffnen.

- So löschen Sie einen Endpunkt (Sie können nicht "Standard" Endpunkt löschen), klicken Sie auf das Kontrollkästchen am Anfang der Endpunktzeile, und klicken Sie auf **Löschen**. Dies entfernt den Endpunkt vom Webdienst.

    > [AZURE.NOTE] Wenn eine Anwendung den Webdienst-Endpunkt verwendet, wenn der Endpunkt gelöscht wird, wird die Anwendung das nächste Mal Fehlermeldung auf den Dienst zuzugreifen versucht.

Klicken Sie auf einen Webdienst-Endpunkt zu öffnen. 

Dashboard Gesamtauslastung des Webdienstes über einen Zeitraum angezeigt. Sie können den Zeitraum Zeitraum Dropdown-Menü oben rechts Verwendung Diagramme anzeigen auswählen. Das Dashboard zeigt folgende Informationen an:

- **Anfragen im Zeitverlauf** Graphen Schritt für die Anzahl der Anfragen innerhalb des ausgewählten Zeitraums. Damit können bestimmen, ob Spitzen in Verwendung befinden.
- **Anforderungsantwort Anfragen** zeigt die Gesamtzahl der Anforderung / Antwort-Aufrufe, die der Dienst über den angegebenen Zeitraum und wie viele Fehler erhalten hat.
- **Durchschnittliche** zeigt Anforderungsantwort berechnen durchschnittlich empfangenen Anfragen Ausführen erforderliche Zeit.
- **Batchanforderungen** zeigt insgesamt Batchanforderungen der Dienst über den angegebenen Zeitraum und wie viele Fehler erhalten hat.
- **Durchschnittliche Wartezeit für Auftrag** zeigt durchschnittlich empfangenen Anfragen Ausführen erforderliche Zeit.
- **Fehler** zeigt die Gesamtanzahl der aufgetretenen Aufrufe an den Webdienst.
- **Kosten** werden Gebühren für den Dienst zugeordneten Fakturierungsplan angezeigt.

Auf der Seite konfigurieren können Sie die folgenden Eigenschaften aktualisieren:

* **Beschreibung** können Sie eine Beschreibung für den Webdienst eingeben. Beschreibung ist ein obligatorisches Feld.
* **Protokollierung** ermöglicht das Aktivieren oder Deaktivieren der Protokollierung auf den Endpunkt Fehler. Weitere Informationen zur Protokollierung finden Sie unter aktivieren [für maschinelles lernen WebServices anmelden](machine-learning-web-services-logging.md).
* **Aktivieren Sie Beispieldaten** können Sie Beispieldaten zu bieten, die den Anforderung / Antwort-Dienst getestet. Wenn Sie den Webdienst in Machine Learning Studio erstellt, die Beispieldaten aus den Daten der mit Ihrem Modell stammt. Der Dienst programmgesteuert erstellt werden die Daten der Beispieldaten entnommen, die als Teil des JSON-Pakets bereitgestellt.

[consume]: machine-learning-consume-web-services.md
[marketplace]: machine-learning-publish-web-service-to-azure-marketplace.md
