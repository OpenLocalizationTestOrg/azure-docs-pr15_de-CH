<properties
   pageTitle="Überträgt den Besitz der Azure-Abonnement | Microsoft Azure"
   description="Übertragen von Azure-Abonnement zu einem anderen Benutzer einige häufig gestellte Fragen (FAQ) über den Prozess"
   services=""
   documentationCenter=""
   authors="genlin"
   manager="stevenpo"
   editor=""
   tags="billing,top-support-issue"/>

<tags
   ms.service="billing"
   ms.workload="na"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/10/2016"
   ms.author="genli"/>

# <a name="transferring-ownership-of-an-azure-subscription"></a>Überträgt den Besitz eines Azure-Abonnements

Tust Du:

- Abrechnung an Ihre Azure-Abonnement an jemanden übergeben müssen?
- Möchten Sie sich für Azure-Konto ändern? Möglicherweise verwendet Ihr Microsoft Account aber Ihre Arbeit oder Schule Konto stattdessen gemeint?
- Azure-Abonnement aus einem Verzeichnis in ein anderes verschieben möchten?
- Azure und Office 365 in verschiedenen Mandanten und konsolidieren möchten?

Jetzt dabei einfach im Microsoft Azure Account Center - nutzungsbasierte, MSDN, Action Pack oder BizSpark Abonnements.  Wir haben die Möglichkeit, Ihr Abonnement für einen anderen Benutzer übertragen hinzugefügt. Anders gesagt können Sie jetzt das Administratorkonto nutzungsbasierte, MSDN, Action Pack oder BizSpark Abonnements, deren Besitzer, ändern egal in welchem, denen Land Sie arbeiten. Wir unterstützen jetzt die Übertragung von Azure Marketplace Einkäufe für diese abonnementtypen.

