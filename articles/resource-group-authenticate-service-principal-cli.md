<properties
   pageTitle="Erstellen Sie Service principal mit Azure CLI | Microsoft Azure"
   description="Beschreibt die Verwendung von Azure CLI eine Active Directory-Anwendung und Dienstprinzipalnamen erstellen und Zugriff auf Ressourcen über rollenbasierte Zugriffskontrolle gewähren. Es veranschaulicht, wie mit einem Kennwort oder Zertifikat authentifiziert."
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
   ms.date="09/30/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-cli-to-create-a-service-principal-to-access-resources"></a>Azure-CLI Dienstprinzipalnamen Zugriff auf die Ressourcen zu verwenden

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)


Wenn Sie eine Anwendung oder Skripts, die auf Ressourcen zugreifen, möchten Sie wahrscheinlich nicht dieser Prozess unter Ihren eigenen Anmeldeinformationen ausgeführt werden. Möglicherweise unterschiedliche Berechtigungen, die Sie für die Anwendung und nicht möchten, dass die Anwendung weiterhin mit Ihren Anmeldeinformationen, wenn Aufgaben ändern. Stattdessen erstellen Sie eine Identität für die Anwendung, die Anmeldeinformationen für die Authentifizierung und Rolle Aufgaben enthält. Jedes Mal, wenn die Anwendung ausgeführt wird, authentifiziert er sich mit diesen Anmeldeinformationen. In diesem Thema veranschaulicht die [CLI für Mac, Linux und Windows Azure](xplat-cli-install.md) verwenden, um eine Anwendung Anmeldeinformationen und Identität ausgeführt.

Mit Azure CLI haben Sie zwei Optionen für die Authentifizierung der AD-Anwendung:

 - Kennwort
 - Zertifikat

