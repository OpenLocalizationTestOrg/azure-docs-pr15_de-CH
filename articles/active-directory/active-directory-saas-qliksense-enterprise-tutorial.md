<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Qlik sinnvoll | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Qlik sinnvoll."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Lernprogramm: Azure Active Directory-Integration mit Qlik sinnvoll

In diesem Lernprogramm lernen Sie Azure Active Directory (Azure AD) Qlik sinnvoll Enterprise integriert.

Azure AD Qlik sinnvoll Enterprise Integration bietet folgende Vorteile:

- Sie können in Azure AD Steuern auf Qlik sinnvoll Enterprise hat
- Sie können die Benutzer automatisch Qlik sinnvoll Unternehmen (Single Sign-On) angemeldete erhalten ihre Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Konfigurieren von Azure AD-Integration mit Qlik sinnvoll benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Qlik Gefühl Enterprise Single Sign-On aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In dieser Übung testen Sie Azure AD einmaliges Anmelden in einer Umgebung.

In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Qlik sinnvoll Enterprise aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Qlik sinnvoll Enterprise aus der Galerie hinzufügen
Zum Konfigurieren der Integration von Qlik sinnvoll Enterprise in Azure AD müssen Sie die Liste der verwalteten SaaS-apps Qlik sinnvoll Enterprise aus der Galerie hinzufügen.

**Um Qlik sinnvoll Enterprise aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory][1]
2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Qlik sinnvoll Enterprise**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_01.png)

7. Wählen Sie im Ergebnisbereich **Qlik sinnvoll Enterprise aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Azure AD einmaliges Anmelden mit Qlik sinnvoll basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Für einmaliges Anmelden zu arbeiten muss Azure AD Gegenstück Benutzer in Qlik Sense Unternehmen einem Benutzer in Azure AD ist. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzerobjekt und dem entsprechenden Benutzer in Qlik Sense Unternehmen hergestellt werden.

Diese Beziehung wird durch den Wert des **Benutzernamens** in Azure AD als **Benutzername** in Qlik Sense Unternehmen zuweisen eingerichtet.

Konfigurieren und Testen Azure AD einmaliges Anmelden mit Qlik Sinn müssen Sie führen Sie die folgenden Bausteine erforderlich:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
3. **[Erstellen einer Qlik sinnvoll Enterprise Benutzer testen](#creating-a-qliksense-enterprise-test-user)** - Gegenstück Britta Simon Qlik sinnvoll Enterprise enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
4. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

In diesem Abschnitt Azure AD einmaliges Anmelden im klassischen Portal aktivieren und einmaliges Anmelden in der Qlik sinnvoll Enterprise-Anwendung konfigurieren.


**Konfigurieren Azure AD einmaliges Anmelden mit Qlik sinnvoll, die folgenden Schritte:**

1. Klicken Sie im Verwaltungsportal auf der Seite **Qlik sinnvoll Enterprise** Application Integration **Konfigurieren einmaliges** öffnen das Dialogfeld **Konfigurieren Sie einmaliges Anmelden** .
     
    ![Einmaliges Anmelden konfigurieren][6] 

2. Auf der Seite **Wie möchten Sie Benutzer anmelden bei Qlik sinnvoll Enterprise** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_03.png) 

3. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_04.png) 

    ein. Geben Sie im Textfeld **Anmelde-URL** die URL vom Benutzer Anmeldung Ihre Qlik sinnvoll Enterprise-Anwendung mit dem folgenden Muster: **https://\<Qlik Sinn voll gekennzeichnete Hostnamen\>: 443 / < virtuelle Proxy Präfix\>/samlauthn/**.
    
    > [AZURE.NOTE] Beachten Sie den Schrägstrich am Ende dieser URI.  Es ist erforderlich.

    b. Klicken Sie auf **Weiter**
 
4. Auf der Seite **Konfigurieren einmaliges Qlik sinnvoll Unternehmen** Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_05.png)

    ein. Klicken Sie auf **Metadaten**, und speichern Sie die Datei auf Ihrem Computer.  Diese Metadaten-Datei vor dem Hochladen auf den Server Qlik Sense bereit.

    b. Klicken Sie auf **Weiter**.

5. Bereiten Sie die Föderation Metadata-XML-Datei so vor, dass die Qlik Sense-Server hochladen können.

    > [AZURE.NOTE] Vor dem Hochladen IdP-Metadaten auf den Server Qlik Sense, muss die Datei bearbeitet werden, um Informationen zum ordnungsgemäßen Betrieb zwischen Azure entfernen und Qlik sinnvoll.

    ![QlikSense][qs24]

    ein. Öffnen Sie die Datei FederationMetaData.xml in einem Text-Editor von Azure heruntergeladen.

    b. Suchen Sie den Wert **RoleDescriptor**.  Es werden vier Einträge (zwei Paare von Start- und Endtags des Elements).

    c. Löschen Sie die Tags RoleDescriptor und alle Informationen zwischen aus der Datei.

    d. Speichern Sie die Datei und benachbarten für Verwendung in diesem Dokument.

