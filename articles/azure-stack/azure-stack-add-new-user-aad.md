<properties
    pageTitle="Ein neues Azure Stapel mieterkonto in Active Directory Azure hinzufügen | Microsoft Azure"
    description="Nach der Bereitstellung von Microsoft Azure Stapel POC, müssen Sie mindestens ein Mieter Benutzerkonto Portal Mieter zu erkunden."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Ein neues Azure Stapel mieterkonto in Active Directory Azure hinzufügen

Nach dem [Bereitstellen von Azure Stapel POC](azure-stack-run-powershell-script.md)benötigen Sie ein Benutzerkonto Mieter Portal Mieter untersuchen und Testen Sie Ihre Angebote und Pläne. Sie können ein mieterkonto mithilfe [des Azure-Portals](#create-an-azure-stack-tenant-account-using-the-azure-portal) oder mit [PowerShell](#create-an-azure-stack-tenant-account-using-powershell)erstellen.

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Erstellen Sie ein Azure Stapel mieterkonto Azure-portal

Ein Azure-Abonnement Azure-Portal verwenden müssen.

1. Melden Sie sich bei [Azure](http://manage.windowsazure.com).

2.  Klicken Sie in der Navigationsleiste Microsoft Azure auf **Active Directory**.

3.  Klicken Sie in der Verzeichnisliste auf das Verzeichnis, das Sie für Azure verwenden möchten oder erstellen Sie eine neue.

4.  **Klicken Sie auf diese Verzeichnisseite.**

5.  Klicken Sie auf **Benutzer hinzufügen**.

6.  Wählen Sie im Assistenten **Benutzer hinzufügen** in der Liste **Benutzer** **Neuer Benutzer in Ihrer Organisation aus**.

7.  Geben Sie im Feld **Benutzername** einen Namen für den Benutzer.

8.  In der **@** auf den entsprechenden Eintrag.

9.  Klicken Sie auf den Pfeil.

10.  Geben Sie auf der Seite **Benutzerprofil** des Assistenten ein **Vorname**, **Nachname**und **Anzeigename**.

11. Wählen Sie in der Liste **Rolle** **Benutzer**.

12. Klicken Sie auf den Pfeil.

13. Das **temporäres Kennwort abrufen** klicken Sie auf **Erstellen**.

14. Kopieren Sie das **neue Kennwort ein**.

15. Melden Sie sich bei Microsoft Azure mit dem neuen Konto an. Ändern Sie das Kennwort ein.

16. Melden Sie sich bei `https://portal.azurestack.local` mit dem neuen Konto Portal Mieter anzeigen.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>Erstellen Sie ein Azure-Stapel mieterkonto mit PowerShell

Wenn Sie ein Azure-Abonnement haben, können Azure-Portal Sie ein Mieter Benutzerkonto hinzufügen. In diesem Fall können Sie den Azure Active Directory-Modul für Windows PowerShell verwenden.

> [AZURE.NOTE] Wenn Sie Microsoft Account (Live ID verwenden) Azure Stapel PoC bereitstellen, können AAD PowerShell Sie mieterkonto erstellen. 

1.  Installieren Sie den [Microsoft Online Services - Anmeldeassistenten für IT-Experten RTW](https://www.microsoft.com/en-us/download/details.aspx?id=41950).

2.  [Azure Active Directory Modul für Windows PowerShell (64-Bit-Version)](http://go.microsoft.com/fwlink/p/?linkid=236297) installieren Sie und öffnen Sie es.

3.  Führen Sie die folgenden Cmdlets:




    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack PoC
   
            $msolcred = get-credential
    
    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".
    
            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId
    
    ```

4.  Melden Sie sich bei Microsoft Azure mit dem neuen Konto an. Ändern Sie das Kennwort ein.

5.  Melden Sie sich bei `https://portal.azurestack.local` mit dem neuen Konto Portal Mieter anzeigen.



