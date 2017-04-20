<properties 
   pageTitle="Verwalten Ihres Speicherkontos StorSimple | Microsoft Azure"
   description="Erläutert die Verwendung der Seite StorSimple Manager konfigurieren, hinzufügen, bearbeiten, löschen oder drehen den Sicherheitsschlüssel ein Speicherkonto StorSimple Virtual Array zugeordnet."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="09/29/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-storage-accounts-for-storsimple-virtual-array"></a>Verwenden des StorSimple Manager-Dienstes Speicherkonten für StorSimple Virtual Array verwalten

## <a name="overview"></a>Übersicht

Die Seite **Konfigurieren** stellt die globale Parameter, die in der StorSimple Manager-Dienst erstellt. Diese Parameter gelten für alle Geräte mit dem Dienst verbundenen und umfassen:

- Speicherkonten 
- Access Control-Datensätze 

In diesem Lernprogramm erläutert die Verwendung der Seite **Konfigurieren** , hinzufügen, bearbeiten oder Löschen von Speicherkonten für virtuelle StorSimple Array. Die Informationen in diesem Lernprogramm gilt nur für StorSimple Virtual Array März 2016 GA-Release-Software ausgeführt.

 ![Seite](./media/storsimple-ova-manage-storage-accounts/configure_service_page.png)  

Speicherkonten enthalten die Anmeldeinformationen, die das Gerät verwendet das Speicherkonto Ihren Cloud-Anbieter auf. Microsoft Azure-Speicherkonten sind für Anmeldeinformationen wie den Kontonamen und die primäre Taste. 

Auf der Seite **Konfigurieren** werden alle für die Abrechnung Abonnement erstellt Speicherkonten in Tabellenform mit den folgenden Informationen angezeigt:

- **Name** – der eindeutige Name für das Konto bei der Erstellung zugewiesen.
- **SSL aktiviert** – ob der SSL aktiviert ist und Gerät Cloud-Kommunikation über den sicheren Kanal.

Sind die häufigsten Aufgaben für Speicherkonten auf der Seite **Konfigurieren** ausgeführt werden können:

- Ein Speicherkonto hinzufügen 
- Ein Speicherkonto bearbeiten 
- Ein Speicherkonto löschen 


## <a name="types-of-storage-accounts"></a>Arten von Speicherkonten

Es gibt drei Arten von Speicher-Konten mit dem StorSimple-Gerät.

