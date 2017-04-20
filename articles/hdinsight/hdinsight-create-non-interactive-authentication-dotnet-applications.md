<properties
    pageTitle="Erstellen Sie nicht-interaktive Authentifizierung .NET HDInsight kühlgeometrie | Microsoft Azure"
    description="Erfahren Sie, wie nicht-interaktive Authentifizierung .NET HDInsight erstellt."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>Erstellen Sie nicht interaktive Authentifizierung .NET HDInsight

Sie können die Anwendung .NET Azure HDInsight Identität der Anwendung (nicht interaktiv) oder unter der Identität des angemeldeten Benutzers (interaktiv) Anwendung ausführen. Ein Beispiel für die interaktive Anwendung Stellenangebote [Senden Struktur/Schwein/Sqoop mit HDInsight .NET SDK](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk). Dieser Artikel veranschaulicht das nicht-interaktive Authentifizierung .NET Anwendung Azure HDInsight und Senden eines Auftrags Struktur erstellen.

Von der Anwendung .NET müssen:

- der Azure-Abonnement Mandanten-ID
- die Azure Directory Client-ID
- der Azure-Verzeichnis Anwendung geheime Schlüssel.  

Der zentrale Prozess umfasst folgende Schritte:

2. Erstellen Sie eine Anwendung Azure-Verzeichnis.
2. Die Anwendung Rollen zuweisen.
3. Entwickeln Sie die Clientanwendung.


##<a name="prerequisites"></a>Erforderliche Komponenten

- HDInsight-Cluster. Wie unter [Einführung](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster)verwenden können. 




## <a name="create-azure-directory-application"></a>Verzeichnis der Azure-Anwendung erstellen 
Wenn Sie eine Active Directory-Anwendung erstellen, erstellt sie eigentlich der Anwendung und einem Dienst principal. Sie können die Anwendung unter der Identität der Anwendung ausführen.

Azure-Verwaltungsportal verwenden Sie derzeit eine neue Active Directory-Anwendung erstellen. Diese Fähigkeit wird der Azure-Portal in einer späteren Version hinzugefügt. Sie können auch folgendermaßen Azure PowerShell oder Azure-Befehlszeilenschnittstelle ausführen. Finden Sie weitere Informationen über PowerShell oder CLI Service principal [authentifizieren Service principal mit Azure-Ressourcen-Manager](../resource-group-authenticate-service-principal.md).

**Erstellen eine Anwendung Azure-Verzeichnis**

1.  Melden Sie sich bei [Azure-Verwaltungsportal]( https://manage.windowsazure.com/)an.
2.  Wählen Sie im linken Bereich **Active Directory** .

    ![Azure klassischen Portal active directory](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\active-directory.png)
    
3.  Wählen Sie das Verzeichnis für die neue Anwendung erstellen. Vorhandenen werden.
4.  Klicken Sie auf **Applikationen** oben vorhandenen Applikationen aufgelistet.
5.  Klicken Sie unten, um eine neue Anwendung hinzufügen auf **Hinzufügen** .
6.  **Namen Sie**, wählen Sie **Web-Anwendung oder Web-API**und klicken Sie auf **Weiter**.

    ![neue Azure active Directory-Anwendung](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application.png)

7.  Geben Sie **URL anmelden** und **App-ID-URI**. **SIGN-ON-URL**bereitstellen Sie, eine Website, die die Anwendung beschreibt. Das Vorhandensein der Website ist nicht gültig. APP-ID URI bereitstellen Sie, die die Anwendung identifiziert. Und klicken Sie dann auf **abgeschlossen**.
Es dauert einen Moment, um die Anwendung zu erstellen.  Nachdem die Anwendung erstellt wurde, zeigt das Portal schnellen Blick auf die neue Anwendung an. Schließen Sie das Portal nicht. 

    ![neue Azure active Directory Anwendungseigenschaften](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application-properties.png)

**Zu dem Anwendungsclient-ID des geheimen Schlüssels**

1.  Klicken Sie auf neu erstellte Anwendung AD Menü **Konfigurieren** .
2.  Stellen Sie eine **Client-ID**ein. Sie benötigen in Ihrer.
3.  Unter **Schlüssel**auf **Dauer** Dropdownliste und wählen Sie entweder **1** oder **2 Jahre**. Der Wert wird nicht angezeigt, bis die Konfiguration zu speichern.
4.  Klicken Sie am unteren Rand der Seite **Speichern** . Wenn der geheime Schlüssel angezeigt wird, eine Kopie des Schlüssels. Sie benötigen in Ihrer.

##<a name="assign-ad-application-to-role"></a>Anwendung Rolle zuweisen

Sie müssen die Anwendung eine [Rolle](../active-directory/role-based-access-built-in-roles.md) Erteilen von Berechtigungen für Aktionen zuweisen. Der Bereich kann auf der Ebene des Abonnements, Ressourcengruppe oder Ressource festgelegt werden. Die Berechtigungen werden auf unteren Bereich (z. B. hinzufügen eine Anwendung Rolle für eine Ressourcengruppe bedeutet die Ressourcengruppe lesen kann und alle darin enthaltenen Ressourcen) geerbt. In diesem Lernprogramm wird der Bereich auf Ressource festgelegt werden.  Da der Azure-Verwaltungsportal Ressourcengruppen unterstützt, hat dieser Teil von Azure-Portal ausgeführt werden. 

**Die Besitzerrolle AD-Anwendung hinzu**

1.  Mit der [Azure-Portal](https://portal.azure.com)anmelden.
2.  Klicken Sie im linken Bereich **Ressourcengruppe** auf.
3.  Klicken Sie auf die Ressourcengruppe, die hdinsight-Cluster enthält, in dem Sie später in diesem Lernprogramm Hive-Abfrage ausgeführt wird. Sind zu viele Ressourcengruppen, können Sie den Filter.
4.  **Klicken Sie auf Cluster-Blatt.**

    ![Cloud und Blitz-Symbol = Schnellstart](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)
5.  Blatt **Benutzer** klicken Sie auf **Hinzufügen** .
6.  Führen Sie die Anweisung der Anwendung **die Rolle** hinzufügen in der letzten Prozedur erstellt. Wenn erfolgreich abgeschlossen, wird im Blatt Benutzer mit der Rolle aufgeführten Anwendung angezeigt.


##<a name="develop-hdinsight-client-application"></a>HDInsight-Clientanwendung entwickeln

Erstellen Sie eine C# .net Anweisungen [Senden Hadoop Aufträge in HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk)gefunden. Ersetzen Sie die GetTokenCloudCredentials-Methode mit folgenden:

    public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
    {
        var authFactory = new AuthenticationFactory();

        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };

        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];

        var accessToken =
            authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never)
                .AccessToken;

        return new TokenCloudCredentials(accessToken);
    }

Die Mandanten-ID über PowerShell abrufen:

    Get-AzureRmSubscription

Oder Azure CLI:

    azure account show --json

      
## <a name="see-also"></a>Siehe auch

- [Hadoop Aufträge in HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Erstellen Sie Active Directory-Anwendung und Service Principal mit portal](../resource-group-create-service-principal-portal.md)
- [Authentifizieren von Dienstprinzipalnamen mit Azure-Ressourcen-Manager](../resource-group-authenticate-service-principal.md)
- [Azure rollenbasierte Zugriffskontrolle](../active-directory/role-based-access-control-configure.md)
