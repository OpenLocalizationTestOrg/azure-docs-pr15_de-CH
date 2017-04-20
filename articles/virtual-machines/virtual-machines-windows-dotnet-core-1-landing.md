<properties
   pageTitle="Azure Virtual Machine DotNet Core Lernprogramm 1 | Microsoft Azure"
   description="Azure Virtual Machine DotNet Core-Lernprogramm"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="automating-application-deployments-to-azure-virtual-machines"></a>Automatisieren der anwendungsbereitstellung auf Azure Virtual Machines

Dieser vierteiligen Reihe details bereitstellen und Konfigurieren von Azure Ressource und Applikationen mit Vorlagen Azure Ressource verwalten. In dieser Reihe eine Beispielvorlage bereitgestellt und die Bereitstellungsvorlage untersucht. Das Ziel dieser Reihe ist informieren über die Beziehung zwischen Azure-Ressourcen und Erfahrung im Bereitstellen von integrierten Azure Ressourcenmanager Vorlagen Hände abgeben. Dieses Dokument setzt grundlegende Kenntnisse Azure Ressourcenmanager, vor diesem Lernprogramm vertraut mit grundlegenden Konzepten von Azure-Ressourcen-Manager.

## <a name="music-store-application"></a>Musik-Store-Anwendung

In dieser Serie verwendeten Beispiel ist eine zentrale Anwendung simuliert eine Musik-Store einkaufen. Diese Anwendung auf Linux oder Windows virtuellen System bereitgestellt werden, Beispiel Bereitstellung für beide erstellt wurden. Die Anwendung umfasst eine Anwendung und einer SQL-Datenbank. Bereitstellen Sie vor der Lektüre dieser Serie Anwendung der Bereitstellung Schaltfläche auf dieser Seite. Bei vollständigem Einsatz sieht aus wie im folgenden Diagramm die Anwendung / Azure-Architektur. 

Musik Speicher-Ressourcen-Manager-Vorlage, [Vorlage Windows Store Musik](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows) finden Sie hier

![Musik-Store-Anwendung](./media/virtual-machines-windows-dotnet-core/music-store.png)

Jede dieser Komponenten einschließlich der zugeordneten Vorlage JSON ist in die folgenden vier Artikel geprüft.

- [**Architektur**](./virtual-machines-windows-dotnet-core-2-architecture.md) – Komponenten wie Websites und Datenbanken müssen auf Azure Computerressourcen wie virtuellen Computern und SQL Azure-Datenbanken gehostet werden. Dieses Dokument führt Azure Ressourcen und Bereitstellen dieser Ressourcen mit einer Azure-Ressourcen-Manager-Vorlage durch Zuordnung berechnen müssen. 

- [**Zugriff und Sicherheit**](./virtual-machines-windows-dotnet-core-3-access-security.md) in Azure-Anwendung hostet, ist zu prüfen, wie die Anwendung zugegriffen wird und wie verschiedene Komponenten Zugriff miteinander. Dieses Dokument beschreibt bereitstellen und Sichern des Zugriffs zwischen Anwendungskomponenten und Internetzugriff einer Anwendung.

- [**Verfügbarkeit und Skalierung**](./virtual-machines-windows-dotnet-core-4-availability-scale.md) – Verfügbarkeit und Skalierung finden Anwendung können Ausführung während Ausfallzeiten Infrastruktur bleiben und Datenverarbeitungsressourcen Anwendung Nachfrage zu skalieren. Dieses Dokumentdetails Komponenten für einen Lastenausgleich bereitzustellen und hochverfügbare Anwendung.

- [**Bereitstellung**](./virtual-machines-windows-dotnet-core-5-app-deployment.md) – bei der Bereitstellung von Applikationen auf Azure Virtual Machines, muss die Methode, die Binärdateien der Anwendung auf dem virtuellen Computer installiert werden, berücksichtigt werden. In diesem Dokument werden automatisierte Anwendungsinstallation Azure Virtual Machine benutzerdefinierte Skript Erweiterungen.

Beim Entwickeln von Azure-Ressourcen-Manager Vorlagen soll die Bereitstellung von Azure Infrastruktur, Installation und Konfiguration der Programme auf Azure Infrastruktur automatisieren. Durcharbeiten dieser Artikel enthält ein Beispiel dieser Erfahrung.

## <a name="deploy-the-music-store-application"></a>Music Store-Anwendung bereitstellen

Mit dieser Schaltfläche kann die Musik Store-Anwendung bereitgestellt werden.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fdotnet-core-sample-templates%2Fmaster%2Fdotnet-core-music-windows%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Azure-Ressourcen-Manager-Vorlage erfordert die folgenden Parameterwerte.

|Parametername |Beschreibung   |
|---|---|
|ADMINUSERNAME   | Admin-Benutzername, der für den virtuellen Computer und Azure SQL-Datenbank verwendet wird.  |
|ADMINPASSWORD | Kennwort, das in Azure Virtual Machine und SQL-Datenbank verwendet wird.  |
|NUMBEROFINSTANCES | Die Anzahl der virtuellen Computer erstellt werden. Jeder dieser virtuellen Computer Musik Store Web-Anwendung hosten und alle Lastenausgleich auf sie ist. |
|PUBLICIPADDRESSDNSNAME | Global eindeutigen DNS-Namen der öffentlichen IP-Adresse zugeordnet. |

Nach Abschluss die vorlagenbereitstellung Durchsuchen der öffentlichen IP-Adresse mit einem beliebigen Internetbrowser. .Net Core Musik-Website angezeigt.

## <a name="next-steps"></a>Nächste Schritte

<hr>

[Schritt 1 - Anwendungsarchitektur mit Azure Ressourcenmanager Vorlagen](./virtual-machines-windows-dotnet-core-2-architecture.md)

[Schritt 2: Zugriff und Sicherheit von Azure Ressourcenmanager Vorlagen](./virtual-machines-windows-dotnet-core-3-access-security.md)

[Schritt 3 - Verfügbarkeit und Skalierung in Azure Ressourcenmanager Vorlagen](./virtual-machines-windows-dotnet-core-4-availability-scale.md)

[Schritt 4 - Bereitstellung mit Azure Ressourcenmanager Vorlagen](./virtual-machines-windows-dotnet-core-5-app-deployment.md)


