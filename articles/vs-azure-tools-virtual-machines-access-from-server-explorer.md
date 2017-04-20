<properties
   pageTitle="Server-Explorer Azure virtuellen Maschinen zugreifen | Microsoft Azure"
   description="Erhalten Sie ein Überblick zum Anzeigen erstellen und Verwalten von Azure virtuelle Maschinen (VMs) im Server-Explorer in Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>Server-Explorer zugreifen Azure virtuelle Computer

Server-Explorer in Visual Studio verwenden, können Sie Informationen über die von Azure gehosteten virtuellen Computer anzeigen.

## <a name="accessing-virtual-machines-in-server-explorer"></a>Zugriff auf virtuelle Computer im Server-Explorer

Wenn Azure gehosteten virtuellen Computer haben, können Sie sie im Server-Explorer zugreifen. Sie müssen zuerst Ihre Azure-Abonnement anmelden mobile Dienste anzeigen. Anmelden, öffnen Sie das Kontextmenü für den Azure-Knoten im Server-Explorer und wählen Sie **Verbinden mit Microsoft Azure**.

### <a name="to-get-information-about-your-virtual-machines"></a>Informationen zu virtuellen Maschinen

1. Im Server-Explorer wählen Sie einen virtuellen Computer, und dann F4, um das Eigenschaftsfenster anzeigen.

    Die folgende Tabelle zeigt, welche Eigenschaften verfügbar sind, aber sie sind alle schreibgeschützt. Ändern sie mithilfe der [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkID=213885).

  	|Eigenschaft|Beschreibung|
  	|---|---|
  	|DNS-Name|Die URL mit der Internet-Adresse des virtuellen Computers.|
  	|Umgebung|Für einen virtuellen Computer ist der Wert dieser Eigenschaft immer Produktion.|
  	|Name|Der Name des virtuellen Computers.|
  	|Größe|Die Größe des virtuellen Computers Arbeitsspeicher und Speicherplatz widerspiegelt, die verfügbar ist. Weitere Informationen finden Sie unter: Größe der virtuellen Computer konfigurieren.|
  	|Status|Werte sind starten gestartet, angehalten, beendet und Status abrufen. Abrufen der Status angezeigt wird, ist der aktuelle Status unbekannt. Die Werte für diese Eigenschaft unterscheiden sich von Werten, die im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885)verwendet werden.|
  	|SubscriptionID|Die Abonnement-ID für Ihre Azure-Konto. Diese Informationen zeigen im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885) durch Anzeigen der Eigenschaften für ein Abonnement.|

1. Wählen Sie einen Endpunktknoten, und zeigen Sie **das Eigenschaftenfenster** .

1. Die folgende Tabelle beschreibt die verfügbaren Eigenschaften der Endpunkte, aber sie sind schreibgeschützt. Verwenden Sie zum Hinzufügen oder Bearbeiten von Endpunkten für eine virtuelle Maschine, [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkID=213885). 

  	|Eigenschaft|Beschreibung|
  	|---|---|
  	|Name|Ein Bezeichner für den Endpunkt.|
  	|Privater Port|Der Anschluss für den Netzwerkzugriff für Ihre Anwendung intern.|
  	|Protokoll|Das Protokoll, das die Transportschicht für diesen Endpunkt verwendet TCP oder UDP.|
  	|Öffentlicher Port|Der Port für öffentlichen Zugriff auf die Anwendung verwendet wird.|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Azure-Rollen in Visual Studio finden Sie unter [Verwenden von Remotedesktop Azure-Rollen](vs-azure-tools-remote-desktop-roles.md).