6. Navigieren Sie zu Qlik sinnvoll Qlik Management Konsole (QMC) als Benutzer virtuelle Proxykonfigurationen erstellen kann.

7. QMC klicken Sie auf das Menüelement virtuellen Proxy.

    ![QlikSense][qs6] 

8. Klicken Sie am unteren Bildschirmrand auf neue Schaltfläche erstellen.

    ![QlikSense][qs7]

9. Die virtuellen Proxy bearbeiten wird angezeigt.  Auf der rechten Seite des Bildschirms wird ein Menü Optionen sichtbar zu machen.

    ![QlikSense][qs9]

10. Geben Sie mit der Kennung Menüoption aktiviert die Identifikationsinformationen für die Proxykonfiguration von Azure virtual ein

    ![QlikSense][qs8]  
    
    ein. Das Feld Beschreibung ist ein angezeigter Name für die virtuelle Proxykonfiguration.  Geben Sie einen Wert für eine Beschreibung.
    
    b. Das Feld Präfix gibt virtuelle Proxyendpunkt für die Verbindung mit Qlik Sense mit Azure AD einmaliges Anmelden.  Geben Sie ein eindeutiges Präfix für diesen virtuellen Proxy.

    c. Sitzung Inaktivitäts-Timeout (Minuten) wird das Timeout für Verbindungen über diesen virtuellen Proxy.

    d. Der Cookie-Header Sitzungsname ist Cookienamen speichern den Sitzungsbezeichner für die Sitzung Qlik Sense erhält ein Benutzer nach erfolgreicher Authentifizierung.  Dieser Name muss eindeutig sein.

11. Klicken Sie auf die Menüoption Authentifizierung sichtbar machen.  Die Authentifizierung wird angezeigt.

    ![QlikSense][qs10]

    ein. Das **anonyme Zugriffsmodus** Dropdown-bestimmt, ob anonyme Benutzer Qlik Sense über virtuelle Proxy zugreifen können.  Die Standardoption ist kein anonymer Benutzer.

    b. **Authentifizierungsmethode** Dropdown-bestimmt das Authentifizierungsschema virtuellen Proxy verwenden.  Wählen Sie aus der Dropdown-Liste SAML.  Dadurch werden weitere Optionen angezeigt.

    c. Eingabe im **SAML-URI Hostfeld**Hostname-Benutzer Zugriff auf Qlik Sense über diesen virtuellen SAML-Proxy eingeben.  Der Hostname ist der Uri des Servers Qlik sinnvoll.

    d. Geben Sie im **SAML Entitäts-ID**den gleichen Wert für die SAML-URI Hostfeld eingegeben.

    e. **SAML IdP Metadaten** ist die Datei bereits im Abschnitt **Bearbeiten Verbundmetadaten Azure AD-Konfiguration** bearbeitet.  Informationen zum ordnungsgemäßen Betrieb zwischen Azure entfernen **bevor Metadaten IdP hochladen die Datei bearbeitet werden muss** und Qlik sinnvoll.  **Siehe die Anweisungen Wenn die Datei noch bearbeitet werden.**  Bearbeitet die Datei klicken Sie auf die Schaltfläche "Durchsuchen" und wählen die bearbeitete Metadatendatei Upload auf die Proxykonfiguration von virtuellen.

    f. Geben Sie den Attributnamen oder Referenzinformationen für das SAML-Attribut darstellt **UserID** Azure AD Qlik Sense-Server schicken.  Schemainformationen steht in Azure-Anwendung Bildschirme Beitragskonfiguration.  Das Name-Attribut **Geben Sie http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**verwenden.

    g. Geben Sie den Wert für das **Benutzerverzeichnis** , die Benutzern zugeordnet, wenn sie über Azure AD Qlik Sense-Server authentifizieren.  Hartcodierte Werte müssen in **Klammern []**eingeschlossen.  Um ein Attribut in Azure AD SAML-Assertion gesendet zu verwenden, geben Sie den Namen des Attributs in diesem Text Feld **ohne** Klammern.

    h. **SAML Signaturalgorithmus** wird das Zertifikat des Anbieter (in diesem Fall Qlik Sense-Server) für die virtuelle Proxykonfiguration.  Wenn Qlik Sense Server ein vertrauenswürdiges Zertifikat generiert Microsoft Enhanced RSA mit AES-Kryptografieanbieter verwendet, ändern Sie SAML Signaturalgorithmus **SHA -**256.

    Ich. Der SAML Zuordnung Attributabschnitt kann zusätzliche Attribute wie mit Sicherheitsregeln zu Qlik gesendet werden.

