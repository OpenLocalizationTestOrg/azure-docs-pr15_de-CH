<properties
    pageTitle="Verschlüsselt: Schützen Sie vertrauliche Daten in Azure SQL-Datenbank mit Verschlüsselung | Microsoft Azure"
    description="Schützen Sie vertrauliche Daten in der SQL-Datenbank in Minuten."
    keywords="Verschlüsselung, Verschlüsselung, Cloud-Verschlüsselung"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="sstein"/>

# <a name="always-encrypted-protect-sensitive-data-in-sql-database-and-store-your-encryption-keys-in-azure-key-vault"></a>Verschlüsselt: Schützen Sie vertrauliche Daten in SQL-Datenbank und speichern Sie Verschlüsselungsschlüssel in Azure Key Vault

> [AZURE.SELECTOR]
- [Azure-Tresor](sql-database-always-encrypted-azure-key-vault.md)
- [Windows-Zertifikatspeicher](sql-database-always-encrypted.md)


Dieser Artikel veranschaulicht die Daten in einer SQL-Datenbank mit Verschlüsselung mithilfe des [Assistenten immer verschlüsselt](https://msdn.microsoft.com/library/mt459280.aspx) in [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/hh213248.aspx)sichern. Darüber hinaus Informationen, die Ihnen zeigen, wie jeder Schlüssel in Azure Key Vault zu speichern.

Verschlüsselte ist immer eine neue Daten-verschlüsselungstechnologie in Azure SQL-Datenbank und SQL Server, die schützt vertrauliche Daten auf dem Server während der Bewegung zwischen Client und Server, während die Daten verwendet werden. Immer gewährleistet verschlüsselte Daten niemals als Klartext in der Datenbanksystem angezeigt. Nach dem Konfigurieren der Verschlüsselung können nur Clientanwendungen oder Anwendungsserver, die Zugriff auf die Schlüssel unverschlüsselte Daten zugreifen. Ausführliche Informationen finden Sie unter [(Datenbankmodul) verschlüsselt](https://msdn.microsoft.com/library/mt163865.aspx).


Nach dem Konfigurieren der Datenbank verschlüsselt erstellen Sie einer Clientanwendung in C# mit Visual Studio arbeiten mit verschlüsselten Daten.

Führen Sie die Schritte in diesem Artikel und wie verschlüsselt für eine SQL Azure-Datenbank eingerichtet. In diesem Artikel erfahren Sie, wie Sie folgende Aufgaben ausführen:

- Mithilfe des Assistenten verschlüsselt in SSMS erstellen [Schlüssel verschlüsselt](https://msdn.microsoft.com/library/mt163865.aspx#Anchor_3).
    - Erstellen Sie einen [Hauptschlüssel Spalte (CMK)](https://msdn.microsoft.com/library/mt146393.aspx).
    - Erstellen einer [Spalte Schlüssel (CEK)](https://msdn.microsoft.com/library/mt146372.aspx).
- Erstellen einer Datenbanktabelle und Spalten zu verschlüsseln.
- Erstellen Sie eine Anwendung, die eingefügt, ausgewählt und Daten aus den verschlüsselten Spalten.


## <a name="prerequisites"></a>Erforderliche Komponenten

In diesem Lernprogramm benötigen Sie:

- Ein Azure-Konto und ein Abonnement. Haben Sie eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)registrieren.
- [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) Version 13.0.700.242 oder höher.
- [.NET Framework 4.6](https://msdn.microsoft.com/library/w0x726c2.aspx) oder höher (auf dem Clientcomputer).
- [Visual Studio](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
- [Azure PowerShell](../powershell-install-configure.md)Version 1.0 oder höher. Typ **(Get-Modul Azure - ListAvailable). Version** , welche Version von PowerShell Sie ausgeführt werden.



## <a name="enable-your-client-application-to-access-the-sql-database-service"></a>Aktivieren Sie die Clientanwendung auf dem SQL-Datenbank-Dienst

Aktivieren Sie die Clientanwendung auf dem SQL-Datenbank-Dienst erforderliche Authentifizierung einrichten und die *ClientId* und *Schlüssel* Sie Ihre Anwendung in den folgenden Code zu authentifizieren müssen.

1. Öffnen Sie das [klassische Azure-Portal](http://manage.windowsazure.com).
2. Wählen Sie **Active Directory** und Active Directory-Instanz, die die Anwendung auf.
3. Klicken Sie auf **Programme**und dann auf **Hinzufügen**.
4. Geben Sie einen Namen für Ihre Anwendung (z. B.: *MyClientApp*), wählen Sie **WEB-Anwendung**und klicken Sie auf den Pfeil, um fortzufahren.
5. **SIGN-ON URL** und **APP-ID-URI** können Geben Sie eine gültige URL (z. B. *http://myClientApp*) und.
6. Klicken Sie auf **Konfigurieren**.
7. Kopieren Sie die **CLIENT-ID** (Sie können diesen Wert im Code später benötigen.)
8. Wählen Sie im Abschnitt **Schlüssel** **1 Jahr** aus der Dropdownliste **Dauer** . (Den Schlüssel kopieren Sie nach dem Speichern in Schritt 14.)
11. Scrollen Sie nach unten, und klicken Sie auf **Anwendung hinzufügen**.
12. Belassen Sie **zeigen** **Microsoft** Apps, und wählen Sie **Microsoft Azure Service Management**. Klicken Sie auf das Häkchen fort.
13. Dropdownliste **Berechtigungen delegiert** **Verwaltung Azure Service** auswählen.
14. Klicken Sie auf **Speichern**.
15. Nachdem der Speichervorgang abgeschlossen ist, kopieren Sie den Schlüsselwert im Abschnitt **Schlüssel** . (Sie können diesen Wert im Code später benötigen.)



## <a name="create-a-key-vault-to-store-your-keys"></a>Erstellen Sie ein Schlüssel Depot Schlüssel speichern

Damit Ihre Clientanwendung und Sie Ihre Kundennummer, ist es Zeit zu einem Schlüssel Depot die Zugriffsrichtlinie konfigurieren, damit Sie und Ihre Anwendung das Depot Geheimnisse (Schlüssel verschlüsselt) zugreifen können. Die Berechtigungen *Erstellen*, *Abrufen*, *Liste*, *Zeichen*, *Vergewissern Sie sich*, *WrapKey*und *UnwrapKey* müssen zum Erstellen eines neuen Spalte Hauptschlüssel und Verschlüsselung mit SQL Server Management Studio.

Sie können schnell ein Schlüssel Depot durch Ausführen des folgenden Skripts erstellen. Eine ausführliche Erläuterung dieser Cmdlets und Weitere Informationen zum Erstellen und Konfigurieren von Key Vault finden Sie unter [Erste Schritte mit Azure Schlüssel](../Key-Vault/key-vault-get-started.md).



    $subscriptionName = '<your Azure subscription name>'
    $userPrincipalName = '<username@domain.com>'
    $clientId = '<client ID that you copied in step 7 above>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $vaultName = 'AeKeyVault'


    Login-AzureRmAccount
    $subscriptionId = (Get-AzureRmSubscription -SubscriptionName $subscriptionName).SubscriptionId
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup –Name $resourceGroupName –Location $location
    New-AzureRmKeyVault -VaultName $vaultName -ResourceGroupName $resourceGroupName -Location $location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -PermissionsToKeys create,get,wrapKey,unwrapKey,sign,verify,list -UserPrincipalName $userPrincipalName
    Set-AzureRmKeyVaultAccessPolicy  -VaultName $vaultName  -ResourceGroupName $resourceGroupName -ServicePrincipalName $clientId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list




## <a name="create-a-blank-sql-database"></a>Erstellen Sie eine leere SQL-Datenbank
1. Mit der [Azure-Portal](https://portal.azure.com/)anmelden.
2. Klicken Sie auf **neue** > **Daten + Speicher** > **SQL-Datenbank**.
3. Erstellen Sie eine **leere** Datenbank **Clinic** auf einer neuen oder vorhandenen Namen. Ausführliche Anleitung zum Erstellen einer Datenbank in Azure-Portal finden Sie unter [Erstellen einer SQL-Datenbank in Minuten](sql-database-get-started.md).

    ![Erstellen einer leeren Datenbank](./media/sql-database-always-encrypted-azure-key-vault/create-database.png)

Sie benötigen die Verbindung später im Lernprogramm Zeichenfolge, also nach dem Erstellen der Datenbank in die neue Klinik Datenbank durchsuchen und Verbindungszeichenfolge kopieren. Abrufen der Verbindungszeichenfolge jederzeit jedoch leicht im Azure-Portal zu kopieren.

1. Klicken Sie auf **SQL-Datenbanken** > **Klinik** > **Datenbank-Verbindungszeichenfolgen anzeigen**.
2. Kopieren Sie die Verbindungszeichenfolge für **ADO.NET**.

    ![Die Verbindungszeichenfolge kopieren](./media/sql-database-always-encrypted-azure-key-vault/connection-strings.png)


## <a name="connect-to-the-database-with-ssms"></a>Verbinden Sie mit der Datenbank mit SSMS

SSMS öffnen und an den Server mit der Klinik Datenbank verbinden.


1. SSMS zu öffnen. (Gehe zu **Verbinden** > -**Datenbank-Engine** **mit Server** -Fenster öffnen, wenn er nicht geöffnet ist.)
2. Geben Sie die Server- und Anmeldeinformationen. Der Servername finden Sie auf der SQL-Datenbank und in der Verbindungszeichenfolge kopiert zuvor. Geben Sie den vollständigen Servernamen ein, einschließlich *database.windows.net*.

    ![Die Verbindungszeichenfolge kopieren](./media/sql-database-always-encrypted-azure-key-vault/ssms-connect.png)

Öffnet das Fenster **Neue Firewall-Regel** in Azure anmelden und SSMS für Sie eine neue Firewallregel erstellen lassen.


## <a name="create-a-table"></a>Erstellen einer Tabelle

In diesem Abschnitt erstellen Sie eine Tabelle für Patientendaten. Anfänglich ist es nicht verschlüsselt – konfigurieren Sie Verschlüsselung im nächsten Abschnitt.

1. Erweitern Sie **Datenbanken**.
1. **Klinik** -Datenbank und **Neue Abfrage**.
2. Neues Abfragefenster und **Ausführen** der folgenden Transact-SQL (T-SQL) fügen sie.


        CREATE TABLE [dbo].[Patients](
         [PatientId] [int] IDENTITY(1,1),
         [SSN] [char](11) NOT NULL,
         [FirstName] [nvarchar](50) NULL,
         [LastName] [nvarchar](50) NULL,
         [MiddleName] [nvarchar](50) NULL,
         [StreetAddress] [nvarchar](50) NULL,
         [City] [nvarchar](50) NULL,
         [ZipCode] [char](5) NULL,
         [State] [char](2) NULL,
         [BirthDate] [date] NOT NULL
         PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] );
         GO


## <a name="encrypt-columns-configure-always-encrypted"></a>Verschlüsseln von Spalten (Konfigurieren verschlüsselt)

SSMS enthält einen Assistenten, leicht verschlüsselt durch Einrichten der Hauptschlüssel Spalte Spalte Schlüssel und verschlüsselten Spalten für Sie konfigurieren.

1. Erweitern Sie **Datenbanken** > **Klinik** > **Tabellen**.
2. Tabelle **Patienten** Maustaste, und wählen Sie **Spalten verschlüsseln** verschlüsselt-Assistenten zu öffnen:

    ![Verschlüsseln von Spalten](./media/sql-database-always-encrypted-azure-key-vault/encrypt-columns.png)

Verschlüsselt-Assistent umfasst die folgenden Abschnitte: **Spaltenauswahl**, **Hauptschlüssel Konfiguration**, **Validierung**und **Zusammenfassung**.

### <a name="column-selection"></a>Spaltenauswahl##

Klicken Sie auf **der Einführungsseite auf der Seite **Spalte** ** **Weiter** . Auf dieser Seite Wählen Sie die Spalten zu verschlüsseln, [den Typ der Verschlüsselung, und welche Spalte (CEK)](https://msdn.microsoft.com/library/mt459280.aspx#Anchor_2) verwenden.

Verschlüsseln Sie Informationen für jeden Patienten **SSN** und **Geburtsdatum** . Deterministische Verschlüsselung verwendet unterstützt Gleichheit Lookups Joins und Gruppieren nach Spalte SSN. Die BirthDate-Spalte verwendet zufällige Verschlüsselung die Vorgänge nicht unterstützt.

**Verschlüsselungstyp** für die Spalte SSN auf **deterministische** und die BirthDate-Spalte auf **Randomized**festgelegt. Klicken Sie auf **Weiter**.

![Verschlüsseln von Spalten](./media/sql-database-always-encrypted-azure-key-vault/column-selection.png)

### <a name="master-key-configuration"></a>Hauptschlüssel-Konfiguration###

**Hauptschlüssel** -Konfigurationsseite wird Ihre CMK einrichten und Schlüsselspeicher wählen, wo die CMK gespeichert werden. Derzeit können Sie eine CMK im Windows-Zertifikatspeicher, Azure Schlüssel Depot oder einem Hardwaresicherheitsmodul (HSM) speichern.

Dieses Lernprogramm zeigt wie die Schlüssel in Azure Schlüssel Depot gespeichert.

1.     **Azure-Tresor**auswählen
1.     Wählen Sie das gewünschte Schlüssel Depot aus der Dropdown-Liste.
1.     Klicken Sie auf **Weiter**.

![Hauptschlüssel-Konfiguration](./media/sql-database-always-encrypted-azure-key-vault/master-key-configuration.png)


### <a name="validation"></a>Validierung###

Sie können jetzt Spalten verschlüsseln oder ein PowerShell-Skripts zur späteren Ausführung zu speichern. In diesem Lernprogramm wählen Sie **gehen jetzt** und auf **Weiter**.

### <a name="summary"></a>Zusammenfassung ###

Überprüfen Sie die Einstellung alle korrigieren, und klicken Sie auf **Fertig stellen** , um das Setup für verschlüsselt abzuschließen.


![Zusammenfassung](./media/sql-database-always-encrypted-azure-key-vault/summary.png)


### <a name="verify-the-wizards-actions"></a>Der Assistent Aktionen überprüfen

Nachdem der Assistent abgeschlossen ist, wird die Datenbank für verschlüsselt eingerichtet. Assistenten folgenden Aktionen ausgeführt:

- Erstellt einen Hauptschlüssel Spalte und in Azure Schlüssel Depot gespeichert.
- Erstellt einen Verschlüsselungsschlüssel Spalte und in Azure Schlüssel Depot gespeichert.
- Die ausgewählten Spalten für Verschlüsselung konfiguriert. Tabelle Patienten hat zurzeit keine Daten, aber alle vorhandenen Daten in den ausgewählten Spalten nun verschlüsselt.

Überprüfen die Erstellung der Schlüssel in SSMS erweitern **Technologieüberblick** > **Security** > **Schlüssel immer verschlüsselt**.


## <a name="create-a-client-application-that-works-with-the-encrypted-data"></a>Erstellen einer Clientanwendung, die mit der verschlüsselten Daten

Verschlüsselt eingerichtet ist, können Sie eine Anwendung erstellen, die den verschlüsselten Spalten *eingefügt* und *wählt* ausführt.  

> [AZURE.IMPORTANT] Wenn unverschlüsselte Daten an den Server verschlüsselt Spalten muss die Anwendung [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx) -Objekte verwenden. Literale Werte ohne SqlParameter-Objekten übergeben führt zu einer Ausnahme.

1. Öffnen Sie Visual Studio, und erstellen Sie eine neue C#. Stellen Sie sicher, dass das Projekt auf **.NET Framework 4.6** oder höher festgelegt ist.
2. Nennen Sie das Projekt **AlwaysEncryptedConsoleAKVApp** , und klicken Sie auf **OK**.
![Neue](./media/sql-database-always-encrypted-azure-key-vault/console-app.png)
3. Die folgenden NuGet-Pakete zu **Tools**installieren > **NuGet Paket-Manager** > **Paket-Manager-Konsole**.

Diese zwei Zeilen in der Paket-Manager-Konsole ausführen.

    Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory



## <a name="modify-your-connection-string-to-enable-always-encrypted"></a>Ändern Sie die Verbindungszeichenfolge verschlüsselt aktivieren

Dieser Abschnitt erläutert die verschlüsselt in der Datenbank-Verbindungszeichenfolge aktivieren.


Aktivieren verschlüsselt, müssen Sie die Verbindungszeichenfolge **Spalte Verschlüsselungseinstellung** Schlüsselwort hinzu und **aktiviert**festgelegt.

Sie können dies direkt in der Verbindungszeichenfolge festlegen oder mithilfe [SqlConnectionStringBuilder](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.aspx)festlegen. Die beispielanwendung im nächsten Abschnitt veranschaulicht, wie **SqlConnectionStringBuilder**.



### <a name="enable-always-encrypted-in-the-connection-string"></a>Die Verbindungszeichenfolge verschlüsselt aktivieren

Das folgende Schlüsselwort zur Verbindungszeichenfolge hinzufügen

    Column Encryption Setting=Enabled


### <a name="enable-always-encrypted-with-sqlconnectionstringbuilder"></a>Verschlüsselt mit SqlConnectionStringBuilder aktivieren

Der folgende Code veranschaulicht die verschlüsselt aktivieren, indem Sie [SqlConnectionStringBuilder.ColumnEncryptionSetting](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectionstringbuilder.columnencryptionsetting.aspx) [aktiviert](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnectioncolumnencryptionsetting.aspx).

    // Instantiate a SqlConnectionStringBuilder.
    SqlConnectionStringBuilder connStringBuilder =
       new SqlConnectionStringBuilder("replace with your connection string");

    // Enable Always Encrypted.
    connStringBuilder.ColumnEncryptionSetting =
       SqlConnectionColumnEncryptionSetting.Enabled;

## <a name="register-the-azure-key-vault-provider"></a>Azure Key Vault-Anbieter registrieren

Im folgenden Codebeispiel wird mit der ADO.NET Azure Key Vault-Anbieter registrieren.

    private static ClientCredential _clientCredential;

    static void InitializeAzureKeyVaultProvider()
    {
       _clientCredential = new ClientCredential(clientId, clientSecret);

       SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
          new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

       Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
          new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

       providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
       SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
    }



## <a name="always-encrypted-sample-console-application"></a>Immer verschlüsselt Beispiel Konsolenanwendungsprojekt

In diesem Beispiel wird veranschaulicht, wie Sie:

- Ändern Sie die Verbindungszeichenfolge verschlüsselt aktivieren.
- Registrieren Sie Azure Schlüssel Depot als die Anwendung wichtige Anbieter.  
- Fügen Sie den verschlüsselten Spalten mit Daten.
- Wählen Sie einen Datensatz durch einen bestimmten Wert in eine verschlüsselte Spalte filtern.

Ersetzen Sie den Inhalt von **"Program.cs"** durch den folgenden Code. Ersetzen Sie die Verbindungszeichenfolge für die globale ConnectionString-Variable in der Zeile, die direkt vor der Main-Methode mit der gültigen Verbindungszeichenfolge Azure-Portal. Dies ist die einzige Änderung dieser Code zu müssen.

Führen Sie die Anwendung verschlüsselt in Aktion zu sehen.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Data;
    using System.Data.SqlClient;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider;

    namespace AlwaysEncryptedConsoleAKVApp
    {
    class Program
    {
        // Update this line with your Clinic database connection string from the Azure portal.
        static string connectionString = @"<connection string from the portal>";
        static string clientId = @"<client id from step 7 above>";
        static string clientSecret = "<key from step 13 above>";


        static void Main(string[] args)
        {
            InitializeAzureKeyVaultProvider();

            Console.WriteLine("Signed in as: " + _clientCredential.ClientId);

            Console.WriteLine("Original connection string copied from the Azure portal:");
            Console.WriteLine(connectionString);

            // Create a SqlConnectionStringBuilder.
            SqlConnectionStringBuilder connStringBuilder =
                new SqlConnectionStringBuilder(connectionString);

            // Enable Always Encrypted for the connection.
            // This is the only change specific to Always Encrypted
            connStringBuilder.ColumnEncryptionSetting =
                SqlConnectionColumnEncryptionSetting.Enabled;

            Console.WriteLine(Environment.NewLine + "Updated connection string with Always Encrypted enabled:");
            Console.WriteLine(connStringBuilder.ConnectionString);

            // Update the connection string with a password supplied at runtime.
            Console.WriteLine(Environment.NewLine + "Enter server password:");
            connStringBuilder.Password = Console.ReadLine();


            // Assign the updated connection string to our global variable.
            connectionString = connStringBuilder.ConnectionString;


            // Delete all records to restart this demo app.
            ResetPatientsTable();

            // Add sample data to the Patients table.
            Console.Write(Environment.NewLine + "Adding sample patient data to the database...");

            InsertPatient(new Patient()
            {
                SSN = "999-99-0001",
                FirstName = "Orlando",
                LastName = "Gee",
                BirthDate = DateTime.Parse("01/04/1964")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0002",
                FirstName = "Keith",
                LastName = "Harris",
                BirthDate = DateTime.Parse("06/20/1977")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0003",
                FirstName = "Donna",
                LastName = "Carreras",
                BirthDate = DateTime.Parse("02/09/1973")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0004",
                FirstName = "Janet",
                LastName = "Gates",
                BirthDate = DateTime.Parse("08/31/1985")
            });
            InsertPatient(new Patient()
            {
                SSN = "999-99-0005",
                FirstName = "Lucy",
                LastName = "Harrington",
                BirthDate = DateTime.Parse("05/06/1993")
            });


            // Fetch and display all patients.
            Console.WriteLine(Environment.NewLine + "All the records currently in the Patients table:");

            foreach (Patient patient in SelectAllPatients())
            {
                Console.WriteLine(patient.FirstName + " " + patient.LastName + "\tSSN: " + patient.SSN + "\tBirthdate: " + patient.BirthDate);
            }

            // Get patients by SSN.
            Console.WriteLine(Environment.NewLine + "Now lets locate records by searching the encrypted SSN column.");

            string ssn;

            // This very simple validation only checks that the user entered 11 characters.
            // In production be sure to check all user input and use the best validation for your specific application.
            do
            {
                Console.WriteLine("Please enter a valid SSN (ex. 999-99-0003):");
                ssn = Console.ReadLine();
            } while (ssn.Length != 11);

            // The example allows duplicate SSN entries so we will return all records
            // that match the provided value and store the results in selectedPatients.
            Patient selectedPatient = SelectPatientBySSN(ssn);

            // Check if any records were returned and display our query results.
            if (selectedPatient != null)
            {
                Console.WriteLine("Patient found with SSN = " + ssn);
                Console.WriteLine(selectedPatient.FirstName + " " + selectedPatient.LastName + "\tSSN: "
                    + selectedPatient.SSN + "\tBirthdate: " + selectedPatient.BirthDate);
            }
            else
            {
                Console.WriteLine("No patients found with SSN = " + ssn);
            }

            Console.WriteLine("Press Enter to exit...");
            Console.ReadLine();
        }


        private static ClientCredential _clientCredential;

        static void InitializeAzureKeyVaultProvider()
        {

            _clientCredential = new ClientCredential(clientId, clientSecret);

            SqlColumnEncryptionAzureKeyVaultProvider azureKeyVaultProvider =
              new SqlColumnEncryptionAzureKeyVaultProvider(GetToken);

            Dictionary<string, SqlColumnEncryptionKeyStoreProvider> providers =
              new Dictionary<string, SqlColumnEncryptionKeyStoreProvider>();

            providers.Add(SqlColumnEncryptionAzureKeyVaultProvider.ProviderName, azureKeyVaultProvider);
            SqlConnection.RegisterColumnEncryptionKeyStoreProviders(providers);
        }

        public async static Task<string> GetToken(string authority, string resource, string scope)
        {
            var authContext = new AuthenticationContext(authority);
            AuthenticationResult result = await authContext.AcquireTokenAsync(resource, _clientCredential);

            if (result == null)
                throw new InvalidOperationException("Failed to obtain the access token");
            return result.AccessToken;
        }

        static int InsertPatient(Patient newPatient)
        {
            int returnValue = 0;

            string sqlCmdText = @"INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate])
     VALUES (@SSN, @FirstName, @LastName, @BirthDate);";

            SqlCommand sqlCmd = new SqlCommand(sqlCmdText);


            SqlParameter paramSSN = new SqlParameter(@"@SSN", newPatient.SSN);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            SqlParameter paramFirstName = new SqlParameter(@"@FirstName", newPatient.FirstName);
            paramFirstName.DbType = DbType.String;
            paramFirstName.Direction = ParameterDirection.Input;

            SqlParameter paramLastName = new SqlParameter(@"@LastName", newPatient.LastName);
            paramLastName.DbType = DbType.String;
            paramLastName.Direction = ParameterDirection.Input;

            SqlParameter paramBirthDate = new SqlParameter(@"@BirthDate", newPatient.BirthDate);
            paramBirthDate.SqlDbType = SqlDbType.Date;
            paramBirthDate.Direction = ParameterDirection.Input;

            sqlCmd.Parameters.Add(paramSSN);
            sqlCmd.Parameters.Add(paramFirstName);
            sqlCmd.Parameters.Add(paramLastName);
            sqlCmd.Parameters.Add(paramBirthDate);

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();
                }
                catch (Exception ex)
                {
                    returnValue = 1;
                    Console.WriteLine("The following error was encountered: ");
                    Console.WriteLine(ex.Message);
                    Console.WriteLine(Environment.NewLine + "Press Enter key to exit");
                    Console.ReadLine();
                    Environment.Exit(0);
                }
            }
            return returnValue;
        }


        static List<Patient> SelectAllPatients()
        {
            List<Patient> patients = new List<Patient>();


            SqlCommand sqlCmd = new SqlCommand(
              "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients]",
                new SqlConnection(connectionString));


            using (sqlCmd.Connection = new SqlConnection(connectionString))

            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patients.Add(new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            });
                        }
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }

            return patients;
        }


        static Patient SelectPatientBySSN(string ssn)
        {
            Patient patient = new Patient();

            SqlCommand sqlCmd = new SqlCommand(
                "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN]=@SSN",
                new SqlConnection(connectionString));

            SqlParameter paramSSN = new SqlParameter(@"@SSN", ssn);
            paramSSN.DbType = DbType.AnsiStringFixedLength;
            paramSSN.Direction = ParameterDirection.Input;
            paramSSN.Size = 11;

            sqlCmd.Parameters.Add(paramSSN);


            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    SqlDataReader reader = sqlCmd.ExecuteReader();

                    if (reader.HasRows)
                    {
                        while (reader.Read())
                        {
                            patient = new Patient()
                            {
                                SSN = reader[0].ToString(),
                                FirstName = reader[1].ToString(),
                                LastName = reader["LastName"].ToString(),
                                BirthDate = (DateTime)reader["BirthDate"]
                            };
                        }
                    }
                    else
                    {
                        patient = null;
                    }
                }
                catch (Exception ex)
                {
                    throw;
                }
            }
            return patient;
        }


        // This method simply deletes all records in the Patients table to reset our demo.
        static int ResetPatientsTable()
        {
            int returnValue = 0;

            SqlCommand sqlCmd = new SqlCommand("DELETE FROM Patients");
            using (sqlCmd.Connection = new SqlConnection(connectionString))
            {
                try
                {
                    sqlCmd.Connection.Open();
                    sqlCmd.ExecuteNonQuery();

                }
                catch (Exception ex)
                {
                    returnValue = 1;
                }
            }
            return returnValue;
        }
    }

    class Patient
    {
        public string SSN { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public DateTime BirthDate { get; set; }
    }
    }



## <a name="verify-that-the-data-is-encrypted"></a>Stellen Sie sicher, dass die Daten

Sie können schnell überprüfen, dass die eigentlichen Daten auf dem Server durch Abfragen der Patientendaten mit SSMS (unter Verwendung der aktuellen Verbindungs, **Spalte Verschlüsselungseinstellung** nicht noch aktiviert) verschlüsselt werden.

Führen Sie die folgende Abfrage in der Datenbank des Technologieüberblicks.

    SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

Sie können sehen, dass die verschlüsselten Spalten keine nur-Text-Daten enthält.

   ![Neue](./media/sql-database-always-encrypted-azure-key-vault/ssms-encrypted.png)


Um SSMS unverschlüsselte Daten zugreifen, können Sie die *Spalte Verschlüsselungseinstellung = aktiviert* Parameter für die Verbindung.

1. SSMS mit der rechten Maustaste des Servers im **Objekt-Explorer** und wählen Sie **Trennen**.
2. Klicken Sie auf **Verbinden** > **-Datenbank-Engine** , öffnen Sie das Fenster **Verbindung mit Server herstellen** und auf **Optionen**.
3. Klicken Sie auf **Zusätzliche Verbindungsparameter** und Typ **Spalte Verschlüsselungseinstellung = aktiviert**.

    ![Neue](./media/sql-database-always-encrypted-azure-key-vault/ssms-connection-parameter.png)

4. Führen Sie die folgende Abfrage in der Datenbank des Technologieüberblicks.

        SELECT FirstName, LastName, SSN, BirthDate FROM Patients;

     Unverschlüsselte Daten in verschlüsselten Spalten sehen.


    ![Neue](./media/sql-database-always-encrypted-azure-key-vault/ssms-plaintext.png)


## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie eine Datenbank erstellen, die verschlüsselt verwendet, sollten Sie Folgendes:

- [Drehen und Ihre Schlüssel](https://msdn.microsoft.com/library/mt607048.aspx).
- [Migrieren von Daten, die bereits mit verschlüsselt verschlüsselt ist](https://msdn.microsoft.com/library/mt621539.aspx).


## <a name="related-information"></a>Informationen

- [Verschlüsselt (Entwicklung)](https://msdn.microsoft.com/library/mt147923.aspx)
- [Transparente Verschlüsselung](https://msdn.microsoft.com/library/bb934049.aspx)
- [SQL Server-Verschlüsselung](https://msdn.microsoft.com/library/bb510663.aspx)
- [Immer verschlüsselt Assistenten](https://msdn.microsoft.com/library/mt459280.aspx)
- [Immer verschlüsselt blog](http://blogs.msdn.com/b/sqlsecurity/archive/tags/always-encrypted/)
