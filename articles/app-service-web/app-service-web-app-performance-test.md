<properties
   pageTitle="Testen Sie Ihre Azure Web app Leistung | Microsoft Azure"
   description="Führen Sie Azure app Webleistungstests überprüfen, wie Ihre app Benutzerlast behandelt. Reaktionszeit messen und Fehler, die möglicherweise auf Probleme hinweisen zu finden."
   services="app-service\web"
   documentationCenter=""
   authors="ecfan"
   manager="douge"
   editor="jimbe"/>

<tags
   ms.service="app-service-web"
   ms.workload="web"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="05/25/2016"
   ms.author="estfan; manasma; ahomer"/>

# <a name="performance-test-your-azure-web-app-under-load"></a>Leistungstest Ihrer Azure Anwendung unter Belastung

Überprüfen Sie Ihre Web-app Leistung starten oder Updates für die Produktion bereitstellen. So können Sie besser beurteilen, ob Ihre Anwendung zum Release bereit ist. Sicherer fühlen Sie, dass Ihre Anwendung den Datenverkehr während Spitzenzeiten oder die nächste Marketingkampagne verarbeiten kann.

Während der öffentlichen Vorschau können Sie Leistungstests Ihrer Anwendung in Azure-Portal.
Diese Tests simulieren Benutzerlast auf Ihre Anwendung über einen bestimmten Zeitraum und Ihre app-Antwort messen. Die Testergebnisse anzeigen, wie schnell eine bestimmte Anzahl von Benutzern Ihrer Anwendung entspricht. Außerdem wird gezeigt, wie viele Anfragen konnte die möglicherweise Probleme mit Ihrer Anwendung.      

![Performance-Probleme in Ihrer Anwendung suchen](./media/app-service-web-app-performance-test/azure-np-perf-test-overview.png)

## <a name="before-you-start"></a>Bevor Sie beginnen

