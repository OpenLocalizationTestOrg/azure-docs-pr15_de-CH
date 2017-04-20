<properties
   pageTitle="Azure Ressourcenmanager Anforderung Grenzwerte | Microsoft Azure"
   description="Beschreibt die Verwendung mit Azure Ressourcenmanager Drosselung Abonnement Grenzwerte erreicht haben."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="tomfitz"/>

# <a name="throttling-resource-manager-requests"></a>Drosselung der Ressourcen-Manager-Anfragen

Für jedes Abonnement und Mieter Ressourcenmanager Grenzwerte Anfragen 15.000 pro Stunde Lese- und Schreibanfragen 1.200 pro Stunde. Die Anwendung oder das Skript diese Grenzwerte erreicht, müssen Sie Ihre Anfragen zu drosseln. In diesem Thema wird das verbleibenden Anfragen bestimmen, bevor der Grenzwert erreicht haben, und reagiert, wenn die erreicht.

Wenn das Limit erreicht ist, wird den HTTP-Statuscode **429 zu viele Anfragen**.

Die Anzahl der Anfragen wird Ihr Abonnement oder Ihrem Mandanten beschränkt. Wenn mehrere, gleichzeitige Anwendung Anfragen in Ihrem Abonnement Ersuchen Anwendungsbereiche hinzugefügt um die Anzahl der verbleibenden Anfragen zu bestimmen.

Abonnement Gültigkeitsbereich Anfragen sind Ihre Abonnement-Id wie beim Abrufen der Ressource übergeben einbeziehen in Ihrem Abonnement gruppiert. Gültigkeitsbereich Mieter Anfragen enthalten nicht die Abonnement-Id, wie gültige Azure Orte werden abgerufen.

## <a name="remaining-requests"></a>Verbleibende Anfragen

Die Anzahl der verbleibenden Anfragen bestimmen anhand der Antwortheader. Jede Anforderung enthält Werte für die Anzahl der verbleibenden Lese- und Schreibanfragen. Die folgende Tabelle beschreibt die Antwortheader für diese Werte prüfen können:

| Antwort-header | Beschreibung |
| --------------- | ----------- |
| x-ms-ratelimit-Remaining-Subscription-Reads | Abonnement Gültigkeitsbereich liest verbleibende |
| x-ms-ratelimit-Remaining-Subscription-Writes | Abonnement Gültigkeitsbereich schreibt verbleibende |
| x-ms-ratelimit-Remaining-Tenant-Reads | Gültigkeitsbereich Mieter liest verbleibende |
| x-ms-ratelimit-Remaining-Tenant-Writes | Gültigkeitsbereich Mieter schreibt verbleibende |
| x-ms-ratelimit-Remaining-Subscription-Resource-Requests | Abonnement Bereich Verbleibende Typ-Anfragen.<br /><br />Dieser Headerwert wird nur zurückgegeben, wenn ein Dienst das Standardlimit überschrieben hat. Ressourcen-Manager fügt diesen Wert Abonnement liest oder schreibt. |
| x-ms-ratelimit-Remaining-Subscription-Resource-Entities-Read | Abonnement beschränkt Typ Auflistung Anfragen verbleiben.<br /><br />Dieser Headerwert wird nur zurückgegeben, wenn ein Dienst das Standardlimit überschrieben hat. Dieser Wert stellt die Anzahl der verbleibenden Auflistung Anfragen (Liste Ressourcen). |
| x-ms-ratelimit-Remaining-Tenant-Resource-Requests | Mieter Bereich Verbleibende Typ-Anfragen.<br /><br />Dieser Header nur für Anfragen Mieter Ebene hinzugefügt und nur dann, wenn ein Dienst hat das Standardlimit überschrieben. Ressourcen-Manager fügt diesen Wert Mieter liest oder schreibt. |
| x-ms-ratelimit-Remaining-Tenant-Resource-Entities-Read | Mandanten beschränkt Typ Auflistung Anfragen verbleiben.<br /><br />Dieser Header nur für Anfragen Mieter Ebene hinzugefügt und nur dann, wenn ein Dienst hat das Standardlimit überschrieben. |

## <a name="retrieving-the-header-values"></a>Die Kopfzeile Werte

Diese Header Werte im Code oder Skript unterscheidet sich nicht vom Header-Wert abrufen. 

Zum Beispiel in **C#**, den Headerwert aus abgerufen **HttpWebResponse** -Objekt **Antwort** durch den folgenden Code:

    response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)

In **PowerShell**den Headerwert aus einem Invoke WebRequest-Vorgang abgerufen.

    $r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
    $r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
    
Oder die verbleibenden Anfragen für Debuggen anzeigen können Sie die **-Debuggen** Parameter in der **PowerShell** -Cmdlet.

    Get-AzureRmResourceGroup -Debug
    
Die viele Informationen, einschließlich der folgenden Antwort zurückgibt:

    ...
    DEBUG: ============================ HTTP RESPONSE ============================

    Status Code:
    OK

    Headers:
    Pragma                        : no-cache
    x-ms-ratelimit-remaining-subscription-reads: 14999
    ...

In **Azure CLI**Headerwert mithilfe ausführlichere Option abgerufen.

    azure group list -vv --json

Die viele Informationen, einschließlich des folgenden Objekts zurückgibt:

    ...
    silly: returnObject
    {
      "statusCode": 200,
      "header": {
        "cache-control": "no-cache",
        "pragma": "no-cache",
        "content-type": "application/json; charset=utf-8",
        "expires": "-1",
        "x-ms-ratelimit-remaining-subscription-reads": "14998",
        ...

## <a name="waiting-before-sending-next-request"></a>Vor dem Senden der nächsten Anforderung warten

Beim Erreichen der Anforderungsgrenze gibt Ressourcenmanager **429** HTTP-Statuscode und **Retry-After** Wert im Header. Der **Retry-After** Wert gibt die Anzahl der Sekunden die Anwendung sollten (oder Ruhezustand) vor dem Senden der nächsten Anforderung. Wenn Sie eine Anforderung senden, vor Ablauf der Wiederholungsversuche, Ihre Anforderung nicht verarbeitet und neue Retry-Wert zurückgegeben.
