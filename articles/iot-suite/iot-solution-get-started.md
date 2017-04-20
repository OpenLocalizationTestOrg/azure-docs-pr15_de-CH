<properties
    pageTitle="MyDriving Azure IoT-Beispiel: Quick Start | Microsoft Azure"
    description="Erste Schritte mit einer Anwendung, die eine umfassende Demonstration ein Systems IoT Architekten mit Microsoft Azure, einschließlich Stream Analytics, maschinelles lernen und Ereignis-Hubs."
    services=""
    documentationCenter=".net"
    suite=""
    authors="harikmenon"
    manager="douge"/>

<tags
    ms.service="multiple"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="03/25/2016"
    ms.author="harikm"/>

# <a name="mydriving-iot-system-quick-start"></a>MyDriving IoT System: Schnellstart

MyDriving ist ein System, die Design und Implementierung einer normalen [Internet der Dinge](iot-suite-overview.md) (IoT) Lösung, die Telemetrie von Devices sammelt und verarbeitet die Daten in der Cloud betrifft Computer lernen, eine adaptive Antwort veranschaulicht. Demo protokolliert Daten über Ihre Autofahrten mit Daten vom Mobiltelefon und einen Adapter, der aus Ihr Auto-Verwaltungssystem Informationen sammelt. Feedback zu Ihren Fahrstil im Vergleich zu anderen Benutzern verwendet dieser Daten.

Echte dient MyDriving zum Erstellen einer eigenen Lösung IoT Einstieg. Doch vor, dass Sie mit der MyDriving der Anwendung selbst als Mitglied unserer Benutzer Testteam. Dadurch Erlebnis der app und das System als Consumer, bevor tief in der Architektur. Es stellt auch Sie HockeyApp cool zu verwalten von Alpha und Beta-Distributionen Apps Benutzer.

## <a name="use-the-mobile-experience"></a>Verwenden des mobilen Zugriffs

Android, iOS und Windows 10-Gerät haben, können Sie MyDriving app.

### <a name="android-and-windows-10-mobile-installation"></a>Android und Windows 10 Mobile installation

Auf dem Gerät:

1.  Entwicklung apps zulassen:

    -   **Android: Bei** > **Security**können apps aus **unbekannten Quellen**.

    -   Windows 10: In **Einstellungen** > **Updates** > **For Developers** **Entwicklermodus**festgelegt.

