<properties
   pageTitle="Azure Service Fabric unter Linux | Microsoft Azure"
   description="Service Fabric-Cluster unterstützen Linux und Java, d. h. Sie bereitstellen können und Host Service Fabric Applikationen auf Linux in Java und C# geschrieben."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="Java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="SubramaR"/>

# <a name="service-fabric-on-linux"></a>Service-Fabric unter Linux

Die Vorschau des Service Fabric unter Linux können Sie erstellen, bereitstellen und verwalten hochverfügbare, hochskalierbare Applikationen unter Linux wie unter Windows. Service Fabric-Frameworks (zuverlässige Dienste und zuverlässige Akteure) stehen in Java unter Linux neben C# (.NET Core).  Sie können auch [ausführbare Service](service-fabric-deploy-existing-app.md) mit allen Sprachen oder Rahmen erstellen. Darüber hinaus unterstützt die Vorschau orchestrieren Andockfenster Container. Andockfenster Container ausgeführt Gast ausführbare Dateien oder systemeigene Service Fabric Services Service Fabric-Frameworks verwenden.

Service Fabric unter Linux ist konzeptionell Fabric Service unter Windows (außer OS Einzelheiten und programming Language Support). Folglich die meisten [Dokumentation](http://aka.ms/servicefabricdocs) gilt bei mit der Technologie vertraut.

> [AZURE.VIDEO service-fabric-linux-preview]

## <a name="supported-operating-systems-and-programming-languages"></a>Unterstützte Betriebssysteme und Programmiersprachen

Die eingeschränkte Vorschau unterstützt die Erstellung von ein Feld Entwicklung Cluster neben mehreren Computern Clustern in Azure Ubuntu Server 16.04 ausgeführt. Die Vorschau unterstützt zuverlässige Akteure und zuverlässige Dienste statusfrei Frameworks in Java und C# neben Gast ausführbare Dateien und orchestriert Andockfenster Container.  

>[AZURE.NOTE] Zuverlässige Sammlungen sind noch nicht in Linux unterstützt. Stand alleine Clustern nicht unterstützt - nur ein Feld oder mehrere Rechner Azure Linux-Clustern werden in der Vorschau.

## <a name="supported-tooling"></a>Unterstützte Tools

Vorschau unterstützt die Interaktion mit dem Cluster über Azure-CLI. Für Java-Entwickler wird mit Eclipse und Yeoman mit Linux und OSX unterstützt Eclipse bereitgestellt. OSX-Integration verwendet Linux VM unter der Haube über Vagrant. Für C#-Entwickler erfolgt Integration Yeoman Anwendungsvorlagen generieren.

## <a name="next-steps"></a>Nächste Schritte


1. [Zuverlässige Akteure](service-fabric-reliable-actors-introduction.md) und [Zuverlässige Dienste](service-fabric-reliable-services-introduction.md) Programmierframeworks vertraut zu machen.

2. [Vorbereiten der Umgebung für Linux](service-fabric-get-started-linux.md)

3. [Vorbereiten der Umgebung für OSX](service-fabric-get-started-mac.md)

4. [Erstellen Sie Ihre erste Service Fabric Java-Anwendung auf Linux](service-fabric-create-your-first-linux-application-with-java.md)
