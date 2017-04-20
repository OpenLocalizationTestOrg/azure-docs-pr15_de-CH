<properties
    pageTitle="Verwalten ein Webdienstes mit Azure Machine Learning Web Service Portal | Microsoft Azure"
    description="Zugriff auf Azure Machine Learning Arbeitsbereiche verwalten und bereitstellen und Verwalten von ML API-Webdiensten"
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="v-donglo"/>


# <a name="manage-a-web-service-using-the-azure-machine-learning-web-services-portal"></a>Verwalten Sie einen Webdienst mithilfe von Azure Machine Learning Web Services-portal

Sie können Computer lernen neue und klassische Webdienste mit Microsoft Azure Machine Learning Web Services-Portal verwalten. Da klassische und neue Web-Webdienste andere zugrunde liegende Technologie basieren, können Sie für jeden etwas andere Verwaltungsfunktionen.

Im Computer lernen Web-Portal können Sie:

- Überwachen Sie, wie der Webdienst verwendet wird.
- Konfigurieren Sie die Beschreibung, den Schlüssel aktualisieren für das Web service (nur neu), aktualisieren Ihre Storage-Konto Schlüssel (nur neu) Protokollierung aktivieren, und aktivieren oder Deaktivieren von Beispieldaten.
- Löschen Sie den Webdienst.
- Erstellen, löschen oder aktualisieren Abrechnung plant (nur neu).
- Hinzufügen und Löschen von Endpunkten (nur Standardansicht)

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="manage-new-web-services"></a>Neue Web Services verwalten

So verwalten Sie Ihre neue Web services

1.  Melden Sie sich mit Ihrem Microsoft Azure-Konto [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/quickstart) -Portal - verwenden Sie das Konto der Azure-Abonnement zugeordnet ist.
2.  Klicken Sie im Menü auf **Webdienste**.

Dies zeigt eine Liste der bereitgestellten Webdienste für Ihr Abonnement. 

Um einen Webdienst zu verwalten, klicken Sie auf Webdienste. Der Web-Seite können Sie:

- Klicken Sie auf den Webdienst verwalten.
- Klicken Sie auf der Fakturierungsplan für den Webdienst zu aktualisieren.
- Löschen Sie einen Webdienst.
- Einen Webdienst zu kopieren und in eine andere Region bereitstellen.

Wenn Sie einen Webdienst klicken, die Webseite Service Schnellstart. Der Schnellstart-Webdienstseite hat zwei Optionen, mit denen Sie Ihren Webdienst zu:

- **DASHBOARD** - können Sie Web Service-Nutzung anzeigen.
- **Konfigurieren** - können Sie beschreibenden Text hinzufügen, aktualisieren Sie den Schlüssel für das Speicherkonto den Webdienst zugeordnet und aktivieren oder Deaktivieren von Beispieldaten.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Überwachen der Verwendung des Webdiensts ###

Klicken Sie auf die Registerkarte **DASHBOARD** .

Dashboard Gesamtauslastung des Webdienstes über einen Zeitraum angezeigt. Sie können den Zeitraum Zeitraum Dropdown-Menü oben rechts Verwendung Diagramme anzeigen auswählen. Das Dashboard zeigt folgende Informationen an:

- **Anfragen im Zeitverlauf** Graphen Schritt für die Anzahl der Anfragen innerhalb des ausgewählten Zeitraums. Damit können bestimmen, ob Spitzen in Verwendung befinden.
- **Anforderungsantwort Anfragen** zeigt die Gesamtzahl der Anforderung / Antwort-Aufrufe, die der Dienst über den angegebenen Zeitraum und wie viele Fehler erhalten hat.
- **Durchschnittliche** zeigt Anforderungsantwort berechnen durchschnittlich empfangenen Anfragen Ausführen erforderliche Zeit.
- **Batchanforderungen** zeigt insgesamt Batchanforderungen der Dienst über den angegebenen Zeitraum und wie viele Fehler erhalten hat.
- **Durchschnittliche Wartezeit für Auftrag** zeigt durchschnittlich empfangenen Anfragen Ausführen erforderliche Zeit.
- **Fehler** zeigt die Gesamtanzahl der aufgetretenen Aufrufe an den Webdienst.
- **Kosten** werden Gebühren für den Dienst zugeordneten Fakturierungsplan angezeigt.

### <a name="configuring-the-web-service"></a>Konfigurieren des Webdiensts ###

Klicken Sie auf die Menüoption **Konfigurieren** .

Sie können die folgenden Eigenschaften aktualisieren:

* **Beschreibung** können Sie eine Beschreibung für den Webdienst eingeben.
* **Titel** können Sie einen Titel für den Webdienst
* **Schlüssel** können Sie die primären und sekundären API-Schlüssel zu drehen.
* **Speicherkontoschlüssel** können Sie den Schlüssel für das Speicherkonto zugeordneten des Webdiensts geändert aktualisieren. 
* **Aktivieren Sie Beispieldaten** können Sie Beispieldaten zu bieten, die den Anforderung / Antwort-Dienst getestet. Wenn Sie den Webdienst in Machine Learning Studio erstellt, die Beispieldaten aus den Daten der mit Ihrem Modell stammt. Der Dienst programmgesteuert erstellt werden die Daten der Beispieldaten entnommen, die als Teil des JSON-Pakets bereitgestellt.

