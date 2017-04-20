<properties
   pageTitle="Erstellen Sie Azure Service principal mit PowerShell | Microsoft Azure"
   description="Beschreibt die Verwendung von Azure PowerShell ein Active Directory-Anwendung und Dienstprinzipalnamen erstellen und Zugriff auf Ressourcen über rollenbasierte Zugriffskontrolle gewähren. Es veranschaulicht, wie mit einem Kennwort oder Zertifikat authentifiziert."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a>Mithilfe von Azure PowerShell Dienstprinzipalnamen Zugriff auf die Ressourcen erstellen

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)

Wenn Sie eine Anwendung oder Skripts, die auf Ressourcen zugreifen, möchten Sie wahrscheinlich nicht dieser Prozess unter Ihren eigenen Anmeldeinformationen ausgeführt werden. Möglicherweise unterschiedliche Berechtigungen, die Sie für die Anwendung und nicht möchten, dass die Anwendung weiterhin mit Ihren Anmeldeinformationen, wenn Aufgaben ändern. Stattdessen erstellen Sie eine Identität für die Anwendung, die Anmeldeinformationen für die Authentifizierung und Rolle Aufgaben enthält. Jedes Mal, wenn die Anwendung ausgeführt wird, authentifiziert er sich mit diesen Anmeldeinformationen. In diesem Thema veranschaulicht die [Azure PowerShell](powershell-install-configure.md) verwenden, um alles einzurichten, Sie für eine Anwendung Anmeldeinformationen und Identität ausgeführt müssen.

PowerShell haben Sie zwei Optionen für die Authentifizierung der AD-Anwendung:

 - Kennwort
 - Zertifikat