- **Automatisch generierte Speicherkonten** – wie der Name schon sagt, wird Speicher Kontoart automatisch generiert, wenn der Dienst zuerst erstellt wird. Hier erfahren Sie, wie diese Speicherkonto erstellt wird, finden Sie unter [Erstellen eines neuen Dienstes](storsimple-ova-manage-service.md#create-a-service). 
- **Speicherkonten Dauerauftrags** – Azure-Speicherkonten dieselbe Abonnement, den Dienst zugeordnet sind. Erfahren Sie mehr über deren Speicher Konten erstellt werden, finden Sie unter [Über Azure-Speicherkonten](../storage/storage-create-storage-account.md). 
- **Speicherkonten außerhalb der Dauerauftrag** – Azure-Speicher-Konten, die nicht mit dem Dienst verbunden und wahrscheinlich vor der Dienst vorhanden sind.

Jedes virtuelle Array StorSimple erstellt einen Container (mit einem Präfix Hcs) dem Konto zugeordneten Speicher. Dieser Container beinhaltet die clouddaten für Ihr Gerät. Löschen Sie dieser Container durch Zugriff über Azure Storage-Diensts, wie Datenverlust diese Aktion führt nicht.

## <a name="add-a-storage-account"></a>Ein Speicherkonto hinzufügen

Sie können die Dienstkonfiguration StorSimple Manager ein Speicherkonto hinzufügen, einen eindeutigen Namen und Zugriffsinformationen Storage-Konto verknüpft sind. Sie können auch secure Sockets Layer (SSL) Modus aktivieren, erstellen Sie einen sicheren Kanal für die Kommunikation zwischen dem Gerät und der Cloud.

Sie können mehrere Konten für einen bestimmten Cloud-Dienstanbieter erstellen. Während das Speicherkonto gespeichert wird, versucht der Dienst Ihren Cloud-Anbieter kommunizieren. Die Anmeldeinformationen und Access-Material, das angegebene authentifiziert zu diesem Zeitpunkt. Ein Speicherkonto wird nur erstellt, wenn die Authentifizierung erfolgreich war. Wenn die Authentifizierung fehlschlägt, wird eine entsprechende Fehlermeldung angezeigt.

Ressourcenmanager Speicherkonten in Azure-Portal erstellt werden auch von StorSimple unterstützt. Speicherkonten Ressourcen-Manager erscheint nicht in der Dropdown-Liste zur Auswahl der Speicher im klassischen Azure-Portal erstellte Konten angezeigt werden. Speicherkonten Ressourcenmanager müssen mit der Prozedur ein Speicherkonto folgendermaßen hinzufügen hinzugefügt werden.

Das Verfahren zum Hinzufügen eines klassischen Azure-Speicher ist unten aufgeführt.

[AZURE.INCLUDE [add-a-storage-account](../../includes/storsimple-ova-configure-new-storage-account.md)]

## <a name="edit-a-storage-account"></a>Ein Speicherkonto bearbeiten

Sie können ein Speicherkonto vom Gerät verwendet. Ein Speicherkonto bearbeiten, die derzeit verwendet wird, sind die Felder geändert die Tastenkombination und den SSL-Modus für das Speicherkonto. Geben Sie neuen Speicher Zugriffstaste oder **SSL aktivieren Modus** Auswahl ändern, und aktualisierte speichern.

#### <a name="to-edit-a-storage-account"></a>So bearbeiten Sie ein Speicherkonto

1. Zielseite Service wählen Sie den Dienst, doppelklicken Sie auf den Namen, und klicken Sie auf **Konfigurieren**.

2. Klicken Sie auf **Storage-Konten hinzufügen oder bearbeiten**.

3. Im Dialogfeld **Speicherkonten hinzufügen/bearbeiten** :

  1. Wählen Sie in der Dropdown-Liste der **Speicherkonten**ein Konto, das Sie ändern möchten. 
  2. Bei Bedarf können Sie **SSL-Modus aktivieren** Auswahl ändern.
  3. Sie können Ihr Konto Zugriff Speicherschlüssel regenerieren. Weitere Informationen finden Sie unter [Konto Speicherschlüssel Regenerieren](storage-create-storage-account.md#manage-your-storage-access-keys). Den neue Speicher kontoschlüssel angeben. Für ein Konto Azure-Speicher ist dies die primäre Taste. 
  4. Klicken Sie auf Check ![Symbol überprüfen](./media/storsimple-ova-manage-storage-accounts/checkicon.png) um die Einstellung zu speichern. Die Einstellung werden auf der Seite **Konfigurieren** aktualisiert. 
  5. Klicken Sie am unteren Rand der Seite auf **Speichern** , um die neue Einstellung zu speichern. 

     ![Ein Speicherkonto bearbeiten](./media/storsimple-ova-manage-storage-accounts/modifyexistingstorageaccount.png)
  
## <a name="delete-a-storage-account"></a>Ein Speicherkonto löschen

> [AZURE.IMPORTANT] Sie können ein Speicherkonto, wenn er nicht verwendet. Wenn ein Speicherkonto verwendet wird, werden Sie benachrichtigt.

#### <a name="to-delete-a-storage-account"></a>So löschen Sie ein Speicherkonto

1. Wählen Sie auf der Zielseite StorSimple Manager Service den Dienst, doppelklicken Sie auf den Namen und klicken Sie dann auf **Konfigurieren**.

2. Zeigen Sie in tabellarischen Übersicht Speicherkonten auf das Konto, das Sie löschen möchten.

3. Extreme rechts für das Storage-Konto wird ein Symbol "löschen" (**X**) angezeigt. Klicken Sie auf **X** , um die Anmeldeinformationen zu löschen.

4. Zur Bestätigung aufgefordert werden, klicken Sie auf **Ja,** um den Löschvorgang fortzusetzen. Tabellarische Liste wird entsprechend aktualisiert.

5. Klicken Sie am unteren Rand der Seite auf **Speichern** , um die neue Einstellung zu speichern.


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie [StorSimple Virtual Array](storsimple-ova-web-ui-admin.md)verwalten.
