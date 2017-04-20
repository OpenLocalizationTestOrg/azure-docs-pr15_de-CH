<properties
    pageTitle="Lernprogramm: Azure Active Directory-Integration mit Cisco Spark | Microsoft Azure"
    description="So konfigurieren Sie einmaliges Anmelden zwischen Azure Active Directory und Cisco Spark."
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
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Lernprogramm: Azure Active Directory-Integration mit Cisco Funken

In diesem Lernprogramm erfahren Sie, wie Cisco Spark Azure Active Directory (Azure AD) integrieren.

Integration von Cisco Spark in Azure AD bietet folgende Vorteile:

- Sie können in Azure AD Steuern auf Cisco Spark hat
- Sie können die Benutzer automatisch angemeldet-Cisco Spark (einmaliges Anmelden) auf erhalten ihre Azure AD-Konten
- Sie können Ihre Konten zentral - klassischen Azure-Portal verwalten

Wenn Sie weitere Informationen zur Integration von SaaS Anwendung in Azure AD wissen möchten, finden Sie unter [Zugriff und single Sign-on Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Konfiguration von Azure AD-Integration mit Cisco Spark benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- **Cisco Spark** einmalige Anmeldung aktivierte Abonnements


> [AZURE.NOTE] Um Testschritte in diesem Lernprogramm empfehlen nicht wir einer.


Um die Schritte in diesem Lernprogramm zu testen, sollten Sie diese Empfehlung befolgen:

- Verwenden Sie Ihre produktionsumgebung nur dies erforderlich ist.
- Wenn Sie eine Testversion Azure AD-Umgebung haben, können Sie einem einmonatigen Testversion [hier](https://azure.microsoft.com/pricing/free-trial/)abrufen.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In dieser Übung testen Sie Azure AD einmaliges Anmelden in einer Umgebung. In diesem Lernprogramm beschriebenen Szenario besteht aus zwei wesentlichen Bausteine:

1. Cisco Spark aus der Galerie hinzufügen
2. Konfigurieren und Testen von Azure AD einmaliges Anmelden


## <a name="adding-cisco-spark-from-the-gallery"></a>Cisco Spark aus der Galerie hinzufügen
Zum Konfigurieren der Integration von Cisco in Azure AD müssen Sie der Liste der verwalteten SaaS-apps Cisco Spark aus dem Katalog hinzufügen.

**Um Cisco Spark aus der Galerie hinzuzufügen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie im oberen Menü auf **Applications** .

    ![Applikationen][2]

4. Klicken Sie am unteren Rand der Seite **Hinzufügen** .

    ![Applikationen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun** auf **eine Anwendung aus dem Katalog hinzufügen**.

    ![Applikationen][4]

6. Geben Sie im Suchfeld **Cisco Spark**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_01.png)

7. Klicken Sie im Ergebnisbereich wählen Sie **Cisco Spark aus**und dann auf **vollständig** die Anwendung hinzufügen.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen von Azure AD einmaliges Anmelden
In diesem Abschnitt Konfigurieren und Testen Azure AD einmaliges Anmelden mit Cisco Spark basierend auf einen Testbenutzer "Britta Simon" bezeichnet.

Azure AD muss für einmaliges Anmelden arbeiten kennen der Benutzer Gegenstück in Cisco Spark an einen Benutzer in Azure AD. In anderen Worten muss eine Verknüpfung Beziehung zwischen Azure AD-Benutzer und die zugehörigen Benutzer in Cisco Spark hergestellt werden.
Diese Beziehung wird eingerichtet, indem den Wert des **Benutzernamens** in Azure AD als **Benutzername** in Cisco Spark. Konfigurieren und Azure AD einmaliges Anmelden mit Cisco Spark testen müssen Sie die folgenden Bausteine durchführen:

1. **[Konfigurieren von Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - der Benutzer dieses Feature verwenden können.
2. **[Benutzer erstellen eine Azure Anzeige testen](#creating-an-azure-ad-test-user)** : Azure AD einmaliges Anmelden mit Britta Simon testen.
4. **[Erstellen einer Cisco Spark Benutzer testen](#creating-a-cisco-spark-test-user)** - Gegenstück Britta Simon Cisco Spark enthalten, die in Azure AD Darstellung Ihres verknüpft ist.
5. **[Zuweisen von Azure AD Benutzer testen](#assigning-the-azure-ad-test-user)** : Britta Simon Azure AD einmaliges Anmelden verwenden aktivieren.
5. **[Testen von Single Sign-On](#testing-single-sign-on)** - überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguration von Azure AD einmaliges Anmelden

Ziel dieses Abschnitts ist Azure AD einmaliges Anmelden im klassischen Azure-Portal aktivieren und -auf der Cisco Spark-Anwendung konfigurieren.

Cisco erwartet Spark SAML-Assertionen bestimmte Attribute enthalten. Konfigurieren Sie die folgenden Attribute für diese Anwendung. Sie können die Werte dieser Attribute auf der Registerkarte **"Atrributes"** der Anwendung verwalten. Der folgende Screenshot zeigt ein Beispiel dafür.

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_03.png) 

**Um mit Cisco Spark Azure AD einmaliges Anmelden konfigurieren, führen Sie die folgenden Schritte:**

1. **Klicken Sie auf im klassischen Azure-Portal, auf Integration **Cisco Spark** -Anwendung im Menü oben.**
     
    ![Einmaliges Anmelden konfigurieren][5]


2. Gehen Sie im Dialogfeld **SAML-token-Attribute** :

    ein. Klicken Sie auf **Hinzufügen Benutzer Attribure** Dialogfeld **Attribut hinzufügen** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_05.png)
    
    b. Geben Sie im Textfeld **Attributname** **Uid**.
    
    c. Wählen Sie aus der Liste **Attribut** **user.userprincipal**.
    
    d. Klicken Sie auf **abgeschlossen**. Dann **Änderungen** am unteren Rand der Seite.

3. Klicken Sie im Menü oben auf **Schnellstart**.

    ![Einmaliges Anmelden konfigurieren][6]

4. Klicken Sie im Verwaltungsportal auf Anwendungsseite Integration **Cisco Spark** **Konfigurieren einmaliges Anmelden** **Konfigurieren Sie einmaliges Anmelden** Dialogfeld öffnen.

    ![Einmaliges Anmelden konfigurieren][7] 

5. Auf der Seite **Wie möchten Sie Benutzer zum Anmelden bei Cisco Spark** **Azure AD einmaliges Anmelden**wählen Sie und klicken Sie auf **Weiter**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_06.png)

6. Führen Sie auf der **App-Einstellungen zu konfigurieren** die folgenden Schritte:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_07.png)


    ein. Geben Sie im Textfeld Anmelde-URL eine URL dem folgenden Muster: `https://web.ciscospark.com/#/signin`.

    b. Klicken Sie auf **Weiter**.


7. Klicken Sie auf der Seite **Konfigurieren einmaliges Anmelden am Cisco Spark** auf **Metadaten**, und speichern Sie die Datei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_09.png)

8. Melden Sie sich bei [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) mit vollständigen Administratorrechten an.
9. **Einstellungen Sie** , und klicken Sie unter **Authentifizierung** **Ändern**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)

10. Wählen Sie **integrieren 3rd Party Identitätsanbieter. (Erweitert)** und gehen Sie zum nächsten Bildschirm.
11. Klicken Sie auf **Datei herunterladen** und speichern Sie die Datei auf Ihrem Computer.
12. Auf der Seite **Importieren Idp Metadaten** entweder ziehen Azure AD-Metadatendatei das Zeichenblatt oder die Option Datei Browser zum Suchen und Laden der Metadatendatei Azure AD. Wählen Sie **benötigen Zertifikat wurde von einer Zertifizierungsstelle in Metadaten (sicherer)** und klicken Sie auf **Weiter**. 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

13. Wählen Sie **SSO-Verbindung testen**und beim Öffnen einer neuen Registerkarte Authentifizierung mit Azure anmelden.
14. Zurück zu Browserregisterkarte **Cisco Cloud Collaboration Management** . Wenn der Test erfolgreich war, wählen Sie **dieser Test war erfolgreich. Aktivieren von Single Sign-On-Option** und klicken Sie auf **Weiter**.

7. Azure AD-Verwaltungsportal wählen Sie die Konfiguration für einzelne Zeichen Bestätigung, und klicken Sie auf **Weiter**.
    
    ![Azure AD einmaliges Anmelden][10]

8. Klicken Sie auf der Seite **Bestätigung für einzelne Zeichen** auf **abgeschlossen**.  
    
    ![Azure AD einmaliges Anmelden][11]

### <a name="creating-an-azure-ad-test-user"></a>Erstellen einen Testbenutzer Azure AD
In diesem Abschnitt erstellen Sie einen Testbenutzer im Verwaltungsportal Britta Simon aufgerufen.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte:**

1. Klicken Sie im **klassischen Azure-Portal**im linken Navigationsbereich auf **Active Directory**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_09.png) 

