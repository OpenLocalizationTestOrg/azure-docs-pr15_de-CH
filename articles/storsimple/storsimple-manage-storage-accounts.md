<properties 
   pageTitle="Verwalten Ihres Speicherkontos StorSimple | Microsoft Azure"
   description="Erläutert, wie Sie mithilfe der StorSimple-Manager konfigurieren-Seite hinzufügen, bearbeiten, löschen oder Rotieren von Sicherheitsschlüsseln für ein Speicherkonto."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/29/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-your-storage-account"></a>Verwenden des StorSimple Manager-Dienstes das Speicherkonto verwalten

## <a name="overview"></a>Übersicht

Seite **Konfigurieren** zeigt die globalen Dienst-Parameter, die in der StorSimple Manager-Dienst erstellt werden können. Diese Parameter gelten für alle Geräte mit dem Dienst verbundenen und umfassen:

- Speicherkonten 
- Bandbreitenvorlagen 
- Access Control-Datensätze 

Dieses Tutorial erläutert die Verwendung der Seite **Konfigurieren** hinzufügen, bearbeiten oder Löschen von Speicherkonten oder Rotieren von Sicherheitsschlüsseln für ein Speicherkonto.

 ![Seite](./media/storsimple-manage-storage-accounts/HCS_ConfigureService.png)  

Speicherkonten enthalten die Anmeldeinformationen, die das Gerät verwendet das Speicherkonto Ihren Cloud-Anbieter auf. Microsoft Azure-Speicherkonten sind für Anmeldeinformationen wie den Kontonamen und die primäre Taste. 

Auf der Seite **Konfigurieren** werden alle für die Abrechnung Abonnement erstellt Speicherkonten in Tabellenform mit den folgenden Informationen angezeigt:

- **Name** – der eindeutige Name für das Konto bei der Erstellung zugewiesen.
- **SSL aktiviert** – ob der SSL aktiviert ist und Gerät Cloud-Kommunikation über den sicheren Kanal.
- **Verwendet von** – die Anzahl der Datenträger mit dem Speicher an.

Sind die häufigsten Aufgaben für Speicherkonten auf der Seite **Konfigurieren** ausgeführt werden können:

- Ein Speicherkonto hinzufügen 
- Ein Speicherkonto bearbeiten 
- Ein Speicherkonto löschen 
- Schlüsselrotation Speicherkonten 

## <a name="types-of-storage-accounts"></a>Arten von Speicherkonten

Es gibt drei Arten von Speicher-Konten mit dem StorSimple-Gerät.