2.  Anschließen Sie unser Team Betatest durch Anmeldung bei und Anmelden an [HockeyApp](https://rink.hockeyapp.net). HockeyApp erleichtert die frühe Versionen Ihrer Anwendung an Benutzer verteilen.

    Verwenden Sie Windows 10 verwenden, Edge-Browser.

    Wäre ein Build 2016 Teilnehmer melden Sie sich mit derselben Microsoft-Konto e-Mail-mithilfe eines Microsoft-Schaltflächen für die Konferenz registriert. Sie sind bereits mit HockeyApp angemeldet.

    ![HockeyApp-Bildschirm](./media/iot-solution-get-started/image1.png)

3.  Downloaden Sie und installieren Sie die Anwendung hier:

    -   [Android](http://rink.io/spMyDrivingAndroid)

    -   [Windows 10](http://rink.io/spMyDrivingUWP)

    Es gibt zwei Elemente. Installieren Sie das Zertifikat im **Vertrauenswürdigen Personen**. Installieren Sie die Anwendung.

*Starten der Anwendung auf Windows 10 Mobile Probleme?* Ihr Telefon möglicherweise ein Update oder zwei hinter. Stellen Sie sicher, dass Sie die neuesten Updates und installieren:

 - [Microsoft.NET.Native.Framework.1.2.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 

 - [Microsoft.NET.Native.Runtime.1.1.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 

 - [Microsoft.VCLibs.ARM.14.00.appx](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)


### <a name="ios-installation"></a>iOS-installation

Wenn Sie Build 2016 besucht, herunter der app als Mitglied unser Testteam HockeyApp

1.  Melden Sie sich auf Ihrem Gerät iOS in [HockeyApp](https://rink.hockeyapp.net)an.
    Verwenden Sie eines der Microsoft-Schaltflächen und melden Sie sich mit demselben Microsoft Konto e-Mails, die bei der Konferenz. (Verwenden Sie nicht die Felder e-Mail und Kennwort.)

    ![HockeyApp-Bildschirm](./media/iot-solution-get-started/image1.png)

2.  Im Dashboard HockeyApp MyDriving auswählen und herunterladen.

3.  Genehmigen Sie die Betaversion von HockeyApp:

    ein. **Standardeinstellungen**zum > **Allgemeine** > **Profile und Device Management.**

    b. Vertrauen Sie **Bit-Stadion GmbH** Zertifikat.

Wenn Sie Build 2016 nicht teilnehmen, können Sie erstellen und Bereitstellen die Anwendung selbst:

1.   Laden Sie den Code [von GitHub].

2.   Erstellen und Bereitstellen von [mithilfe von Xamarin].

Details finden Sie im [Leitfaden MyDriving](http://aka.ms/mydrivingdocs).

## <a name="get-an-obd-adapter-optional"></a>Ruft einen OBD-Adapter (optional)

Dies ist der Teil, das einem Produktionssystem Internet der Dinge! Können Sie die Anwendung ohne aber mehr Spaß mit echt ist und nicht teuer.

Diagnosesysteme (OBD) ist Ihr Auto Garage verwendet Ihr Auto und ungerade Geräusche und Warnleuchten diagnostizieren. Sei Ihr Auto große Antike finden Sie in der Kabine normalerweise hinter einer Klappe unter dem Dashboard einen Socket an. Mit dem richtigen Anschluss können Sie Metriken der Motorleistung zu erhalten und bestimmte vornehmen. Zu zweit aus den üblichen OBD-Stecker erhältlich. Verbindung mit Bluetooth oder Wi-Fi, eine Anwendung auf Ihrem Telefon.

In diesem Fall werden wir Ihr Auto in die Cloud zu verbinden. Die direkte Verbindung von OBD Handy ist unsere Anwendung als Relay funktioniert Ihr Auto Telemetrie erfolgt direkt an den Hub MyDriving IoT dort bearbeitet Ihre Fahrten und Ihren Fahrstil bewerten.

OBD-Geräts anschließen:

1.  Überprüfen Sie, ob Ihr Auto OBD-Socket.

2.  OBD-Adapter zu erhalten:

    -   Wenn ein Android oder Windows Phone verwenden, benötigen Sie einen Bluetooth-fähigen OBD II-Adapter. [BAFX Produkte 34t5 Bluetooth OBDII Scan-Tool]verwendet.

    -   Wenn Sie eine iOS-Telefon verwenden, benötigen Sie einen Adapter Wi-Fi-fähige OBD. Verwendet [ScanTool OBDLink MX Wi-Fi: OBD Adapter-Diagnose Scanner].

3.  Anleitung mit der OBD-Adapter für die Verbindung zu Ihrem Telefon. Beachten Sie die folgenden Punkte:

    -   Ein Bluetooth-Adapter muss mit dem Telefon auf **der Einstellungsseite** kombiniert werden.

    -   Ein WLAN-Adapter müssen eine Adresse im Bereich 192.168.xxx.xxx.

4.  Haben Sie mehrere Autos, erhalten Sie einen separaten Adapter für jedes (maximal drei).

Haben Sie einen OBD-Adapter, die app noch sendet Daten Position und Geschwindigkeit des Telefons GPS-Empfänger an Back-End und fragt, ob Sie eine OBD simulieren möchten.

Erfahren Sie mehr darüber, wie die Anwendung Daten vom OBD-Adapter und Optionen für das Erstellen eigener OBD-Geräts in Abschnitt 2.1, "IoT Geräte" im [MyDriving-Referenzhandbuch](http://aka.ms/mydrivingdocs).

## <a name="use-the-app"></a>Verwenden der app

Starten Sie die Anwendung. Gibt es einen anfänglichen Schnellstart Funktionsweise durchlaufen.

### <a name="track-your-trips"></a>Verfolgen Sie Ihre

Tippen Sie die Aufnahmetaste (großer roter Kreis am unteren Bildschirmrand) eine starten und wieder beenden.

![Der Datensatz Taste Reise verfolgen](./media/iot-solution-get-started/image2.png)

Jedem Start eine Reise ist keine OBD-Geräts, werden Sie gefragt, ob Sie den Simulator verwenden möchten.

Am Ende eine Tippen Sie auf die Schaltfläche Beenden und Sie erhalten eine Zusammenfassung.

![Beispiel für eine Zusammenfassung](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a>Überprüfen Sie Ihre

![Beispiel einer früheren Reise](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a>Überprüfen Sie Ihr Profil

![Beispiel eines Fahrstil Profils](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a>Senden Sie uns Testfeedback

Da wir MyDriving Jumpstart unterstützen Ihre eigenen Systeme IoT erstellt, möchte es gerne von Ihnen wissen, wie gut funktioniert. Lassen Sie uns wissen Sie, wenn:

- Probleme oder Probleme ausführen.

- Gibt eine Erweiterung, die sie für Ihr Szenario machen.

- Finden Sie bestimmte Bedürfnisse effizienter.

- Sie haben weitere Vorschläge zur Verbesserung dieser Dokumentation oder MyDriving.

In der MyDriving-Anwendung selbst, können integrierte HockeyApp Feedbackmechanismus: IOS- und Android nur Schütteln Sie Ihr Telefon oder das **Feedback** Menübefehl. Dadurch wird automatisch einen Screenshot zugeordnet, damit wir wissen, was Sie sprechen. Und sind leider Abstürze, HockeyApp Crashprotokolle, um ihnen erzählen. Sie können auch Feedback über das [Portal HockeyApp].

Sie können auch [auf GitHub], oder einen Kommentar unten (En-us Edition).

Wir freuen uns auf Sie!

## <a name="next-steps"></a>Nächste Schritte

-   [MyDriving-Referenzhandbuch](http://aka.ms/mydrivingdocs) zu verstehen, wie wir entworfen und erstellt das gesamte MyDriving System durchsuchen.

-   [Erstellen und ein eigenes System](iot-solution-build-system.md) mit unserer Azure Resource Manager-Skripts. [MyDriving-Referenzhandbuch](http://aka.ms/mydrivingdocs) schrittweise auch Bereiche, in denen meisten Anpassungen machen.

  [von GitHub]: https://github.com/Azure-Samples/MyDriving
  [Xamarin verwenden]: https://developer.xamarin.com/guides/ios/getting_started/installation/
  [BAFX Produkte 34t5 Bluetooth OBDII Scan-Tool]: http://www.amazon.com/gp/product/B005NLQAHS
  [ScanTool OBDLink MX WLAN: OBD-Adapter-Diagnose Scanner]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
  [HockeyApp portal]: https://rink.hockeyapp.org
  [Problem auf GitHub]: https://github.com/Azure-Samples/MyDriving/issues
