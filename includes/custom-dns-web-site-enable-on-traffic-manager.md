Nachdem die Datensätze für Ihren Domänennamen weitergegeben haben, sollten Sie möglicherweise Ihren Browser verwenden, um sicherzustellen, dass Ihre benutzerdefinierten Domänennamen auf Ihrer Anwendung in Azure App Service verwendet werden kann.

> [AZURE.NOTE] Es dauert einige Zeit Ihre CNAME über das DNS-System übertragen. Einen Dienst wie <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> können Sie überprüfen, ob der CNAME-Eintrag verfügbar ist.

Wenn Ihrer Anwendung als Endpunkt Traffic Manager noch nicht hinzugefügt haben, müssen Sie dazu Auflösung als benutzerdefinierte Domain Name Routen Traffic Manager arbeiten wird. Traffic Manager und Routen zu Ihrer Anwendung. Verwenden Sie die Informationen in [Delete Endpoints](../articles/traffic-manager/traffic-manager-endpoints.md) Ihrer Anwendung als Endpunkt der Traffic Manager-Profil hinzufügen.

> [AZURE.NOTE] Ihrer Anwendung nicht aufgelistet ist, wenn Sie einen Endpunkt hinzufügen, sicher, dass **Standard** App Service Plan-Modus konfiguriert ist. **Standardmodus für Ihr Web app** verwenden mit Traffic Manager arbeiten.

1. Öffnen Sie in Ihrem Browser [Azure-Portal](https://portal.azure.com).

1. Auf der Registerkarte **Web Apps** Ihrer Anwendung klicken Sie auf **Bearbeiten Sie**und wählen Sie **benutzerdefinierte Domänen**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

1. Klicken Sie auf **Add Hostname**Blatt **Custom Domains** .
    
1. Verwenden Sie Textfelder **Hostname** geben den Domänennamen Traffic Manager Web app zugeordnet.

    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)

1. Klicken Sie auf **Überprüfen** , um die Domänenkonfiguration Namen speichern.

7.  Beim Klicken auf **Validate** Azure Auftakt Domain Verification Workflow. Dies überprüft Domäne Besitz Hostname Verfügbarkeit und Bericht erfolgreich oder Fehler mit ausführlichen Anleitung zur Fehlerbehebung.    

8.  Nach erfolgreicher Überprüfung **Hostname hinzufügen** Schaltfläche aktiv, und Sie werden Hostnamen zuordnen. Jetzt Ihre benutzerdefinierten Domänennamen in einem Browser navigieren. Dem app mit benutzerdefinierten Domänennamen sollte jetzt angezeigt werden. 

    Nach Abschluss der Konfiguration wird der benutzerdefinierten Domänennamen im Abschnitt **Domänennamen** Ihrer Anwendung aufgeführt.

An diesem Punkt sollten Sie möglicherweise den Namen Traffic Manager Domänennamen in Ihren Browser eingeben und erfolgreich gelangen zu Ihrer Anwendung.
