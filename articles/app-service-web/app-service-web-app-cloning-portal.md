<properties
    pageTitle="Web App Klonen mit Azure-Portal"
    description="Erfahren Sie, wie Ihr Web Apps neue Web Apps mithilfe von Azure-Portal zu klonen."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/08/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-azure-portal"></a>Klonen von Azure-Portal mit Azure App Service-App#

Cloning Feature in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) können Sie einfach vorhandene Web apps eine neu erstellte Anwendung in einer anderen Region oder in derselben Region klonen. Dadurch können Kunden eine Anzahl von apps in verschiedenen Regionen schnell und einfach.

Cloning-App ist zurzeit nur für Premium-Tier-Anwendung Servicepläne unterstützt. Das neue Feature verwendet die gleichen Grenzen wie Web Apps Sicherungsfeature, finden Sie unter [Sichern einer Web-app in Azure App Service](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 


## <a name="cloning-an-existing-app"></a>Klonen einer vorhandenen Anwendung ##

Web app muss in den Modus **Premium** zu klonen für Web app ausgeführt werden.

1. Öffnen Sie in [Azure-Portal](https://portal.azure.com/)Web app Blade.
2. Klicken Sie auf **Extras**. Blatt **Tools** klicken Sie **Clone App**.

    ![][1]

    > [AZURE.NOTE]
    > Ist die Web app nicht bereits im **Premium** -Modus, erhalten Sie eine Meldung mit den unterstützten Modi zum Klonen app. An diesem Punkt müssen Sie **Aktualisieren**auswählen.
    
3. **Clone-App** Blatt Benennen der neuen Web app, Ressourcengruppe und App Service-Plan. Der Benutzer wird auch wählen, ob eine Reihe von Quelle Web app Klonen oder nicht.

    ![][2]

4. Nach dem Klicken auf **Erstellen** wird die Plattform arbeiten auf einen Klon der Quelle Web app.

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Klonen einer vorhandenen Anwendung einer App Service-Umgebung##

**Clone App** Blatt müssen Kunden in einer vorhandenen App Service-Umgebung ein Anwendungspool auswählen.

## <a name="current-restrictions"></a>Aktuelle Einschränkungen ##

Diese Funktion ist zurzeit in der Vorschau, wir arbeiten daran, neue Funktionen hinzufügen sind folgende bekannten Einschränkungen auf aktuelle Anwendung Klonen in Azure-Portal:

- Azure Traffic Manager Settings werden nicht geklont.
- Automatische Einstellungen werden nicht geklont.
- Sicherungszeitplan Einstellungen werden nicht geklont.
- VNET-Einstellungen werden nicht geklont.
- App Erkenntnisse werden nicht automatisch auf Ziel Web app eingerichtet
- Einfache Authentifizierung Einstellungen werden nicht geklont.
- Kudu Erweiterung werden nicht geklont.
- Tipp Regeln werden nicht geklont.
- Inhalte werden nicht geklont.


### <a name="references"></a>Referenzen ###
- [Web App Klonen mit PowerShell](app-service-web-app-cloning.md)
- [Sichern Sie eine Webanwendung in Azure App Service](web-sites-backup.md)
- [Erstellen eine App Service-Umgebung](app-service-web-how-to-create-an-app-service-environment.md)
- [Erstellen Sie eine Webanwendung in einer App Service-Umgebung](app-service-web-how-to-create-a-web-app-in-an-ase.md)
- [Einführung in App Service-Umgebung](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png