- **Automatisch generierte Speicherkonten** – wie der Name schon sagt, wird Speicher Kontoart automatisch generiert, wenn der Dienst zuerst erstellt wird. Hier erfahren Sie, wie diese Speicherkonto erstellt wird, finden Sie unter [Schritt 1: erstellen ein neues Dienstes](storsimple-deployment-walkthrough-u1.md#step-1-create-a-new-service) in [Ihrem lokalen StorSimple-Gerät bereitstellen](storsimple-deployment-walkthrough.md). 
- **Speicherkonten Dauerauftrags** – Azure-Speicherkonten dieselbe Abonnement, den Dienst zugeordnet sind. Erfahren Sie mehr über deren Speicher Konten erstellt werden, finden Sie unter [Über Azure-Speicherkonten](../storage/storage-create-storage-account.md). 
- **Speicherkonten außerhalb der Dauerauftrag** – Azure-Speicher-Konten, die nicht mit dem Dienst verbunden und wahrscheinlich vor der Dienst vorhanden sind.

## <a name="add-a-storage-account"></a>Ein Speicherkonto hinzufügen

Ein Speicherkonto können durch einen eindeutigen Namen und Zugriffsinformationen Speicherkonto (mit angegebenen Cloud Service Provider) verbunden sind. Sie können auch secure Sockets Layer (SSL) Modus aktivieren, erstellen Sie einen sicheren Kanal für die Kommunikation zwischen dem Gerät und der Cloud.

Sie können mehrere Konten für einen bestimmten Cloud-Dienstanbieter erstellen. Beachten Sie jedoch, dass nach dem Erstellen ein Speicherkonto Cloud-Dienstanbieter ändern können.

Während das Speicherkonto gespeichert wird, versucht der Dienst Ihren Cloud-Anbieter kommunizieren. Die Anmeldeinformationen und Access-Material, das angegebene authentifiziert zu diesem Zeitpunkt. Ein Speicherkonto wird nur erstellt, wenn die Authentifizierung erfolgreich war. Wenn die Authentifizierung fehlschlägt, wird eine entsprechende Fehlermeldung angezeigt.

Ressourcenmanager Speicherkonten in Azure-Portal erstellt werden auch von StorSimple unterstützt. Ressourcenmanager Speicherkonten erscheint nicht in der Dropdown-Liste zur Auswahl beim Versuch einen volumecontainer nur erstellten Speicherkonten im klassischen Azure-Portal erstellen angezeigt. Speicherkonten Ressourcenmanager müssen mithilfe der Prozedur ein Speicherkonto beschrieben hinzufügen hinzugefügt werden.

> [AZURE.NOTE] Das Verfahren zum Hinzufügen ein Speicherkonto hängt StorSimple Software-Version, die Sie verwenden. Müssen Sie die richtigen Schritte für Ihre Version von StorSimple.


[AZURE.INCLUDE [add-a-storage-account-update1](../../includes/storsimple-configure-new-storage-account-u1.md)]

[AZURE.INCLUDE [add-a-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Ein Speicherkonto bearbeiten

Sie können ein Speicherkonto, die von einem volumecontainer verwendet wird. Wenn Sie ein Speicherkonto, die derzeit verwendet wird bearbeiten, ist das einzige Feld geändert die Zugriffstaste für das Speicherkonto. Sie können neue Speicher Zugriffstaste angeben und aktualisierte speichern.

#### <a name="to-edit-a-storage-account"></a>So bearbeiten Sie ein Speicherkonto

1. Zielseite Service wählen Sie den Dienst, doppelklicken Sie auf den Namen, und klicken Sie auf **Konfigurieren**.

2. Klicken Sie auf **Storage-Konten hinzufügen oder bearbeiten**.

3. Im Dialogfeld **Speicherkonten hinzufügen/bearbeiten** :

  1. Wählen Sie in der Dropdown-Liste der **Speicherkonten**ein Konto, das Sie ändern möchten. Dazu zählen auch die Speicherkonten, die automatisch generiert wurden, wenn der Dienst erstellt wurde.
  2. Bei Bedarf können Sie **SSL-Modus aktivieren** Auswahl ändern.
  3. Sie können Ihr Konto Zugriff Speicherschlüssel drehen. Weitere Informationen zum Ausführen der Schlüsselrotation finden Sie unter [Schlüsselrotation Speicherkonten](#key-rotation-of-storage-accounts) .
  4. Klicken Sie auf Check ![Symbol überprüfen](./media/storsimple-manage-storage-accounts/HCS_CheckIcon.png) um die Einstellung zu speichern. Die Einstellung werden auf der Seite **Konfigurieren** aktualisiert. Klicken Sie auf **Speichern** , um die neue Einstellung zu speichern.

     ![Ein Speicherkonto bearbeiten](./media/storsimple-manage-storage-accounts/HCs_AddEditStorageAccount.png)
  
## <a name="delete-a-storage-account"></a>Ein Speicherkonto löschen

> [AZURE.IMPORTANT] Sie können ein Speicherkonto, nur, wenn sie nicht von einem volumecontainer verwendet wird. Wenn ein Speicherkonto von einem volumecontainer verwendet wird, zuerst löschen Sie volumecontainer und anschließend zugeordnete Speicherkonto.

#### <a name="to-delete-a-storage-account"></a>So löschen Sie ein Speicherkonto

1. Wählen Sie auf der Zielseite StorSimple Manager Service den Dienst, doppelklicken Sie auf den Namen und klicken Sie dann auf **Konfigurieren**.

2. Zeigen Sie in tabellarischen Übersicht Speicherkonten auf das Konto, das Sie löschen möchten.

3. Extreme rechts für das Storage-Konto wird ein Symbol "löschen" (**X**) angezeigt. Klicken Sie auf **X** , um die Anmeldeinformationen zu löschen.

4. Zur Bestätigung aufgefordert werden, klicken Sie auf **Ja,** um den Löschvorgang fortzusetzen. Tabellarische Liste wird entsprechend aktualisiert.

## <a name="key-rotation-of-storage-accounts"></a>Schlüsselrotation Speicherkonten

Aus Sicherheitsgründen ist die Schlüsselrotation häufig in Rechenzentren erforderlich. 

> [AZURE.NOTE] Microsoft Azure-Speicherkonten gelten folgende Schlüsselrotation und Drehung Verfahren. Wenn Sie eine andere Cloud-Anbieter verwenden, können Sie speicherkontoschlüssel des Anbieters Dashboard verwalten.
 
Jedes Microsoft Azure-Abonnement kann ein oder mehrere zugeordnete Speicherkonten verfügen. Abonnement und Access Schlüssel für jedes Storage-Konto der Zugriff auf diese Konten gesteuert. 

Wenn Sie ein Speicherkonto erstellen, generiert Microsoft Azure zwei 512-Bit-Speicher Zugriffstasten, die für die Authentifizierung verwendet werden, wenn das Speicherkonto zugegriffen wird. Mit zwei Speicher-Zugriffstasten können Sie Schlüssel ohne Unterbrechung der Speicherdienst oder diesen Dienst neu generieren. Der Schlüssel, der gerade verwendet wird, ist *der Primärschlüssel* und der Sicherungsschlüssel als *sekundären* Schlüssel bezeichnet. Eine der beiden Tasten muss angegeben werden, wenn Ihr Microsoft Azure StorSimple-Gerät Ihrem Cloud Speicher zugreift.

## <a name="what-is-key-rotation"></a>Was ist die Schlüsselrotation?

Normalerweise verwenden Sie Applikationen eine der Tasten auf Ihre Daten zugreifen. Nach einer gewissen Zeit haben Sie Ihre Anwendung mit den zweiten Schlüssel wechseln. Nachdem Sie Ihre Anwendung auf den sekundären Schlüssel gewechselt haben, können Sie zurückziehen den ersten Schlüssel und einen neuen Schlüssel generieren. Mithilfe zweier Schlüssel auf diese Weise ermöglicht den Anwendungszugriff auf die Daten ohne Ausfallzeiten.

Die speicherkontoschlüssel werden immer im Dienst in verschlüsselter Form gespeichert. Jedoch können diese über den Dienst StorSimple Manager zurückgesetzt. Der Dienst erhalten den Primärschlüssel und den sekundären Schlüssel für alle Speicherkonten im gleichen Abonnement einschließlich Storage Service sowie der Standardspeicher erstellten Konten generiert, wenn der Dienst StorSimple Manager wurde erstellt. StorSimple Manager-Dienst wird immer diese Schlüssel von Azure-Verwaltungsportal und dann verschlüsselt speichern.

## <a name="rotation-workflow"></a>Drehung workflow

Microsoft Azure-Administrator kann Regenerieren oder die primäre oder sekundäre Taste direkt auf das Speicherkonto (über Microsoft Azure Storage Service) ändern. Der StorSimple Manager-Dienst wird diese Änderung nicht automatisch angezeigt.

StorSimple Manager-Dienst über die Änderung informieren Sie StorSimple Manager-Dienst Zugriff auf Zugriff auf das Speicherkonto, und synchronisieren Sie die primäre oder sekundäre Taste (je nachdem, welche geändert wurde). Der Dienst Ruft den neuesten Schlüssel, Schlüssel verschlüsselt, und den verschlüsselten Schlüssel an das Gerät sendet.

#### <a name="to-synchronize-keys-for-storage-accounts-in-the-same-subscription-as-the-service-azure-only"></a>Schlüssel für Speicherkonten in dieselbe Abonnement als Service (nur Azure) synchronisieren

1. Klicken Sie auf der Seite **Dienste** auf die Registerkarte **Konfigurieren** .

2. Klicken Sie auf **Storage-Konten hinzufügen oder bearbeiten**.

3. Klicken Sie im Dialogfeld folgendermaßen Sie vor:

  1. Wählen Sie das Speicherkonto mit dem Schlüssel, den Sie synchronisieren möchten. Konto Speicherschlüssel werden verschlüsselt, wenn sie angezeigt werden.
  2. In der StorSimple Manager-Dienst müssen Sie den Schlüssel aktualisieren, der zuvor in Microsoft Azure Storage-Diensts geändert wurde. Wenn die primäre Taste (regeneriert) geändert wurde, klicken Sie auf **Primärschlüssel zu synchronisieren**. Wenn der sekundäre Schlüssel geändert wurde, klicken Sie auf **Synchronisieren Sekundärschlüssel**.

    ![Synchronisieren von Schlüsseln](./media/storsimple-manage-storage-accounts/HCS_KeyRotationStorageAccountSameSubscriptionAsService.png)

#### <a name="to-synchronize-keys-for-storage-accounts-outside-of-the-service-subscription"></a>Schlüssel für Speicherkonten außerhalb der Abonnement synchronisieren

1. Klicken Sie auf der Seite **Dienste** auf die Registerkarte **Konfigurieren** .

2. Klicken Sie auf **Storage-Konten hinzufügen oder bearbeiten**.

3. Klicken Sie im Dialogfeld folgendermaßen Sie vor:

  1. Wählen Sie das Speicherkonto mit Tastenkombination, die Sie aktualisieren möchten.
  2. Sie müssen die Speicher Zugriffstaste StorSimple Manager-Dienst aktualisieren. In diesem Fall sehen Sie die Zugriffstaste Speicher. Geben Sie den neuen Schlüssel im y **Speicherschlüssel Konto Zugriff**. 
  3. Speichern. Die Zugriffstaste Storage-Konto sollte jetzt aktualisiert werden.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [StorSimple Sicherheit](storsimple-security.md).
- Weitere Informationen zur [Verwendung des StorSimple Manager-Dienstes zum Verwalten von StorSimple-Gerät](storsimple-manager-service-administration.md)
