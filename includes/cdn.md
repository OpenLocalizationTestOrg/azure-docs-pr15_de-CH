# <a name="using-cdn-for-azure"></a>Verwenden für Azure CDN

Azure Content Delivery Network (CDN) bietet Entwicklern eine globale Lösung für Inhalte durch Zwischenspeichern Blobs und statische Inhalte Serverinstanzen auf physischen Knoten in den Vereinigten Staaten, Europa, Asien, Australien und Südamerika. Eine aktuelle Liste der CDN Knotenpositionen anzeigen Sie [Azure CDN Knotenpositionen]

Dieser Vorgang umfasst folgende Schritte:

* [Schritt 1: Erstellen Sie ein Speicherkonto](#Step1)
* [Schritt 2: Erstellen eines neuen CDN-Endpunkts für das Speicherkonto](#Step2)
* [Schritt 3: Zugriff auf Ihre Inhalte CDN](#Step3)
* [Schritt 4: Entfernen des CDN-Inhalts](#Step4)

Die Vorteile von CDN Azure Daten gehören:

-   Bessere Leistung und Erfahrung für Endbenutzer, die eine Inhaltsquelle sind, und Applikationen, müssen viele 'Internet Trips Inhalt geladen
-   Umfangreiche verteilte unmittelbaren bei hohen Belastung besser behandeln sagen zu Beginn ein Ereignis wie eine Produktlaunch

CDN-Kunden können jetzt Azure CDN im [klassischen Azure-Portal]. Das CDN ist ein Add-on zu Ihrem Abonnement und einen separaten [Fakturierungsplan].

<a id="Step1"> </a>
<h2>Schritt 1: Erstellen Sie ein Speicherkonto</h2>

Gehen Sie folgendermaßen vor, ein neues Speicherkonto ein Azure-Abonnement erstellen. Ein Speicherkonto ermöglicht Zugriff auf Azure-Speicherdienste. Das Speicherkonto stellt die höchste Ebene des Namespace für den Zugriff auf alle Komponenten des Azure-Speicher: BLOB-Services, Warteschlangendienste und -Dienste. Weitere Informationen zu Azure-Speicherdienste finden Sie unter [Verwenden der Azure-Speicherdienste](http://msdn.microsoft.com/library/azure/gg433040.aspx).

Erstellen Sie ein Speicherkonto muss Dienstadministrator oder Co-Administrator für das Abonnement zugeordnet werden.

> [AZURE.NOTE] Informationen zu diesem Vorgang mithilfe der Azure-Dienst-API finden Sie unter Referenzthema [Speicherkonto erstellen](http://msdn.microsoft.com/library/windowsazure/hh264518.aspx) .

**Ein Speicherkonto Azure-Abonnement erstellen**

1.  [Klassische Azure-Portal]anmelden.
2.  Klicken Sie in der unteren linken Ecke auf **neu**. Im Dialogfeld **neu** , klicken Sie auf **Speicher** **Schnell erstellen** **Data Services**.

    Das Dialogfenster **Speicherkonto erstellen** .

    ![Konto erstellen][create-new-storage-account]

4. Geben Sie im Feld **URL** eine Subdomainnamen. Dieser Eintrag kann 3 24 Kleinbuchstaben und Zahlen enthalten.

    Dieser Wert wird der Hostname in den URI an Ressourcen-Blob, Warteschlange oder Tabelle für das Abonnement verwendet wird. Um eine Ressource Container im Blob-Dienst zu beheben, verwenden Sie einen URI im folgenden Format, * &lt;StorageAccountLabel&gt; * bezieht sich auf den eingegebenen **URL eingeben**:

    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;Mycontainer&gt; *

    **Wichtig:** Das URL-Label muss Subdomäne Speicherkonto URI und für alle gehosteten Dienste in Azure eindeutig.

    Dieser Wert dient auch als Namen für dieses Speicherkonto im Portal oder dieses Konto programmgesteuerten Zugriff auf.

5.  Wählen Sie aus der Dropdown-Liste **Region/Gruppe** eine Region oder eine Gruppe für das Speicherkonto. Soll der Speicherdienste im gleichen Rechenzentrum mit anderen Windows Azure Services, die Sie verwenden, wählen Sie eine Gruppe anstelle einer Region. Dadurch kann die Leistung verbessert und keine Gebühren für Ausgang.  

    **Hinweis:** Erstellen Sie eine Gruppe **Einstellungsbereich Verwaltungsportal** öffnen klicken Sie auf **Gruppen**und **Hinzufügen einer Gruppe** oder **Hinzufügen**klicken. Sie können auch mit der Windows Azure Management API Gruppen verwalten. Weitere Informationen finden Sie unter [Vorgänge für Gruppen].

6. Wählen Sie aus der Dropdownliste **Abonnement** das Speicherkonto mit Abonnements.
7.  Klicken Sie auf **Konto erstellen**. Erstellen Sie das Speicherkonto möglicherweise mehrere Minuten.
8.  Überprüfen das Storage-Konto erfolgreich erstellt wurde, überprüfen Sie, ob das Konto in die Artikel **Speicher** mit dem Status **Online**angezeigt.

<a id="Step2"> </a>
<h2>Schritt 2: Erstellen eines neuen CDN-Endpunkts für das Speicherkonto</h2>

Ermöglichen den Zugriff auf ein Speicherkonto CDN oder gehosteten Dienst sind alle öffentlich verfügbaren Objekte CDN Rand Zwischenspeichern. Wenn Sie ein Objekt, die derzeit in der CDN zwischengespeichert ändern, wird der neue Inhalt nicht über die CDN bis CDN seinen Inhalt aktualisiert, wenn die zwischengespeicherten Inhalte TTL Ablauf.

**Neue CDN für das Speicherkonto erstellen**

1. Klicken Sie im [klassischen Azure-Portal]im Navigationsbereich auf **CDN**.

2. Klicken Sie auf der Multifunktionsleiste auf **neu**. Wählen Sie im Dialogfeld **neue** **Anwendungsdienste** **CDN**und dann **Schnell erstellen**.

3. Wählen Sie in der Dropdownliste **Ursprungsdomäne** Speicherkonto, die, das Sie im vorherigen Abschnitt aus der Liste der verfügbaren Speicherkonten erstellt. 

4. Klicken Sie auf **Erstellen** , um den neuen Endpunkt zu erstellen.

5. Sobald Endpunkt erstellt wird, wird er in eine Liste von Endpunkten für das Abonnement. Die Listenansicht zeigt die URL auf zwischengespeicherte Inhalte sowie der Ursprungsdomäne zugreifen. 

    Die Ursprungsdomäne ist der Speicherort, aus dem CDN Inhalt zwischengespeichert. Die Ursprungsdomäne kann ein Speicherkonto oder einen Cloud-Dienst; für die Zwecke dieses Beispiels wird ein Speicherkonto verwendet. Inhalt wird zwischengespeichert, Edge-Server nach einer Cache-Einstellung, die Sie angeben, oder der Standardheuristik Zwischenspeichern Netzwerk. 


    > [AZURE.NOTE] Die Konfiguration für den Endpunkt erstellt sofort nicht zur Verfügung; Es dauert bis zu 60 Minuten die Registrierung über das CDN-Netzwerk weitergegeben. Benutzer versuchen, die CDN sofort verwenden erhalten Statuscode 400 (Ungültige Anforderung), bis der Inhalt über die CDN verfügbar ist.

<a id="Step3"> </a>
<h2>Schritt 3: Access CDN content</h2> 

Um zwischengespeicherte Inhalte auf das CDN zuzugreifen, verwenden Sie die CDN-URL im Portal. Die Adresse für zwischengespeicherte BLOBs werden ähnlich der folgenden:

http://<*CDNNamespace*\>.vo.msecnd.net/ <*MyPublicContainer*\>/<*BlobName*\>

<a id="Step4"> </a>
<h2>Schritt 4: Entfernen von Inhalten aus dem CDN</h2>

Wenn Sie kein Objekt in Azure Content Delivery Network (CDN) zwischenspeichern möchten, nehmen Sie die folgenden Schritte aus:

-   Ein Azure BLOB können Sie das Blob vom öffentlichen Container löschen.
-   Container stellen Private statt öffentlichen. Weitere Informationen finden Sie unter [Einschränken des Zugriffs auf Container und Blobs](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/#restrict-access-to-containers-and-blobs) .
-   Sie können mit dem Verwaltungsportal CDN-Endpunkt löschen oder deaktivieren.
-   Sie können Ihr gehosteten Dienst reagiert nicht mehr auf das Objekt.

Ein Objekt im CDN zwischengespeichert bleiben bis TTL für das Objekt Ablauf Cache. Wenn der TTL Ablauf überprüfen CDN, ob CDN-Endpunkt gültig ist und das Objekt weiterhin anonym zugegriffen werden kann. Ist dies nicht, wird das Objekt nicht zwischengespeichert.

Die Möglichkeit, Inhalte sofort löschen wird auf Azure-Verwaltungsportal derzeit nicht unterstützt. Wenden Sie [Azure-support](https://azure.microsoft.com/support/options/) benötigen Sie sofort Inhalt löschen. 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

-   [Erstellen einer Gruppe in Azure]
-   [Wie: Speicherkonten für Azure-Abonnement verwalten]
-   [Über Servicemanagement API]
-   [Zum Zuordnen von CDN Inhalt zu einer benutzerdefinierten Domäne]

  [Create Storage Account]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/
  [Azure CDN Knotenpositionen]: http://msdn.microsoft.com/library/windowsazure/gg680302.aspx
  [Azure-Verwaltungsportal]: https://manage.windowsazure.com/
  [Fakturierungsplan]: /pricing/calculator/?scenario=full
  [Erstellen einer Gruppe in Azure]: http://msdn.microsoft.com/library/azure/ee460798.aspx
  [Overview of the Azure CDN]: http://msdn.microsoft.com/library/windowsazure/ff919703.aspx
  [Über Servicemanagement API]: http://msdn.microsoft.com/library/windowsazure/ee460807.aspx
  [Zum Zuordnen von CDN Inhalt zu einer benutzerdefinierten Domäne]: http://msdn.microsoft.com/library/windowsazure/gg680307.aspx


[create-new-storage-account]: ./media/cdn/CDN_CreateNewStorageAcct.png
[Previous Management Portal]: ../../Shared/Media/previous-portal.png
