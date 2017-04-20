<properties 
    pageTitle="Lizenzierung Microsoft® Glätten Streaming-Client Portieren Kit" 
    description="Weitere Informationen über das Microsoft® reibungslose Streaming Client Portieren der Lizenzierung." 
    services="media-services" 
    documentationCenter="" 
    authors="xpouyat,vsood" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016"  
    ms.author="xpouyat"/>

#<a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a>Lizenzierung Microsoft® Glätten Streaming-Client Portieren Kit

##<a name="overview"></a>Übersicht

Microsoft reibungslose Streaming Client Portieren Kit (kurz**SSPK** ) ist eine eingebettete Gerätehersteller, Kabel und Mobilfunkbetreiber Inhaltsanbietern, Hersteller, unabhängige Softwareanbieter (ISVs) und Lösungsanbieter, Produkte und Dienstleistungen für adaptives streaming von Inhalten in Smooth Streaming-Format streaming Hilfe optimiert Clientimplementierung Smooth Streaming. SSPK ist ein Gerät und Plattform unabhängige Implementierung der Smooth Streaming-Client, der vom Lizenznehmer Geräte und Plattformen portiert werden kann. 

Nachstehend ist eine Architektur und IIS reibungslose Streaming Portieren Kit ist die reibungslose Streaming-Implementierung von Microsoft bereitgestellt und enthält die grundlegende Logik für die Wiedergabe von Smooth Streaming. Dies ist dann von Partnern für ein bestimmtes Gerät oder Plattform portiert, entsprechende Schnittstellen implementiert. 

![SSPK](./media/media-services-sspk/sspk-arch.png)

##<a name="description"></a>Beschreibung

SSPK wird Begriffe lizenziert, die hervorragende Geschäftswert bieten. SSPK-Lizenz bietet mit:

- Reibungslose Streaming Portieren Herkunft in C++ 
  - Reibungslose Streaming Clientfunktionen implementiert
  - Analyse, Heuristik Pufferung Logik usw. Format hinzugefügt.
- Player-Anwendung APIs 
  - Programmierschnittstellen für die Interaktion mit einem Media Player-Anwendung
- Plattform Abstraction Layer (PAL) Schnittstelle 
  - Programmierschnittstellen für die Interaktion mit dem Betriebssystem (Threads, Sockets)
- Hardware (Layer, HAL) Schnittstelle 
  - Programmierschnittstellen für die Interaktion mit A / V-Decoder (decodieren, Rendern)
- Digital Rights Management (DRM) Schnittstelle 
  - Programmierschnittstellen für die Behandlung von DRM durch DRM Abstraction Layer (DAL)
  - Microsoft PlayReady Portieren Kit getrennt wird aber über diese Schnittstelle integriert. Weitere Informationen zur Lizenzierung von Microsoft PlayReady Device finden Sie [hier](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).
- Implementierung-Beispiele 
  - PAL beispielhafte für Linux
  - HAL beispielhafte für GStreamer

##<a name="licensing-options"></a>Lizenzoptionen

Microsoft reibungslose Streaming Client Portieren Kit stehen Lizenznehmer unter zwei unterschiedlichen Lizenzverträge: eine glatte Streaming Client Interim-Produkte und weitere reibungslose Streaming Client Endprodukten an Endbenutzer verteilen.
 
- Für Chipsatzhersteller, Systemintegratoren und unabhängige Softwareanbieter (ISVs), die einen Herkunftscode Portieren Kit Interim Produkte benötigen, sollten Microsoft reibungslose Streaming Client Portieren Kit **Interim-Lizenz** ausgeführt werden.
- Gerätehersteller oder unabhängige Softwareanbieter, Rechte für reibungslose Streaming Client Endprodukten für Endbenutzer benötigen, soll Microsoft reibungslose Streaming Client Portieren Kit **Endgültige Produktlizenz** ausgeführt werden.

###<a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a>Microsoft Smooth Streaming-Client Portieren Kit Interim-Lizenz

Unter dieser Lizenz bietet Microsoft eine reibungslose Streaming Client Portieren Kit und die erforderlichen geistigen Eigentumsrechte entwickeln und verteilen glatte Streaming Client Interim-Produkte anderer reibungslose Streaming Client Portieren Kit Gerät Lizenznehmer, die reibungslose Streaming Client Endprodukten verteilen.

####<a name="fee-structure"></a>Gebührenstruktur

