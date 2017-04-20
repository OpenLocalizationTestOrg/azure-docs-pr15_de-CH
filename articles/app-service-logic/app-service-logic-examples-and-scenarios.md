<properties
   pageTitle="Logik-Apps Beispiele und Szenarien | Microsoft Azure"
   description="Beispiele für gemeinsame Logik apps und erfahren, wie allgemeine Szenarien implementieren"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-examples-and-common-scenarios"></a>Logik Apps Beispiele und Szenarien

Dieses Dokument Details Szenarien und Beispiele zu helfen, einige der Methoden verstehen können Logik apps Geschäftsprozesse automatisieren. 

## <a name="custom-triggers-and-actions"></a>Benutzerdefinierte Trigger und Aktionen

Es gibt mehrere Logik-app von einer anderen Anwendung auslösen kann. Nachfolgend einige Beispiele:

- [Erstellen eines benutzerdefinierten Trigger oder Aktion](app-service-logic-create-api-app.md)
- [Langwierige Aktionen](app-service-logic-create-api-app.md)
- [HTTP-Anforderung Trigger (POST)](app-service-logic-http-endpoint.md)
- [Webhook Auslöser und Aktionen](app-service-logic-create-api-app.md)
- [Abruf-Trigger](app-service-logic-create-api-app.md)

### <a name="scenarios"></a>Szenarien

- [Synchrone Anforderung-Antwort](app-service-logic-http-endpoint.md)
- [Anforderung-Antwort mit SMS](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="error-handling-and-logging"></a>Fehlerbehandlung und Protokollierung

- [Ausnahme- und Fehlerbehandlung](app-service-logic-exception-handling.md)
- [Konfigurieren von Azure Alarme und Diagnose](app-service-logic-monitor-your-logic-apps.md)

### <a name="scenarios"></a>Szenarien

- [Anwendungsbeispiel: Fehler- und Ausnahmebehandlung](app-service-logic-scenario-error-and-exception-handling.md)

## <a name="deploying-and-managing"></a>Bereitstellen und verwalten

- [Erstellen Sie eine automatische Bereitstellung](app-service-logic-create-deploy-template.md)
- [Erstellen und Bereitstellen von Logik apps aus Visual Studio](app-service-logic-deploy-from-vs.md)
- [Monitor Logik apps](app-service-logic-monitor-your-logic-apps.md)

## <a name="content-types-conversions-and-transformations"></a>Inhaltstypen, Umwandlungen und Transformationen

Logik-Apps [Workflow Definition Language](http://aka.ms/logicappsdocs) enthält viele Funktionen zum Konvertieren und verschiedene Inhaltstypen ermöglichen.  Außerdem wird das Modul alles tun, um Datenflüsse durch den Workflow Inhaltstypen erhalten.

- [Behandlung von Inhaltstypen](app-service-logic-content-type.md) wie Application/Json Application/Xml und nur-text
- [Erstellen von Workflow-Definitionen](app-service-logic-author-definitions.md)
- [Workflow-Definition-Referenzhandbuch](http://aka.ms/logicappsdocs)

## <a name="batches-and-looping"></a>Stapeln und Schleifen

- [SplitOn](app-service-logic-loops-and-scopes.md)
- [ForEach](app-service-logic-loops-and-scopes.md)
- [Bis](app-service-logic-loops-and-scopes.md)

## <a name="integrating-with-azure-functions"></a>Integration in Azure Funktionen

- [Azure Funktionen-integration](app-service-logic-azure-functions.md)

### <a name="scenarios"></a>Szenarien

- [Azure-Funktion als Trigger Service Bus](app-service-logic-scenario-function-sb-trigger.md)

## <a name="http-rest-and-soap"></a>HTTP, REST und SOAP

 - [SOAP aufrufen](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)


Wir werden Beispiele und Szenarien zu diesem Dokument hinzufügen. Verwenden Sie Kommentare Abschnitt, um uns mitzuteilen, welche Beispiele oder Szenarien möchten Sie hier.