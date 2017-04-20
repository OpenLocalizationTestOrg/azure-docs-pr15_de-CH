<properties
 pageTitle="Planer Grenzen und-Standards"
 description="Planer Grenzen und-Standards"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="scheduler-limits-and-defaults"></a>Planer Grenzen und-Standards

## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>Planer Kontingente, Grenzen, Standardwerte und drosseln

[AZURE.INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a>X-ms-Request-Id-Header

Jede Anforderung für den Zeitplandienst gibt einen Antwortheader mit dem Namen**X-ms-Anfrage-Id**zurück. Dieser Header enthält einen nicht transparenten Wert, der die Anforderung eindeutig identifiziert.

Wenn eine Anforderung konsistent fehlschlägt und Sie haben sichergestellt, dass die Anforderung korrekt formuliert wird, kann dieser Wert mithilfe der Fehlerbericht an Microsoft. Enthalten Sie in Ihrem Bericht den Wert der X-ms-Request-Id, die Dauer, die der Anforderung, den Bezeichner des Dauerauftrags, Job-Auflistung oder Auftrag und den Typ des Vorgangs, der versucht, die Anforderung.

## <a name="see-also"></a>Siehe auch


 [Was ist der Taskplaner?](scheduler-intro.md)

 [Azure Scheduler Konzepte, Terminologie und Entitätshierarchie](scheduler-concepts-terms.md)

 [Erste Schritte mit Planer im Azure-portal](scheduler-get-started-portal.md)

 [Pläne und Fakturierung in Azure Scheduler](scheduler-plans-billing.md)

 [Azure Scheduler REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler-PowerShell-Cmdlets verweisen](scheduler-powershell-reference.md)

 [Azure Scheduler hohe Verfügbarkeit und Zuverlässigkeit](scheduler-high-availability-reliability.md)

 [Azure Scheduler ausgehende Authentifizierung](scheduler-outbound-authentication.md)
