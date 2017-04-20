<properties 
   pageTitle="Bereitstellen den StorSimple Manager-Dienst für virtuelle StorSimple Array | Microsoft Azure"
   description="Erläutert das Erstellen und Löschen des StorSimple Manager-Dienstes im klassischen Azure-Portal und der Dienstregistrierungsschlüssel Verwaltung beschrieben."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/19/2016"
   ms.author="alkohli" />

# <a name="deploy-the-storsimple-manager-service-for-storsimple-virtual-array"></a>Bereitstellen des StorSimple Manager-Dienstes für virtuelle StorSimple

## <a name="overview"></a>Übersicht

StorSimple Manager-Dienst in Microsoft Azure ausgeführt wird und eine Verbindung mit mehreren StorSimple. Nach dem Erstellen des Diensts können es Geräte aus der Microsoft Azure-Verwaltungsportal in einem Browser ausgeführt. Dadurch werden alle Geräte überwachen, die mit der StorSimple Manager-Dienst von einem zentralen Speicherort, Verwaltungsaufwand Hauptausführungsthread verbunden sind.

Die Zielseite StorSimple Manager listet die StorSimple Manager-Dienste, mit denen Sie Ihr StorSimple-Speichergeräten verwalten. Für jeden Dienst StorSimple Manager stellt die folgende Informationen auf der Seite StorSimple Manager:

- **Name** – der Name der StorSimple Manager-Dienst bei der Erstellung zugewiesen wurde. Der Dienstname kann geändert werden, nachdem der Dienst erstellt wurde.

- **Status** – der Status des Dienstes **Active**, **Erstellen**oder **Online**.

- **Position** – Standort in der StorSimple-Gerät bereitgestellt wird.

- **Abonnement** -Abrechnung Abonnements Ihren Dienst zugeordnet ist.

Allgemeinen StorSimple Manager-Seite ausgeführten Aufgaben sind:

- Erstellen eines Dienstes
- Löschen eines Dienstes
- Den Dienst-Registrierungsschlüssel zu erhalten
- Regenerieren Sie den Dienst-Registrierungsschlüssel

In diesem Lernprogramm beschreibt, wie diese Aufgaben ausführen. Die Informationen in diesem Artikel gilt nur für virtuelle StorSimple-Arrays. Weitere StorSimple 8000-Serie werden Sie [StorSimple Manager Service bereitstellen](storsimple-manage-service.md).

## <a name="create-a-service"></a>Erstellen eines Dienstes

Verwenden Sie die Option **Firma** StorSimple Manager-Dienst zu erstellen, wenn Sie Ihr StorSimple Gerät bereitstellen möchten. Um einen Dienst zu erstellen, benötigen Sie:

- Ein Abonnement mit einer Konzernvertrag
- Ein aktives Microsoft Azure Storage-Konto
- Rechnungsinformationen, die für die Verwaltung verwendet wird

Sie können auch ein Standard-Speicherkonto generieren beim Erstellen des Dienstes.

Ein Dienst kann mehrere Geräte verwalten. Ein Gerät kann nicht jedoch mehrere Dienste umfassen. Großes Unternehmen können mehrere Dienstinstanzen mit verschiedenen Abonnements, Organisationen oder sogar Bereitstellungsstandorte arbeiten.  

> [AZURE.NOTE] Benötigen Sie separate Instanzen des StorSimple Manager-Dienst StorSimple 8000-Serie Geräte und virtuellen StorSimple-Arrays verwalten.

Führen Sie die folgenden Schritte aus, um einen Dienst zu erstellen.

[AZURE.INCLUDE [storsimple-ova-create-new-service](../../includes/storsimple-ova-create-new-service.md)]

## <a name="delete-a-service"></a>Löschen eines Dienstes

Vor dem Löschen eines Dienstes stellen Sie sicher, dass keine Geräte verwendet werden. Ist der Dienst verwendet, deaktivieren Sie die angeschlossenen Geräten. Vorgang deaktivieren trennt die Verbindung zwischen dem Gerät und der Dienst jedoch beibehalten die Daten in der Cloud. 

