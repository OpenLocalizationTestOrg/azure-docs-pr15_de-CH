<properties 
    pageTitle="Für WebApp Mutual TLS-Authentifizierung konfigurieren" 
    description="Informationen Sie zum Konfigurieren Ihrer Anwendung um Clientzertifikatsauthentifizierung TLS verwenden." 
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/08/2016" 
    ms.author="naziml"/>    

# <a name="how-to-configure-tls-mutual-authentication-for-web-app"></a>Für WebApp Mutual TLS-Authentifizierung konfigurieren

## <a name="overview"></a>Übersicht ##
Sie können Ihrer Anwendung Azure ermöglicht verschiedene Arten der Authentifizierung für sie Zugriff einschränken. Eine Möglichkeit hierzu ist die Authentifizierung mit einem Clientzertifikat wird die Anfrage über TLS/SSL. Dieser Mechanismus wird als mutual TLS-Authentifizierung oder Clientzertifikat, Authentifizierung und dieser Artikel zum Einrichten Ihrer Anwendung Client Zertifikatsauthentifizierung ausführlich.

> **Hinweis:** Wenn Sie Ihre Website über HTTP und HTTPS nicht zugreifen, erhalten Sie ein Clientzertifikat nicht. So erfordert die Anwendung von Clientzertifikaten sollten Sie keine Anfragen für die Anwendung über HTTP zulassen.


[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="configure-web-app-for-client-certificate-authentication"></a>WebApp Clientzertifikatauthentifizierung konfigurieren ##
Einrichten Ihrer Anwendung Clientzertifikaten müssen Sie die Einstellung ClientCertEnabled Website Ihrer Anwendung hinzufügen und auf True festgelegt. Diese Einstellung steht derzeit nicht über die Verwaltungsfunktionen im Portal und die REST-API müssen hierfür verwendet werden.

Das [ARMClient-Tool](https://github.com/projectkudu/ARMClient) können Sie um zu den REST API-Aufruf zu erleichtern. Nachdem das Tool einloggen müssen Sie den folgenden Befehl:

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose
    
Ersetzen alles in {} Informationen zu Ihrer Anwendung und Erstellen einer Datei aufgerufen enableclientcert.json mit folgenden JSON Content:

> {"Location": "Meine Web App Location"   
>   "Eigenschaften": {  
>     "ClientCertEnabled": true}}  

Ändern Sie den Wert des "Location", wo Ihrer Anwendung befindet z. B. US North Central oder West USA usw..

> **Hinweis:** Wenn Sie Powershell ARMClient ausführen, müssen zu den @ Symbol für JSON-Datei mit einem Graviszeichen ".

## <a name="accessing-the-client-certificate-from-your-web-app"></a>Zugriff auf das Client-Zertifikat von Ihrer Anwendung ##
Wenn Sie ASP.NET verwenden, konfigurieren Sie Ihre app-Clientzertifikatauthentifizierung verwendet werden das Zertifikat über die **HttpRequest.ClientCertificate** -Eigenschaft. Für andere Stacks Anwendung stehen das Client-Zertifikat in Ihrer Anwendung durch einen base64-codierten Wert im Anforderungsheader "X-ARR-ClientCert". Die Anwendung ein Zertifikat aus diesen Wert erstellen und dann für Authentifizierung und Autorisierung in der Anwendung verwenden.

## <a name="special-considerations-for-certificate-validation"></a>Besonderheiten bei der Überprüfung des Zertifikats ##
Das Client-Zertifikat an die Anwendung gesendet wird gehen nicht durch Validierung der Azure Web Apps-Plattform. Überprüfung dieses Zertifikats obliegt Web app. Hier ist ASP.NET Beispielcode, die Zertifikateigenschaften Authentifizierung überprüft.

    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;
                
                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;
                
                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                // If you also want to test if the certificate chains to a Trusted Root Authority you can uncomment the code below
                //
                //X509Chain certChain = new X509Chain();
                //certChain.Build(certificate);
                //bool isValidCertChain = true;
                //foreach (X509ChainElement chElement in certChain.ChainElements)
                //{
                //    if (!chElement.Certificate.Verify())
                //    {
                //        isValidCertChain = false;
                //        break;
                //    }
                //}
                //if (!isValidCertChain) return false;

                return true;
            }
        }
    }
