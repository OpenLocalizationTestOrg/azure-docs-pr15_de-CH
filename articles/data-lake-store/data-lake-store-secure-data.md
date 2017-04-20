<properties 
   pageTitle="Sichern von Daten in Azure Datenspeicher See | Microsoft Azure" 
   description="Erfahren Sie, wie Daten in Azure See Datenspeicher Gruppen und Zugriffssteuerungslisten" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/29/2016"
   ms.author="nitinme"/>

# <a name="securing-data-stored-in-azure-data-lake-store"></a>Sichern von Daten in Azure See Datenspeicher

Sichern von Daten in Azure See Datenspeicher ist ein Konzept.

1. Zunächst erstellen Sicherheitsgruppen in Azure Active Directory (AAD). Diese Sicherheitsgruppen werden zum Implementieren von rollenbasierte Zugriffskontrolle (RBAC) in Azure-Portal. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffskontrolle in Microsoft Azure](../active-directory/role-based-access-control-configure.md).

2. Sicherheitsgruppen AAD See Datenspeicher Azure-Konto zuordnen Diese Steuerelemente greifen auf See Datenspeicher Konto aus dem Portal und Management von Portal oder APIs.

3. Sicherheitsgruppen AAD Access Zugriffssteuerungslisten (ACLs) auf See Datenspeicher zuweisen.

4. Darüber hinaus können Sie auch einen IP-Adressbereich für Clients festlegen, die die Daten im Datenspeicher See zugreifen können.

