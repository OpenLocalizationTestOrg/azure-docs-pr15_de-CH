<properties
   pageTitle="SSMS Unterstützung für Azure AD MFA SQL-Datenbank mit SQL Data Warehouse | Microsoft Azure"
   description="Verwenden Sie mehrere berücksichtigt Authentifizierung mit SSMS für SQL-Datenbank und SQL Datawarehouse."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="10/04/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="ssms-support-for-azure-ad-mfa-with-sql-database-and-sql-data-warehouse"></a>SSMS Unterstützung für Azure AD MFA SQL-Datenbank mit SQL Data Warehouse

Azure SQL-Datenbank und Azure SQL Data Warehouse unterstützen jetzt Verbindungen von SQL Server Management Studio (SSMS) *Universelle Active Directory-Authentifizierung*verwenden. Universelle Active Directory-Authentifizierung ist eine interaktive Arbeit, die *Azure mehrstufige Authentifizierung* (MFA) unterstützt. Azure MFA hilft Schutz Zugriff auf Daten und Applikationen Benutzer Nachfrage nach einem einfachen Vorgang. Bietet starke Authentifizierung mit einfachen Überprüfungsoptionen – Anruf, SMS, Smartcards mit Pin oder mobile-app-Benachrichtigung – Benutzer die Methode sie bevorzugen. Eine mehrstufige Authentifizierung finden Sie unter [Mehrstufige Authentifizierung](../multi-factor-authentication/multi-factor-authentication.md).

SSMS unterstützt nun:

- Interaktive MFA mit Azure AD mit Popup-Dialogfeld Feld Validierung.
- Nicht interaktive Kennwort für Active Directory und Active Directory-integrierte Authentifizierung Methoden, die in vielen verschiedenen Programmen (ADO.NET JDBC, ODBC usw.) verwendet werden können. Diese beiden Methoden führen keine Popup-Dialogfelder.

Wenn das Benutzerkonto für MFA konfiguriert ist erfordert interaktive Authentifizierung Arbeitsablauf weitere Benutzerinteraktion Popup-Dialogfelder, Smartcards usw.. Wenn das Benutzerkonto für MFA konfiguriert ist, muss der Benutzer Azure Universal Authentifizierung für die Verbindung auswählen. Wenn das Benutzerkonto nicht MFA benötigen, kann Benutzer weiterhin die beiden Azure Active Directory-Authentifizierung Optionen verwenden.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>Universelle Authentifizierung Grenzen für SQL-Datenbank und SQL Data Warehouse

- SSMS ist ein Tool für MFA durch universelle Active Directory-Authentifizierung aktiviert.
- Für eine Instanz von SSMS Universal Authentifizierung kann nur ein Azure Active Directory-Konto anmelden. Anmelden als ein anderes Azure AD-Konto müssen Sie eine andere Instanz von SSMS verwenden. (Diese Einschränkung ist auf universelle Active Directory-Authentifizierung, auf verschiedenen Servern mit Active Directory-Kennwort-Authentifizierung, Active Directory-integrierte Authentifizierung oder SQL Server-Authentifizierung anmelden können).
- SSMS unterstützt universelle Active Directory-Authentifizierung für Objekt-Explorer Abfrage-Editor und Query Store Visualisierung.
- DacFx weder der Schema-Designer unterstützt universelle Authentifizierung.
- MSA-Konten sind für universelle Active Directory-Authentifizierung nicht unterstützt.
- Universelle Active Directory-Authentifizierung wird nicht in SSMS für Benutzer unterstützt, die in die aktuelle Active Directory aus anderen Azure Active Directory importiert werden. Diese Benutzer werden nicht unterstützt, da müsste eine Mandanten-ID die Konten überprüft und kein zum Bereitstellen Mechanismus.
- Es gibt keine zusätzliche softwareanforderungen für universelle Active Directory-Authentifizierung, außer dass Sie eine unterstützte Version von SSMS verwenden müssen.