> [AZURE.IMPORTANT] Nachdem ein Dienst gelöscht wurde, kann der Vorgang storniert werden. 

Führen Sie die folgenden Schritte aus, um einen Dienst zu löschen.

### <a name="to-delete-a-service"></a>So löschen Sie einen Dienst

1. Wählen Sie auf der Seite **StorSimple Manager** den Dienst, den Sie löschen möchten.

1. Klicken Sie am unteren Rand der Seite **Löschen** .

1. Klicken Sie auf **Ja** , wenn die Benachrichtigung zur Bestätigung. Es dauert einige Minuten für den Dienst gelöscht werden.

## <a name="get-the-service-registration-key"></a>Den Dienst-Registrierungsschlüssel zu erhalten

Nachdem Sie einen Dienst erstellt haben, müssen Sie Ihr StorSimple-Gerät mit dem Dienst registrieren. Registrieren Sie Ihre erste StorSimple Gerät benötigen Sie Service-Registrierungsschlüssel. Um zusätzliche Geräte mit einem vorhandenen StorSimple-Dienst zu registrieren, benötigen Sie die Registrierungsschlüssel und der Verschlüsselungsschlüssel für Daten (die auf dem ersten Gerät während der Registrierung erzeugt wird). Weitere Informationen zu den Verschlüsselungsschlüssel Daten finden Sie unter [den Verschlüsselungsschlüssel Daten aus der lokalen Web-Benutzeroberfläche](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key). 

Führen Sie die folgenden Schritte der Dienstregistrierungsschlüssel.

[AZURE.INCLUDE [storsimple-ova-get-service-registration-key](../../includes/storsimple-ova-get-service-registration-key.md)]

Den Dienst-Registrierungsschlüssel an einem sicheren Ort aufbewahren Sie benötigen diesen Schlüssel als auch den Verschlüsselungsschlüssel für Daten, um zusätzliche Geräte bei diesem Dienst registriert. Nach Erhalt der Dienstregistrierungsschlüssel, müssen Sie das Gerät über Windows PowerShell StorSimple Schnittstelle konfigurieren.

## <a name="regenerate-the-service-registration-key"></a>Regenerieren Sie den Dienst-Registrierungsschlüssel

Sie müssen einen Dienst-Registrierungsschlüssel regenerieren, wenn Sie Schlüsselrotation ausführen oder die Liste der Dienstadministratoren. Wenn Sie den Schlüssel neu erstellen, neue Schlüssel dient nur nachfolgender Geräte registrieren. Geräte, die bereits registriert werden dadurch nicht beeinflusst.

Führen Sie die folgenden Schritte aus, um einen Dienst-Registrierungsschlüssel zu regenerieren.

### <a name="to-regenerate-the-service-registration-key"></a>Den Dienst-Registrierungsschlüssel neu erstellen

1. Klicken Sie auf der Seite **StorSimple Manager** auf **Registrierungsschlüssel**.

1. Klicken Sie im Dialogfeld **Dienstregistrierungsschlüssel** auf **Regenerieren**.

1. Sie sehen eine Meldung angezeigt. Klicken Sie auf **OK** , um die Regenerierung fortgesetzt.

1. Neue Service-Registrierungsschlüssel wird angezeigt.

1. Kopieren Sie diesen Schlüssel, und speichern sie für neue Geräte bei diesem Dienst registriert.

1. Klicken Sie auf das Häkchensymbol ![Häkchensymbol](./media/storsimple-ova-manage-service/image7.png) Um dieses Dialogfeld zu schließen.


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mit virtuellen StorSimple [beginnen](storsimple-ova-deploy1-portal-prep.md) .
    
- Erfahren Sie, wie [das StorSimple-Gerät verwalten](storsimple-ova-web-ui-admin.md).

 
