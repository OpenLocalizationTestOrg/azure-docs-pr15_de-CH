<properties
    pageTitle="Azure AD Java Einstieg | Microsoft Azure"
    description="Wie eine Java-Befehlszeile-Anwendung erstellen, die Benutzer meldet sich bei eine API zugreifen."
    services="active-directory"
    documentationCenter="java"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
  ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>


# <a name="using-java-command-line-app-to-access-an-api-with-azure-ad"></a>Mithilfe von Java Befehlszeile App auf einer API mit Azure

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD macht es einfach und unkompliziert Ihre Web app Identitätsmanagement Auslagern mit einzelnen an- und Abmeldung mit nur wenigen Zeilen Code.  In Java webapps erreichen Sie dies mit Microsofts Implementierung communitygestützte ADAL4J.

  Hier verwenden wir ADAL4J an:
- Die app Azure AD als Identitätsanbieter melden Sie Benutzer an.
- Einige Informationen über den Benutzer angezeigt.
- Abmelden des Benutzers von der Anwendung.

Dazu benötigen Sie:

1. Registrieren einer Anwendung in Azure AD
2. Richten Sie Ihre app der ADAL4J Bibliothek.
3. Verwenden der ADAL4J-Bibliothek Azure AD anmelden und Abmelde Anfragen erteilen.
4. Daten über die Benutzer ausgegeben.

Um zu beginnen, [das Skelett app](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) [herunterladen](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\/archive/complete.zip)oder vollständiges Beispiel.  Sie benötigen auch einen Azure AD-Mandanten, Ihre Anwendung registrieren.  Haben Sie eine bereits [erfahren Sie, wie man](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>1. Registrieren einer Anwendung in Azure AD
Aktivieren Sie Ihre app zum Authentifizieren von Benutzern müssen Sie zuerst eine neue Anwendung in Ihrem Mandanten registrieren.

- Melden Sie sich bei Azure-Verwaltungsportal.
- Klicken Sie im linken Navigationsbereich auf **Active Directory**.
- Wählen Sie Mieter wo Sie registrieren möchten.
- Klicken Sie auf die Registerkarte **Programme** , und klicken Sie auf Hinzufügen in der unteren Schublade.
- Befolgen Sie, und erstellen Sie eine neue **Anwendung oder WebAPI**.
    - Der **Name** der Anwendung wird die Anwendung an Endbenutzer beschrieben.
    - **Zeichen-URL** ist der Basis-URL der Anwendung.  Das Skelett-Standardwert ist `http://localhost:8080/adal4jsample/`.
    - **URI der App-ID** ist eine eindeutige Kennung für die Anwendung.  Die Konvention ist mit `https://<tenant-domain>/<app-name>`, z.B.`http://localhost:8080/adal4jsample/`
- Nach Abschluss Anmeldung zuweisen AAD app eine eindeutige Client-ID.  Sie benötigen diesen Wert in den nächsten Abschnitten so kopieren Sie sie von der Registerkarte konfigurieren.

Einmal im Portal für Ihre Anwendung erstellen Sie einen **Geheimen Schlüssel** für die Anwendung und kopieren Sie.  Sie benötigen ihn später.


## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisites-using-maven"></a>2. richten Sie Ihre app ADAL4J Bibliothek und Komponenten mit Maven
Konfigurieren Sie hier ADAL4J um das Authentifizierungsprotokoll OpenID verbinden verwenden.  Anmelden und Abmelden Anforderungen, die Sitzung des Benutzers verwalten und Informationen über den Benutzer unter anderem wird ADAL4J verwendet werden.

-   Im Stammverzeichnis des Projekts öffnen/erstellen `pom.xml` und die `// TODO: provide dependencies for Maven` und durch folgenden Wortlaut ersetzt:

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-the-java-publicclient-file"></a>3. erstellen Sie Java PublicClient-Datei

Wie bereits erwähnt, verwenden die Graph-API wir Daten über den angemeldeten Benutzer. Erstellen wir dies einfach eine Datei ein **Verzeichnisobjekt** und eine einzelne Datei für den **Benutzer** damit OO Muster von Java verwendet werden kann.

1. Erstellen Sie eine Datei namens `DirectoryObject.java` die wir verwenden grundlegende Daten über alle DirectoryObject gespeichert (kann man dies später für andere Diagramm-Abfragen verwenden kann). Sie können Ausschneiden und diese unten einfügen:

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


##<a name="compile-and-run-the-sample"></a>Kompilieren und Ausführen des Beispiels

Zurück zum Stammverzeichnis ändern, und führen Sie den folgenden Befehl zum Beispiel einfach mit `maven`. Hiermit wird die `pom.xml` Datei Abhängigkeiten schrieb.

`$ mvn package`

Sie haben jetzt eine `adal4jsample.war` Datei der `/targets` Verzeichnis. Sie in den Tomcat-Container bereitstellen, und besuchen Sie die URL 

`http://localhost:8080/adal4jsample/`


> [AZURE.NOTE] 
Es ist sehr einfach WAR mit der neuesten Tomcat-Server bereitstellen. Navigieren Sie einfach zu `http://localhost:8080/manager/` und gehen Sie zum Hochladen der "adal4jsample.war" Datei. Es wird Autodeploy Sie mit den richtigen Endpunkt.

##<a name="next-steps"></a>Nächste Schritte

Herzlichen Glückwunsch! Sie haben nun eine funktionierende Java-Anwendung, die die Fähigkeit zum Authentifizieren von Benutzern, sicher Aufrufen von APIs mit OAuth 2.0 und grundlegende Informationen über den Benutzer.  Wenn nicht bereits geschehen ist jetzt die Zeit Ihren Mandanten mit Benutzern aufgefüllt.

Zu Referenzzwecken können vollständiges Beispiel (ohne Ihre Werte) [als eine .zip bereitgestellt wird](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip)oder es von GitHub Klonen:

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