12. Klicken Sie auf den Lastenausgleich Menüoption sichtbar machen.  Der Netzwerklastenausgleich wird angezeigt.

    ![QlikSense][qs11]

13. Klicken Sie auf Hinzufügen neuer Server Knoten, select Engine Knoten oder Knoten Qlik Sense Sitzungen für den Lastenausgleich zu senden, und klicken Sie auf die Schaltfläche hinzufügen.

    ![QlikSense][qs12]

14. Klicken Sie auf die erweiterte Option sichtbar machen. Das Fenster erweitert wird angezeigt.

    ![QlikSense][qs13]

    ein. Host weiße Liste identifiziert Hostnamen beim Verbinden mit Qlik Sense-Server akzeptiert werden.  **Geben Sie den Hostnamen, den Benutzer beim Verbinden mit Qlik Sense-Server angeben.** Der Hostname ist der gleiche Wert wie SAML Host Uri ohne die https://.

15. Klicken Sie auf die Schaltfläche Übernehmen.

    ![QlikSense][qs14]

16. Klicken Sie auf OK, um die Warnung zu akzeptieren, die Proxys verknüpften virtuellen Proxy neu gestartet werden, besagt.

    ![QlikSense][qs15]

17. Auf der rechten Seite des Bildschirms wird das zugeordnete Menü.  Klicken Sie auf die Menüoption Proxys.

    ![QlikSense][qs16]

18. Der Proxy wird angezeigt.  Klicken Sie auf den Link unten Verknüpfen eines virtuellen Proxy.

    ![QlikSense][qs17]

19. Wählen Sie Proxyknoten, der Unterstützung dieser virtuellen Proxy-Verbindung und klicken Sie auf Verknüpfung.  Nach dem verknüpfen, wird der Proxy unter zugeordneten Proxys aufgeführt.

    ![QlikSense][qs18]
    ![QlikSense][qs19]

20. Nach ungefähr fünf bis zehn Sekunden erscheint die Meldung QMC aktualisieren.  Klicken Sie auf QMC aktualisieren.

    ![QlikSense][qs20]

21. Beim Aktualisieren des QMC klicken Sie auf das Menüelement virtuellen Proxys. Neue virtuelle Proxy-SAML-Eintrag wird in der Tabelle auf dem Bildschirm aufgeführt.  Klick auf virtuellen Proxy-Eintrag.

    ![QlikSense][qs51]

22. Am unteren Bildschirmrand wird SP herunterladen Metadaten aktivieren.  Klicken Sie Download SP Metadaten die Metadaten in einer Datei speichern.

    ![QlikSense][qs52]

23. Die Datei sp Metadaten.  Beachten Sie die **%EntityID** und **AssertionConsumerService** -Eintrag.  Diese Werte entsprechen den **Bezeichner** und die **URL anmelden** in Azure AD-Anwendungskonfiguration. Wenn sie kein passendes sollten Sie sie im Konfigurationsassistenten Azure AD App ersetzen.

    ![QlikSense][qs53]

24. Wählen Sie im klassischen Portal die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

25. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
 
    ![Azure AD einmaliges Anmelden][11]


### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
In diesem Abschnitt erstellen Sie einen Testbenutzer im Verwaltungsportal Britta Simon aufgerufen.


![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** folgendermaßen:  ![ein Azure AD-Testbenutzer erstellen](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Auf das **Benutzerprofil** , gehen Sie folgendermaßen vor: ![ein Azure AD-Testbenutzer erstellen](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   


### <a name="creating-an-qlik-sense-enterprise-test-user"></a>Ein Qlik Gefühl Enterprise Testbenutzer erstellen

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon Qlik sinnvoll Unternehmens. Arbeiten Sie mit Qlik sinnvoll Enterprise Support-Team in Qlik Sense Unternehmensplattform Benutzer hinzu.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon mit Azure einmaliges Qlik sinnvoll Enterprise Zugang gewähren.

![Benutzer zuweisen][200] 

**Britta Simon Qlik sinnvoll Unternehmen zuzuweisen, führen Sie die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Qlik sinnvoll Enterprise**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_50.png) 

3. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203]

4. Wählen Sie in der Liste Benutzer **Britta Simon**.

5. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]


## <a name="testing-single-sign-on"></a>Einmaliges testen

In diesem Abschnitt Testen Sie Ihre Azure AD einzelne anmelden Konfiguration mit der.

Beim Anklicken von Qlik sinnvoll Enterprise Kachel in der Sie sollte automatisch Ihrer Anwendung Qlik sinnvoll Enterprise angemeldete erhalten.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_205.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs21]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_21.png
[qs22]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_22.png
[qs23]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_23.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs25]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_25.png
[qs26]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_26.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png