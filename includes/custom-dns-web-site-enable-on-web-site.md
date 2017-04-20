Nachdem die Datensätze für Ihren Domänennamen weitergegeben wurden, müssen Sie sie mit Ihrer Anwendung zuordnen. Gehen Sie mithilfe Ihres Webbrowsers Domänennamen aktivieren.

> [AZURE.NOTE] Es dauert einige Zeit TXT-Datensätze, die in den vorherigen Schritten verbreiten über das DNS-System erstellt. Sie können Ihrer Anwendung den Domänennamen des hinzufügen, bis der TXT-Datensatz weitergegeben wurde. Bei Verwendung ein A-Datensatzes können Sie Hinzufügen der Datensatz ein Domänenname Ihrer Anwendung erst im vorherigen Schritt erstellten TXT-Datensatz weitergegeben wurde.
>
> Können Sie einen Dienst wie <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> überprüfen die TXT-Datensätze verfügbar.

1. Öffnen Sie in Ihrem Browser [Azure-Portal](https://portal.azure.com).

2. Auf der Registerkarte **Web Apps** auf den Namen Ihrer Anwendung, und wählen Sie **benutzerdefinierte Domänen**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. Klicken Sie auf **Add Hostname**Blatt **Custom Domains** .
    
4. Mithilfe der Textfelder **Hostname** Geben Sie die Domänennamen dieser Webanwendung zugeordnet.

    ![](./media/custom-dns-web-site/add-custom-domain.png)

6.  Klicken Sie auf **Überprüfen**.

7.  Beim Klicken auf **Validate** Azure Auftakt Domain Verification Workflow. Dies überprüft Domäne Besitz Hostname Verfügbarkeit und Bericht erfolgreich oder Fehler mit ausführlichen Anleitung zur Fehlerbehebung.    

An diesem Punkt sollten Sie möglicherweise den benutzerdefinierten Domänennamen in Ihrem Browser eingeben und erfolgreich gelangen zu Ihrer Anwendung.