> [AZURE.NOTE] Um Ihr Abonnement zu einem anderen zu ändern, finden Sie unter [Switch der Azure-Abonnement zu einem anderen Angebot](billing-how-to-switch-azure-offer.md) Weitere Informationen. Benötigen Sie weitere Hilfe zu diesem Artikel, bitte [wenden Sie](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Ihr Problem schnell gelöst.

## <a name="how-to-transfer-ownership-of-an-azure-subscription"></a>Übertragen von Azure-Abonnement an

> [AZURE.VIDEO transfer-an-azure-subscription]

1.  Melden Sie sich unter <https://account.windowsazure.com/Subscriptions>. Sie müssen das Kontoadministrator ein "Ownership"-Übertragung sein. Weitere Informationen dazu, wie Sie herausfinden, wer der Kontoadministrator des Abonnements finden Sie unter [häufig gestellte Fragen](#faq).

2.  Wählen Sie das Abonnement übertragen.

3.  Klicken Sie auf **Abonnement übertragen** .

    ![Azure-Abonnements Konto](./media/billing-subscription-transfer/image1.png)

4.  Folgen Sie den Empfänger an.

    ![Dialogfeld für Übertragung](./media/billing-subscription-transfer/image2.PNG)

5.  Der Empfänger erhalten automatisch eine e-Mail mit einem Link akzeptieren.

    ![Abonnement übertragen e-Mail an Empfänger](./media/billing-subscription-transfer/image3.png)

6.  Klicken Sie auf den Link und die Anweisungen, einschließlich ihre Zahlungsinformationen eingeben.

    ![Abonnement übertragen Webseite](./media/billing-subscription-transfer/image4.png)

    ![Zweite Abonnement übertragen-Webseite](./media/billing-subscription-transfer/image5.png)

7. Erfolg! Das Abonnement wird jetzt übertragen.

<a id="faq"></a>
## <a name="frequently-asked-questions-faq"></a>Häufig gestellte Fragen (FAQ)

-   **Wie kann ich wissen, wer der Kontoadministrator des Abonnements?**

    Sie können bestätigen, die Konto-Administrator des Abonnements wie folgt:

    1. Mit der [Azure-Portal](https://portal.azure.com)anmelden.
    2. Wählen Sie im Hub **Abonnement**.
    3. Abonnement überprüfen möchten, und wählen Sie **Eigenschaften**.
    4. Wählen Sie **Eigenschaften**. Konto-Administrator des Abonnements wird im **Administratorkonto** angezeigt.  

-   **Führt eine Übertragung des Abonnements Ausfallzeiten?**

    Es gibt keine Auswirkung auf den Dienst. Effektiv hebt das Abonnement das aktuelle Konto Administrator und erstellt eine neue unter Empfänger dies das neue Abonnement zugrunde liegenden Azure Services zugeordnet. Die Abonnement-ID bleibt unverändert.

-   **Verwendung dieser Mechanismus Verzeichnis für Abonnement ändern?**-   
    Ein Azure-Abonnement wird im Verzeichnis erstellt, das Admin-Konto gehört. Also, um das Verzeichnis zu ändern, übertragen Sie das Abonnement Benutzerkonto im Zielverzeichnis. Abschluss dieser Benutzer die Schritte zur Übertragung akzeptieren verschiebt das Abonnement automatisch in das Zielverzeichnis.

-   **Wenn ich Abrechnung an ein Abonnement von einer anderen Organisation übernehmen, weiterhin sie Zugriff auf Ressourcen?**

    Das Abonnement zu einem anderen Mandanten übertragen, verlieren vorherige Mieter zugeordneten Benutzer auf das Abonnement zugreifen. Selbst wenn ein Benutzer kein Dienstadministrator oder Co-Administrator mehr ist, müssen sie noch Zugriff auf das Abonnement durch andere Sicherheitsmechanismen. Dazu gehören:
    - Management-Zertifikate, die der Benutzer Administratorrechte auf Abonnementressourcen gewähren. Weitere Informationen finden Sie unter [Erstellen und Hochladen Verwaltungszertifikat für Azure](https://msdn.microsoft.com/library/azure/gg551722.aspx)
    -   Zugriffstasten für Dienste wie Speicher. Weitere Informationen finden Sie unter [anzeigen, kopieren und Zugriffstasten Regenerieren Speicher](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
    -   Remote Access Anmeldeinformationen für Dienste wie Azure Virtual Machines

    Dies ist keine vollständige Liste. Der Empfänger sollten vertrauliche Daten mit dem Dienst verknüpft ist, benötigen sie Zugriff auf ihre Ressourcen aktualisieren. Die meisten Ressourcen können wie folgt aktualisiert:

    1.   Azure-Portal zu: [ *https://portal.azure.com*](https://portal.azure.com)

    2.    Klicken Sie auf Durchsuchen-&gt; alle Ressourcen

    3.    Wählen Sie die Ressource. Ressource-Blade wird geöffnet.

    4.    **Klicken Sie auf Blatt Ressource.** Hier können Sie anzeigen und Aktualisieren von vorhandenen Schlüssel.


-   **Übertragen des Abonnements in die Rechnungszyklus wechseln Empfänger Zahlen für die gesamte Abrechnung?**

    Der Absender ist verantwortlich für die Zahlung für den Gebrauch, der bis zu dem Zeitpunkt gemeldet wurde, dass die Übertragung abgeschlossen ist. Der Empfänger ist verantwortlich für Verbrauch gemeldet von der Übertragung ab. Möglicherweise werden einige Verwendung, die vor der Übertragung stattgefunden aber danach gemeldet wurde. Dadurch wird der Empfänger Rechnung enthalten.

-   **Verfügt der Empfänger auf Nutzung und Abrechnungsverlauf?**

    Zu diesem Zeitpunkt ist die einzige Informationen an den Empfänger der Betrag der letzten Rechnung (oder des aktuellen Saldos, wenn das Abonnement übertragen wurde, bevor die erste Rechnung generiert wurde). Der Rest der Abrechnungsverlauf und übertragen nicht mit dem Abonnement.

-   **Kann das Angebot während der Übertragung werden geändert?**

    Das Angebot muss unverändert bleiben. Um Ihr Angebot zu ändern, müssen Sie [wenden](http://go.microsoft.com/fwlink/?LinkID=619338).

-   **Kann ein Abonnement auf einem Benutzerkonto in ein anderes Land werden übertragen?**

    Nein, derzeit nicht unterstützt wird. Benutzerkonto für den Empfänger muss in demselben Land.

-   **Kann der Empfänger eine andere Zahlungsmethode verwenden?**

    Ja. Gibt es hier Einschränkungen: jetzt zwei Konten Abrechnung Geschichte Abonnement verteilt. Der Vorteil ist jedoch, dass Sie dies tun können, ohne an den [Support wenden](http://go.microsoft.com/fwlink/?LinkID=619338).

-   **Wird die Zahlungsweise belastet nach der Übertragung der Azure-Abonnement?**

    Annahme eine Übertragung des Abonnements eine Kreditkarte oder ähnliche Zahlungsmethode vorzulegen Abonnement bezahlen. Beispielsweise wenn Bob ein Abonnement Jane überträgt Jane die Übertragung akzeptiert, erforderlich Jane auch Zahlungsmethode, mit dem sie das Abonnement bezahlen. Nach Abschluss die Übertragung Bob nicht mehr für das Abonnement berechnet er Jane übertragen.

## <a name="next-steps-after-accepting-ownership-of-a-subscription"></a>Schritte nach der Annahme eines Abonnements an

1. Sie können nun den Kontoadministrator. Überprüfen Sie und aktualisieren Sie die Dienstadministratoren und Co-Administratoren. Verwalten Sie Admins im [klassischen Azure-Portal](https://manage.windowsazure.com) unter Einstellungen. [Erfahren Sie mehr](http://go.microsoft.com/fwlink/?LinkID=533293).
2. Rollenbasierte Zugriffskontrolle (RBAC) verwenden Sie für Ihr Abonnement und Services. Besuchen Sie [Azure-Portal](https://portal.azure.com) [erfahren Sie mehr über RBAC](http://go.microsoft.com/fwlink/?LinkID=544802)
3. Anmeldeinformationen für dieses Abonnement-Services zu aktualisieren. Dazu gehören:
    - Management-Zertifikate, die der Benutzer Administratorrechte auf Abonnementressourcen gewähren. Weitere Informationen finden Sie unter [Erstellen und Hochladen einer Management Zertifikat für Azure](https://msdn.microsoft.com/library/azure/gg551722.aspx)
    -   Zugriffstasten für Dienste wie Speicher. Weitere Informationen finden Sie unter [anzeigen, kopieren und Zugriffstasten Regenerieren Speicher](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
    -   Remote Access Anmeldeinformationen für Dienste wie Azure Virtual Machines
4. Abrechnung für dieses Abonnement bei [Azure Account Center](https://account.windowsazure.com/Subscriptions)[Weitere](http://go.microsoft.com/fwlink/?LinkID=533292) Alerts aktualisieren  
5.  Wenn Sie mit einem Partner zusammenarbeiten, sollten Sie die Partner-ID für dieses Abonnement aktualisieren. Dies ist in der [Mitte der Azure-Konto](https://account.windowsazure.com/Subscriptions)möglich.

> [AZURE.NOTE] Wenn Sie noch weitere Fragen haben, bitte [wenden Sie](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Ihr Problem schnell gelöst.