* Ein [Azure-Abonnement](https://account.windowsazure.com/subscriptions)benötigen, wenn Sie nicht bereits eine haben. Erfahren Sie, wie Sie [ein Azure-Konto frei](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

* Sie benötigen [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) -Konto zu Ihrer Leistung testen. Ein geeignetes Konto wird beim Einrichten der Leistungstest automatisch erstellt. Oder ein neues Konto erstellen oder ein vorhandenes Konto verwenden, wenn Sie Konto ist. 

* Bereitstellen Sie Ihre Anwendung für Tests in einer nicht produktiven Umgebung. Haben Sie Ihre app verwenden Sie einen App Service-Plan als den Plan in der Produktion verwendet. Auf diese Weise nicht Sie Einfluss auf vorhandenen Kunden oder Ihre Anwendung in der Produktion verlangsamen. 

## <a name="set-up-and-run-your-performance-test"></a>Richten Sie ein und führen Sie der Leistungstest aus

0.  [Azure-Portal](https://portal.azure.com)anmelden. Um Visual Studio Team Services-Konto zu verwenden, die Sie besitzen, melden Sie sich als Konto an.

0.  Gehen Sie zu Ihrer Anwendung.

    ![Besuchen Sie durchsuchen alle Web Apps Ihrer Anwendung](./media/app-service-web-app-performance-test/azure-np-web-apps.png)

0.  Fahren Sie mit **Leistungstests**.

    ![Extras, Leistungstest](./media/app-service-web-app-performance-test/azure-np-web-app-details-tools-expanded.png)
 
0. Nun verknüpfen Sie ein Konto für [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) Ihre Leistungsverlauf beibehalten.

    Haben Sie ein Team Services-Konto verwenden, wählen Sie das Konto. Wenn nicht, erstellen Sie ein neues Konto.

    ![Team Services-Konto auswählen oder ein neues Konto erstellen](./media/app-service-web-app-performance-test/azure-np-no-vso-account.png)

0.  Erstellen des Performance-Tests. Legen Sie die Details, und führen Sie den Test. 

Sie sehen die Ergebnisse in Echtzeit, während der Test ausgeführt wird.

Angenommen Sie, wir haben eine app, die an letzten Jahr Urlaub Verkauf gab. Dieses Ereignis dauerte 15 Minuten mit einer Belastung von 100 gleichzeitigen Kunden. Wir möchten die Kunden dieses Jahr verdoppeln. Wir möchten Kundenzufriedenheit die Ladezeit von 5 Sekunden auf 2 Sekunden reduziert. So testen wir unsere aktualisierten app-Leistung mit 250 Benutzern für 15 Minuten.

Wir werden laden unsere Anwendung simulieren, indem Sie virtuelle Benutzer (Kunden) generieren, die unsere Website gleichzeitig. Dies zeigt uns wie viele Anfragen fehlschlägt oder langsam reagiert.

  ![Erstellen und Einrichten der Leistungstest ausführen](./media/app-service-web-app-performance-test/azure-np-new-performance-test.png)

   *  Standard-URL für Ihre Webanwendung wird automatisch hinzugefügt. 
   Sie können die URL testen andere Seiten (nur HTTP GET-Anfragen) ändern.

   *  Umweltbedingungen simulieren Latenz und wählen Sie die Benutzer am nächsten zum Generieren von laden.

  Hier ist der Test durchgeführt. Während der ersten Minute langsamer möchten wir unsere Seite geladen.

  ![Leistungstest mit Echtzeitdaten](./media/app-service-web-app-performance-test/azure-np-running-perf-test.png)

  Nach Abschluss des Tests erfahren wir, dass die Seite nach der ersten Minute schneller geladen. Dadurch identifizieren, in denen wir die Problembehandlung starten möchten.

  ![Abgeschlossene Leistungstest zeigt Ergebnisse, einschließlich fehlgeschlagene Anfragen](./media/app-service-web-app-performance-test/azure-np-perf-test-done.png)

## <a name="test-multiple-urls"></a>Testen Sie mehrere URLs

Sie können auch mit mehreren URLs, die eine End-to-End-Szenario dargestellt durch Hochladen einer Datei Visual Studio Web Test Leistungstests ausführen. Einige der Methoden können Sie ein Visual Studio Webtest erstellt Datei:

* [Datenverkehr mit Fiddler und als Visual Studio Web Test-Datei exportieren](http://docs.telerik.com/fiddler/Save-And-Load-Traffic/Tasks/VSWebTest)
* [Erstellen Sie in Visual Studio Load Testdatei](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release)

Hochladen und Visual Studio Web Test-Datei ausführen:
 
0. Die obigen Schritte Blade **Neue Webleistungstest** geöffnet.
   Auf diesem Blatt die Option CONFIGFURE testen verwenden, öffnen Sie das Blade **mit konfigurieren** .  

    ![Öffnen der konfigurieren Auslastungstests blade](./media/app-service-web-app-performance-test/multiple-01-authoring-blade.png)

0. Überprüfen Sie **Visual Studio Web Test** den TESTTYP fest und wählen Sie die HTTP-Archivdatei.
    Verwenden Sie das Symbol "Ordner" Auswahldialogfeld Datei öffnen.

    ![Mehrere URL Visual Studio Web Test-Datei hochladen](./media/app-service-web-app-performance-test/multiple-01-authoring-blade2.png)

    Nachdem die Datei hochgeladen wurde, sehen Sie die Liste der URLs in der URL Detailabschnitt getestet werden.
 
0. Geben Sie die Benutzerlast Testdauer und **Tests**auswählen.

    ![Auswählen von Benutzern und Dauer](./media/app-service-web-app-performance-test/multiple-01-authoring-blade3.png)

    Nachdem der Test abgeschlossen ist, sehen Sie die Ergebnisse in zwei Bereichen. Links werden Performnace Informationen als eine Reihe von Diagrammen.

    ![Im Ergebnisbereich Leistung](./media/app-service-web-app-performance-test/multiple-01a-results.png)

    Rechten Bereich zeigt eine Liste der fehlgeschlagenen Anfragen mit den Fehlertyp der oft kam.

    ![Die Anforderung Fehler im Bereich](./media/app-service-web-app-performance-test/multiple-01b-results.png)

0. Führen Sie den Test erneut, wählen Sie oben im rechten Bereich auf das Symbol **erneut ausführen** .

    ![Erneutes Ausführen des Tests](./media/app-service-web-app-performance-test/multiple-rerun-test.png)

##  <a name="q--a"></a>Fragen & Antworten

#### <a name="q-is-there-a-limit-on-how-long-i-can-run-a-test"></a>F: Gibt es eine Beschränkung auf wie lange ich einen Test ausführen? 

**A**: Ja, den Test bis zu einer Stunde in Azure-Portal ausgeführt werden können.

#### <a name="q-how-much-time-do-i-get-to-run-performance-tests"></a>F: wie lange erhalte Leistungstests ausführen? 

**A**: nach öffentlichen Vorschau erhalten Sie 20.000 virtueller benutzerminuten (VUMs) jeden Monat mit Visual Studio Team Services-Konto. Ein VUM ist die Anzahl der virtuellen Benutzer multipliziert mit der Anzahl der Minuten im Test. Wenn Ihre Bedürfnisse freie Grenze überschreiten, können weitere kaufen und nur bezahlen, was Sie verwenden.

#### <a name="q-where-can-i-check-how-many-vums-ive-used-so-far"></a>F: Wo kann ich prüfen, wie viele VUMs ich bisher verwendet habe?

**A**: Überprüfen Sie diesen Betrag im Azure-Portal.

![Gehen Sie zu Ihrem Team Services-Konto](./media/app-service-web-app-performance-test/azure-np-vso-accounts.png)

![Überprüfen von VUMs verwendet](./media/app-service-web-app-performance-test/azure-np-vso-accounts-vum-summary.png)

#### <a name="q-what-is-the-default-option-and-are-my-existing-tests-impacted"></a>Was ist die Standardoption und betroffen meine vorhandene Tests sind?

**A**: die Standardoption für Auslastungstests Leistung ist ein manueller Test - wie vor mehreren URL zu testen Portal hinzugefügt wurde.
Vorhandene konfigurierte URL verwenden und funktionieren wie zuvor.

#### <a name="q-what-features-not-supported-in-the-visual-studio-web-test-file"></a>F: Welche features in Visual Studio Web Test-Datei nicht unterstützt?

**A**: derzeit unterstützt dieses Feature nicht Webtest-Plug-ins, Datenquellen und Extraktionsregeln. Sie müssen die Webtest-Datei entfernen diese bearbeiten. Wir hoffen, Unterstützung für diese Funktionen in zukünftigen Updates hinzuzufügen.

#### <a name="q-does-it-support-any-other-web-test-file-formats"></a>F: unterstützt wird anderen Webtest-Dateiformaten?
  
**A**: vorhanden nur Visual Studio Web Test Dateien werden unterstützt.
Wir gerne von Ihnen hören, benötigen Sie Unterstützung für andere Dateiformate. E-Mail an [vsoloadtest@microsoft.com](mailto:vsoloadtest@microsoft.com).

#### <a name="q-what-else-can-i-do-with-a-visual-studio-team-services-account"></a>F: Was kann ich mit Visual Studio Team Services-Konto?

**Ein**: das neue Konto finden Sie, ```https://{accountname}.visualstudio.com```. Freigeben von Code, erstellen, testen, Arbeit und liefern Software – alles in der Cloud mit einem Tool oder Sprache. Hier erfahren Sie, wie [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) -Features und Dienste dienen mehr zusammenarbeiten und kontinuierlich bereitstellen.

## <a name="see-also"></a>Siehe auch

* [Einfache Cloud-Leistungstests ausführen](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-simple-cloud-load-test)
* [Apache Jmeter Leistungstests ausführen](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-jmeter-test)
* [Aufzeichnen und Wiedergeben von Cloud-basierten Auslastungstests](https://www.visualstudio.com/docs/test/performance-testing/getting-started/record-and-replay-cloud-load-tests)
* [Leistungstest Ihre app in die cloud](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing)
