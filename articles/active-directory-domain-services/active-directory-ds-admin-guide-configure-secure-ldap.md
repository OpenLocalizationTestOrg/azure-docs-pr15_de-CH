<properties
    pageTitle="Sicheres LDAP (LDAPS) in Azure Active Directory-Domänendiensten konfigurieren | Microsoft Azure"
    description="Konfigurieren Sie sichere LDAP (LDAPS) für eine verwaltete Azure Active Directory-Domänendienste-Domäne"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Konfigurieren Sie sichere LDAP (LDAPS) für eine verwaltete Azure Active Directory-Domänendienste-Domäne
Dieser Artikel beschreibt wie Sie sichere Lightweight Directory Access Protocol (LDAPS) für Ihre verwaltete Azure Active Directory-Domänendienste-Domäne aktivieren können. Sicheres LDAP ist auch bekannt als "Lightweight Directory Access Protocol (LDAP) über Secure Sockets Layer (SSL) / Transport Layer Security (TLS)".

## <a name="before-you-begin"></a>Bevor Sie beginnen
In diesem Artikel aufgelisteten Aufgaben, müssen wie folgt vor:

1. Ein gültiges **Azure-Abonnement**.

2. Ein **Azure AD-Verzeichnis** - entweder mit einem lokalen Verzeichnis oder ein Verzeichnis nur Cloud synchronisiert.

3. **Azure Active Directory-Domänendienste** muss für Azure AD-Verzeichnis aktiviert. Wenn Sie dies getan haben, führen Sie alle Aufgaben im [Handbuch Erste Schritte](./active-directory-ds-getting-started.md)beschrieben.

