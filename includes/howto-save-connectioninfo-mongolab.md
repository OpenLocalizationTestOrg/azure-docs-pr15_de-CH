Es ist, zwar möglich, ein MongoLab URI in den Code einfügen empfehlen wir in der Umgebung zur Vereinfachung der Verwaltung konfigurieren. Auf diese Weise ändert der URI können Sie es über das Azure-Portal aktualisieren ohne Code.


1. Wählen Sie im Portal Azure **Web Apps**.
1. Klicken Sie auf Web app im Web Apps-Liste.  
![WebAppEntry][entry-website]  
Web App Dashboard wird angezeigt.

1. Klicken Sie in der Menüleiste auf **Konfigurieren** .  
![WebAppDashboardConfig][focus-mongolab-websitedashboard-config]

1. Scrollen Sie im Abschnitt Connection Strings.  
![WebAppConnectionStrings][focus-mongolab-websiteconnectionstring]

1. Geben Sie MONGOLAB_URI **Namen**.
1. Fügen Sie **Wert**der Verbindungszeichenfolge im Abschnitt erhalten.
1. Wählen Sie **Benutzerdefiniert** in **der Dropdown-Liste (anstelle des **SQLAzure**)** .
1. Klicken Sie auf der Symbolleiste auf **Speichern** .  
![SaveWebApp][button-website-save]

**Hinweis:** Azure fügt die **CUSTOMCONNSTR\_ ** Code über Verweise auf diese Variable, weshalb Präfix **CUSTOMCONNSTR\_MONGOLAB_URI.**

[entry-website]: ./media/howto-save-connectioninfo-mongolab/entry-website.png
[focus-mongolab-websitedashboard-config]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websitedashboard-config.png
[focus-mongolab-websiteconnectionstring]: ./media/howto-save-connectioninfo-mongolab/focus-mongolab-websiteconnectionstring.png
[button-website-save]: ./media/howto-save-connectioninfo-mongolab/button-website-save.png
