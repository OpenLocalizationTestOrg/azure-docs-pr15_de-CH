<properties
   pageTitle="Transparente Verschlüsselung (DSA) für SQL Server Stretch Datenbank in Azure aktivieren | Microsoft Azure"
   description="Aktivieren Sie transparente Verschlüsselung (DSA) für SQL Server Stretch Datenbank in Azure"
   services="sql-server-stretch-database"
   documentationCenter=""
   authors="douglaslMS"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-server-stretch-database"
   ms.workload="data-management"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/14/2016"
   ms.author="douglaslMS"/>

# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>Aktivieren Sie transparente Verschlüsselung (DSA) für Stretch Datenbank auf Azure
> [AZURE.SELECTOR]
- [Azure-portal](sql-server-stretch-database-encryption-tde.md)
- [TSQL](sql-server-stretch-database-tde-tsql.md)

Transparent Data Encryption (DSA) schützt gegen bösartige Aktivitäten durch Echtzeit-Verschlüsselung und Entschlüsselung der Datenbank zugeordneten Backups und Transaktionsprotokolldateien ruhende ohne Änderung der Anwendung ausführen.

TDE verschlüsselt die Speicherung der gesamten Datenbank einen symmetrischen Schlüssel Chiffrierschlüssel der Datenbank bezeichnet. Integrierte Serverzertifikat Datenbank-Verschlüsselungsschlüssel geschützt. Das integrierte Zertifikat ist eindeutig für jede Azure-Server. Microsoft wird diese Zertifikate automatisch mindestens alle 90 Tage. Eine allgemeine Beschreibung der TDE finden Sie [Transparent Data Encryption (DSA)].

##<a name="enabling-encryption"></a>Aktivieren der Verschlüsselung

Um eine Azure TDE aktivieren, das Speichern von Daten migriert Stretch-fähigen SQL Server-Datenbank gehen Sie folgendermaßen vor:

1. Öffnen Sie die Datenbank in [Azure-portal](https://portal.azure.com)
2. Blatt Datenbank klicken Sie auf **die Schaltfläche**
3. Wählen Sie die Option **transparente Verschlüsselung**![][1]
4. Wählen Sie **die Einstellung** , und wählen Sie dann **Speichern**
![][2]


##<a name="disabling-encryption"></a>Deaktivieren der Verschlüsselung

Deaktivieren TDE für eine Azure-Datenbank, die die Daten speichert Stretch-fähigen SQL Server-Datenbank migriert, gehen Sie folgendermaßen vor:

1. Öffnen Sie die Datenbank in [Azure-portal](https://portal.azure.com)
2. Blatt Datenbank klicken Sie auf **die Schaltfläche**
3. Wählen Sie die Option **transparente Verschlüsselung**
4. Wählen Sie die Einstellung **aus** , und wählen Sie dann **Speichern**




<!--Anchors-->
[Transparente Verschlüsselung (DSA)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
