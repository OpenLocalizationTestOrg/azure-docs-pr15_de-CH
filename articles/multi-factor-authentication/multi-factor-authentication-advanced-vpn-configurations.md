<properties
    pageTitle="Erweiterte Szenarien 3rd Party VPNs mit Azure mehrstufige Authentifizierung"
    description="Diese Seite enthält Informationen für ein schrittweises Setup-Konfiguration für Azure MFA mit 3rd Party Produkte."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban" 
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="advanced-scenarios-with-azure-multi-factor-authentication-and-3rd-party-vpn"></a>Erweiterte Szenarien 3rd Party VPN mit Azure mehrstufige Authentifizierung
Azure mehrstufige Authentifizierung lässt sich nahtlos mit einer Vielzahl von Lösungen 3. VPN-Verbindung.  Dazu gehören Cisco® ASA VPN-Anwendung, Citrix NetScaler SSL VPN-Anwendung und die Juniper Netzwerke Secure Access-Pulse Secure verbinden sichere SSL VPN-Anwendung.

## <a name="cisco-asa-vpn-appliance-and-azure-multi-factor-authentication"></a>Cisco ASA VPN-Appliance und Azure mehrstufige Authentifizierung
Azure mehrstufige Authentifizierung nahtlos mit der Cisco® ASA VPN-Appliance zur zusätzlichen Sicherheit für Cisco AnyConnect VPN®-Benutzernamen und Zugriff auf.  Dies kann mit LDAP oder RADIUS-Protokoll.  Wählen Sie eine der folgenden ausführliche schrittweise Konfigurationshandbücher herunterladen.

Konfigurationshandbuch  | Beschreibung
------------- | ------------- |
[Cisco Anyconnect VPN und Azure MFA-Konfiguration für LDAP ASA](http://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | Nahtlose Integration Ihrer Cisco ASA VPN-Appliance mit Azure MFA mit LDAP|
[Cisco Anyconnect VPN und Azure MFA-Konfiguration für RADIUS ASA](http://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Nahtlose Integration Ihrer Cisco ASA VPN-Appliance mit Azure MFA mittels RADIUS

## <a name="citrix-netscaler-ssl-vpn-and-azure-multi-factor-authentication"></a>Citrix NetScaler SSL VPN und Azure mehrstufige Authentifizierung
Azure mehrstufige Authentifizierung nahtlos mit Ihrem Citrix NetScaler SSL VPN-Anwendung für zusätzliche Sicherheit Citrix NetScaler SSL VPN-Logins und Zugriff auf.  Dies kann mit LDAP oder RADIUS-Protokoll.  Wählen Sie eine der folgenden ausführliche schrittweise Konfigurationshandbücher herunterladen.

Konfigurationshandbuch  | Beschreibung
------------- | ------------- |
[Citrix NetScaler SSL VPN und Azure MFA-Konfiguration für LDAP](http://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | Nahtlose Integration mit Azure MFA-Appliance mit LDAP Citrix NetScaler SSL VPN|
[Citrix NetScaler SSL VPN und Azure MFA-Konfiguration für RADIUS](http://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Nahtlose Integration mit Azure MFA mit RADIUS der Citrix NetScaler SSL VPN-Anwendung

##<a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-multi-factor-authentication"></a>Juniper-Pulse Secure SSL VPN-Anwendung und Azure mehrstufige Authentifizierung
Azure mehrstufige Authentifizierung nahtlos mit der Juniper-Pulse Secure SSL VPN-Anwendung für zusätzliche Sicherheit Juniper-Pulse Secure SSL VPN-Logins und Zugriff auf.  Dies kann mit LDAP oder RADIUS-Protokoll.  Wählen Sie eine der folgenden ausführliche schrittweise Konfigurationshandbücher herunterladen.

Konfigurationshandbuch  | Beschreibung
------------- | ------------- |
[Juniper-Impuls sicheren SSL VPN und Azure MFA-Konfiguration für LDAP](http://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx)| Nahtlose Integration der Juniper-Pulse Secure SSL VPN mit Azure MFA-Gerät mithilfe von LDAP|
[Juniper-Impuls sicheren SSL VPN und Azure MFA-Konfiguration für RADIUS](http://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Nahtlose Integration mit Azure MFA mit RADIUS Juniper-Pulse Secure SSL VPN-Anwendung
