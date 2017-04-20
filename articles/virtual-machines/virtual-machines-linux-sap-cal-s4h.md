<properties 
pageTitle="Bereitstellen von S/4 HANA oder BW-4 HANA auf Azure VM | Microsoft Azure" 
description="Bereitstellen von S/4 HANA oder BW-4 HANA auf Azure VM" 
services="virtual-machines-linux" 
documentationCenter="" 
authors="hermanndms" 
manager="timlt" 
editor="" 
tags="azure-resource-manager" 
  keywords=""/> 
<tags 
  ms.service="virtual-machines-linux" 
  ms.devlang="na" 
  ms.topic="article" 
  ms.tgt_pltfrm="vm-linux" 
  ms.workload="infrastructure-services" 
  ms.date="09/15/2016" 
  ms.author="hermannd"/> 


# <a name="deploying-s4-hana-or-bw4-hana-on-microsoft-azure"></a>Bereitstellen von S/4 HANA oder BW-4 HANA auf Microsoft Azure 

Dieser Artikel beschreibt, wie s/4 HANA auf Microsoft Azure über SAP Cloud Appliance Library 3.0 bereitgestellt.
Die Screenshots zeigen den Prozess Schritt für Schritt. Bereitstellen von anderen SAP HANA-Lösungen, wie BW-4 HANA genauso aus. Eine muss eine andere Lösung auswählen.

Mit SAP Cloud Appliance (SAP CAL) finden Sie [hier](https://cal.sap.com/). Es ist ein Blog von SAP zu neuen [SAP Cloud Appliance Library 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience). 


Die folgenden Screenshots zeigen schrittweise s/4 HANA auf Microsoft Azure bereitstellen. Der Prozess funktioniert ebenso für andere Projektmappen LikeBW/4 HANA.


![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-1b.jpg)

Das erste Bild zeigt alle SAP-CAL HANA-basierten Lösungen auf Microsoft Azure.
Exemplarisch die "SAP s/4 HANA lokale Edition" (Lösung unten im Screenshot) wurde der Prozess durchlaufen.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic-2.jpg)

Zunächst muss ein neues SAP-CAL-Konto erstellt werden. Derzeit sind zwei Optionen für Azure - standard Azure und Azure in China Festland, die Partner 21Vianet betrieben wird.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic3b.jpg)

Dann muss eine Azure-Abonnement-ID eingeben, die der Azure-Portal - Siehe auch weiter unten wie zu finden. Danach muss ein Zertifikat Azure Management heruntergeladen werden.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic6b.jpg)

In der neuen Azure findet Portal das Element "Abonnements" auf der linken Seite. Klicken Sie auf, um alle aktiven Abonnements für den Benutzer anzuzeigen.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic7b.jpg)

Auswählen eines Abonnements und dann "Verwaltungszertifikate" erklärt, die es ist ein neues Konzept mit "Dienstprizipale" für das neue Modell der Azure-Ressourcen-Manager.
SAP-CAL ist nicht noch für dieses neue Modell und "klassisch" und erste Azure-Portal Management Zertifikate weiterhin benötigt.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic4b.jpg)

Hier eine erste Azure-Portal angezeigt. Der Upload Verwaltungszertifikat bietet SAP CAL Berechtigungen zum Erstellen von virtuellen Maschinen innerhalb einer Kundenabonnement. Unter "ABONNEMENTS" finden Registerkarte eine Abonnement-ID, hat SAP CAL-Portal eingegeben werden.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic5.jpg)

Auf der zweiten Registerkarte kann dann die Zeugnisse hochladen, die vor dem SAP CAL heruntergeladen wurde.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic8.jpg)

Ein kleines Dialogfeld erscheint, wählen Sie die heruntergeladene Datei.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic9.jpg)

Nachdem das Zertifikat hochgeladen wurde die Verbindung zwischen SAP-CAL und Kunden Azure-Abonnement kann in SAP CAl getestet werden. Ein wenig Meldung sollte die sagt, dass die Verbindung gültig ist.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic10.jpg)

Nach dem Einrichten eines Kontos ist eine Lösung wählen, die bereitgestellt werden soll, und erstellen Sie eine Instanz.
Modus "einfach" ist es wirklich einfach. Geben Sie einen Instanznamen wählen Sie Azure Region und definieren Sie das Hauptkennwort für die Lösung.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic11.jpg)

Nach einiger Zeit je nach Größe und Komplexität der Lösung (eine Schätzung wird SAP-CAL) wird als "aktiv" und betriebsbereit angezeigt. Es ist sehr einfach.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic12.jpg)

Einige Details der Lösung sehen welche VMs bereitgestellt wurden. In diesem Fall wurden drei Azure VMs Größen und Zweck erstellt.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic13.jpg)

Azure-Portal den virtuellen Computern finden mit demselben Instanznamen im SAP-CAL angegeben wurde.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic14b.jpg)

Jetzt kann mit der Lösung über die Schaltfläche Verbinden Portal SAP-CAL. Kleines Dialogfeld enthält einen Link zu einem Benutzerhandbuch, das beschreibt die Standardanmeldeinformationen an der Lösung arbeiten.

![](./media/virtual-machines-linux-sap-cal-s4h/s4h-pic15.jpg)

Eine weitere Option ist zu Anmeldung auf dem Client Windows VM beispielsweise vorkonfigurierte SAP-GUI.







