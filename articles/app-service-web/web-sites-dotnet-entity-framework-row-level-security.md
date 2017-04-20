<properties
    pageTitle="Lernprogramm: Web app mit einer mehrinstanzenfähigen Entity Framework mit Sicherheit auf Zeilenebene"
    description="Informationen Sie zum Entwickeln einer ASP.NET MVC 5 Web app mit Multi-Tenant SQL-Datenbank backent, Entity Framework mit Sicherheit auf Zeilenebene."
  metaKeywords="azure asp.net mvc entity framework multi tenant row level security rls sql database"
    services="app-service\web"
    documentationCenter=".net"
    manager="jeffreyg"
  authors="tmullaney"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="04/25/2016"
    ms.author="thmullan"/>

# <a name="tutorial-web-app-with-a-multi-tenant-database-using-entity-framework-and-row-level-security"></a>Lernprogramm: Web app mit einer mehrinstanzenfähigen Entity Framework mit Sicherheit auf Zeilenebene

Dieses Lernprogramm zeigt, wie einer mehrinstanzenfähigen Web App mit einem "[freigegebene Datenbank, freigegebenen Schemas](https://msdn.microsoft.com/library/aa479086.aspx)" Mandanten mit Entity Framework und [Zeilenbasierter Sicherheit](https://msdn.microsoft.com/library/dn765131.aspx). In diesem Modell eine einzelne Datenbank enthält Daten für viele Mandanten und jede Zeile in jeder Tabelle bezieht sich "Mieter ID" Auf Sicherheit (RLS), ein neues Feature von Azure SQL-Datenbank dient dem Mieter Zugriff auf die Daten. Dies erfordert nur eine einzelne, kleine Änderung zur Anwendung. Durch die Zentralisierung der Datenzugriffslogik Mandanten in der Datenbank selbst RLS Anwendungscode vereinfacht und reduziert das Risiko von versehentlichen Datenverlust zwischen.

Beginnen wir mit der einfachen Contact Manager Anwendung [ASP.NET MVP app-Authentifizierung mit SQL DB erstellen und in Azure App Service bereitstellen](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md). Rechts erlaubt die Anwendung alle Benutzer alle Kontakte anzeigen (Mandanten):

![Contact Manager Anwendung vor der Aktivierung der RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-Before.png)

Mit ein paar kleinen Änderungen werden wir Unterstützung für mehrere Mandanten hinzufügen, damit Benutzer können nur die Kontakte angezeigt, die Ihnen gehören.

## <a name="step-1-add-an-interceptor-class-in-the-application-to-set-the-sessioncontext"></a>Schritt 1: Hinzufügen einer Interceptor-Klasse in der Anwendung SESSION_CONTEXT festlegen

Es ist eine Anwendung ändern zu müssen. Da allen Benutzern der Datenbank die gleiche Verbindungszeichenfolge (d. h. dieselbe SQL-Anmeldung) herstellen, gibt es keine Möglichkeit für eine RLS wissen, welcher Benutzer für filtern soll. Dieser Ansatz ist sehr häufig in ASP.NET-Webanwendungen ermöglicht effiziente Verbindungs-pooling, denn es heißt es können Benutzer der aktuellen Anwendung innerhalb der Datenbank identifiziert. Die Lösung besteht darin, daß das Schlüssel-Wert-Paar für die aktuelle Benutzer-ID im [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806) sofort nach dem Öffnen einer Verbindungs vor Fragen festlegen. SESSION_CONTEXT ist ein Schlüssel-Wert-Speicher Sitzungsebene und Grundsätzen RLS verwenden gespeicherten Benutzer-ID zur Identifizierung des aktuellen Benutzers.

Wir fügen einen [Interceptor](https://msdn.microsoft.com/data/dn469464.aspx) (insbesondere [DbConnectionInterceptor](https://msdn.microsoft.com/library/system.data.entity.infrastructure.interception.idbconnectioninterceptor)), ein neues Feature in Entity Framework (EF) 6 festzulegende automatisch die aktuelle Benutzer-ID in SESSION_CONTEXT durch eine T-SQL-Anweisung ausführen, wenn EF eine Verbindung öffnet.

1.  Öffnen Sie das ContactManager-Projekt in Visual Studio.
2.  Mit der rechten Maustaste auf den Ordner Models im Projektmappen-Explorer und wählen Sie hinzufügen > Klasse.
3.  Nennen Sie die neue Klasse "SessionContextInterceptor.cs", und klicken Sie auf Hinzufügen.
4.  Ersetzen Sie den Inhalt des SessionContextInterceptor.cs durch den folgenden Code.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure.Interception;
using Microsoft.AspNet.Identity;

namespace ContactManager.Models
{
    public class SessionContextInterceptor : IDbConnectionInterceptor
    {
        public void Opened(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
            // Set SESSION_CONTEXT to current UserId whenever EF opens a connection
            try
            {
                var userId = System.Web.HttpContext.Current.User.Identity.GetUserId();
                if (userId != null)
                {
                    DbCommand cmd = connection.CreateCommand();
                    cmd.CommandText = "EXEC sp_set_session_context @key=N'UserId', @value=@UserId";
                    DbParameter param = cmd.CreateParameter();
                    param.ParameterName = "@UserId";
                    param.Value = userId;
                    cmd.Parameters.Add(param);
                    cmd.ExecuteNonQuery();
                }
            }
            catch (System.NullReferenceException)
            {
                // If no user is logged in, leave SESSION_CONTEXT null (all rows will be filtered)
            }
        }
        
        public void Opening(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void BeganTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void BeginningTransaction(DbConnection connection, BeginTransactionInterceptionContext interceptionContext)
        {
        }

        public void Closed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Closing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void ConnectionStringGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSet(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionStringSetting(DbConnection connection, DbConnectionPropertyInterceptionContext<string> interceptionContext)
        {
        }

        public void ConnectionTimeoutGetting(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void ConnectionTimeoutGot(DbConnection connection, DbConnectionInterceptionContext<int> interceptionContext)
        {
        }

        public void DataSourceGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DataSourceGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void DatabaseGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void Disposed(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void Disposing(DbConnection connection, DbConnectionInterceptionContext interceptionContext)
        {
        }

        public void EnlistedTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void EnlistingTransaction(DbConnection connection, EnlistTransactionInterceptionContext interceptionContext)
        {
        }

        public void ServerVersionGetting(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void ServerVersionGot(DbConnection connection, DbConnectionInterceptionContext<string> interceptionContext)
        {
        }

        public void StateGetting(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }

        public void StateGot(DbConnection connection, DbConnectionInterceptionContext<System.Data.ConnectionState> interceptionContext)
        {
        }
    }

    public class SessionContextConfiguration : DbConfiguration
    {
        public SessionContextConfiguration()
        {
            AddInterceptor(new SessionContextInterceptor());
        }
    }
}
```

Das ist die einzige anwendungsänderung erforderlich. Nun erstellen und veröffentlichen Sie die Anwendung.

## <a name="step-2-add-a-userid-column-to-the-database-schema"></a>Schritt 2: Hinzufügen einer Benutzer-ID-Spalteninhalts mit dem Datenbankschema

Als Nächstes müssen wir eine Benutzer-ID-Spalte zur Tabelle Contacts (Mandant) Benutzer jede Zeile zugeordnet hinzufügen. Wir werden das Schema direkt in der Datenbank ändern, wir haben dieses Feld in unserem EF-Datenmodell enthalten.

Verbinden mit der Datenbank direkt mit SQL Server Management Studio oder Visual Studio und führen Sie die folgenden T-SQL:

```
ALTER TABLE Contacts ADD UserId nvarchar(128)
    DEFAULT CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
```

Dadurch wird die Contacts-Tabelle eine Benutzer-ID-Spalte hinzugefügt. Wir verwenden Datentyp nvarchar(128) mit Benutzer-IDs in der Tabelle AspNetUsers gespeichert und eine DEFAULT-Einschränkung, die automatisch die Benutzer-ID für neu eingefügte Zeilen in SESSION_CONTEXT gespeicherten Benutzer-ID zu erstellen.

Die Tabelle sieht nun folgendermaßen aus:

![SSMS Contacts-Tabelle](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-Contacts.png)

Wenn neue Kontakte erstellt werden, werden sie automatisch die richtige Benutzer-ID zugewiesen. Zu Demozwecken jedoch Wir weisen wenige diese Kontakte mit einem vorhandenen Benutzer.

Wenn Sie einige Benutzer in der Anwendung bereits erstellt haben (z. B. mit lokalen, Google und Facebook Firmen), Sie sehen in der AspNetUsers-Tabelle. Im Screenshot unten gibt es nur ein Benutzer bisher.

![SSMS AspNetUsers Tabelle](./media/web-sites-dotnet-entity-framework-row-level-security/SSMS-AspNetUsers.png)

Kopieren Sie die Id für user1@contoso.com, und T-SQL-Anweisung unten einfügen. Diese Anweisung um drei Kontakte mit dieser Benutzer-ID zuzuordnen.

```
UPDATE Contacts SET UserId = '19bc9b0d-28dd-4510-bd5e-d6b6d445f511'
WHERE ContactId IN (1, 2, 5)
```

## <a name="step-3-create-a-row-level-security-policy-in-the-database"></a>Schritt 3: Erstellen einer Sicherheitsrichtlinie auf der Datenbank

Der letzte Schritt ist eine Sicherheitsrichtlinie erstellen, die Benutzer-ID im SESSION_CONTEXT automatisch Abfragen zurückgegebenen Ergebnisse filtern.

Zwar weiterhin mit der Datenbank verbunden, führen Sie folgende T-SQL:

```
CREATE SCHEMA Security
go

CREATE FUNCTION Security.userAccessPredicate(@UserId nvarchar(128))
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS accessResult
    WHERE @UserId = CAST(SESSION_CONTEXT(N'UserId') AS nvarchar(128))
go

CREATE SECURITY POLICY Security.userSecurityPolicy
    ADD FILTER PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts,
    ADD BLOCK PREDICATE Security.userAccessPredicate(UserId) ON dbo.Contacts
go

```

Dieser Code führt drei Dinge. Zunächst wird ein neues Schema als bewährte Methode für die Zentralisierung und Einschränken des Zugriffs auf die RLS-Objekte erstellt. Anschließend wird eine Prädikatfunktion, die "1" zurückgegeben wird, wenn die Benutzer-ID einer Zeile UserId in SESSION_CONTEXT entspricht erstellt. Es erstellt eine Sicherheitsrichtlinie, die diese Funktion als Filter und Block Prädikat auf die Contacts-Tabelle hinzugefügt. Filterprädikat werden Abfragen nur Zeilen zurückgegeben, die dem aktuellen Benutzer gehören und das Prädikat Block stellt die die Anwendung jemals versehentlich eine Zeile für den falschen Benutzer einfügen.

Führen Sie die Anwendung, und melden Sie sich als user1@contoso.com. Jetzt werden wir diese Benutzer-ID bereits zugeordnet Kontakte Benutzer:

![Contact Manager Anwendung vor der Aktivierung der RLS](./media/web-sites-dotnet-entity-framework-row-level-security/ContactManagerApp-After.png)

Um diese weiter zu überprüfen, versuchen Sie einen neuen Benutzer registrieren. Sie sehen keine Kontakte, da keine ihnen zugewiesen wurden. Wenn sie einen neuen Kontakt erstellen, wird Ihnen zugewiesen, und nur diese werden angezeigt.

## <a name="next-steps"></a>Nächste Schritte

Das wars! Einfache Contact Manager Web app wurde in einer mehrinstanzenfähigen eine konvertiert jeder Benutzer seine eigene Kontaktliste hat. Mithilfe von zeilenbasierter Sicherheit haben wir die Komplexität der Mieter Datenzugriffslogik in Anwendungscode erzwingen vermieden. Diese Transparenz kann die Anwendung die geschäftlichen Problems konzentrieren und auch reduziert das Risiko von undicht versehentlich Daten wie die Anwendung die codebase wächst.

Dieses Lernprogramm ist nur oberflächlich was mit RLS möglich ist. Beispielsweise können komplexere oder detaillierte Datenzugriffslogik, und es ist mehr als nur die aktuelle Benutzer-ID in der SESSION_CONTEXT gespeichert. Es kann auch [RLS mit den Clientbibliotheken elastische Datenbank Tools integrieren](../sql-database/sql-database-elastic-tools-multi-tenant-row-level-security.md) Multi-Tenant-Splitter in einer Scale-Out-Datenebene unterstützen.

Diese Möglichkeiten arbeiten wir auch RLS noch besser zu machen. Haben Sie Fragen, Ideen oder was, die Sie sehen möchten, lassen Sie bitte in den Kommentaren. Wir schätzen Ihr Feedback!