Dieser Artikel enthält eine Anleitung zum Azure-Portal verwenden, um Aufgaben auszuführen. Ausführliche Informationen über die Implementierung von Datenspeicher See Sicherheit auf Konto- und Daten finden Sie unter [Sicherheit in Azure See Datenspeicher](data-lake-store-security-overview.md). Tiefer gehende Informationen über von ACLs in Azure See Datenspeicher Implementierung finden Sie unter [Übersicht der Zugriffskontrolle im Datenspeicher See](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie dieses Lernprogramm beginnen, müssen Sie Folgendes:

- **Ein Azure-Abonnement**. Finden Sie [kostenlose Testversion von Azure zu erhalten](https://azure.microsoft.com/pricing/free-trial/).
- **Ein Azure-See Datenspeicher Konto**. Informationen zum Erstellen finden Sie unter [Erste Schritte mit Azure See Datenspeicher](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Erstellen von Sicherheitsgruppen in Active Directory Azure

Anweisungen AAD Sicherheitsgruppen erstellen und der Gruppe Benutzer hinzufügen finden Sie unter [Verwalten von Sicherheitsgruppen in Active Directory Azure](../active-directory/active-directory-accessmanagement-manage-groups.md).

## <a name="assign-users-or-security-groups-to-azure-data-lake-store-accounts"></a>Zuweisen von Benutzern oder Sicherheitsgruppen auf See Datenspeicher Azure

Wenn Sie Benutzern oder Sicherheitsgruppen Azure See Datenspeicher zuweisen, steuern Zugriff auf die Verwaltungsvorgänge für das Konto mit der Azure-Portal Azure-Ressourcen-Manager-APIs. 

1. Azure See Datenspeicher Konto. Im linken Fensterbereich klicken Sie auf **Durchsuchen**, klicken Sie auf **See Datenspeicher**und klicken Sie den Kontonamen ein Benutzers oder Sicherheit Gruppe zugewiesen werden soll aus dem Blade See Datenspeicher.

2. **Klicken Sie auf Ihr Konto Blatt See Datenspeicher.** Klicken Sie auf **Benutzer**aus dem Blade **Settings** .

    ![Azure See Datenspeicher Konto der Sicherheitsgruppe zuweisen] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Azure See Datenspeicher Konto der Sicherheitsgruppe zuweisen")

3. Blatt **Benutzer** standardmäßig **Abonnement-Admins** -Gruppe als Besitzer aufgeführt. 

    ![Hinzufügen von Benutzern und Rollen] (./media/data-lake-store-secure-data/adl.add.group.roles.png "Hinzufügen von Benutzern und Rollen")
 
    Gibt es zwei Wege zum Hinzufügen einer Gruppe und entsprechende Rollen zuweisen.

    * Das Konto einer Benutzergruppe hinzu, und weisen Sie eine Rolle oder
    * Hinzufügen einer Rolle und Rolle Benutzer/Gruppen zuweisen.

    In diesem Abschnitt betrachten wir den ersten Ansatz eine Gruppe hinzufügen und Zuweisen von Rollen. Führen Sie ähnliche Schritte zunächst eine Rolle und dieser Rolle dann Gruppen zuweisen.
    
4. Blatt **Benutzer** klicken Sie auf **Hinzufügen** , um das Blade **Hinzufügen Zugriff** öffnen. Blatt **Hinzufügen Zugriff** auf **Wählen Sie eine Rolle**, und wählen Sie eine Rolle für die Benutzergruppe.

     ![Hinzufügen einer Rolle für den Benutzer] (./media/data-lake-store-secure-data/adl.add.user.1.png "Hinzufügen einer Rolle für den Benutzer")

    Der **Besitzer** und die Rolle **eines Beitragenden** bieten Zugriff auf eine Vielzahl von Verwaltungsfunktionen Akonto See Daten. Für Benutzer, die mit Daten im See Daten interagieren, können Sie diese **Rolle **hinzufügen. Der Gültigkeitsbereich dieser Rollen beschränkt Verwaltungsvorgänge Bezug auf See Datenspeicher Azure-Konto.

    Für Daten definieren Operationen einzelne Dateisystemberechtigungen was die Benutzer tun können. Daher ein Benutzer einer Rolle kann nur administrative Einstellungen des Kontos anzeigen aber kann möglicherweise lesen und schreiben Daten Dateisystemberechtigungen zugewiesen. Datenspeicher See Dateisystemberechtigungen werden zur [Sicherheitsgruppe als ACLs im Dateisystem Azure See Datenspeicher zuweisen](#filepermissions).



5. Blatt **Hinzufügen Zugriff** **Hinzufügen** öffnen Blade **Hinzufügen** klicken. Suchen Sie dieses Blatt in Azure Active Directory erstellten Sicherheitsgruppe. Haben viele Suche verwenden Sie das Textfeld oben auf den Gruppennamen zu filtern. Klicken Sie auf **auswählen**.

    ![Eine Sicherheitsgruppe hinzufügen] (./media/data-lake-store-secure-data/adl.add.user.2.png "Eine Sicherheitsgruppe hinzufügen")

    Möchten Sie Gruppe/Benutzer hinzufügen, die nicht aufgelistet ist, können Sie sie einladen, indem Sie das Symbol **Laden** und die e-Mail-Adresse für die Benutzergruppe.

6. Klicken Sie auf **OK**. Wie unten dargestellt hinzugefügt Sicherheitsgruppe sollte angezeigt werden.

    ![Sicherheitsgruppe hinzugefügt] (./media/data-lake-store-secure-data/adl.add.user.3.png "Sicherheitsgruppe hinzugefügt")

7. Die Benutzer/Gruppe hat jetzt Zugriff auf See Datenspeicher Azure-Konto. Wenn Sie bestimmten Benutzern Zugriff gewähren möchten, können Sie sie zur Sicherheitsgruppe hinzufügen. Wenn Sie den Zugriff für einen Benutzer widerrufen möchten, können Sie sie auch aus der Sicherheitsgruppe entfernen. Sie können ein Konto auch mehrere Sicherheitsgruppen zuweisen. 

## <a name="filepermissions"></a>Benutzer oder Gruppe als ACLs im Dateisystem Azure See Datenspeicher zuweisen

Azure Data Lake Dateisystem Benutzer/Gruppen zuweisen, legen Sie die Zugriffskontrolle auf Daten in Azure See Datenspeicher.

1. Klicken Sie in Ihrem Konto Blade See Datenspeicher auf **Daten-Explorer**.

    ![Verzeichnisse in dem Datenspeicher-Konto erstellen] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Verzeichnisse Daten See-Konto erstellen")

2. Blatt **Daten-Explorer** auf die Datei oder den Ordner die ACL konfiguriert werden soll und **Klicken Sie auf**. Um eine ACL zuzuweisen, müssen Sie **Zugriff** aus dem Blade **Vorschau** klicken.

    ![Festlegen von ACLs auf See Daten] (./media/data-lake-store-secure-data/adl.acl.1.png "Festlegen von ACLs auf See Daten")

3. **Access** -Blade listet standard Zugriff und benutzerdefinierten Zugriff Stamm bereits zugewiesen. Klicken Sie auf **Hinzufügen** , um Stufe ACLs hinzugefügt.

    ![Liste Standard- und benutzerdefinierten Zugriff] (./media/data-lake-store-secure-data/adl.acl.2.png "Liste Standard- und benutzerdefinierten Zugriff")

    * **Standard** ist Zugriff auf UNIX-Format, geben Sie lesen, schreiben, ausführen (Rwx) für drei verschiedene Klassen: Besitzer und Gruppe.
    * **Benutzerdefinierte Zugriff** entspricht die POSIX-ACLs, die Berechtigungen für bestimmte Benutzer oder Gruppen, und nicht nur der Datei Besitzer oder Gruppe ermöglicht. 
    
    Weitere Informationen finden Sie unter [Bietet ACLs](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). Informationen für die Implementierung von ACLs in Lake Datenspeicher finden Sie unter [Zugriffskontrolle im Datenspeicher See](data-lake-store-access-control.md).

4. Klicken Sie auf **Hinzufügen** , **Fügen Sie benutzerdefinierte Zugriff** Blade öffnen. Dieses Blatt klicken Sie auf **Benutzer oder Gruppe auswählen**und dann Blatt **Benutzer oder Gruppe** Suchen in Azure Active Directory erstellten Sicherheitsgruppe. Haben viele Suche verwenden Sie das Textfeld oben auf den Gruppennamen zu filtern. Klicken Sie auf die Gruppe, die Sie hinzufügen möchten und klicken Sie auf **auswählen**.

    ![Gruppe hinzufügen] (./media/data-lake-store-secure-data/adl.acl.3.png "Gruppe hinzufügen")

5. Klicken Sie auf **Select-Berechtigungen**und die Berechtigungen möchten Berechtigungen standardmäßig ACL, ACL oder beides zugreifen. Klicken Sie auf **OK**.

    ![Zuweisen von Berechtigungen zu einer Gruppe] (./media/data-lake-store-secure-data/adl.acl.4.png "Zuweisen von Berechtigungen zu einer Gruppe")

    Weitere Informationen zu Berechtigungen im Datenspeicher See und Standard-Access ACLs finden Sie in der [Zugriffskontrolle im Datenspeicher See](data-lake-store-access-control.md).


6. Blatt **Hinzufügen benutzerdefinierten Zugriff** klicken Sie auf **OK**. Die neu hinzugefügte Gruppe zugeordneten Berechtigungen wird jetzt im Blatt **Zugriff** aufgeführt.

    ![Zuweisen von Berechtigungen zu einer Gruppe] (./media/data-lake-store-secure-data/adl.acl.5.png "Zuweisen von Berechtigungen zu einer Gruppe")

    > [AZURE.IMPORTANT] In der aktuellen Version können Sie nur 9 Einträge unter **Benutzerdefiniert**. Wenn Sie mehr als 9 Benutzer hinzufügen möchten, sollten Sie Sicherheitsgruppen erstellen Sicherheitsgruppen Benutzer hinzuzufügen, fügen Sie diesen Sicherheitsgruppen für Datenspeicher See Konto Zugriff ermöglichen.

7. Bei Bedarf können Sie auch die Zugriffsberechtigungen ändern, nachdem die Gruppe hinzugefügt haben. Deaktivieren Sie oder aktivieren Sie das Kontrollkästchen für jeden Berechtigungstyp (Lesen, schreiben, ausführen) soll anhand entfernen oder die Sicherheitsgruppe die Berechtigung zuweisen. Klicken Sie auf Änderungen **Speichern** oder **verwerfen** die Änderungen rückgängig machen.

## <a name="set-ip-address-range-for-data-access"></a>-IP-Adressbereich für den Datenzugriff

Azure See Datenspeicher können Sie weitere Sperren auf Datenspeicher auf Netzwerkebene. Sie können Firewall aktivieren eine IP-Adresse oder einen IP-Adressbereich für vertrauenswürdige Clients definieren. Wenn aktiviert, können nur Clients mit IP-Adressen im Bereich im Speicher.

![Firewall Settings und IP-Zugriff] (./media/data-lake-store-secure-data/firewall-ip-access.png "Firewall Settings und IP-Adresse")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Sicherheitsgruppen Sie für ein Konto Azure See Datenspeicher

Wenn Sie Sicherheitsgruppen Azure See Datenspeicher Konten entfernen, ändern Sie nur Zugriff, Verwaltungsvorgänge für das Konto mit der Azure-Portal Azure-Ressourcen-Manager-APIs.

1. **Klicken Sie auf Ihr Konto Blatt See Datenspeicher.** Klicken Sie auf **Benutzer**aus dem Blade **Settings** .

    ![Azure Data Lake Konto der Sicherheitsgruppe zuweisen] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Azure Data Lake Konto der Sicherheitsgruppe zuweisen")

2. Blatt **Benutzer** klicken Sie auf die Sicherheitsgruppe, die Sie entfernen möchten.

    ![Sicherheitsgruppe entfernen] (./media/data-lake-store-secure-data/adl.add.user.3.png "Sicherheitsgruppe entfernen")

3. Blatt für die Sicherheitsgruppe klicken Sie auf **Entfernen**.

    ![Sicherheitsgruppe entfernt] (./media/data-lake-store-secure-data/adl.remove.group.png "Sicherheitsgruppe entfernt")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Azure See Datenspeicher Dateisystem entfernen Sie Sicherheitsgruppe ACLs

Beim Entfernen von Sicherheitsgruppen ACLs von Azure See Datenspeicher Dateisystem ändern Sie Zugriff auf die Daten im Datenspeicher See.

1. Klicken Sie in Ihrem Konto Blade See Datenspeicher auf **Daten-Explorer**.

    ![Verzeichnisse Daten See-Konto erstellen] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Verzeichnisse Daten See-Konto erstellen")

2. Blatt **Daten-Explorer** klicken Sie auf die Datei oder den Ordner in der ACL entfernt werden soll und klicken Sie in Ihrem Konto Blade **Access** -Symbol. Um ACL für eine Datei zu entfernen, müssen Sie **Zugriff** aus dem Blade **Vorschau** klicken.

    ![Festlegen von ACLs auf See Daten] (./media/data-lake-store-secure-data/adl.acl.1.png "Festlegen von ACLs auf See Daten")

3. Blatt **Zugriff** aus dem Abschnitt **Benutzerdefinierte Zugriff** klicken Sie auf die Sicherheitsgruppe, die Sie entfernen möchten. Blatt **Benutzerdefinierten Zugriff** auf **Entfernen** , und klicken Sie auf **OK**.

    ![Zuweisen von Berechtigungen zu einer Gruppe] (./media/data-lake-store-secure-data/adl.remove.acl.png "Zuweisen von Berechtigungen zu einer Gruppe")


## <a name="see-also"></a>Siehe auch

- [Übersicht über Azure See Datenspeicher](data-lake-store-overview.md)
- [Daten von Azure Storage Blobs See Datenspeicher](data-lake-store-copy-data-azure-storage-blob.md)
- [Verwenden Sie Azure Data Lake Analytics See Datenspeicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Verwenden von Azure HDInsight Lake Datenspeicher](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Erste Schritte mit See Datenspeicher PowerShell](data-lake-store-get-started-powershell.md)
- [Erste Schritte mit See Datenspeicher .NET SDK](data-lake-store-get-started-net-sdk.md)
- [Access Diagnoseprotokolle für Datenspeicher See](data-lake-store-diagnostic-logs.md)