### <a name="managing-billing-plans"></a>Rechnungsadresse Pläne verwalten ###

Klicken Sie auf die Menüoption **Pläne** auf Quickstart Services. Klicken Sie auf Plan bestimmten Webdienst zugeordnet Plans verwalten.

* **Neu** können Sie einen neuen Plan erstellen.
* **Software-Plan Instanz** können Sie einen vorhandenen Plan Kapazität "Skalieren".
* **Upgrade/DownGrade** können Sie "einen vorhandenen Plan Kapazität skalieren".
* **Löschen** können Sie einen Plan löschen.

Klicken Sie auf einen Plan zum Anzeigen der Dashboards. Das Dashboard bietet Snapshot oder Plan Verwendung über einen ausgewählten Zeitraum. Klicken Sie auf Dropdown **Punkt** oben rechts Dashboard, wählen Sie den Zeitraum an. 

Plan-Schaltpult enthält die folgenden Informationen:

* **Beschreibung** zeigt Informationen über Kosten und Kapazitäten der Plan zugeordnet.
* **Plan Verwendung** zeigt die Anzahl der Transaktionen und Computingstunden gegen den Plan berechnet.
* **Web Services** zeigt die Anzahl der Webdienste, die Verwendung dieses Plans.
* **Anfang von Webdienstaufrufen** zeigt den vier Web aufrufen, die mit dem Plan erhoben werden.
* **Top-Webdienste Uhr berechnen** zeigt vier Webdienste mit computeressourcen, die den Plan angerechnet.

## <a name="manage-classic-web-services"></a>Standardansicht WebServices verwalten

> [AZURE.NOTE] Die Verfahren in diesem Abschnitt sind für Classic-Webdienste über das Azure Machine Learning Web Portal verwalten. Informationen zum Verwalten von klassischen Webdienste über Machine Learning Studio und klassischen Azure-Portal finden Sie unter [Manage Azure Machine Learning-Arbeitsbereich](machine-learning-manage-workspace.md).

Die klassische Webdienste verwalten:

1.  Melden Sie sich mit Ihrem Microsoft Azure-Konto [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/quickstart) -Portal - verwenden Sie das Konto der Azure-Abonnement zugeordnet ist.
2.  Klicken Sie im Menü auf **Klassische Webdienste**.

Um einen klassischen Webdienst zu verwalten, klicken Sie auf **Klassische Webdienste**. Der klassische Web-Seite können Sie:

- Klicken Sie auf den Webdienst zugeordneten Endpunkte anzeigen.
- Löschen Sie einen Webdienst.

Beim Verwalten eines klassischen Webdienstes verwalten Sie einzelnen Endpunkte getrennt. Wenn Sie einen Webdienst in der Web-Seite klicken, öffnet die Liste der dem Dienst zugeordnete Endpunkte. 

Auf der Seite klassische Webdienst-Endpunkt können hinzufügen und Löschen von Endpunkten für den Dienst. Weitere Informationen zum Hinzufügen von Endpunkten finden Sie [Endpunkte erstellen](machine-learning-create-endpoint.md).

Klicken Sie auf einen der Endpunkte Service Schnellstart Webseite öffnen. Auf der Seite "Schnellstart" sind zwei Optionen, mit denen Sie Ihren Webdienst zu verwalten:

- **DASHBOARD** - können Sie Web Service-Nutzung anzeigen.
- **Konfigurieren** - können Sie beschreibenden Text hinzufügen, aktivieren und Deaktivieren der Schlüssel für den Speicher berücksichtigt den Webdienst zugeordnet und aktivieren und Deaktivieren von Beispieldaten Aktualisieren von Fehler protokollieren.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Überwachen der Verwendung des Webdiensts ###

Klicken Sie auf die Registerkarte **DASHBOARD** .

Dashboard Gesamtauslastung des Webdienstes über einen Zeitraum angezeigt. Sie können den Zeitraum Zeitraum Dropdown-Menü oben rechts Verwendung Diagramme anzeigen auswählen. Das Dashboard zeigt folgende Informationen an:

- **Anfragen im Zeitverlauf** Graphen Schritt für die Anzahl der Anfragen innerhalb des ausgewählten Zeitraums. Damit können bestimmen, ob Spitzen in Verwendung befinden.
- **Anforderungsantwort Anfragen** zeigt die Gesamtzahl der Anforderung / Antwort-Aufrufe, die der Dienst über den angegebenen Zeitraum und wie viele Fehler erhalten hat.
- **Durchschnittliche** zeigt Anforderungsantwort berechnen durchschnittlich empfangenen Anfragen Ausführen erforderliche Zeit.
- **Batchanforderungen** zeigt insgesamt Batchanforderungen der Dienst über den angegebenen Zeitraum und wie viele Fehler erhalten hat.
- **Durchschnittliche Wartezeit für Auftrag** zeigt durchschnittlich empfangenen Anfragen Ausführen erforderliche Zeit.
- **Fehler** zeigt die Gesamtanzahl der aufgetretenen Aufrufe an den Webdienst.
- **Kosten** werden Gebühren für den Dienst zugeordneten Fakturierungsplan angezeigt.

