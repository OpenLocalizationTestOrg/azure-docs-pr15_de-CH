## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor wir CDN Management Code schreiben können, müssen wir einige Vorbereitung unserer Code mit Azure-Ressourcen-Manager kommunizieren kann.  Dazu benötigen Sie:

* Erstellen einer Ressourcengruppe CDN-Profil enthalten, den Sie in diesem Lernprogramm erstellen
* Azure Active Directory-Authentifizierung für die Anwendung zu konfigurieren
* Der Ressourcengruppe weisen Sie Berechtigungen zu, sodass nur autorisierte Benutzer von unserer Azure AD-Mandanten mit unseren CDN interagieren können

### <a name="creating-the-resource-group"></a>Die Ressourcengruppe erstellen

1. [Azure-Portal](https://portal.azure.com)anmelden.

2. Klicken Sie auf die Schaltfläche **neu** in der oberen linken dann **Management**und **Ressourcengruppe**.
    
    ![Erstellen eine neue Ressourcengruppe](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)

3. Rufen Sie die Ressourcengruppe *CdnConsoleTutorial*.  Wählen Sie Ihr Abonnement und Nähe.  Wenn Sie möchten, können Sie das Kontrollkästchen **Pin Dashboard** zu Ressourcengruppe dem Dashboard im Portal klicken.  Dies wird später erleichtert.  Nachdem Sie Ihre Auswahl getroffen haben, klicken Sie auf **Erstellen**.

    ![Benennen der Ressourcengruppe](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)

4. Nachdem die Ressourcengruppe erstellt wurde und Sie Ihr Dashboard Heften nicht, finden es Sie durch Klicken auf **Durchsuchen**und **Ressourcengruppen**.  Klicken Sie auf die Ressourcengruppe, um ihn zu öffnen.  Notieren Sie Ihre **Abonnement-ID**an.  Wir werden ihn später benötigen.

    ![Benennen der Ressourcengruppe](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>Azure AD-Anwendung erstellen und Anwenden von Berechtigungen

Es gibt zwei Ansätze zum app-Authentifizierung mit Active Directory Azure: einzelne Benutzer oder einen Dienstprinzipal. Dienstprinzipal ähnelt ein Dienstkonto in Windows.  Statt einen bestimmten Benutzer Berechtigungen zum interagieren mit dem CDN, gewähren wir stattdessen Berechtigungen Service principal.  Automatisierte, nicht interaktive Prozesse dienen im Allgemeinen Dienstprinzipale.  Obwohl in diesem Lernprogramm eine interaktive Konsole-Anwendung schreiben, konzentrieren wir uns auf den Service Ansatz.

Erstellt einen Dienstprinzipal umfasst mehrere Schritte, einschließlich Active Directory Azure-Anwendung erstellen.  Dazu gehen wir, [folgen Sie dieser Anleitung](../articles/resource-group-create-service-principal-portal.md).

> [AZURE.IMPORTANT] Achten Sie darauf, dass alle [verknüpften Lernprogramm](../articles/resource-group-create-service-principal-portal.md)folgen.  Es ist *äußerst wichtig* , genau wie beschrieben ausführen.  Unbedingt beachten Ihrer **Mandanten-ID** **Mieter Domänennamen** (häufig eine *. onmicrosoft.com* Domäne, wenn Sie eine benutzerdefinierte Domäne angegeben haben), **Client-ID**und **Client Authentifizierungsschlüssel**, da wir diese später benötigen.  Ihre **Kundennummer** und **Client Authentifizierungsschlüssel**vorsichtig sein, diese Anmeldeinformationen von jedem auszuführenden Vorgänge als Service Principal verwendet werden können. 
>   
> Wenn Schritt [Konfigurieren Multi-Tenant-Anwendung](../articles/resource-group-create-service-principal-portal.md#configure-multi-tenant-application)Namens angezeigt wird, wählen Sie **Nein**.
> 
> Wenn Schritt [Anwendung Rolle zuweisen](../articles/resource-group-create-service-principal-portal.md#assign-application-to-role)angezeigt wird, statt die Rolle **Leser** zugewiesen Sie **CDN Profil** Teilnehmerrolle verwenden Sie wir *CdnConsoleTutorial*zuvor erstellten Ressourcengruppe  Nach der Anwendung der Beitragendenrolle **CDN Profil** auf der Ressourcengruppe zuweisen zu diesem Lernprogramm zurück. 

Einmal erstellte Ihre Service principal und der **CDN Profil Beitragender** Rolle **Benutzer** Blade für die Ressourcengruppe sieht ähnlich.

![Benutzer-blade](./media/cdn-app-dev-prep/cdn-service-principal-include.png)


### <a name="interactive-user-authentication"></a>Interaktive Benutzerauthentifizierung

Statt einen Dienstprinzipal Sie lieber einzelne interaktiven Authentifizierung haben, ist der Prozess ähnlich für einen Dienstprinzipal.  Tatsächlich müssen Sie dasselbe Verfahren, aber einige kleinere ändern.

> [AZURE.IMPORTANT] Nur folgendermaßen Sie weiter Wenn Sie principal Dienst einzelne Benutzer anstelle.

1. Beim Erstellen der Anwendung statt **Anwendung**wählen Sie **systemeigene Anwendung aus** 
    
    ![Systemeigene Anwendung](./media/cdn-app-dev-prep/cdn-native-application-include.png)
    
2. Auf der nächsten Seite werden Sie einen **URI umleiten**aufgefordert werden.  Der URI wird nicht überprüft, aber Eingaben erinnern.  Sie werden ihn später benötigen. 

3. Es braucht keinen **Authentifizierungsschlüssel Client**erstellen.

4. Anstatt einen Dienstprinzipal der Beitragendenrolle **CDN Profil** , gehen wir einzelnen Benutzern oder Gruppen zuweisen.  In diesem Beispiel sehen Sie, dass ich **CDN Profil** Teilnehmerrolle *CDN Demo Benutzer* zugewiesen haben.  
    
    ![Zugriff einzelner Benutzer](./media/cdn-app-dev-prep/cdn-aad-user-include.png)

