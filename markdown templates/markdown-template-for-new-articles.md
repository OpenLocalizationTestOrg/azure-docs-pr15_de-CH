<properties
   pageTitle="Seitentitel in der Browser-Registerkarte und Suchergebnissen angezeigt"
   description="Artikelbeschreibung, Zielseiten und die meisten Suchergebnisse angezeigt werden"
   services="service-name"
   documentationCenter="dev-center-name"
   authors="GitHub-alias-of-only-one-author"
   manager="manager-alias"
   editor=""/>

<tags
   ms.service="required"
   ms.devlang="may be required"
   ms.topic="article"
   ms.tgt_pltfrm="may be required"
   ms.workload="required"
   ms.date="mm/dd/yyyy"
   ms.author="Your MSFT alias or your full email address;semicolon separates two or more"/>

# <a name="markdown-template-article-heading-1-or-h1-heading-at-the-top-of-the-article"></a>Abzug Vorlage (Artikel Überschrift 1 oder H1): Überschrift am Anfang des Artikels

Der Abzug von dieser Vorlage kopieren, kopieren Artikel in Ihrem lokalen Repo oder GitHub UI Raw klicken und Kopieren der Abzug.

  ![ALT-Text; Lassen Sie leer. Beschreiben Sie Bild.][8]

Einleitung: Lorem Dolor Amet, Adipiscing Elit. Phasellus Interdum Nulla Risus Lacinia Porta lautete Imperdiet SED. Mauris Dolor Mauris Tempus Sed Lacinia nec Euismod nicht Felis. Nunc Semper Porta Ultrices. Maecenas Neque Nulla Condimentum Vitae Ipsum Sit Amet, Dignissim Aliquet nisi.

## <a name="heading-2-h2"></a>Überschrift 2 (H2)

Aenean sit Amet Leo nec Purus Placerat Fermentum Ac Gravida Odio. Aenean Tellus Lectus Faucibus in Rhoncus in Faucibus Sed Urna.  Volutpat mi-Id Purus Ultrices Iaculis nec nicht Neque. [Linktext für Link außerhalb azure.microsoft.com](http://weblogs.asp.net/scottgu). Nullam Maxime Dolor unter Aliquam Pharetra. Vivamus Ac Hendrerit Mauris [Beispiel Linktext für Link zu einem Artikel in einen-Ordner](../articles/expressroute/expressroute-bandwidth-upgrade.md).

[Google] 10 Mal mehr Verkehr erhalten [ gog] als von [Yahoo]  [ yah] oder [MSN] [msn].

> [AZURE.NOTE] Notiztext eingezogen.  Veröffentlichung wird das Wort "Hinweis" hinzugefügt. UT EU-Pretium Lacus. Nullam Purus est Iaculis Sed est Ebene, Euismod abschrecken Odio. Curabitur Lacinia Erat Tristique Iaculis Rutrum nisi Erat sem Sodales, EU-Condimentum Turpis nisi eine Purus.

1. Aenean sit Amet Leo nec **Purus** Placerat Fermentum Ac Gravida Odio.

2. Aenean Tellus Lectus Faucius in **Rhoncus** in Faucibus Sed Urna. Suspendisse Volutpat mi-Id Purus Ultrices Iaculis nec nicht Neque.

    ![ALT-Text; Lassen Sie leer. Kollektor Auto racing Rot.][5]

3. Nullam Maxime Dolor unter Aliquam Pharetra. Vivamus Ac Hendrerit Mauris. SED Dolor Dui Condimentum et Varius ein abschrecken am lautete.

    ![ALT-Text; Lassen Sie nicht leer][6]


Suspendisse Volutpat mi-Id Purus Ultrices Iaculis nec nicht Neque. Nullam Maxime Dolor unter Aliquam Pharetra. Vivamus Ac Hendrerit Mauris. Otrus Informatus: [1 Link zu einem anderen azure.microsoft.com Dokumentation Thema](virtual-machines-windows-hero-tutorial.md)

## <a name="heading-h2"></a>Überschrift (H2)

UT EU-Pretium Lacus. Nullam Purus est Iaculis Sed est Ebene, Euismod abschrecken Odio.

1. Curabitur Lacinia Erat Tristique Iaculis Rutrum nisi Erat sem Sodales, EU-Condimentum Turpis nisi eine Purus.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:
        (NSDictionary *)launchOptions
        {
            // Register for remote notifications
            [[UIApplication sharedApplication] registerForRemoteNotificationTypes:
            UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
            return YES;
        }

2. Vestibulum Ante Ipsum Primis in Faucibus Orci Luctus et Ultrices Posuere Cubilia.

        // Because toast alerts don't work when the app is running, the app handles them.
        // This uses the userInfo in the payload to display a UIAlertView.
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
        (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
            [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
            @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }


    > [AZURE.NOTE] Duis Sed Durchmesser nicht <i>lautete Molestie</i> Pharetra Eget eine est. [Verbindung 2 zu einem anderen Thema der azure.microsoft.com-Dokumentation](web-sites-custom-domain-name.md)


Quisque Commodo Eros Ebene Lectus Euismod Auctor Eget sit Amet Leo. Mitgliedsorganisationen Faucibus Suscipit Tellus Dignissim Ultrices.

## <a name="heading-2-h2"></a>Überschrift 2 (H2)

1. Maecenas Sed-Condimentum nisi. Suspendisse Potenti.

  + Fusce
  + Malesuada
  + SEM

2. Nullam in Massa Eu Tellus Tempus Hendrerit.

    ![ALT-Text; Lassen Sie leer. Beispiel für ein Channel 9 Video.][7]

3. Quisque Felis Enim Fermentum Ut Aliquam nec Pellentesque Pulvinar Magna.




<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Nächste Schritte

Vestibül Ante Ipsum Primis in Faucibus Orci Luctus et Ultrices Posuere Cubilia Curae; Nullam Ultricies Ipsum Vitae Volutpat Hendrerit Purus Durchmesser Pretium Eros, Lebenslauf Tincidunt Nulla Lorem Sed Turpis: [3 Link zu einem anderen azure.microsoft.com Dokumentation Thema](storage-whatis-account.md).

<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png
[8]: ./media/markdown-template-for-new-articles/copytemplate.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[gog]: http://google.com/        
[yah]: http://search.yahoo.com/  
[msn]: http://search.msn.com/    