### <a name="configuring-the-web-service"></a>Konfigurieren des Webdiensts ###

Klicken Sie auf die Menüoption **Konfigurieren** .

Sie können die folgenden Eigenschaften aktualisieren:

* **Beschreibung** können Sie eine Beschreibung für den Webdienst eingeben. Beschreibung ist ein obligatorisches Feld.
* **Protokollierung** ermöglicht das Aktivieren oder Deaktivieren der Protokollierung auf den Endpunkt Fehler. Weitere Informationen zur Protokollierung finden Sie unter aktivieren [für maschinelles lernen WebServices anmelden](machine-learning-web-services-logging.md).
* **Aktivieren Sie Beispieldaten** können Sie Beispieldaten zu bieten, die den Anforderung / Antwort-Dienst getestet. Wenn Sie den Webdienst in Machine Learning Studio erstellt, die Beispieldaten aus den Daten der mit Ihrem Modell stammt. Der Dienst programmgesteuert erstellt werden die Daten der Beispieldaten entnommen, die als Teil des JSON-Pakets bereitgestellt.

## <a name="grant-or-suspend-access-to-web-services-for-users-in-the-portal"></a>Erteilen oder Zugriff auf Webdienste für Benutzer im Portal anhalten

Klassische Azure-Portal können Sie zulassen oder Verweigern des Zugriffs auf bestimmte Benutzer.

### <a name="access-for-users-of-new-web-services"></a>Für Benutzer der neuen Web services

Um andere Benutzer Ihre Webdienste im Azure Machine Learning Web Portal zu aktivieren, müssen Sie sie als co-Administratoren Ihre Azure-Abonnements hinzufügen.

Melden Sie sich mit Ihrem Microsoft Azure-Konto [Azure-Verwaltungsportal](https://manage.windowsazure.com/) - verwenden Sie das Konto der Azure-Abonnement zugeordnet ist.

1. Klicken Sie im Navigationsbereich **Klicken Sie**und klicken Sie auf **Administratoren**.
2. Klicken Sie am unteren Rand des Fensters auf **Hinzufügen**. 
3. Geben Sie im Dialogfeld ADD A CO-ADMINISTRATOR der e-Mail-adressedes der Person ein, die Sie als Co-Administrator hinzufügen, und wählen Sie dann das Abonnement Co-Administrator auf.
4. Klicken Sie auf **Speichern**.

### <a name="access-for-users-of-classic-web-services"></a>Für Benutzer von Classic-Webdiensten

So verwalten Sie einen Arbeitsbereich

Melden Sie sich mit Ihrem Microsoft Azure-Konto [Azure-Verwaltungsportal](https://manage.windowsazure.com/) - verwenden Sie das Konto der Azure-Abonnement zugeordnet ist.

1. Klicken Sie im Bereich Dienste Microsoft Azure **MASCHINELLES lernen**.
1. Klicken Sie auf den Arbeitsbereich, den Sie verwalten möchten.
1. Klicken Sie auf die Registerkarte **Konfigurieren** .

In der Konfigurationsregisterkarte können Sie durch Klicken auf **Verweigern**Zugriff auf den Computerlernen Arbeitsbereich anhalten. Benutzer werden nicht mehr in Machine Learning Studio Arbeitsbereich öffnen. Klicken Sie auf **Zulassen**, um den Zugriff wiederherzustellen.

Bestimmten Benutzern:

Um weitere Konten verwalten, die Zugriff auf den Arbeitsbereich in Machine Learning Studio, klicken Sie auf **Sign in ML Studio** **DASHBOARD** -Registerkarte. Den Arbeitsbereich im Machine Learning Studio geöffnet. Klicken Sie hier auf die **Registerkarte** und dann **Benutzer**. Sie können **Weitere Benutzer EINLADEN** , um Benutzern Zugriff auf den Arbeitsbereich oder einen Benutzer auswählen und auf **Entfernen**klicken.

> [AZURE.NOTE] **Zeichen in ML Studio** Link öffnet Machine Learning Studio mit Microsoft Account Sie derzeit angemeldet sind. Microsoft Account klassischen Azure-Portal anmelden, um einen Arbeitsbereich erstellen verwendeten muss nicht automatisch über die Berechtigung zum Öffnen dieses Arbeitsbereichs. Um einen Arbeitsbereich zu öffnen, müssen Sie angemeldet sein Microsoft Account, der als Besitzer des Arbeitsbereichs definiert wurde, oder vom Besitzer an den Arbeitsbereich eingeladen werden sollen.
