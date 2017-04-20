<properties
    pageTitle="Azure-Endpunkte"
    description="Beschreibt die Einstellungen Azure-Endpunkt in Azure Toolkit für Eclipse."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn268600.aspx -->

# <a name="azure-service-endpoints"></a>Azure-Endpunkte #

Azure Dienstendpunkte bestimmen, ob Ihre Anwendung bereitgestellt und die globale Azure-Plattform verwaltet Azure 21Vianet in China oder Private Azure-Plattform betrieben. Im Dialogfeld **Endpunkte** können Sie die Dienstendpunkte angeben soll. Öffnen Sie das Dialogfeld **Endpunkte** in Eclipse, **Fenster**, klicken Sie auf **Voreinstellungen**, **Azure**erweitern und **Endpunkte**klicken. Das Feld **Aktiv** festgelegt der Azure-Endpunkte für Azure Projekte im aktuellen Arbeitsbereich verwendet werden soll.

Die folgenden zeigt das Dialogfeld **Endpunkte** .

![][ic719493]

## <a name="to-set-the-service-endpoints"></a>Die Dienstendpunkte festlegen ##

Können Sie im Dialogfeld **Dienstendpunkte** eine der folgenden Aktionen durchführen:

* Möchten Sie die globalen Azure Plattform aus der Dropdownliste **Aktiv** wählen Sie **windowsazure.com** und auf **OK**.
* Wenn Azure von 21Vianet in China aus der Dropdownliste **Aktiv** betrieben werden soll Wählen Sie **windowsazure.cn (China)** und klicken Sie auf **OK**.
* Wenn Sie eine private Azure-Plattform verwenden möchten:
    1. Klicken Sie auf **Bearbeiten**.
    2. Dialogfeld, dass das **Dienstendpunkte** Dialogfeld geschlossen wird, und die Einstellung legt Datei geöffnet. Klicken Sie auf **OK**.
    3. Erstellen Sie in der Datei preferencesets.xml ein neues `preferenceset` Element. Erstellen Sie für dieses neue `name`, `blob`, `management`, `portalURL` und `publishsettings` Attribute und Werte für sie entsprechen Ihrer privaten Azure Plattform hinzufügen. Sie können die Werte für die vorhandene `preferenceset` Elemente als Vorlagen. **Hinweis**: der Wert für die `blob` Attribut muss den Text "Blob" in der URL enthalten.
    4. Speichern und Schließen von preferencesets.xml.
    5. Öffnen Sie im Dialogfeld **Dienstendpunkte** .
    6. Dropdownliste **Aktiviert** wählen Sie der aktiven, die Sie erstellt haben und klicken Sie auf **OK**.
    7. Nach dem Erstellen Ihrer privaten Azure Plattform `preferenceset` Element, ändern Sie die Werte, die mit der Schaltfläche **Bearbeiten** im Dialogfeld **Dienste-Endpunkt** . Sie können mehrere private Azure-Plattform `preferenceset` Elemente, falls gewünscht.

## <a name="see-also"></a>Siehe auch ##

[Azure-Toolkit für Eclipse][]

[Azure Toolkit installieren für Eclipse][] 

[Erstellen eine Hello World-Anwendung für Azure in Eclipse][]

Weitere Informationen zum Verwenden von Azure mit Java finden Sie unter [Azure Java Developer Center][].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure-Toolkit für Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Erstellen eine Hello World-Anwendung für Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Azure Toolkit installieren für Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719493]: ./media/azure-toolkit-for-eclipse-azure-service-endpoints/ic719493.png
