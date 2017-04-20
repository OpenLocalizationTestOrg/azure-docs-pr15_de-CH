<properties
    pageTitle="Veröffentlichen eine Anwendung in Azure RemoteApp | Microsoft Azure"
    description="Informationen Sie zum Vorgang in Azure RemoteApp veröffentlichen."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-to-publish-an-app-in-remoteapp"></a>Veröffentlichen Sie eine Anwendung in RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Nach dem Erstellen der Auflistung RemoteApp müssen Sie apps oder Ressourcen für Benutzer zu veröffentlichen. Die Vorlage Bilder mit Ihrem Abonnement nur wenige apps standardmäßig - Freigeben von anderen apps veröffentlicht haben, müssen Sie veröffentlichen.

> [AZURE.NOTE] Müssen Sie eine Anwendung aktualisieren? Sie müssen aktualisieren [das Bild](remoteapp-update.md) zuerst.

Klicken Sie auf der Registerkarte **Veröffentlichen** im Portal **Veröffentlichen**. Hinzufügen eine Anwendung aus dem **Startmenü der Vorlagenbild** oder den Pfad an, auf dem die Anwendung installiert ist. Wählen Sie aus **dem Startmenü** hinzugefügt werden soll app aus der Liste veröffentlichen. Möchten Sie den Pfad für die Anwendung anzugeben, geben Sie einen Namen für die Anwendung und den Pfad zur app. Verwenden von Variablen im Pfad - beispielsweise "% SystemDrive%" anstelle von "c:\".

> [AZURE.NOTE] Wenn Sie Ihre app aus **dem Startmenü** hinzufügen möchten, Sie müssen *hinzugefügt, app die * *Start* * Menü auf Ihrem Vorlagenbild.* Andernfalls RemoteApp nur sehen was *wird* im **Startmenü** und Sie werden verwirrt. 

>Um sicherzustellen, dass Ihre Anwendung im **Startmenü** ist, platzieren Sie eine Verknüpfungsdatei - **lnk** - in %systemdrive%\ProgramData\Microsoft\Windows\Start im Ordner.

> Wenn Sie vergessen, als die Vorlage erstellt die Anwendung zum **Startmenü** hinzufügen, wählen Sie den Pfad zu der Anwendung hinzufügen. (Oder erstellen Ihre Vorlagenbild aber ein bisschen mehr Arbeit.)


 