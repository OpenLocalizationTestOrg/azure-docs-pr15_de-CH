<properties
   pageTitle="Benutzerdefinierte DNS-Datensätze für Web app erstellen | Microsoft Azure  "
   description="Benutzerdefinierte Domain DNS-Datensätze für Azure DNS verwenden WebApp erstellen"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a>Erstellen Sie DNS-Datensätze für eine Webanwendung in einer benutzerdefinierten Domäne

Azure DNS können Sie eine benutzerdefinierte Domäne für Ihr Web apps hosten. Beispielsweise eine Azure Web app erstellen und die Benutzer entweder Zugriff mit contoso.com oder www.contoso.com als FQDN.

Dazu müssen Sie zwei Datensätze erstellen:

- Eine auf contoso.com Stammdatensatz "A"
- "CNAME"-Eintrag für den Www-Namen, der auf den A-Eintrag verweist

Beachten Sie, dass Sie einen A-Eintrag für eine Webanwendung in Azure erstellen, der A-Datensatz manuell muss aktualisiert, wenn der zugrunde liegenden IP-für das Web app ändert Adresse.

## <a name="before-you-begin"></a>Bevor Sie beginnen

Bevor Sie beginnen, müssen Sie zunächst eine DNS-Zone in Azure DNS erstellen und delegieren die Zone in Ihrer Registrierungsstelle Azure DNS.

1. Um eine DNS-Zone zu erstellen, gehen Sie in [Erstellen einer DNS-Zone](dns-getstarted-create-dnszone.md).
2. Um DNS Azure DNS zu delegieren, gehen Sie in [DNS-Domäne delegieren](dns-domain-delegation.md).

Nach dem Erstellen einer Zone und Azure DNS delegieren, können Sie dann Datensätze für Ihre benutzerdefinierte Domain erstellen.


## <a name="1-create-an-a-record-for-your-custom-domain"></a>1. erstellen Sie 1. einen A-Eintrag für Ihre benutzerdefinierte domain

Ein A-Datensatz wird verwendet, um die Zuordnung eines Namens zu der IP-Adresse. Im folgenden Beispiel weisen wir @ als ein Datensatz eine IPv4-Adresse:

### <a name="step-1"></a>Schritt 1

Einen A-Datensatz erstellen und Zuweisen einer Variablen $rs

    $rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600

### <a name="step-2"></a>Schritt 2

IPv4-Wert auf die zuvor erstellte Datensatz hinzufügen "@" mit der $rs-Variablen zugewiesen. Zugewiesene IPv4-Wert werden die IP-Adresse Ihrer Anwendung.

Um die IP-Adresse für eine Webanwendung finden, folgen Sie den Schritten [Konfigurieren einer benutzerdefinierten Domänennamen in Azure App Service](../web-sites-custom-domain-name.md#Find-the-virtual-IP-address).

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address <your web app IP address>

### <a name="step-3"></a>Schritt 3

Änderungen Sie, die Datensatzgruppe. Mit `Set-AzureRMDnsRecordSet` Änderungen an den Eintrag Azure DNS hochladen:

    Set-AzureRMDnsRecordSet -RecordSet $rs

## <a name="2-create-a-cname-record-for-your-custom-domain"></a>2. erstellen Sie einen CNAME-Eintrag für Ihre benutzerdefinierte domain

Wenn Ihre Domäne Azure DNS bereits verwaltet (siehe [DNS-Domäne Delegierung](dns-domain-delegation.md)können Sie im folgenden Beispiel erstellen Sie einen CNAME-Eintrag für contoso.azurewebsites.net.

### <a name="step-1"></a>Schritt 1

PowerShell öffnen und einen neuen Datensatz CNAME-Datensatz Erstellen einer Variablen $rs zuzuweisen. In diesem Beispiel wird ein Datensatz CNAME mit"live" von 600 Sekunden im DNS-Zone mit dem Namen "contoso.com" erstellen.

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Schritt 2

Nachdem die CNAME-Datensätze erstellt, müssen Sie ein Aliaswert auf Web app erstellen.

Die zuvor zugewiesene Variable "$rs" mit können folgenden PowerShell-Befehl Sie Alias für Web app contoso.azurewebsites.net erstellen.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Schritt 3

Wechselt mit der `Set-AzureRMDnsRecordSet` Cmdlet:

    Set-AzureRMDnsRecordSet -RecordSet $rs

Den Datensatz überprüft wurde ordnungsgemäß von Abfragen mit Nslookup, "www.contoso.com" wie folgt:

    PS C:\> nslookup
    Default Server:  Default
    Address:  192.168.0.1

    > www.contoso.com
    Server:  default server
    Address:  192.168.0.1

    Non-authoritative answer:
    Name:    <instance of web app service>.cloudapp.net
    Address:  <ip of web app service>
    Aliases:  www.contoso.com
    contoso.azurewebsites.net
    <instance of web app service>.vip.azurewebsites.windows.net

## <a name="create-an-awverify-record-for-web-apps"></a>Erstellen eines "Awverify" für Web-apps


Wenn Sie einen A-Eintrag für Ihre Webanwendung verwenden möchten, gehen Sie durch eine Überprüfung, um sicherzustellen, dass die benutzerdefinierte Domäne besitzen. Dieser Schritt erfolgt durch einen speziellen CNAME-Eintrag mit dem Namen "Awverify" erstellen. Dieser Abschnitt gilt nur ein Datensätze.


### <a name="step-1"></a>Schritt 1

Erstellen Sie den Eintrag "Awverify". Im folgenden Beispiel erstellen wir "Aweverify" Datensatz für contoso.com Besitz der benutzerdefinierten Domäne überprüfen.

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "awverify" -RecordType "CNAME" -Ttl 600

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Schritt 2

Sobald der Datensatz "Awverify" erstellt, zuordnen Sie alias-set CNAME-Eintrag Im folgenden Beispiel weisen wir den CNAMe-Eintrag alias auf awverify.contoso.azurewebsites.net festgelegt.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Schritt 3

Wechselt mit der `Set-AzureRMDnsRecordSet cmdlet`(siehe folgenden Befehl).

    Set-AzureRMDnsRecordSet -RecordSet $rs



## <a name="next-steps"></a>Nächste Schritte

Führen Sie die Schritte zum Konfigurieren Ihrer Anwendung um eine benutzerdefinierte Domäne verwenden [einen benutzerdefinierten Domänennamen für App-Dienst konfigurieren](../app-service-web/web-sites-custom-domain-name.md) .








