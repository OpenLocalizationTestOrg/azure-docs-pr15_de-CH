<properties
    pageTitle="Ansicht SAML zurückgegebene Access Control Service (Java)"
    description="Informationen Sie zum Anzeigen von SAML Access Control Service in Java-Anwendung in Azure zurückgegeben."
    services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016" 
    ms.author="robmcm" />

# <a name="how-to-view-saml-returned-by-the-azure-access-control-service"></a>Der Azure-Dienst zurückgegebene SAML anzeigen

Dabei zeigen Sie zum Anzeigen der zugrunde liegenden Security Assertion Markup Language (SAML) Azure Access Control Service (ACS) an die Anwendung zurückgegeben. Das Handbuch baut Thema [wie Authentifizieren von Webbenutzern mit Azure Access Control Service verwenden Eclipse][] mit Code, der die SAML-Informationen angezeigt. Die fertiggestellte Anwendung sieht etwa wie folgt.

![SAML Ausgabe][saml_output]

Weitere Informationen zu ACS finden Sie im Abschnitt [Weiter](#next_steps) .

> [AZURE.NOTE]
> Azure Access Services-Steuerelement Filter ist eine Community Technology Preview. Pre-Release-Software ist es offiziell von Microsoft nicht unterstützt.

## <a name="prerequisites"></a>Erforderliche Komponenten

Führen Sie die Aufgaben in diesem Handbuch, das Beispiel wie [Authentifizieren von Webbenutzern mit Azure Access Control Service verwenden Eclipse][] und als Ausgangspunkt für diese praktische Einführung verwendet.

## <a name="add-the-jspwriter-library-to-your-build-path-and-deployment-assembly"></a>Die Build-Pfad und Bereitstellung Assembly JspWriter Bibliothek hinzufügen

Fügen Sie der Bibliothek mit der **javax.servlet.jsp.JspWriter** Klasse der Build Pfad und Bereitstellung der Assembly. Wenn Sie Tomcat verwenden, ist die Bibliothek **Jsp-api.jar**, die Apache **Lib** -Ordner befindet.

1. Eclipse Projekt-Explorer mit der rechten Maustaste **MyACSHelloWorld**, klicken Sie auf **Pfad erstellen** **Erstellen Pfad konfigurieren**klicken, klicken Sie auf der Registerkarte **Bibliotheken** und **Externe JARs hinzufügen**klicken.
2. Navigieren Sie im Dialogfeld **Auswahl JAR-** zu der erforderlichen JAR, wählen sie und klicken Sie auf **Öffnen**.
3. **Eigenschaften für MyACSHelloWorld** Dialogfeld geöffnet klicken Sie auf **Bereitstellung Assembly**.
4. Klicken Sie im Dialogfeld **Web Deployment Assembly** **Hinzufügen**.
5. Im Dialogfeld **Neue Assemblydirektive** auf **Java Pfadangaben erstellen** , und klicken Sie auf **Weiter**.
6. Wählen Sie die entsprechende Bibliothek, und klicken Sie auf **Fertig stellen**.
7. Klicken Sie auf **OK** , um das Dialogfeld **Eigenschaften für MyACSHelloWorld** zu schließen.

## <a name="modify-the-jsp-file-to-display-saml"></a>Ändern Sie die JSP-Datei SAML anzeigen

Ändern Sie die **index.jsp** um den folgenden Code verwenden.

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {
        
            try
            {
                String nodeName;
                int nChild, i;
                
                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");
                   
                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }
                   
                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                   {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    
    
                                 // If it is a text node, just print the text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out the child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        
                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate the names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish the sentence.
                                            out.print(".");
                                        }
                                            
                                     }
                                     out.println("<br>");
                                     
                                     // Process the child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>
    
        <%
        try 
        {
            String data  = (String) request.getAttribute("ACSSAML");
            
            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();
            
            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA); 
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();
            
            // Iterate the child nodes of the doc.
            NodeList list = doc.getChildNodes();
    
            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e) 
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }
        
        %>
    </body>
    </html>

## <a name="run-the-application"></a>Führen Sie die Anwendung

1. Führen Sie die Anwendung im Emulator Computer bereitstellen Sie oder in Azure mithilfe von [Authentifizieren von Webbenutzern mit Azure Access Control Service verwenden Eclipse][]dokumentierten Schritte.
2. Starten Sie einen Browser, und öffnen Sie die Webanwendung. Nach der Anmeldung der Anwendung sehen Sie SAML Informationen, einschließlich der Security Assertion vom Identitätsanbieter bereitgestellt.

## <a name="next-steps"></a>Nächste Schritte

Weiter erkunden des ACS-Funktionen und komplexere Szenarien experimentieren, siehe [Access Control Service 2.0][].

[Prerequisites]: #pre
[Modify the JSP file to display SAML]: #modify_jsp
[Add the JspWriter library to your build path and deployment assembly]: #add_library
[Run the application]: #run_application
[Next steps]: #next_steps
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Webbenutzer mit Azure Access Control Service mit Eclipse Authentifizierung]: ../active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
 