## <a name="configuration-steps"></a>Konfigurationsschritte

Implementierung der kombinierten Authentifizierung erfordert vier grundlegende Schritte.

1. **Konfigurieren einer Azure Active Directory** – Weitere Informationen finden Sie in [Ihrer lokalen Identitäten Azure Active Directory integrieren](../active-directory/active-directory-aadconnect.md), [Fügen Sie Ihren eigenen Domänennamen Azure AD](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/) [unterstützt jetzt Microsoft Azure mit Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Verwalten von Azure AD-Verzeichnis](https://msdn.microsoft.com/library/azure/hh967611.aspx)und [Verwalten Azure AD mit Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).

2. **Konfigurieren Sie MFA** -schrittweise Anleitung finden Sie unter [Konfigurieren von Azure mehrstufige Authentifizierung](../multi-factor-authentication/multi-factor-authentication-whats-next.md). 

3. **Konfigurieren der SQL-Datenbank oder SQL Data Warehouse Azure AD-Authentifizierung** – eine schrittweise Anleitung finden Sie unter [Verbinden mit SQL-Datenbank oder SQL Data Warehouse durch Verwendung von Azure Active Directory-Authentifizierung](sql-database-aad-authentication.md).

4. **SSMS herunterladen** – auf dem Clientcomputer downloaden der neuesten SSMS (mindestens August 2016), vom [Download SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx).

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Mit universellen Authentifizierung SSMS verbinden

Verbindung zu SQL-Datenbank oder SQL Data Warehouse mithilfe der neuesten SSMS anzeigen folgendermaßen

1. Wählen Sie Verbindung mit Universal-Authentifizierung im Dialogfeld **Verbindung mit Server herstellen** , **Universelle Active Directory-Authentifizierung**.
![1mfa Universal verbinden][1]

2. Für SQL-Datenbank und SQL Data Warehouse klicken Sie auf **Optionen** und geben Sie im Dialogfeld **Optionen** für die Datenbank. Klicken Sie auf **Verbinden**.
3. Erscheint das Dialogfeld **Anmelden bei Ihrem Konto** ermöglichen Sie das Konto und Kennwort des Azure Active Directory.
![2mfa-anmelden][2]

    > [AZURE.NOTE] Für universelle Authentifizierung mit einem Konto die nicht MFA verbinden Sie an dieser Stelle. MFA-Anwendern mit folgenden Schritten fortfahren.
 
4. Möglicherweise zwei MFA Dialogfelder angezeigt. Einmal Vorgang hängt von der MFA-Administrator festlegen und daher sind optional. Für eine Domäne MFA aktiviert diesen Schritt ist in manchen Fällen (z. B. die Domäne Benutzer eine Smartcard und eine Pin erfordert).  
![3mfa einrichten][3]

5. Die zweite möglich einmal im Dialogfeld können Sie die Details Ihrer Authentifizierungsmethode. Die möglichen Optionen werden vom Systemadministrator konfiguriert.
![4mfa überprüfen 1][4]
 
6. Azure Active Directory Bestätigung Daten an Sie gesendet. Empfängt den Verifizierungscode geben im **Verifizierungscode eingeben** , und klicken Sie auf **Anmelden**.
![5mfa überprüfen 2][5]

Nach Abschluss der Überprüfung verbindet SSMS normalerweise vorausgesetzt gültige Anmeldeinformationen und Zugriff.

##<a name="next-steps"></a>Nächste Schritte  

Andere Zugriff auf Ihre Datenbank gewähren: [SQL Datenbankauthentifizierung und Autorisierung: Gewährung Zugriff](sql-database-manage-logins.md)  
Sicherstellen, dass andere Benutzer können durch die Firewall: [Konfigurieren einer Azure SQL-Datenbank auf Serverebene mithilfe Firewallregel Azure-Portal](sql-database-configure-firewall-settings.md)


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png

