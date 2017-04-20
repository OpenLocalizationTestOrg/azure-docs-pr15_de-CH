<properties
    pageTitle="Echtzeit-Alarme Azure CDN | Microsoft Azure"
    description="Echtzeit-Alarme in Microsoft Azure CDN. Echtzeit-Alarme informieren über die Leistung der Endpunkte in Ihrem Profil CDN."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="casoper"/>

# <a name="real-time-alerts-in-microsoft-azure-cdn"></a>Echtzeit-Alarme in Microsoft Azure CDN

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]


## <a name="overview"></a>Übersicht

Dieses Dokument erläutert Echtzeit-Alarme in Microsoft Azure CDN. Diese Funktion bietet in Echtzeit über die Leistung der Endpunkte in Ihrem Profil CDN.  Sie können e-Mail- oder HTTP-Alerts für einrichten:

* Bandbreite
* Statuscodes
* Cache-Status
* Anschlüsse

## <a name="creating-a-real-time-alert"></a>Erstellen einer Echtzeit-Warnung

1. Suchen Sie in [Azure-Portal](https://portal.azure.com)Ihres Profils CDN.

    ![CDN Profil blade](./media/cdn-real-time-alerts/cdn-profile-blade.png)

2. Blatt Profil CDN klicken Sie auf **Verwalten** .

    ![CDN Profil Blade Schaltfläche verwalten](./media/cdn-real-time-alerts/cdn-manage-btn.png)

    Das CDN-Verwaltungsportal wird geöffnet.

3. Hovern Sie über die Registerkarte **Analytics** und Maus über Flyout **In Echtzeit Statistiken** .  Klicken Sie auf **Echtzeit-Alarme**.

    ![CDN-Verwaltungsportal](./media/cdn-real-time-alerts/cdn-premium-portal.png)

    Die Liste der vorhandenen Warnungskonfigurationen (falls vorhanden) wird angezeigt.

4. Klicken Sie **Warnung hinzufügen** .

    ![Schaltfläche Alert hinzufügen](./media/cdn-real-time-alerts/cdn-add-alert.png)

    Formular zum Erstellen einer neuen Warnung wird angezeigt.

    ![Neue Warnung Formular](./media/cdn-real-time-alerts/cdn-new-alert.png)

5. Soll diese Warnung aktiv sein, wenn Sie auf **Speichern**klicken, aktivieren Sie das Kontrollkästchen **Warnung aktiviert** .

6. Geben Sie im Feld **Name** einen beschreibenden Namen für die Warnung.

7. Wählen Sie in der Dropdownliste **Medientyp** **HTTP Large Object**.

    ![Medientyp HTTP große Objekt ausgewählt](./media/cdn-real-time-alerts/cdn-http-large.png)

    > [AZURE.IMPORTANT] Wählen Sie den **Medientyp** **HTTP Large Object** .  Die anderen Optionen werden nicht von **Azure CDN von Verizon**verwendet.  Wählen Sie **HTTP Large Object** wird die Warnung niemals ausgelöst fehlschlägt.

8. Erstellen Sie einen **Ausdruck** , indem eine **Metrik**, **Operator**und **Triggerwert**überwachen.

    - **Metrik**wählen Sie den Typ der Bedingung, die überwacht werden sollen.  **Mbit/s Bandbreite** ist die Bandbreite in Megabit pro Sekunde.  **Gesamt-Verbindungen** ist die Anzahl der gleichzeitigen HTTP-Verbindungen Edge-Server.  Definitionen der verschiedenen Cache Status und Statuscodes finden Sie unter [Azure CDN Cache-Statuscodes](https://msdn.microsoft.com/library/mt759237.aspx) und [Azure CDN HTTP-Statuscodes](https://msdn.microsoft.com/library/mt759238.aspx)
    - **Operator** ist der mathematische Operator, der die Beziehung zwischen der Metrik und der Trigger wird.
    - **Triggerwert** ist der Schwellenwert, der erfüllt sein muss, bevor eine Benachrichtigung gesendet wird.

    In dem folgenden Beispiel ich habe Ausdruck gibt an, dass möchte benachrichtigt werden, wenn die Anzahl der 404 Statuscodes größer als 25 ist.

    ![Echtzeit-alert Beispielausdruck](./media/cdn-real-time-alerts/cdn-expression.png)

9. Geben Sie für **Intervall**Häufigkeit den Ausdruck ausgewertet werden soll.

10. Wählen Sie in der Dropdownliste **auf benachrichtigen** Wenn möchten Sie benachrichtigt werden, wenn der Ausdruck wahr ist.
    
    - **Zustand** zeigt an, dass eine Benachrichtigung gesendet wird, wenn die angegebene Bedingung zuerst erkannt wird.
    - **Ende Bedingung** gibt an, dass eine Benachrichtigung gesendet wird, wenn die angegebene Bedingung nicht mehr erkannt wird. Diese Benachrichtigung kann nur ausgelöst werden, nachdem unser Netzwerk Überwachungssystem hat festgestellt, dass die angegebene Bedingung aufgetreten ist.
    - **Kontinuierliche** gibt an, dass eine Benachrichtigung jedes Mal gesendet wird,, Netzwerk Überwachung die angegebene Bedingung erkennt. Denken Sie daran, die Überwachung Netzwerk nur einmal pro Intervall für die angegebene Bedingung überprüfen.
    - **Zustand und Ende** gibt an, dass eine Benachrichtigung beim ersten gesendet wird, dass die angegebene Bedingung erkannt und wieder als Bedingung wird nicht mehr erkannt.

11. Aktivieren Sie per e-Mail benachrichtigt werden soll, das Kontrollkästchen **Benachrichtigung per E-Mail** .  

    ![Benachrichtigen von e-Mail-Formular](./media/cdn-real-time-alerts/cdn-notify-email.png)
    
    Geben Sie im Feld **an** die e-Mail-Adresse Sie Benachrichtigungen soll gesendet. **Betreff** und **Nachrichtentext**kann der Standardwert oder die Nachricht mit der Liste der **verfügbaren Schlüsselwörter** Daten dynamisch einfügen, wenn die Nachricht möglicherweise anpassen.

    > [AZURE.NOTE] Testen Sie die e-Mail-Benachrichtigung **Benachrichtigung testen** klicken, jedoch erst, nachdem die Konfiguration die Warnung gespeichert wurde.

12. Aktivieren Sie Benachrichtigung an einen Webserver gesendet werden soll, das Kontrollkästchen **Benachrichtigen über HTTP Post** .

    ![Benachrichtigen von HTTP-Post-Formular](./media/cdn-real-time-alerts/cdn-notify-http.png)

    Geben Sie im Feld **Url** URL Sie HTTP-Nachricht soll gebucht. Geben Sie im Textfeld **Header** der HTTP-Header in der Anforderung gesendet werden.  **Körper** kann die Nachricht mit der Liste der **verfügbaren Schlüsselwörter** Daten dynamisch einfügen, wenn die Nachricht anpassen.  **Header** und **Text** standardmäßig XML-Nutzlast wie das Beispiel unten.

    ```
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">
        <![CDATA[Expression=Status Code : 404 per second > 25&Metric=Status Code : 404 per second&CurrentValue=[CurrentValue]&NotificationCondition=Condition Start]]>
    </string>
    ```

    > [AZURE.NOTE] Testen Sie die HTTP Post-Benachrichtigung **Benachrichtigung testen** klicken, jedoch erst, nachdem die Konfiguration die Warnung gespeichert wurde.

13. Klicken Sie auf **Speichern** , um die Warnung Konfiguration speichern.  **Warnung aktiviert** in Schritt 5 aktiviert, ist die Warnung jetzt aktiv.

## <a name="next-steps"></a>Nächste Schritte

- Analyse [in Echtzeit Statistiken in Azure CDN](cdn-real-time-stats.md)
- Tiefer [Erweiterte HTTP-](cdn-advanced-http-reports.md) Berichte
- Analysieren [von Verwendungsmustern](cdn-analyze-usage-patterns.md)