4. Ein **Zertifikat ermöglicht sicheres LDAP**.
    - **Empfohlen** – ein Zertifikat von der Unternehmenszertifizierungsstelle oder einer öffentlichen Zertifizierungsstelle erhalten. Diese Konfigurationsoption ist sicherer.
    - Alternativ auch können [ein selbst signiertes Zertifikat](#task-1---obtain-a-certificate-for-secure-ldap) erstellen Sie wie in diesem Artikel.

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a>Vorschriften für das sichere LDAP-Zertifikat
Erwerben Sie ein gültiges Zertifikat entsprechend den folgenden Richtlinien vor dem Aktivieren der sicheren LDAP. Fehler auftreten, wenn Sie versuchen, sicheres LDAP für die verwalteten Domäne mit einem ungültigen/falsche Zertifikat aktivieren.

1. **Vertrauenswürdige Herausgeber** – die Bescheinigung muss von einer Zertifizierungsstelle vertrauenswürdig für Computer, die Verbindung zu der Domäne, sicheres LDAP verwenden müssen. Diese Behörde möglicherweise Enterprise Zertifizierungsstelle oder einer öffentlichen Zertifizierungsstelle vertrauen diese Computer.

2. **Lebensdauer** - Zertifikat muss für die nächsten 3 bis 6 Monate gültig sein. Bei Ablauf des Zertifikats sicherer LDAP-Zugriff auf Ihre verwalteten Domäne gestört.

3. **Antragstellername** – der Antragstellername des Zertifikats muss einen Platzhalter für die verwalteten Domäne. Beispielsweise wenn Ihre Domäne "contoso100.com" heißt, das Zertifikat muss ' *. contoso100.com ". Legen Sie den DNS-Namen (Alternativer Antragstellername) dieser Platzhalter Namen.

3. **Schlüsselverwendung** - Zertifikat muss für Folgendes verwendet - digitale Signaturen und Schlüssel verschlüsseln konfiguriert werden.

4. **Zertifikatzweck** - Zertifikat muss zur SSL-Serverauthentifizierung gültig sein.

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Aufgabe 1 – Abrufen eines Zertifikats für sichere LDAP
Die erste Aufgabe umfasst ein Zertifikat für sichere LDAP-Zugriff auf die verwalteten Domäne verwendet. Sie haben zwei Optionen:

- Erhalten Sie ein Zertifikat von einer Zertifizierungsstelle. Die Behörde kann Ihre Organisation Unternehmenszertifizierungsstelle oder einer öffentlichen Zertifizierungsstelle sein.

- Erstellen Sie ein selbstsigniertes Zertifikat.


### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>Option (empfohlen) - ein sicheres LDAP-Zertifikat von einer Zertifizierungsstelle erhalten
Wenn Ihre Organisation einen Enterprise Infrastruktur öffentlicher Schlüssel (PKI) bereitgestellt werden, müssen Sie ein Zertifikat aus der Enterprise-Zertifizierungsstelle (CA) für Ihre Organisation. Wenn Ihre Organisation die Zertifikate von einer öffentlichen Zertifizierungsstelle erhält, müssen Sie LDAP-Sicherheitszertifikat von dieser öffentlichen Zertifizierungsstelle erhalten.

Beim Anfordern eines Zertifikats sicherzustellen Sie, dass die Anforderungen der [Anforderung für das sichere LDAP-Zertifikat](#requirements-for-the-secure-ldap-certificate)folgen.

> [AZURE.NOTE] Clientcomputer, die Verbindung mit der verwalteten Domäne, sicheres LDAP verwenden müssen vom LDAPS Aussteller vertrauen.


### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>Option B - Erstellen eines selbstsignierten Zertifikats für sichere LDAP
Sie können ein selbstsigniertes Zertifikat für sichere LDAP erstellen wenn:

- Zertifikate in Ihrem Unternehmen nicht von einer Unternehmenszertifizierungsstelle ausgestellt oder
- Sie erwarten kein Zertifikat von einer öffentlichen Zertifizierungsstelle verwenden.

**Erstellen Sie ein selbstsigniertes Zertifikat mithilfe von PowerShell**

Öffnen Sie auf Ihrem Windows-Computer ein neues PowerShell als **Administrator** , und geben Sie folgende Befehle ein neues selbstsigniertes Zertifikat erstellen.

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

Ersetzen Sie im obigen Beispiel 'contoso100.com' mit dem DNS-Domänennamen der verwalteten Domäne Azure Active Directory-Domänendienste.

![Wählen Sie Azure AD-Verzeichnis](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

Das neu erstellte selbstsignierte Zertifikat befindet sich im Zertifikatspeicher des lokalen Computers.


## <a name="task-2---export-the-secure-ldap-certificate-to-a-pfx-file"></a>Aufgabe 2: Exportieren der sicheres LDAP Zertifikat ein. PFX-Datei
Bevor Sie diese Aufgabe ausführen, sicherstellen Sie, dass das sichere LDAP-Zertifikat von der Enterprise-Zertifizierungsstelle oder einer öffentlichen Zertifizierungsstelle erhalten haben oder ein selbstsigniertes Zertifikat erstellt haben.

Die folgenden Schritte, das LDAPS-Zertifikat exportiert ein. PFX-Datei.

1. Drücken Sie die Schaltfläche **Start** , und geben Sie **R**. Geben Sie im Dialogfeld **Ausführen** **Mmc** , und klicken Sie auf **OK**.

    ![Starten Sie die MMC-Konsole](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)

2. Die **Benutzerkontensteuerung** dazu aufgefordert werden klicken Sie auf **Ja** um MMC (Microsoft Management Console) als Administrator zu starten.

3. Klicken Sie im Menü **Datei** auf **Hinzufügen/Entfernen-Snap-Ins...**.

    ![MMC-Konsole Snap-In hinzufügen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)

4. Wählen Sie im Dialogfeld **Hinzufügen oder Entfernen von Snap-Ins** das Snap-in **Zertifikate** , und klicken Sie auf die **Hinzufügen >** Schaltfläche.

    ![MMC-Konsole Zertifikate-Snap-in hinzufügen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)

5. Der Assistent **Zertifikate-Snap-in** **Computerkonto** und klicken Sie auf **Weiter**.

    ![Zertifikate-Snap-in Computerkonto hinzufügen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)

6. Wählen Sie auf der Seite **Computer auswählen** **Lokaler Computer: (der Computer, auf diese Konsole ausgeführt wird)** und klicken Sie auf **Fertig stellen**.

    ![Zertifikat-Snap-in - select Computer hinzufügen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)

7. Klicken Sie im Dialogfeld **Hinzufügen oder Entfernen von Snap-Ins** auf **OK** , um das Zertifikate-Snap-in zu MMC hinzufügen.

    ![MMC - geschehen Zertifikate-Snap-in hinzufügen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)

8. Klicken Sie im MMC-Fenster **Konsolenstamm**erweitern. Das Zertifikate-Snap-in laden sollte angezeigt werden. Klicken Sie auf **Zertifikate (lokaler Computer)** zu erweitern. Erweitern Sie den Knoten **Eigene Zertifikate** , gefolgt von den Knoten **Zertifikate** klicken.

    ![Speicher für persönliche Zertifikate öffnen](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)

9. Das selbstsignierte Zertifikat sollte erstellten angezeigt werden. Überprüfen Sie die Eigenschaften des Zertifikats, um sicherzustellen, dass der Fingerabdruck Windows PowerShell gemeldet, wenn das Zertifikat erstellt hat.

10. Wählen Sie selbst signiertes Zertifikat und **Klicken Sie mit der rechten Maustaste**. Aus dem Kontextmenü wählen Sie **Alle Tasks** und **Exportieren**.

    ![Zertifikat exportieren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)

11. Klicken Sie im **Zertifikatexport-Assistenten**auf **Weiter**.

    ![Assistenten exportieren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)

12. Wählen Sie auf der Seite **Privatschlüssel exportieren** **Ja, privaten Schlüssel exportieren**, und klicken Sie auf **Weiter**.

    ![Exportieren der privaten Schlüssel des Zertifikats](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [AZURE.WARNING] Sie müssen den privaten Schlüssel mit dem Zertifikat exportieren. Wenn Sie eine PFX, die nicht den privaten Schlüssel für das Zertifikat enthält bereitstellen, schlägt sicheren LDAP-Abfragen der verwalteten Domäne.

13. Wählen Sie auf der Seite **Exportdateiformat** **Privater Informationsaustausch - PKCS #12 (. PFX)** als Dateiformat für das exportierte Zertifikat.

    ![Exportdateiformat des Zertifikats](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [AZURE.NOTE] Nur die. PFX-Dateiformat unterstützt. Exportieren Sie das Zertifikat nicht die. CER-Dateiformat.

14. Auf **der Seite** wählen Sie die Option **Kennwort** und geben ein Kennwort zum Schutz der. PFX-Datei. Speichern Sie dieses Kennwort, da es in der nächsten Aufgabe benötigt wird. Klicken Sie auf **Weiter** um fortzufahren.

    ![Kennwort für Zertifikat exportieren ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [AZURE.NOTE] Notieren Sie sich dieses Kennwort. Sie benötigen, und gleichzeitig sicheren LDAP-Abfragen für diese verwalteten Domäne in [Aufgabe 3: sicheres LDAP für die verwalteten Domäne aktivieren](#task-3---enable-secure-ldap-for-the-managed-domain)

15. Geben Sie auf der Seite **Exportdatei** den Dateinamen und den Speicherort, in dem Sie das Zertifikat exportieren möchten.

    ![Pfad zum Zertifikat exportieren](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)

16. Klicken Sie auf der folgenden Seite auf **Fertig stellen** , um das Zertifikat in eine PFX-Datei exportieren. Bestätigung sollte angezeigt werden, wenn das Zertifikat exportiert wurde.

    ![Bescheinigung durchgeführt](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="task-3---enable-secure-ldap-for-the-managed-domain"></a>Aufgabe 3: sicheres LDAP für die verwalteten Domäne aktivieren
Aktivieren Sie sicheres LDAP führen Sie die folgenden Konfigurationsschritte aus:

1. Navigieren Sie zu der **[Azure-Verwaltungsportal](https://manage.windowsazure.com)**.

2. **Active Directory** -Knoten im linken Bereich auswählen.

3. Wählen Sie das Azure AD-Verzeichnis (auch als "Mieter" bezeichnet), für das Azure Active Directory-Domänendienste aktiviert haben.

    ![Wählen Sie Azure AD-Verzeichnis](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Klicken Sie auf die Registerkarte **Konfigurieren** .

    ![Registerkarte Verzeichnis konfigurieren](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Blättern Sie zum Abschnitt **Domänendienste**. Eine Option mit dem Titel **Secure LDAP (LDAPS)** , wie im folgenden Screenshot gezeigt sollte angezeigt werden:

    ![Domain Services Konfigurationsabschnitt](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)

6. Klicken Sie auf **Zertifikat konfigurieren...** , um das Dialogfeld **Zertifikat für LDAP konfigurieren** zu.

    ![Zertifikat für sichere LDAP konfigurieren](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)

7. Klicken Sie auf das Ordnersymbol unter **PFX-Datei mit Zertifikat** an die PFX-Datei enthält das Zertifikat für sichere LDAP-Zugriff auf die verwalteten Domäne verwenden möchten. Beim Exportieren des Zertifikats der PFX-Datei angegebenen Kennwort ebenfalls eingeben. Klicken Sie unten auf die Schaltfläche Fertig.

    ![Sichere LDAP PFX-Datei und ein Kennwort angeben](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)

8. **Domänendienste** Registerkarte **Konfigurieren** sollten abgeblendet erhalten und ist in den **Ausstehend...** Zustand für einige Minuten. Während dieses Zeitraums LDAPS-Zertifikat auf Richtigkeit überprüft und sicheres LDAP für die verwalteten Domäne konfiguriert ist.

    ![Sicheres LDAP - aussteht](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

    > [AZURE.NOTE] Dauert ca. 10 bis 15 Minuten sicheres LDAP für die verwalteten Domäne aktivieren. Wenn bereitgestellte sichere LDAP-Zertifikat nicht die erforderlichen Kriterien übereinstimmen, sicheres LDAP nicht für das Verzeichnis aktiviert ist und einen Fehler angezeigt. Beispielsweise der Domänenname falsch ist, das Zertifikat ist abgelaufen oder läuft bald usw. ab.

9. Wenn sicheres LDAP erfolgreich für Ihre verwalteten Domäne aktiviert die **Ausstehend...** Meldung sollte verschwinden. Den Fingerabdruck des Zertifikats angezeigt, sollte angezeigt werden.

    ![Sicheres LDAP aktiviert](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>


## <a name="task-4---enable-secure-ldap-access-over-the-internet"></a>Aufgabe 4: sichere LDAP-Zugriff über das Internet aktivieren
**Optionale Aufgabe** - nicht verwaltete Domäne über LDAPS über das Internet zugreifen, Konfiguration überspringen möchten.

Vor Beginn dieses Vorgangs sicher, dass Sie in [Aufgabe 3](#task-3---enable-secure-ldap-for-the-managed-domain)beschriebenen Schritte ausgeführt haben.

1. Sie sollten eine Option **Aktivieren sichere LDAP-Zugriff über das INTERNET** im Abschnitt **Domänendienste** Seite **Konfigurieren** angezeigt. Diese Option ist standardmäßig auf **NO** da Internetzugang der verwalteten Domäne über sicheres LDAP standardmäßig deaktiviert ist.

    ![Sicheres LDAP aktiviert](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)

2. Umschalten auf **Ja** **SICHERES LDAP-Zugriff über das INTERNET aktivieren** . Klicken Sie auf die Schaltfläche **Speichern** auf den unteren Bereich.
    ![Sicheres LDAP aktiviert](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)

3. **Domänendienste** Registerkarte **Konfigurieren** sollten abgeblendet erhalten und ist in den **Ausstehend...** Zustand für einige Minuten. Nach einiger Zeit ist Internetzugang der verwalteten Domäne über sicheres LDAP aktiviert.

    ![Sicheres LDAP - aussteht](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

    > [AZURE.NOTE] Dauert ca. 10 Minuten Internetzugang über sicheres LDAP für die verwalteten Domäne aktivieren.

4. Sicherer LDAP-Zugriff über das Internet der verwalteten Domäne erfolgreich aktiviert, der **Ausstehend...** Meldung sollte verschwinden. Externe IP-Adresse, die auf das Verzeichnis über LDAPS im Feld **Externe IP-Adresse für LDAPS Zugriff**verwendet werden kann, sollte angezeigt werden.

    ![Sicheres LDAP aktiviert](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-to-access-the-managed-domain-from-the-internet"></a>Aufgabe 5 - DNS, um Zugriff auf die verwalteten Domäne aus dem Internet konfigurieren
**Optionale Aufgabe** - nicht verwaltete Domäne über LDAPS über das Internet zugreifen, Konfiguration überspringen möchten.

Vor Beginn dieses Vorgangs sicher, dass Sie in [Aufgabe 4](#task-4---enable-secure-ldap-access-over-the-internet)beschriebenen Schritte ausgeführt haben.

Wenn Sie LDAP-Zugriff über das Internet für Ihre verwalteten Domäne aktiviert haben, müssen Sie DNS so aktualisieren, dass Clientcomputer diese verwalteten Domäne finden. Am Ende von Aufgabe 4 wird eine externe IP-Adresse auf der Registerkarte **Konfigurieren** **EXTERNEN IP-Adresse für LDAPS Zugriff**angezeigt.

Konfigurieren Sie Ihren externen DNS-Anbieter diese externe IP-Adresse der DNS-Name der verwalteten Domäne (beispielsweise "contoso100.com") zeigt. In unserem Beispiel müssen den folgenden DNS-Eintrag zu erstellen:

    contoso100.com  -> 52.165.38.113

Das ist es – können nun verwalteten Domäne über sicheres LDAP über das Internet herstellen.

> [AZURE.WARNING] Beachten Sie, dass Clientcomputer müssen den Aussteller des Zertifikats LDAPS herstellen über LDAPS der verwalteten Domäne zu vertrauen. Wenn Sie eine Organisationszertifizierungsstelle oder öffentlich vertrauenswürdige Zertifizierungsstelle verwenden, müssen Sie nicht tun, da Clientcomputer diese Zertifikatsaussteller vertrauen. Wenn Sie ein selbstsigniertes Zertifikat verwenden, müssen Sie den öffentlichen Teil das selbstsignierte Zertifikat in vertrauenswürdigen Zertifikatspeicher auf dem Clientcomputer installieren.

<br>

## <a name="related-content"></a>Verwandte Inhalte

- [Eine verwaltete Azure Active Directory-Domänendienste-Domäne verwalten](active-directory-ds-admin-guide-administer-domain.md)