Dieses Thema beschreibt, wie Sie beides in Azure-CLI. Wenn Sie von Programmierungsframework (solche Python, Ruby oder Node.js) Azure anmelden möchten, möglicherweise Kennwortauthentifizierung besten. Finden Sie vor der Entscheidung, ob ein Kennwort oder Zertifikat verwendet im Abschnitt [Sample Applications](#sample-applications) Beispiele in anderen Frameworks zu authentifizieren.

## <a name="active-directory-concepts"></a>Active Directory-Konzepte

In diesem Artikel erstellen Sie zwei Objekte - Anwendung Active Directory (AD) und Service principal. Die Anwendung ist die globale Darstellung der Anwendung. Es enthält die Anmeldeinformationen (eine Id und ein Kennwort oder Zertifikat). Der Service principal ist Vertretung der Anwendung in Active Directory. Es enthält die Funktion Zuordnung. Dieses Thema konzentriert sich auf eine einzelne Mieter Anwendung, wo die Anwendung nur eine Organisation ausgeführt soll. Normalerweise verwenden Sie einzelne Mieter Applikationen LOB Anwendung, die ausgeführt, innerhalb Ihrer Organisation. In einer Single-Tenant-Anwendung haben Sie AD app und einen Dienstprinzipalnamen.

Benötige Sie wundern sich vielleicht - Warum beide Objekte ich? Dieser Ansatz ist sinnvoller, bei Multi-Tenant-Applikationen. Normalerweise verwenden Sie Multi-Tenant-Applikationen für Software als Service (SaaS) Anwendung ausgeführt wird, die Anwendung in vielen verschiedenen Subskriptionen. Sie haben für Multi-Tenant-Applikationen AD app und mehrere Service-Prinzipale (eine in jeder Active Directory, die Zugriff auf die Anwendung gewährt). Zum Einrichten einer Multi-Tenant-Anwendung finden Sie unter [Entwicklerhandbuch Bewilligung mit der Azure-Ressourcen-Manager-API](resource-manager-api-authentication.md).

## <a name="required-permissions"></a>Erforderliche Berechtigungen

Um dieses Thema abzuschließen, müssen Sie über ausreichende Berechtigungen in Azure Active Directory und der Azure-Abonnement. Insbesondere muß eine app in Active Directory erstellen und den Dienstprinzipalnamen eine Rolle zuweisen. 

In Active Directory muss Ihr Konto Administrator (z.B. **Globale Administratoren** oder **Benutzer Admin**). Wenn Ihr Konto **die Rolle** zugewiesen wird, müssen Sie ein Administrator die Berechtigungen erhöhen.

Ihr Abonnement kündigen müssen `Microsoft.Authorization/*/Write` Zugriff bis [Besitzerrolle](./active-directory/role-based-access-built-in-roles.md#owner) oder Benutzeradministratorrolle [Zugriff](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) gewährt wird. Wenn Ihrem Konto **der Beitragendenrolle** zugewiesen ist, Fehlermeldung Sie eine beim Versuch, den Dienstprinzipalnamen einer Rolle zuweisen. Wieder müssen Sie Ihren Abonnementadministrator Zugriffsrechte gewähren.

Gehen Sie nun einen Abschnitt [Kennwort](#create-service-principal-with-password) oder [Zertifikat](#create-service-principal-with-certificate) Authentifizierung.

## <a name="create-service-principal-with-password"></a>Erstellen Sie Service principal mit Kennwort

In diesem Abschnitt führen Sie die Schritte zum Erstellen der Anwendungdes mit einem Kennwort und Service principal Rolle zuweisen.

Lassen Sie uns diese Schritte.

1. Melden Sie sich bei Ihrem Konto an.

        azure login

1. Sie haben zwei Optionen zum Erstellen der Anwendungdes. Sie können AD-Anwendung und dem Dienst principal in einem Schritt erstellen oder separat erstellen. Erstellen sie in einem Schritt benötigen Sie keine Homepage und Bezeichner URIs für Ihre Anwendung angeben. Erstellen sie separat, möchten Sie diese Werte für Web app festgelegt. In diesem Schritt werden beide Optionen angezeigt.

     - Die Anwendung Service principal in einem Schritt erstellen und geben Sie den Namen der Anwendung und einem Kennwort wie im folgenden dargestellt:
     
            azure ad sp create -n exampleapp -p {your-password}     
     
     - Um die Anwendung zu erstellen, geben Sie den Namen der Anwendung, eine Homepage URI, Bezeichner URIs und ein Kennwort wie im folgenden dargestellt:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example -p <Your_Password>

         Der Befehl gibt einen AppId-Wert. Erstellen Sie einen Dienstprinzipal bieten Sie diesen Wert als Parameter in den folgenden Befehl:
     
            azure ad sp create -a <AppId>
     
     Wenn Ihr Konto nicht die [erforderlichen Berechtigungen](#required-permissions) auf Active Directory haben, sehen Sie eine Fehlermeldung "Authentication_Unauthorized" oder "kein Abonnement gefunden im Kontext".
    
     Bei beiden Optionen wird der neuen Dienstprinzipalnamen zurückgegeben. Die **Objekt-Id** ist erforderlich, wenn Sie Berechtigungen erteilen. Die Guid mit **Dienstprinzipalnamen** aufgeführt wird beim Anmelden benötigt. Diese Guid ist der gleiche Wert wie app-Id. In der Anwendung Beispiel ist dieser Wert die **Client-ID**genannt. 
    
        info:    Executing command ad sp create
        + Creating application exampleapp
        / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
        data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
        data:    Display Name:            exampleapp
        data:    Service Principal Names:
        data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
        data:                             https://www.contoso.org/example
        info:    ad sp create command OK

2. Berechtigungen der Service principal für Ihr Abonnement. In diesem Beispiel wird **der Leserrolle Leseberechtigung für alle Ressourcen in der Anmeldung gewährt** den Dienstprinzipalnamen hinzufügen. Andere Rollen finden Sie unter [RBAC: integrierte Rollen](./active-directory/role-based-access-built-in-roles.md). Bieten Sie für den Parameter **ServicePrincipalName** **ObjectId** , die Sie beim Erstellen der Anwendung verwendet. 

        azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/

     Wenn Ihr Konto nicht über ausreichende Berechtigungen zum Zuweisen einer Rolle verfügt, wird eine Fehlermeldung angezeigt. Die Meldung zeigt Ihr Konto **hat keine Berechtigung zum Ausführen der Aktion 'Microsoft.Authorization/roleAssignments/write' für Bereich "Abonnements / {Guid}"**. 

Das wars! Ihre Anwendung und Dienstprinzipalnamen werden eingerichtet. Im nächste Abschnitt veranschaulicht mit Anmeldeinformationen über CLI Azure anmelden. Wenn Sie die Anmeldeinformationen in der codeanwendung verwenden möchten, müssen Sie nicht mit diesem Thema fortfahren. Sie können Beispiele für die Anmeldung der Anwendung-Id und Kennwort [Sample Applications](#sample-applications) wechseln. 

### <a name="provide-credentials-through-azure-cli"></a>Anmeldeinformationen Sie über Azure-CLI

Jetzt müssen Sie als Anwendung Operationen anmelden.

1. Anmelden als Dienst Principal müssen Sie die Mandanten-Id des Verzeichnisses für Ihre AD bereitstellen. Ein Mieter ist eine Instanz von Active Directory. Rufen Sie die Mandanten-Id für das gerade authentifizierten Abonnement verwenden:

        azure account show

     Die zurückgibt:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Wenn die Mandanten-Id einem anderen Abonnement erhalten möchten, verwenden Sie folgenden Befehl:

        azure account show -s {subscription-id}

2. Verwenden Sie ggf. die Client-Id für Anmeldung abrufen:

        azure ad sp show -c exampleapp --json

     Der Wert für Anmeldung ist die Guid der Dienstprinzipalnamen aufgeführt.

        [
          {
            "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "7132aca4-1bdb-4238-ad81-996ff91d8db4"
            ]
          }
        ]

3. Melden Sie sich als Service Principal.

        azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}

    Sie sind für das Kennwort aufgefordert. Geben Sie das Kennwort, das Sie beim Erstellen der Anwendungdes angegeben.

        info:    Executing command login
        Password: ********

Sie sind jetzt als Dienstprinzipalnamen für den Dienstprinzipalnamen authentifiziert, die Sie erstellt haben.

## <a name="create-service-principal-with-certificate"></a>Dienst mit principal erstellen

In diesem Abschnitt führen Sie die Schritte an:

- ein selbstsigniertes Zertifikat erstellen
- Erstellen der Anwendungdes und das Zertifikat der Dienstprinzipalnamen
- der Dienstprinzipalnamen Rolle zuweisen

Diese Schritte müssen Sie [OpenSSL](http://www.openssl.org/) installiert.

1. Erstellen Sie ein selbstsigniertes Zertifikat.

        openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'

2. Kombinieren Sie die öffentlichen und privaten Schlüsseln.

        cat privkey.pem cert.pem > examplecert.pem

3. Öffnen Sie die Datei **examplecert.pem** und die lange Zeichenfolge **--BEGIN CERTIFICATE--** und **--Beenden Zertifikat---**suchen. Kopieren der Daten. Sie übergeben diese Daten als Parameter, der den Service principal erstellen.

1. Melden Sie sich bei Ihrem Konto an.

        azure login

1. Sie haben zwei Optionen zum Erstellen der Anwendungdes. Sie können AD-Anwendung und dem Dienst principal in einem Schritt erstellen oder separat erstellen. Erstellen sie in einem Schritt benötigen Sie keine Homepage und Bezeichner URIs für Ihre Anwendung angeben. Erstellen sie separat, möchten Sie diese Werte für Web app festgelegt. In diesem Schritt werden beide Optionen angezeigt.

     - Die Anwendung Service principal in einem Schritt erstellen und geben Sie den Namen der app und die Zertifikatdaten wie im folgenden dargestellt:
     
            azure ad sp create -n exampleapp --cert-value <certificate data>
     
     - Um die Anwendung zu erstellen, geben Sie den Namen der app, eine Homepage URI, Bezeichner URIs und die Zertifikatdaten wie im folgenden dargestellt:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example --cert-value <certificate data>

         Der Befehl gibt einen AppId-Wert. Erstellen Sie einen Dienstprinzipal bieten Sie diesen Wert als Parameter in den folgenden Befehl:
     
            azure ad sp create -a <AppId>
  
     Wenn Ihr Konto nicht die [erforderlichen Berechtigungen](#required-permissions) auf Active Directory haben, sehen Sie eine Fehlermeldung "Authentication_Unauthorized" oder "kein Abonnement gefunden im Kontext".
    
     Bei beiden Optionen wird der neuen Dienstprinzipalnamen zurückgegeben. Die Objekt-Id ist erforderlich, wenn Sie Berechtigungen erteilen. Die Guid mit **Dienstprinzipalnamen** aufgeführt wird beim Anmelden benötigt. Diese Guid ist der gleiche Wert wie app-Id. In der Anwendung Beispiel ist dieser Wert die **Client-ID**genannt. 
    
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
2. Berechtigungen der Service principal für Ihr Abonnement. In diesem Beispiel wird **der Leserrolle Leseberechtigung für alle Ressourcen in der Anmeldung gewährt** den Dienstprinzipalnamen hinzufügen. Andere Rollen finden Sie unter [RBAC: integrierte Rollen](./active-directory/role-based-access-built-in-roles.md). Bieten Sie für den Parameter **ServicePrincipalName** **ObjectId** , die Sie beim Erstellen der Anwendung verwendet. 

        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/

     Wenn Ihr Konto nicht über ausreichende Berechtigungen zum Zuweisen einer Rolle verfügt, wird eine Fehlermeldung angezeigt. Die Meldung zeigt Ihr Konto **hat keine Berechtigung zum Ausführen der Aktion 'Microsoft.Authorization/roleAssignments/write' für Bereich "Abonnements / {Guid}"**. 

### <a name="provide-certificate-through-automated-azure-cli-script"></a>Bescheinigung über automatisierte Azure CLI-Skript

Jetzt müssen Sie als Anwendung Operationen anmelden.

1. Anmelden als Dienst Principal müssen Sie die Mandanten-Id des Verzeichnisses für Ihre AD bereitstellen. Ein Mieter ist eine Instanz von Active Directory. Rufen Sie die Mandanten-Id für das gerade authentifizierten Abonnement verwenden:

        azure account show

     Die zurückgibt:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Wenn die Mandanten-Id einem anderen Abonnement erhalten möchten, verwenden Sie folgenden Befehl:

        azure account show -s {subscription-id}

1. Den Fingerabdruck des Zertifikats abrufen und entfernen Sie nicht benötigte Zeichen zu verwenden:

        openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
    
     Die gibt einen Fingerabdruckwert ähnelt:

        30996D9CE48A0B6E0CD49DBB9A48059BF9355851

2. Verwenden Sie ggf. die Client-Id für Anmeldung abrufen:

        azure ad sp show -c exampleapp

     Der Wert für Anmeldung ist die Guid der Dienstprinzipalnamen aufgeführt.

        [
          {
            "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "4fd39843-c338-417d-b549-a545f584a745",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "4fd39843-c338-417d-b549-a545f584a745"
            ]
          }
        ]

1. Melden Sie sich als Service Principal.

        azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}

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
- Weitere Informationen über Zertifikate und Azure CLI finden Sie [zertifikatbasierte Authentifizierung mit Azure Service Prinzipalen von Linux-Befehlszeile](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx). 