Dieses Thema beschreibt, wie Sie beides in PowerShell. Wenn Sie von Programmierungsframework (solche Python, Ruby oder Node.js) Azure anmelden möchten, möglicherweise Kennwortauthentifizierung besten. Finden Sie vor der Entscheidung, ob ein Kennwort oder Zertifikat verwendet im Abschnitt [Sample Applications](#sample-applications) Beispiele in anderen Frameworks zu authentifizieren.

## <a name="active-directory-concepts"></a>Active Directory-Konzepte

In diesem Artikel erstellen Sie zwei Objekte - Anwendung Active Directory (AD) und Service principal. Die Anwendung ist die globale Darstellung der Anwendung. Es enthält die Anmeldeinformationen (eine Id und ein Kennwort oder Zertifikat). Der Service principal ist Vertretung der Anwendung in Active Directory. Es enthält die Funktion Zuordnung. Dieses Thema konzentriert sich auf eine einzelne Mieter Anwendung, wo die Anwendung nur eine Organisation ausgeführt soll. Normalerweise verwenden Sie einzelne Mieter Applikationen LOB Anwendung, die ausgeführt, innerhalb Ihrer Organisation. In einer Single-Tenant-Anwendung haben Sie AD app und einen Dienstprinzipalnamen.

Benötige Sie wundern sich vielleicht - Warum beide Objekte ich? Dieser Ansatz ist sinnvoller, bei Multi-Tenant-Applikationen. Normalerweise verwenden Sie Multi-Tenant-Applikationen für Software als Service (SaaS) Anwendung ausgeführt wird, die Anwendung in vielen verschiedenen Subskriptionen. Sie haben für Multi-Tenant-Applikationen AD app und mehrere Service-Prinzipale (eine in jeder Active Directory, die Zugriff auf die Anwendung gewährt). Zum Einrichten einer Multi-Tenant-Anwendung finden Sie unter [Entwicklerhandbuch Bewilligung mit der Azure-Ressourcen-Manager-API](resource-manager-api-authentication.md).

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Um dieses Thema abzuschließen, müssen Sie über ausreichende Berechtigungen in Azure Active Directory und der Azure-Abonnement. Insbesondere muß eine app in Active Directory erstellen und den Dienstprinzipalnamen eine Rolle zuweisen. 

In Active Directory muss Ihr Konto Administrator (z.B. **Globale Administratoren** oder **Benutzer Admin**). Wenn Ihr Konto **die Rolle** zugewiesen wird, müssen Sie ein Administrator die Berechtigungen erhöhen.

Ihr Abonnement kündigen müssen `Microsoft.Authorization/*/Write` Zugriff bis [Besitzerrolle](./active-directory/role-based-access-built-in-roles.md#owner) oder Benutzeradministratorrolle [Zugriff](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) gewährt wird. Wenn Ihrem Konto **der Beitragendenrolle** zugewiesen ist, Fehlermeldung Sie eine beim Versuch, den Dienstprinzipalnamen einer Rolle zuweisen. Wieder müssen Sie Ihren Abonnementadministrator Zugriffsrechte gewähren.

Gehen Sie nun einen Abschnitt [Kennwort](#create-service-principal-with-password) oder [Zertifikat](#create-service-principal-with-certificate) Authentifizierung.

## <a name="create-service-principal-with-password"></a>Erstellen Sie Service principal mit Kennwort

In diesem Abschnitt führen Sie die Schritte an:

- Erstellen Sie die Anwendung mit einem Kennwort
- der Dienstprinzipalnamen erstellen
- der Dienstprinzipalnamen Rolle zuweisen

Um schnell durchführen dieser Schritte finden Sie unter den folgenden drei Cmdlets. 

     $app = New-AzureRmADApplication -DisplayName "{app-name}" -HomePage "https://{your-domain}/{app-name}" -IdentifierUris "https://{your-domain}/{app-name}" -Password "{your-password}"
     New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
     New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Lassen Sie uns diese Schritte genauer um sicherzustellen, dass Sie den Prozess kennen.

1. Melden Sie sich bei Ihrem Konto an.

        Add-AzureRmAccount

1. Erstellen Sie eine neue Active Directory-Anwendung, indem Sie einen Anzeigenamen ein URI, der die Anwendung beschreibt, die URIs, die die Anwendung und das Kennwort für die Identität der Anwendung identifizieren.

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org/exampleapp" -IdentifierUris "https://www.contoso.org/exampleapp" -Password "<Your_Password>"

     Für einzelne Mandanten Applikationen die URIs werden nicht überprüft.
     
     Wenn Ihr Konto nicht die [erforderlichen Berechtigungen](#required-permissions) auf Active Directory haben, sehen Sie eine Fehlermeldung "Authentication_Unauthorized" oder "kein Abonnement gefunden im Kontext".

1. Prüfen Sie Neues Anwendungsobjekt 

        $app
        
     Beachten Sie insbesondere die Eigenschaft **ApplicationId** Service Prinzipale, Arbeitsaufträge für Benutzerrollen, erstellen und das Zugriffstoken benötigt wird.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}

2. Übergabe Ihrer Id der Active Directory-Anwendung erstellen Sie einen Dienstprinzipalnamen für Ihre Anwendung.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

3. Berechtigungen der Service principal für Ihr Abonnement. In diesem Beispiel wird **der Leserrolle Leseberechtigung für alle Ressourcen in der Anmeldung gewährt** den Dienstprinzipalnamen hinzufügen. Andere Rollen finden Sie unter [RBAC: integrierte Rollen](./active-directory/role-based-access-built-in-roles.md). Bieten Sie für den Parameter **ServicePrincipalName** **ApplicationId** , die Sie beim Erstellen der Anwendung verwendet. 

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Wenn Ihr Konto nicht über ausreichende Berechtigungen zum Zuweisen einer Rolle verfügt, wird eine Fehlermeldung angezeigt. Die Meldung zeigt Ihr Konto **hat keine Berechtigung zum Ausführen der Aktion 'Microsoft.Authorization/roleAssignments/write' für Bereich "Abonnements / {Guid}"**. 

Das wars! Ihre Anwendung und Dienstprinzipalnamen werden eingerichtet. Im nächste Abschnitt veranschaulicht die Anmeldeinformationen über PowerShell einloggen. Wenn Sie die Anmeldeinformationen in der codeanwendung verwenden möchten, springen Sie auf [Sample Applications](#sample-applications). 

### <a name="provide-credentials-through-powershell"></a>Anmeldeinformationen über PowerShell

Jetzt müssen Sie als Anwendung Operationen anmelden.

1. Erstellen Sie ein **PSCredential** -Objekt mit Ihren Anmeldeinformationen mit dem Befehl **Get-Credential** . Sie benötigen die **ApplicationId** vor dem Ausführen dieses Befehls so sicherstellen, dass Sie verfügbar sind, einzufügen.

        $creds = Get-Credential

2. Sie werden aufgefordert, Ihre Anmeldeinformationen einzugeben. Verwenden Sie für den Benutzernamen **ApplicationId** , die Sie beim Erstellen der Anwendung verwendet. Verwenden Sie das Kennwort ein, die Sie beim Erstellen des Kontos angegeben.

     ![Anmeldeinformationen eingeben](./media/resource-group-authenticate-service-principal/arm-get-credential.png)

2. Anmelden als Dienst Principal müssen Sie die Mandanten-Id des Verzeichnisses für Ihre AD bereitstellen. Ein Mieter ist eine Instanz von Active Directory. Wenn Sie nur über ein Abonnement verfügen, können Sie:

        $tenant = (Get-AzureRmSubscription).TenantId
    
     Haben Sie mehrere Abonnements Abonnements geben sich Active Directory befindet. Weitere Informationen finden Sie unter [wie Azure-Abonnements Azure Active Directory zugeordnet sind](./active-directory/active-directory-how-subscriptions-associated-directory.md).

        $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

4. Melden Sie sich als Service Principal durch Festlegen dieses Konto einen Dienstprinzipal und Anmeldeinformationsobjekt. 

        Add-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId $tenant
        
     Sie sind jetzt als Dienstprinzipal für die Active Directory-Anwendung authentifiziert, die Sie erstellt.

### <a name="save-access-token-to-simplify-log-in"></a>Speichern Sie Zugriffstoken zur Vereinfachung der Anmeldung

Um zu vermeiden, bietet Service principal Anmeldeinformationen jedes Mal anmelden, können Sie das Zugriffstoken speichern.

1. Speichern Sie das Profil für die Verwendung der aktuellen Zugriffstoken in einer späteren Sitzung.

        Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
     Öffnen Sie das Profil und untersuchen Sie seinen Inhalt. Beachten Sie, dass ein Zugriffstoken enthält. 
        
2. Anstatt manuell Anmelden erneut laden des Profils.

        Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
> [AZURE.NOTE] Das Zugriffstoken abläuft, damit mit einem gespeicherten Profil nur funktioniert für das Token gültig ist.
        
## <a name="create-service-principal-with-certificate"></a>Dienst mit principal erstellen

In diesem Abschnitt führen Sie die Schritte an:

- ein selbstsigniertes Zertifikat erstellen
- Erstellen Sie die Anwendung mit dem Zertifikat
- der Dienstprinzipalnamen erstellen
- der Dienstprinzipalnamen Rolle zuweisen

Um schnell durchführen dieser Schritte Azure PowerShell 2.0 für Windows 10 oder Windows Server 2016 Technical Preview finden Sie die folgenden Cmdlets. 

    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Lassen Sie uns diese Schritte genauer um sicherzustellen, dass Sie den Prozess kennen. Dieser Artikel veranschaulicht die Aufgaben bei Verwendung früherer Versionen von Azure PowerShell oder Betriebssystemen.

### <a name="create-the-self-signed-certificate"></a>Selbstsigniertes Zertifikat erstellen

PowerShell mit Windows 10 und Windows Server 2016 Technical Preview-Version hat eine aktualisierte **Neu SelfSignedCertificate** -Cmdlet für ein selbst signiertes Zertifikat generieren. Frühere Betriebssysteme haben das New-SelfSignedCertificate-Cmdlet bietet hierfür erforderlichen Parameter Stattdessen müssen Sie ein Modul generiert das Zertifikat importieren. In diesem Thema werden beide Ansätze für das Zertifikat des Betriebssystems haben Sie generieren 

- Haben Sie **Windows 10 oder Windows Server 2016 Technical Preview**, führen Sie den folgenden Befehl ein selbstsigniertes Zertifikat erstellen: 

        $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
       
- Wenn Sie **keinen Windows 10 oder Windows Server 2016 Technical Preview**, Sie das [selbstsignierte Zertifikat Generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) von Microsoft Script Center herunterladen müssen. Extrahieren Sie den Inhalt und importieren Sie die Cmdlets müssen Sie.
     
        # Only run if you could not use New-SelfSignedCertificate
        Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
    
     Generieren Sie das Zertifikat.
    
        $cert = New-SelfSignedCertificateEx -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"

Sie können haben Ihr Zertifikat und Ihre AD app erstellen.

### <a name="create-the-active-directory-app-and-service-principal"></a>Erstellen Sie die Active Directory-app und Service principal

1. Den Wert des Schlüssels aus dem Zertifikat abrufen

        $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

2. Mit der Azure-Konto anmelden.

        Add-AzureRmAccount

3. Erstellen Sie eine neue Active Directory-Anwendung, indem Sie einen Anzeigenamen ein URI, der die Anwendung beschreibt, die URIs, die die Anwendung und das Kennwort für die Identität der Anwendung identifizieren.

     Wenn Sie Azure PowerShell 2.0 (August 2016 oder höher) verwenden das folgende Cmdlet:

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Verwenden Sie Azure PowerShell 1.0 das folgende Cmdlet:
    
        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -KeyValue $keyValue -KeyType AsymmetricX509Cert  -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Für einzelne Mandanten Applikationen die URIs werden nicht überprüft.
    
    Wenn Ihr Konto nicht die [erforderlichen Berechtigungen](#required-permissions) auf Active Directory haben, sehen Sie eine Fehlermeldung "Authentication_Unauthorized" oder "kein Abonnement gefunden im Kontext".
        
    Prüfen Sie Neues Anwendungsobjekt 

        $app

    Beachten Sie die Eigenschaft **ApplicationId** Service Prinzipale, Arbeitsaufträge für Benutzerrollen, erstellen und Zugriffstoken benötigt wird.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}


5. Übergabe Ihrer Id der Active Directory-Anwendung erstellen Sie einen Dienstprinzipalnamen für Ihre Anwendung.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

6. Berechtigungen der Service principal für Ihr Abonnement. In diesem Beispiel wird **der Leserrolle Leseberechtigung für alle Ressourcen in der Anmeldung gewährt** den Dienstprinzipalnamen hinzufügen. Andere Rollen finden Sie unter [RBAC: integrierte Rollen](./active-directory/role-based-access-built-in-roles.md). Bieten Sie für den Parameter **ServicePrincipalName** **ApplicationId** , die Sie beim Erstellen der Anwendung verwendet.

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Wenn Ihr Konto nicht über ausreichende Berechtigungen zum Zuweisen einer Rolle verfügt, wird eine Fehlermeldung angezeigt. Die Meldung zeigt Ihr Konto **hat keine Berechtigung zum Ausführen der Aktion 'Microsoft.Authorization/roleAssignments/write' für Bereich "Abonnements / {Guid}"**.

Das wars! Ihre Anwendung und Dienstprinzipalnamen werden eingerichtet. Im nächste Abschnitt veranschaulicht mit Zertifikat über PowerShell anmelden.

### <a name="provide-certificate-through-automated-powershell-script"></a>Bescheinigung über automatische PowerShell-Skript

Anmelden als Dienst Principal müssen Sie die Mandanten-Id des Verzeichnisses für Ihre AD bereitstellen. Ein Mieter ist eine Instanz von Active Directory. Wenn Sie nur über ein Abonnement verfügen, können Sie:

    $tenant = (Get-AzureRmSubscription).TenantId
    
Haben Sie mehrere Abonnements Abonnements geben sich Active Directory befindet. Weitere Informationen finden Sie unter [Verwalten der Azure AD-Verzeichnis](./active-directory/active-directory-administer.md).

    $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

Authentifizierung in Ihrem Skript Geben Sie das Konto ist ein Service principal und den Fingerabdruck des Zertifikats, die Id der Anwendung und Mandanten-Id an Ihr Skript automatisieren, diese Werte als Variablen gespeichert und während der Ausführung abrufen und Sie in Ihr Skript aufnehmen.

    Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint $cert.Thumbprint -ApplicationId $app.ApplicationId -TenantId $tenant

Sie sind jetzt als Dienstprinzipal für die Active Directory-Anwendung authentifiziert, die Sie erstellt.

## <a name="sample-applications"></a>Anwendungsbeispiele

Die folgende Beispiel-Applikationen anzeigen Anmelden als Service principal

**.NET**

- [Bereitstellen einer SSH aktiviert VM mit einer Vorlage mit .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Verwalten von Azure-Ressourcen und Ressourcengruppen mit .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Erste Schritte mit Ressourcen - Bereitstellung mit Azure Ressourcenmanager Vorlage - in Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Erste Schritte mit Ressourcen - Ressourcengruppe verwaltet - in Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Bereitstellen einer SSH aktiviert VM mit einer Vorlage in Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Verwalten von Azure Ressourcen und Ressourcengruppen mit Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Bereitstellen einer SSH aktiviert VM mit einer Vorlage in Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Azure-Ressourcen und Ressourcengruppen mit Node.js verwalten](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

- [Bereitstellen einer SSH aktiviert VM mit einer Vorlage in Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Verwalten von Azure Ressourcen und Ressourcengruppen Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Nächste Schritte
  
- Detaillierte Schritte zur Integration von einer Anwendung in Azure zum Verwalten von Ressourcen finden Sie im [Entwicklerhandbuch Bewilligung mit der Azure-Ressourcen-Manager-API](resource-manager-api-authentication.md).
- Eine ausführlichere Erklärung und Prinzipale Service finden Sie unter [Anwendung und Service Principal-Objekte](./active-directory/active-directory-application-objects.md). 
- Weitere Informationen zu Active Directory-Authentifizierung finden Sie unter [Authentifizierungsszenarien Azure AD](./active-directory/active-directory-authentication-scenarios.md).



