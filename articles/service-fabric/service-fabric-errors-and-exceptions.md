<properties
   pageTitle="Gemeinsame FabricClient-Ausnahmen | Microsoft Azure"
   description="Beschreibt die allgemeine Ausnahmen und Fehler, die beim Ausführen der Anwendung und Verwaltung Clustervorgänge FabricClient APIs ausgelöst werden können."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>Allgemeine Ausnahmen und Fehler beim Arbeiten mit FabricClient-APIs
Die [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) -APIs ermöglichen Cluster und Anwendungsadministratoren Verwaltungsaufgaben auf Service Fabric-Anwendung, Service oder Cluster. Z. B. Bereitstellung, Aktualisierung und Entfernung, Überprüfen des Zustands eines Clusters oder einen Dienst testen Anwendungsentwickler und Clusteradministratoren können APIs FabricClient Service Fabric-Cluster und Applikationen entwickeln.

Es gibt viele verschiedene Arten von Operationen, die mit FabricClient ausgeführt werden können.  Jede Methode kann Ausnahmen für Fehler durch fehlerhafte Eingabe, Laufzeitfehler oder vorübergehende Infrastruktur auslösen.  Finden Sie die API-Referenzdokumentation finden, welche Ausnahmen durch eine bestimmte Methode ausgelöst werden. Es gibt Ausnahmen, die von vielen anderen [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) APIs ausgelöst werden können. Die folgende Tabelle listet die Ausnahmen, die für die FabricClient-APIs sind.

|Ausnahme| Wird ausgelöst, wenn|
|---------|:-----------|
|[System.Fabric.FabricObjectClosedException](https://msdn.microsoft.com/library/system.fabric.fabricobjectclosedexception.aspx)|Das [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) -Objekt ist geschlossen. Freigeben des [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) -Objekts verwenden und ein neues [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) -Objekt zu instanziieren. |
|[System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx)|Der Vorgang hat das Zeitlimit überschritten. [OperationTimedOut](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) wird zurückgegeben, wenn die Operation mehr als MaxOperationTimeout abgeschlossen hat.|
|[System.UnauthorizedAccessException](https://msdn.microsoft.com/en-us/library/system.unauthorizedaccessexception.aspx)|Fehler bei der zugriffsüberprüfung für den Vorgang. E_ACCESSDENIED zurückgegeben.|
|[System.Fabric.FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)|Beim Ausführen des Vorgangs ist ein Laufzeitfehler aufgetreten. FabricClient-Methoden können potenziell [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)auslösen, die [ErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricexception.errorcode.aspx) -Eigenschaft gibt die Ursache für die Ausnahme. Fehlercodes werden in der [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) -Enumeration definiert.|
|[System.Fabric.FabricTransientException](https://msdn.microsoft.com/library/system.fabric.fabrictransientexception.aspx)|Der Vorgang ist aufgrund einer vorübergehenden Fehlerzustand Art. Beispielsweise kann ein Vorgang fehl, da ein Quorum von Replikaten vorübergehend nicht erreichbar ist. Vorübergehende Ausnahmen entsprechen fehlgeschlagene Vorgänge wiederholt werden können.|

Einige häufige Fehler von [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) , die in [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)zurückgegeben werden:

|Fehler| Bedingung|
|---------|:-----------|
|CommunicationError|Ein Kommunikationsfehler verursacht den Vorgang fehlschlägt, wiederholen Sie den Vorgang.|
|InvalidCredentialType|Der Anmeldeinformationstyp ist ungültig.|
|InvalidX509FindType|Die X509FindType ist ungültig.|
|InvalidX509StoreLocation|Die X509 Speicherort ist ungültig.|
|InvalidX509StoreName|Die X509 Shop-Name ist ungültig.|
|InvalidX509Thumbprint|Das X509 Zertifikat Fingerabdruck ist ungültig.|
|InvalidProtectionLevel|Die Schutzebene ist ungültig.|
|InvalidX509Store|X509 Zertifikatspeicher kann nicht geöffnet werden.|
|InvalidSubjectName|Der Antragstellername ist ungültig.|
|InvalidAllowedCommonNameList|Das Format der gemeinsamen Liste Zeichenfolge ist ungültig. Es ist eine durch Kommas getrennte Liste.|
