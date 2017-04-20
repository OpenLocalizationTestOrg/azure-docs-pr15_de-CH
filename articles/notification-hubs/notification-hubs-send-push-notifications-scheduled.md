<properties
    pageTitle="Geplante Benachrichtigungen senden | Microsoft Azure"
    description="Dieses Thema beschreibt Azure Notification Hubs mit Benachrichtigungen geplant."
    services="notification-hubs"
    documentationCenter=".net"
    keywords="Pushbenachrichtigungen Push-Benachrichtigung Pushbenachrichtigungen planen"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="how-to-send-scheduled-notifications"></a>Gewusst wie: Senden geplante Benachrichtigung


##<a name="overview"></a>Übersicht

Wenn Sie ein Szenario haben, in dem irgendwann in der Zukunft benachrichtigen möchten, aber haben nicht so Reaktivieren der Back-End-Code zum Senden der Benachrichtigung. Standard-Tier Benachrichtigungshubs unterstützt Sie Benachrichtigungen, 7 Tage in der Zukunft planen können.

Beim Senden einer Benachrichtigung verwenden Sie die [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) -Klasse einfach im SDK Hubs Benachrichtigung fest, wie im folgenden Beispiel gezeigt:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Außerdem können Sie eine zuvor geplante Benachrichtigung über seine NotificationId Abbrechen:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Es gibt keine Grenzwerte für die Anzahl der geplanten Benachrichtigungen senden können.