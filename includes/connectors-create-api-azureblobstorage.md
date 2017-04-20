### <a name="prerequisites"></a>Erforderliche Komponenten
- Ein Azure-Konto; Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
- Ein [Azure BLOB-Speicher Konto](../articles/storage/storage-create-storage-account.md) einschließlich Storage-Kontonamen und die Zugriffstaste. Diese Informationen sind in den Eigenschaften des Speicherkontos im Azure-Portal aufgeführt. Weitere Informationen zur [Azure-Speicher](../articles/storage/storage-introduction.md).

Verbinden Sie bevor Ihr Konto Azure BLOB-Speicher in einer Anwendung Logik verwenden Ihr Konto Azure BLOB-Speicher. Diese möglich problemlos in Ihre Anwendung Logik der Azure-Portal.  

Verbinden Sie Ihre Azure BLOB-Speicher-Konto mit den folgenden Schritten:  

1. Erstellen Sie eine Logik-app. Fügen Sie im Designer Logik Apps Trigger hinzu, und fügen Sie eine Aktion. Wählen Sie in der Dropdown-Liste **Anzeigen von Microsoft verwalteten APIs** , und geben Sie "Blob" in das Suchfeld. Wählen Sie eine der Aktionen aus:  

    ![Azure BLOB-Speicher Verbindungsvorgangs erstellen](./media/connectors-create-api-azureblobstorage/azureblobstorage-1.png)  

2. Wenn Sie zuvor alle Verbindungen Azure-Speicher erstellt haben, werden Sie aufgefordert, die Details der Verbindung:   

    ![Azure BLOB-Speicher Verbindungsvorgangs erstellen](./media/connectors-create-api-azureblobstorage/connection-details.png)  

3. Geben Sie die Speicher-Konto. Eigenschaften mit einem Sternchen sind erforderlich.

    | Eigenschaft | Details |
|---|---|
| Verbindungsname * | Geben Sie einen beliebigen Namen für die Verbindung. |
| Kontoname Azure-Speicher * | Geben Sie den speicherkontonamen. Speicherkontoname ist Speicher Eigenschaften in Azure-Portal angezeigt. |
| Azure-Speicher Konto Zugriffstaste * | Geben Sie ein Konto Speicherschlüssel. Zugriffstasten werden in Speichereigenschaften im Azure-Portal angezeigt. |

    Diese Anmeldeinformationen zum Autorisieren Ihrer Anwendung Logik zu, auf Ihre Daten zugreifen. 

4. Wählen Sie **Erstellen**.

5. Beachten Sie, dass die Verbindung hergestellt wurde. Jetzt gehen Sie die Schritte in Ihrer Anwendung Logik: 

    ![Azure BLOB-Speicher Verbindungsvorgangs erstellen](./media/connectors-create-api-azureblobstorage/azureblobstorage-3.png)  