2. Wählen Sie aus der Liste **Verzeichnis** das Verzeichnis für das Verzeichnisintegration soll.

3. Klicken Sie zum Anzeigen der Benutzerliste im oberen Menü auf **Benutzer**.
    
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. Klicken Sie das Dialogfeld **Benutzer hinzufügen** der Symbolleiste an der Unterseite, **Benutzer hinzufügen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

5. Auf der Seite **Erzählen zu diesem Benutzer** die folgenden Schritte:
 
    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_05.png) 

    ein. Wählen Sie als Typ der Benutzer neuen Benutzer in der Organisation.

    b. Geben Sie im **Textfeld**von Benutzernamen **BrittaSimon**.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf das **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_06.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**.  

    b. Im Feld **Nachname** Typ **Simon**.

    c. Geben Sie im Textfeld **Anzeigename** **Britta Simon**.

    d. Wählen Sie in der Liste **Rolle** **Benutzer**.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Seite **erhalten temporäres Kennwort** **Erstellen**.

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_07.png) 

8. Führen Sie die folgenden Schritte auf der Seite **Passwort zu erhalten** :

    ![Erstellen einen Testbenutzer Azure AD](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_08.png) 

    ein. Notieren Sie den Wert für das **Neue Kennwort**.

    b. Klicken Sie auf **abgeschlossen**.   



