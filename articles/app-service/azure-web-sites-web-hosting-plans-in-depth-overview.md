<properties
    pageTitle="Azure App Service-Pläne ausführliche Übersicht | Microsoft Azure"
    description="Erfahren Sie, wie App Servicepläne für Azure App arbeiten und wie sie Ihre Erfahrung profitieren."
    keywords="App, Azure app Dienst, Skalierung, skalierbare Anwendung Serviceplan app Servicekosten"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="byvinyal"/>

# <a name="azure-app-service-plans-in-depth-overview"></a>Azure App Service-Pläne ausführliche Übersicht#

Ein App Service-Plan stellt einen Satz von Funktionen und Kapazität, die Sie in mehreren apps freigeben können. Webapps, Mobile Apps, Funktion Apps oder API-Apps in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) ausführen in einer App Service-Plan. Diese fünf Tarifen unterstützen: *frei*, *Shared*, *Basic*, *Standard*und *Premium*. Jede Ebene verfügt über eigene Funktionen und Kapazität. Apps in dieselbe Abonnement und Standort können einen Plan freigeben. Alle apps, die gemeinsam einen Plan können die Funktionen und Features, die den Plan Ebene definiert werden. Alle apps Plan zugeordnet Ausführen der Ressourcen, die der Plan definiert.

Beispielsweise wenn Ihr Plan "klein" doppelt in der standard-Service konfiguriert ist, alle apps mit diesem Plan auf beide Instanzen ausgeführt und haben Zugriff auf Funktionen der Service-Ebene. Plan-Instanzen auf denen apps ausführen, sind vollständig verwaltbar und hochverfügbar bereitgestellt.

Dieser Artikel behandelt die Hauptmerkmale Tier und Skalierung der App Service-Plan und wie sie spielen beim Verwalten Ihrer apps.

## <a name="apps-and-app-service-plans"></a>Apps und App Service-Pläne

Eine app in App Service kann jeweils nur eine App Service-Plan zugeordnet werden.

Apps und Pläne sind in einer Ressourcengruppe enthalten. Eine Ressourcengruppe dient als Lebenszyklus Grenze für jede Ressource darin. Ressourcengruppen können Sie um Teile einer Anwendung gemeinsam zu verwalten.

Da eine einzelne Ressourcengruppe mehrere App Service-Pläne verfügen kann, können Sie anderen apps auf verschiedenen physischen Ressourcen zuordnen. Beispielsweise können Sie Ressourcen Entwicklung, Test und Produktion Umgebung trennen. Mit separaten Umgebung für Produktion und Entwicklung/Test können Sie Ressourcen zu isolieren. Auf diese Weise konkurriert Auslastungstests gegen eine neue Version Ihrer Apps für dieselben Ressourcen wie die apps Produktion nicht dem Kunden dienen.

Wenn Sie mehrere Pläne in einer einzigen Ressourcengruppe verfügen, können Sie auch eine Anwendung definieren, die geografische Regionen abdeckt. Beispielsweise enthält eine hochverfügbare Anwendung in beiden Regionen mindestens zwei Pläne für jede Region und eine app jeden Plan zugeordnet. In diesem Fall werden alle Kopien der Anwendung dann in einer einzigen Ressourcengruppe enthalten. Mit einer Ressourcengruppe mehrere Pläne mit mehreren apps erleichtert das verwalten, steuern und den Zustand der Anwendung anzeigen.

## <a name="create-an-app-service-plan-or-use-existing-one"></a>Einen App Service-Plan erstellen oder vorhandene verwenden

Wenn Sie eine Anwendung erstellen, sollten Sie eine Ressourcengruppe erstellen. Auf der anderen Seite ist die Anwendung, die Sie erstellen eine Komponente für eine größere Anwendung sollten diese app innerhalb der Ressourcengruppe erstellt werden, die für die größere Anwendung zugeordnet ist.

Ob die neue Anwendung eine völlig neue Anwendung oder eine größere ist, können Sie mit einer vorhandenen Anwendung Serviceplan hosten oder einen neuen erstellen. Diese Entscheidung geht mehr Kapazität und erwartet.

Diese app wird viele Ressourcen und haben unterschiedliche Skalierungsfaktoren von anderen apps gehostet wird in einen vorhandenen Plan, sollten in einen eigenen Plan isolieren.

Beim Erstellen eines Plans können Sie einen neuen Satz von Ressourcen für Ihre app reservieren und mehr Kontrolle über die Ressource: Zuteilung denn jeden Plan einen eigenen Satz von Instanzen.

Da von apps über Pläne verschieben können Sie verändern, die Ressourcen in der größeren Anwendung zugeordnet sind.

Schließlich, erstellen Sie Sie möchten eine Anwendung in einer anderen Region und Region keinen vorhandenen Plan einen Plan in diesem Bereich zu Ihrer Anwendung hosten.

## <a name="create-an-app-service-plan"></a>Erstellen eines App Service-Plans

