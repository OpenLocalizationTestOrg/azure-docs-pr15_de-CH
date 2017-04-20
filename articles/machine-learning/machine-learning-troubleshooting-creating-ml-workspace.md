<properties
    pageTitle="Beheben: Erstellen und Verbinden mit einem Arbeitsbereich Computerlernen | Microsoft Azure"
    description="Lösung für Probleme und Herstellen einer Verbindung mit einer Azure Machine Learning-Arbeitsbereich"
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
    ms.date="09/09/2016"
    ms.author="garye"/>


# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a>Fehlerbehebung: Erstellen und Verbinden mit einem Computer Learning-Arbeitsbereich

Dieses Handbuch bietet Lösungen für einige häufig Probleme beim Einrichten der Azure Machine Learning Arbeitsbereiche.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Besitzer des Arbeitsbereichs

Beim Erstellen eines neuen Arbeitsbereichs maschinelles lernen ID Geben Sie im Feld Besitzer des ARBEITSBEREICHS muss ein gültiges microsoftkonto (früher Windows Live ID), z. B. john-contoso@live.com oder john-contoso@hotmail.com. Es kann keine nicht-Microsoft-Konto, wie Ihr e-Mail-Konto sein. Um ein kostenloses microsoftkonto zu erstellen, gehen Sie zu [www.live.com](http://www.live.com).

Beachten Sie, dass den Arbeitsbereich erstellt der Azure-Portal anmelden verwendete Konto nicht automatisch Berechtigung zum *Öffnen* dieses Arbeitsbereichs, sofern Sie dieses Konto Besitzer. Zum Öffnen eines Arbeitsbereichs in Machine Learning Studio müssen Sie angemeldet sein Microsoft Account, der als Besitzer des Arbeitsbereichs definiert wurde, oder vom Besitzer an den Arbeitsbereich eingeladen werden sollen. Von klassischen Azure-Portal können Sie jedoch *Verwalten* im Arbeitsbereich, einschließlich der Möglichkeit zum Ändern und konfigurieren Sie den Zugriff.

Weitere Informationen zum Verwalten eines Arbeitsbereichs finden Sie unter [Manage Azure Machine Learning-Arbeitsbereich].

[Verwalten von Azure Machine Learning-Arbeitsbereich]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a>Zulässige Bereiche

Computerlernen ist derzeit in eine begrenzte Anzahl von Bereichen. Wenn Ihr Abonnement nicht einer dieser Regionen enthält, möglicherweise die Fehlermeldung angezeigt, "Sie haben keine Abonnements in zugelassenen Gebieten."

Anfordern eine Region zu Ihrem Abonnement hinzugefügt werden, wählen Sie **Kontakt Microsoft Support** von Azure-Verwaltungsportal **Abrechnung** als Problemtyp wählen Sie aus und folgen Sie Ihre Anfrage.

![Wenden Sie sich an Microsoft Support Services][screen1]

## <a name="storage-account"></a>Konto

Der Computerlernen Service benötigt ein Speicherkonto Datenspeicher. Ein Speicherkonto verwenden oder ein neue Speicherkonto erstellen, wenn Sie neuen Computer Learning-Arbeitsbereich erstellen (Kontingent ein neues Speicherkonto erstellt haben).

<!-- These instructions no longer work, but I'm not sure what to replace them with
To see if you can create a new storage account, in the Classic Portal, go to **Settings** and then click **Usage**.
-->

![Arbeitsbereich erstellen][screen2]

Nachdem der Computerlernen Arbeitsbereich erstellt wird, können Sie anmelden Machine Learning Studio mit Microsoft-Konto, das als Besitzer des Arbeitsbereichs angegeben. Die Fehlermeldung verwenden "Arbeitsbereich nicht gefunden" (ähnlich dem folgenden Screenshot) die folgenden Schritte Ihr Browsercookies löschen.

![Arbeitsbereich nicht gefunden][screen3]

**Browser-Cookies löschen**

Wenn Sie Internet Explorer verwenden, klicken Sie auf die Schaltfläche " **Extras** " in der oberen rechten Ecke, und wählen Sie **Internetoptionen**.  

![Internetoptionen][screen4]

Klicken Sie auf der Registerkarte **Allgemein** auf **löschen...**

![Registerkarte Allgemein][screen5]

Im Dialogfeld " **Browserverlauf löschen** " **Cookies und Websitedaten** ausgewählt ist, und klicken Sie auf **Löschen**.

![Cookies löschen][screen6]

Nach dem Löschen der Cookies starten Sie den Browser und wechseln Sie dann zur Seite [Microsoft Azure Machine Learning](https://studio.azureml.net) . Bei Aufforderung einen Benutzernamen und ein Kennwort Geben Sie das Microsoft-Konto, das als Besitzer des Arbeitsbereichs angegeben.

Unser Ziel ist problemlosen von Rechner Erfahrung. Bitte senden Sie Kommentare und Fragen im [Forum Azure maschinelles lernen](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) helfen Sie kontinuierlich verbessern.

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
