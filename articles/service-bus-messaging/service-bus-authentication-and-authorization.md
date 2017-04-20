<properties 
    pageTitle="Service Bus Authentifizierung und Autorisierung | Microsoft Azure"
    description="Übersicht über freigegebene Access Signatur (SAS)-Authentifizierung."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="service-bus-authentication-and-authorization"></a>Service Bus Authentifizierung und Autorisierung

Clientanwendungen können Azure Service Bus Authentifizierung entweder gemeinsame Zugriff Signatur (SAS) oder durch Azure Active Directory Access Control (auch bekannt als Access Control Service oder ACS) authentifizieren. Freigegebene SAS-Authentifizierung ermöglicht Applikationen zur Authentifizierung von Service Bus mit einer Zugriffstaste konfiguriert den Namespace oder Entität spezifische Berechtigungen zugeordnet sind. Dieser Schlüssel können Sie um SAS-Token zu generieren, mit denen Kunden Service Bus authentifizieren.

>AZURE. WICHTIGE SAS empfiehlt über ACS bietet eine einfache, flexible und einfach zu verwendende Authentifizierungsschema für Service Bus. Anwendung kann SAS in Szenarios verwenden, in denen sie nicht das Konzept eines "Benutzerberechtigung." verwalten müssen

## <a name="shared-access-signature-authentication"></a>Authentifizierung mit freigegebenem Zugriff Signatur

[SAS-Authentifizierung](service-bus-sas-overview.md) können Sie erteilen Sie einem Benutzerzugriff auf Ressourcen mit bestimmten Service Bus. SAS-Authentifizierung in Service Bus umfasst die Konfiguration eines kryptografischen Schlüssels mit rechten für eine Ressource Service Bus. Kunden erhalten dann Zugriff auf die Ressource mit einem SAS-Token aus der Ressourcen-URI zugegriffen und eine Ablaufzeit mit konfigurierten Schlüssel signiert.

Sie können Schlüssel für SAS Service Bus-Namespace. Der Schlüssel gilt für alle Messaging-Entitäten in diesem Namespace. Sie können auch Schlüssel Servicebuswarteschlangen und-Themen. SAS unterstützt auch Service Bus leitet.

Um SAS verwenden, können Sie ein [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) -Objekt auf ein Namespace, eine Warteschlange oder ein Thema besteht aus den folgenden konfigurieren:

- *Schlüssel* , die die Regel identifiziert.

- *PrimaryKey* ist eine kryptografische Schlüssel Zeichen/SAS-Token zu bestätigen.

- *Sekundärer Schlüssel* ist eine kryptografische Schlüssel Zeichen/SAS-Token zu bestätigen.

- *Rechte* , die Auflistung hören, senden oder verwalten Rechte darstellt.

Autorisierungsregeln auf Namespace-Ebene konfiguriert können Clients Zugriff auf alle Elemente in einem Namespace mit dem Token signiert mit dem entsprechenden Schlüssel gewähren. Bis zu 12 Autorisierung Regeln Service Bus-Namespace, Warteschlange oder Thema konfigurierbar. Standardmäßig wird ein [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx) alle bei jeder Namespace konfiguriert zunächst bereitgestellt.

Um auf eine Entität zugreifen, muss der Client einen SAS-Token mit einer bestimmten [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx)generiert. Das SAS-Token wird mit HMAC-SHA256 eine Ressourcenzeichenfolge aus der Ressourcen-URI, Zugriff in Anspruch genommen wird, und ein Ablaufdatum mit einem kryptografischen Schlüssel zugeordnete die Autorisierungsregel generiert.

SAS-Authentifizierung Unterstützung für Service Bus ist in Azure .NET SDK, Version 2.0 und höher enthalten. SAS unterstützt [SharedAccessAuthorizationRule](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sharedaccessauthorizationrule.aspx). Alle APIs, die eine Verbindungszeichenfolge als Parameter akzeptieren beinhalten Unterstützung für SAS-Verbindungszeichenfolgen.

## <a name="acs-authentication"></a>ACS-Authentifizierung

Authentifizierung über ACS Service Bus wird durch eine begleitende "-Sb" ACS-Namespace. Soll einen Companion ACS Namespace für einen Service Bus-Namespace erstellt werden, können nicht den Service Bus-Namespace mit klassischen Azure-Portal erstellen. Sie müssen den Namespace über das [Neue AzureSBNamespace](https://msdn.microsoft.com/library/azure/dn495165.aspx) -PowerShell-Cmdlet erstellen. Zum Beispiel:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $true
```

Um zu vermeiden, Erstellen von ACS Namespaces, Befehl:

```
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $false
```

Beispielsweise wenn einen Service Bus Namespace mit dem Namen **contoso.servicebus.windows.net**erstellen, ein Companion ACS Namespace mit dem Namen **Contoso-sb.accesscontrol.windows.net** automatisch bereitgestellt. Für alle Namespaces, die vor August 2014 erstellt wurden, wurde auch ein zugehörige ACS-Namespace erstellt.

Eine Standard-Dienstidentität "Owner" mit allen rechten wird standardmäßig in diesem Begleiter ACS-Namespace bereitgestellt. Detaillierte Kontrolle auf jede Entität Service Bus über ACS erhalten die geeigneten Vertrauensstellungen konfigurieren. Sie können weitere Identitäten zur Verwaltung von Service Bus Entitäten.

Um eine Entität zugreifen, fordert der Client einen Token SWT ACS mit entsprechenden durch seine Anmeldeinformationen. SWT-Token muss dann als Teil der Anforderung an Service Bus gesendet ermöglichen die Autorisierung des Clients für den Zugriff auf die Entität.

ACS-Authentifizierung Unterstützung für Service Bus ist in Azure .NET SDK, Version 2.0 und höher enthalten. Diese Authentifizierung unterstützt [SharedSecretTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.sharedsecrettokenprovider.aspx). Alle APIs, die eine Verbindungszeichenfolge als Parameter akzeptieren beinhalten Unterstützung für ACS-Verbindungszeichenfolgen.

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie [SAS-Authentifizierung mit Service Bus](service-bus-shared-access-signature-authentication.md) für Weitere Informationen zu SAS.

Übersicht SAS Service Bus finden Sie unter [Shared Access Signatures](service-bus-sas-overview.md).

Finden Sie weitere Informationen zu ACS-Tokens in [wie: ACS OAuth WRAP-Protokoll einen Token anfordern](https://msdn.microsoft.com/library/hh674475.aspx).



