<properties 
    pageTitle="Bekannte Netzwerke | Microsoft Azure" 
    description="Bekannte Netzwerke konfigurieren, können Sie vermeiden, IP-Adressen, die von Ihrer Organisation Anmeldungen über mehrere Regionen und Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten enthalten sind." 
    services="active-directory" 
    documentationCenter="" 
    authors="markusvi" 
    manager="femila"  
    editor=""/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="markvi"/>

# <a name="known-networks"></a>Bekannte Netzwerke


Azure Active Directory-Zugriff und Berichte können Einblick in die Integrität und Sicherheit des Verzeichnisses Ihrer Organisation zu. Mit diesen Informationen bestimmen Verzeichnis Admin besser Sicherheitsrisiken liegen kann, damit sie angemessen, diese Risiken planen können.

Es ist möglich, dass die Berichte "*Anmeldungen über mehrere Regionen*" und "*Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten*" falsch IP-Adressen kennzeichnen, die tatsächlich von der Organisation gehören. 

Beispielsweise kann dies bei: 

- Ein Benutzer in der Boston Office Remote Rechenzentrum in San Francisco angemeldet hat löst Bericht "Ins über mehrere Regionen signieren" 

- Versucht ein Benutzer Ihrer Organisation anmelden mit einem falschen Kennwort Trigger Bericht "Signieren ins mit verdächtigen Aktivitäten von IP-Adressen" 

Um diesen Fällen verhindern irreführenden Sicherheitsberichte, sollten Sie bekannte IP-Adressbereiche zur Liste Ihrer Organisation öffentliche IP-Adresse hinzufügen.    


###<a name="to-add-your-organizations-public-ip-address-ranges-perform-the-following-steps"></a>Um Ihrer Organisation öffentliche IP-Adressbereiche hinzuzufügen, führen Sie die folgenden Schritte: 

1.  Anmeldung bei [Azure-Verwaltungsportal](https://manage.windowsazure.com).

2.  Klicken Sie im linken Bereich auf **Active Directory**. <br><br>![Funktionsweise von Cloud App Discovery](./media/active-directory-known-networks/known-netwoks-01.png)

3.  Wählen Sie auf der Registerkarte **Verzeichnis** das Verzeichnis.

4.  Klicken Sie im Menü oben auf **Konfigurieren**. <br><br>![Funktionsweise von Cloud App Discovery](./media/active-directory-known-networks/known-netwoks-02.png)

5.  Gehen Sie auf die Registerkarte konfigurieren zu **Ihr Unternehmen öffentliche IP-Adressbereiche** <br><br>![Funktionsweise von Cloud App Discovery](./media/active-directory-known-networks/known-netwoks-03.png)

6.  Klicken Sie auf **Bekannte IP-Adressbereiche**.

7.  Die Adressbereiche hinzufügen im Dialogfeld und klicken die Kontrollkästchen benötigen. <br><br>![Funktionsweise von Cloud App Discovery](./media/active-directory-known-networks/known-netwoks-04.png)


**Zusätzliche Ressourcen**


* [Ihre Verwendung Berichte anzeigen](active-directory-view-access-usage-reports.md)
* [Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [Anmeldungen über mehrere Regionen](active-directory-reporting-sign-ins-from-multiple-geographies.md)