>[AZURE.TIP] Haben Sie eine App Service-Umgebung können Sie Dokumentation zu App Service-Umgebungen hier überprüfen: [Erstellen einer App Service-Plan in einer App Service-Umgebung](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md#createplan)

Einen leeren App Service-Plan können die App Plan durchsuchen Erfahrung oder als Teil der Anwendung erstellen.

Klicken Sie im [Azure-Portal](https://portal.azure.com) **neu** > **Web + Mobile**, und wählen Sie dann **Web App** oder andere App Service app.
![Erstellen einer Anwendung in Azure-Portal.][createWebApp]

Sie können dann wählen oder den App Service-Plan für die neue Anwendung erstellen.

 ![Erstellen eines App Service-Plans.][createASP]

Erstellen Sie einen neuen App Service-Plan auf **[+] neu erstellen**, geben Sie den Namen der **App Service-Plan** und wählen Sie einen geeigneten **Speicherort**. **Preisstufe**auf und wählen Sie dann einen entsprechenden Tarif für den Dienst. Wählen Sie **Ansicht alle** Weitere Preisoptionen **frei** und **Shared**anzeigen. Nach Auswahl den Tarif klicken Sie **Wählen** .

## <a name="move-an-app-to-a-different-app-service-plan"></a>Verschieben Sie eine Anwendung in einen anderen App Service-plan

Sie können eine andere app Service-Plan in [Azure-Portal](https://portal.azure.com)eine Anwendung verschieben. Sie können apps zwischen verschieben, solange die Pläne in derselben Ressourcengruppe und geografische Region.

Um eine Anwendung in einen anderen Plan zu verschieben, gehen Sie zu der Anwendung, die Sie verschieben möchten. Suchen Sie sich im Menü **Einstellungen** **Ändern App Service-Plan**.

**Änderungsplan App Service** öffnet **App Service-Plan** -Auswahl. An dieser Stelle Sie einen vorhandenen Plan auswählen oder einen neuen erstellen. Nur gültige Pläne (in derselben Ressourcengruppe und Lage) werden angezeigt.

![App Service-Plan-Selektor.][change]

Jedes Paket hat seinen eigenen Tarif. Beispielsweise beim Verschieben einer Website von einem freien Tier auf Standard, Ihre Anwendung jetzt können alle Funktionen und Ressourcen der Standard-Stufe.

## <a name="clone-an-app-to-a-different-app-service-plan"></a>Klonen einer Anwendung an einen anderen App Service-plan
Wenn Sie die Anwendung in einen anderen Bereich verschieben möchten, ist eine Alternative Anwendung klonen. Klonen wird eine Kopie der Anwendung in einer neuen oder vorhandenen App Service-Plan oder App Service-Umgebung in jeder Region.

 ![Klonen einer app.][appclone]

**Klon-Anwendung** finden Sie im Menü **Extras** .

Klonen hat einige Nachteile zu [Azure App Service App](../app-service-web/app-service-web-app-cloning-portal.md)Klonen mit Azure-Portal stehen.

## <a name="scale-an-app-service-plan"></a>Skalieren eines App Service-Plans

Es gibt drei Möglichkeiten, einen Plan zu skalieren:

- **Ändern des Plans Tarif**. Beispielsweise ein Plan in der grundlegenden auf Standard oder Premium konvertiert werden kann und alle apps, die mit diesem Plan sind die neue Service-Tier bietet Features verwenden.
- **Ändern der Größe des Plans**. Beispielsweise kann ein Plan in der grundlegenden Ebene, die kleine Instanzen geändert werden große Instanzen. Alle apps, die mit diesem Plan jetzt können mehr Arbeitsspeicher und CPU-Ressourcen, die größere Instanz bietet.
- **Ändern der Anzahl der Instanzen des Plans**. Beispielsweise kann ein Standard Plan, der drei Instanzen skaliert ist 10 Instanzen skaliert werden. Ein kann 20 Instanzen (Verfügbarkeit) skaliert werden. Alle apps, die mit diesem Plan jetzt können mehr Arbeitsspeicher und CPU-Ressourcen, die größere Instanzenzahl bietet.

Sie können die Preise Tier und Instanz ändern durch Klicken auf das Element **Skalieren** unter Einstellungen für die app oder App Service-Plan. App Service-Plan zuweisen und betreffen alle apps hostet.

 ![Legen Sie Werte Skalieren einer Anwendung.][pricingtier]

## <a name="app-service-plan-cleanup"></a>App Service-Plan bereinigen
**App Service-Pläne** , die keine apps zugeordnet noch anfallen denn sie konfigurierte die App Service-Plan Skalierungseigenschaften Rechenkapazität reservieren.
Um unerwartete Kosten zu vermeiden, bei die letzte Anwendung gehosteten App Service-Plan gelöscht wird, wird der resultierende leere App Serviceplan ebenfalls gelöscht.


## <a name="summary"></a>Zusammenfassung

App Service-Pläne sind Funktionen und Kapazität, die Sie in Ihre apps freigeben können. App Service-Pläne können Sie bestimmte apps auf Ressourcen und optimieren Ihre Azure Ressourcenverwendung. So möchten Sie Ihre Umgebung testen sparen können Sie einen Plan mehrere apps freigeben. Sie können auch für Ihre produktionsumgebung liegenden durch Skalierung über mehrere Regionen und Pläne.

## <a name="whats-changed"></a>Was hat sich geändert

* Eine Anleitung zur Änderung von Websites zu App Service finden Sie: [Azure App Service und seine Auswirkung auf vorhandene Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)

[pricingtier]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/appserviceplan-pricingtier.png
[assign]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/assing-appserviceplan.png
[change]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/change-appserviceplan.png
[createASP]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-appserviceplan.png
[createWebApp]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/create-web-app.png
[appclone]: ./media/azure-web-sites-web-hosting-plans-in-depth-overview/app-clone.png