### <a name="creating-a-cisco-spark-test-user"></a>Cisco Spark Testbenutzer erstellen

In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in Cisco Spark. In diesem Abschnitt erstellen Sie einen Benutzer namens Britta Simon in Cisco Spark.

1. Wechseln Sie zur [Cisco Cloud Collaboration Management](https://admin.ciscospark.com/) mit vollständigen Administratorrechten.
2. Klicken Sie auf **Benutzer** und **Benutzer verwalten**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. Wählen Sie im Fenster **Verwalten Benutzer** **manuell hinzufügen oder Ändern von Benutzern aus** und auf **Weiter**.
4. Wählen Sie **Namen und e-Mail-Adresse**. Dann füllen Sie das Textfeld folgendermaßen:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 

    ein. Geben Sie im Textfeld **Vorname** **Britta**

    b. Geben Sie im Textfeld **Nachname** **Simon**

    c. Geben Sie in das Textfeld **e-Mail-Adresse****britta.simon@contoso.com**

5. Klicken Sie auf das Pluszeichen Britta Simon hinzufügen. Klicken Sie dann auf **Weiter**.
6. Klicken Sie im Fenster **Dienste für Benutzer hinzufügen** auf **Speichern** und dann auf **Fertig stellen**.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen von Azure AD-Testbenutzer

In diesem Abschnitt können Sie Britta Simon mit Azure einmaliges Cisco Spark Zugang gewähren.

![Benutzer zuweisen][200] 

**Um Cisco Spark Britta Simon zuzuweisen, führen Sie die folgenden Schritte:**

1. Das Verwaltungsportal Ansicht Applications in der Verzeichnisansicht öffnen, klicken Sie auf **Programme** im oberen Menü.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Cisco Spark**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_14.png) 

1. Klicken Sie im Menü oben auf **Benutzer**.

    ![Benutzer zuweisen][203] 

1. Wählen Sie in der Liste alle Benutzer **Britta Simon**.

2. Klicken Sie auf unten auf **zuweisen**.

    ![Benutzer zuweisen][205]


### <a name="testing-single-sign-on"></a>Einmaliges testen

Dieser Abschnitt soll Azure AD einzelne Anmeldung Überprüfen der Konfiguration mithilfe der.

Beim Anklicken von Cisco Spark Kachel in der Sie sollte automatisch zur Anwendung Cisco Spark angemeldete erhalten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Lernprogramme zum SaaS-Apps in Azure Active Directory integrieren](active-directory-saas-tutorial-list.md)
* [Was ist Zugriff und single Sign-on Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_205.png