Eine einmalige Lizenzgebühr 50.000 bietet Zugriff auf reibungslose Streaming Client Portieren der. 

###<a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a>Microsoft Smooth Streaming-Client Portieren Kit Final-Lizenz

Unter dieser Lizenz bietet Microsoft alle erforderlichen geistigen Eigentumsrechte reibungslose Streaming Client Interim-Produkte von anderen Lizenznehmern reibungslose Streaming Client Portieren Kit erhalten und unternehmensspezifische reibungslose Streaming Client Endprodukten an Endbenutzer verteilen.

####<a name="fee-structure"></a>Gebührenstruktur

Reibungslose Streaming Client Endprodukt wird unter einer Lizenz-Modell als unter angeboten:

- US $0,10 pro Implementierung geliefert
- Die Lizenz ist 50.000 US-Dollar jährlich maximal
- Keine Lizenz für die ersten 10.000 Gerät Implementierung jedes Jahr 

##<a name="licensing-procedure-and-sspk-access"></a>Genehmigungsverfahren und SSPK zugreifen

Kontaktieren Sie [sspkinfo@microsoft.com](mailto:sspkinfo@microsoft.com) für alle Lizenzen Abfragen.

[SSPK Verteilung Portal](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) steht registrierten Interim Lizenznehmer.

Interim und endgültige SSPK Lizenznehmer können technische Fragen senden [smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).

##<a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a>Microsoft Smooth Streaming-Client Zwischenprodukt Vereinbarung Lizenznehmer

- Geschickte Business Solutions, Inc.
- Erweiterte digitale Übertragung SA
- AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.
- Albis Technologies Ltd.
- Alticast Corporation
- Amazon Digital Services, Inc.
- AVC Multimedia Software Co., Ltd.
- Cavium, Inc.
- EchoStar Einkauf Corporation
- Enseo, Inc.
- Fluendo S.A.
- HANDAN BroadInfoCom Co., Ltd.
- Infomir GMBH
- Irdeto USA Inc.
- Liberty Global Services BV
- MediaTek Inc.
- MStar Co., Ltd
- Nintendo Co., Ltd.
- OpenTV, Inc.
- Safran Digital begrenzt
- Sichuan Changhong Electric Co., Ltd
- SoftAtHome
- Sony Corporation
- Tatung Technology Inc.
- TCL Technoly (Huizhou) Co., Ltd
- Vestel Elektronik Sanayi Ve Ticaret A.S.
- VisualOn, Inc.
- ZTE Corporation

##<a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a>Microsoft Smooth Streaming-Client Endprodukt Vereinbarung Lizenznehmer

- Erweiterte digitale Übertragung SA
- AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.
- Albis Technologies Ltd.
- Amazon Digital Services, Inc.
- AmTRAN Co., Ltd
- Arcadyan Technology Corporation
- ATMACA ELEKTRONİK SAN. VE TİC. A.Ş
- British Sky Broadcasting Limited
- CastPal Technology Inc., Shenzhen
- Compal Electronics, Inc.
- Dongguan digitale AV Corp., Ltd
- EchoStar Einkauf Corporation
- Enseo, Inc.
- Filmflex Filme beschränkt
- Fluendo S.A.
- Gibson Innovationen begrenzt
- Haier Informationen Applicantion SRL
- HANDAN BroadInfoCom Co., Ltd.
- Homecast GmbH
- Hon Hai Precision Industry Co., Ltd.
- Infomir GMBH
- Kaonmedia GmbH
- KDDI Corporation
- Nintendo Co., Ltd.
- Orange SA
- Safran Digital begrenzt
- Sagemcom Breitband SAS
- Shenzhen Coship Electronics CO., LTD
- Shenzhen Jiuzhou Electric Co., Ltd
- Skyworth digitale Technologie Co., Ltd. Shenzhen
- Sichuan Changhong Electric Co., Ltd.
- Skardin Industrial Corp.
- Himmel Deutschland Fernsehen GmbH & Co. KG
- SmarDTV S.A.
- SoftAtHome
- Sony Corporation
- TCL überseeischen Marketing (Macao kommerziellen Offshore) beschränkt
- Farbenfrohen Technologien, SAS
- Globale Tongfang Ltd.
- Toshiba Lifestyle Products & Services Corporation
- Universelle Medienunternehmen /Slovakia/ GmbH
- VIZIO, Inc.
- Wistron Corporation
- ZTE Corporation

##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
