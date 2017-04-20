<properties
   pageTitle="Wie Azure Funktionen | Microsoft Azure"
   description="Kennen Sie Azure Funktionen wie Ihre Arbeitslasten ereignisgesteuerte Bedürfnisse skalieren."
   services="functions"
   documentationCenter="na"
   authors="dariagrigoriu"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure Funktionen, Funktionen, Verarbeitung, Webhooks, dynamische Compute, serverlose Architektur"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="reference"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="dariagrigoriu"/>

# <a name="how-to-scale-azure-functions"></a>Wie Azure Funktionen

## <a name="introduction"></a>Einführung

Ein Vorteil der Azure-Funktionen werden Serverressourcen nur bei Bedarf verwendet werden. Dies bedeutet, dass Sie nicht im Leerlauf VMs bezahlen oder Kapazitätsreserven für Wenn Sie es benötigen. Stattdessen weist die Plattform rechenleistung Code ausgeführt wird, ggf. Last skaliert und dann verkleinert, wenn der Code nicht ausgeführt wird.

Der Mechanismus für diese neue Funktion ist dynamische Service-Plan.  

In diesem Artikel Überblick einen wie dynamische Service-Plan funktioniert und wie die Plattform auf Anforderung zum Ausführen des Codes.

Wenn Sie nicht mit Azure vertraut sind, müssen Sie den Artikel [Azure Funktionen](functions-overview.md) zum besseren Verständnis die Funktionen überprüfen.

## <a name="configure-azure-functions"></a>Azure Funktionen konfigurieren

Zwei Skalierung betreffen:

* [Azure App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) oder dynamische Service-plan
* Größe des Speichers für die Umgebung

Die Kosten einer Funktion ändert sich je nach Serviceplan, den Sie auswählen. Mit einem dynamischen Serviceplan hängt Kosten Ausführungszeit, Speichergröße und Häufigkeit der Ausführung. Kosten entstehen nur wenn Code ausgeführt wird.

Ein App Service-Plan enthält die Funktionen vorhandener VMs, der auch anderer Code ausgeführt werden können. Nachdem Sie die VMs monatlich bezahlen, ist kostenlos für die Ausführungsfunktionen auf.

## <a name="choose-a-service-plan"></a>Wählen Sie ein

Wenn Sie Funktionen erstellen, können Sie dynamische Serviceplan oder eine [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)ausführen auswählen.
In App Service-Plan führen Ihre Funktionen auf einer dedizierten VM wie Web apps arbeiten für Basic, Standard oder Premium-SKUs.
Dieser dedizierte VM apps und Funktionen zugewiesen und ist immer verfügbar, ob Code aktiv oder nicht ausgeführt wird. Dies ist eine gute Option bei vorhandenen, ausgelastete VMs, die bereits von anderen Code oder Funktionen kontinuierlich oder Fast ununterbrochen ausgeführt werden soll. Eine VM entkoppelt Kosten Runtime und Speichergröße. Daher können Sie die Kosten vieler langer Funktionen, die Kosten für eine oder mehrere VMs beschränken, die auf.

[AZURE.INCLUDE [Dynamic Service plan](../../includes/functions-dynamic-service-plan.md)]
