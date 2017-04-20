<properties
    pageTitle="Zugriff auf Schlüssel Depot Firewall | Microsoft Azure"
    description="Erfahren Sie, wie eine Anwendung einen Firewall Schlüsseltresor zugreifen"
    services="key-vault"
    documentationCenter=""
    authors="amitbapat"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/13/2016"
    ms.author="ambapat"/>

# <a name="accessing-key-vault-behind-firewall"></a>Zugriff auf Schlüsseltresor Firewall
### <a name="q-my-key-vault-client-application-needs-to-be-behind-a-firewall-what-portshostsip-addresses-should-i-open-to-enable-access-to-key-vault"></a>F: Meine Key Vault-Clientanwendung muss hinter einer Firewall, welche Hosts/Ports/IP-Adressen muss ich öffnen, um Zugriff auf wichtige Vault?

Die wichtigsten Vault-Clientanwendung muss Zugriff auf wichtige Depot mehrere Endpunkte für verschiedene Funktionen können.

- Authentifizierung (Azure Active Directory)
- Verwaltung der Schlüssel Depot (einschließlich erstellen/lesen/aktualisieren/löschen und Richtlinien für Zugriff) über Azure-Ressourcen-Manager
- Zugreifen auf und Verwalten von Objekten (Schlüssel und Kennwörter) Schlüssel Depot selbst gespeichert, durchläuft Key Vault bestimmten Endpunkt (z.B. [https://yourvaultname.vault.azure.net](https://yourvaultname.vault.azure.net)).  

Abhängig von der Konfiguration und Umgebung gibt es einige Unterschiede.   

## <a name="ports"></a>Anschlüsse

Alle Schlüssel Tresor für alle 3 Funktionen (Authentifizierung, Management und Ebene Zugriff) Netzwerkverkehr über HTTPS: Port 443. Jedoch CRL werden gelegentlich Bildschirmbild (Port 80). Kunden, die Unterstützung von OCSP CRL sollte nicht erreichen, aber gelegentlich [http://cdp1.public-trust.com/CRL/Omniroot2025.crl](http://cdp1.public-trust.com/CRL/Omniroot2025.crl)erreichen können.  

## <a name="authentication"></a>Authentifizierung

Key Vault-Clientanwendung müssen Endpunkte für Authentifizierung Azure Active Directory zugreifen. Endpunkt verwendet hängt AAD Tenant-Konfiguration und der Principal - Benutzerprinzipalnamen, Dienstprinzipalnamen und die Art des Kontos, z. B. Microsoft Account oder Org-ID.  

| Principal-Typ | Endpunktport: |
|----------------|---------------|
| Benutzer mit Microsoft Account<br> (z.B.)user@hotmail.com) | **Global:**<br> Login.microsoftonline.com:443<br><br> **Azure China:**<br> Login.chinacloudapi.CN:443<br><br>**Azure US-Regierung:**<br> Login-us.microsoftonline.com:443<br><br>**Azure Deutschland:**<br> Login.microsoftonline.de:443<br><br> und <br>Login.Live.com:443   |
| Benutzer-Service Principal AAD (z. B. Organisations-ID mituser@contoso.com) | **Global:**<br> Login.microsoftonline.com:443<br><br> **Azure China:**<br> Login.chinacloudapi.CN:443<br><br>**Azure US-Regierung:**<br> Login-us.microsoftonline.com:443<br><br>**Azure Deutschland:**<br> Login.microsoftonline.de:443 |
| Benutzer-Service Principal mit Org ID + ADFS oder anderen verbundenen Endpunkt (z.B.user@contoso.com) | Die obigen Endpunkte für Org plus ADFS oder andere verbundene Endpunkte |

Es gibt andere komplexen Szenarien. [Azure Active Directory Authentifizierung fließen](/documentation/articles/active-directory-authentication-scenarios/), [Integration von Applikationen Azure Active Directory](/documentation/articles/active-directory-integrating-applications/) und [Active Directory Authentifizierungsprotokolle](https://msdn.microsoft.com/library/azure/dn151124.aspx) finden Sie weitere Informationen.  

## <a name="key-vault-management"></a>Key Vault-Management

Für Vault Schlüsselverwaltungsserver (CRUD und Richtlinien festlegen) muss Key Vault-Clientanwendung Azure Ressourcenmanager Endpunkt zugreifen.  

| Typ des Vorgangs | Endpunktport: |
|----------------|---------------|
| Key Vault Control-Plane-Operationen<br> über Azure Ressourcenmanager | **Global:**<br> Management.Azure.com:443<br><br> **Azure China:**<br> Management.chinacloudapi.CN:443<br><br> **Azure US-Regierung:**<br> Management.usgovcloudapi.NET:443<br><br> **Azure Deutschland:**<br> Management.microsoftazure.de:443 |
| Azure Active Directory Graph API | **Global:**<br> Graph.Windows.NET:443<br><br> **Azure China:**<br> Graph.chinacloudapi.CN:443<br><br> **Azure US-Regierung:**<br> Graph.Windows.NET:443<br><br> **Azure Deutschland:**<br> Graph.cloudapi.de:443 |

## <a name="key-vault-operations"></a>Key Vault-Operationen

Für alle Schlüssel Vault-Objekt (Schlüssel und Kennwörter) Management und kryptografischen Operationen muss Key Vault Key Vault-Endpunkt zugreifen. Je nach Ihrem Schlüsseltresor unterscheidet das DNS-Suffix Endpunkt. Key Vault-Endpunkt hat das Format: < Vault-Name >. < Region-spezifische-Dns-Suffix > wie in der folgenden Tabelle beschrieben.  

| Typ des Vorgangs | Endpunktport: |
|----------------|---------------|
| Key Vault-Operationen wie kryptografischen Schlüsseln erstellt/lesen/aktualisieren/löschen-Schlüssel und geheime Daten festlegen/abrufen Tags und andere Attribute Key Vault-Objekte (Schlüssel/Geheimnisse)     | **Global:**<br> &lt;Vault-Name&gt;. vault.azure.net:443<br><br> **Azure China:**<br> &lt;Vault-Name&gt;. vault.azure.cn:443<br><br> **Azure US-Regierung:**<br> &lt;Vault-Name&gt;. vault.usgovcloudapi.net:443<br><br> **Azure Deutschland:**<br> &lt;Vault-Name&gt;. vault.microsoftazure.de:443 |

## <a name="ip-address-ranges"></a>IP-Adressbereiche

Key Vault Dienst wiederum andere Azure Ressourcen PaaS-Infrastruktur verwendet, ist daher nicht zu einer bestimmten IP-Adressen, die wichtigsten Vault-Endpunkte jederzeit. Aber wenn die Firewall nur IP-Adresse unterstützt Bereiche dann Dokument [Microsoft Azure Datacenter IP-Bereiche Siehe](https://www.microsoft.com/download/details.aspx?id=41653) .   Für Authentifizierung und (Azure Active Directory) muss die Anwendung mit [Authentifizierung](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)und Identität Adressen beschriebenen Endpunkte verbinden können.

## <a name="next-steps"></a>Nächste Schritte

- Haben Sie Fragen zum Schlüssel Depot, besuchen Sie die [Azure Key Vault-Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)
