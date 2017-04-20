<properties 
   pageTitle="Debuggen von Azure Cloud Services | Microsoft Azure"
   description="Debuggen von Azure-Cloud-Diensten"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="debugging-cloud-services"></a>Debuggen von Clouddiensten

Ansätze können Azure Anwendung mithilfe der Azure-Tools für Microsoft Visual Studio und Azure SDK:

- Sie können eine Azure-Anwendung von Visual Studio beim Entwickeln, Debuggen, wie Visual C# oder Visual Basic-Anwendung. Weitere Informationen finden Sie in der [Cloud-Dienst auf dem lokalen Computer Debuggen](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer).

- Azure-Diagnose können sich Informationen in Rollen Code, ob die Rollen in der oder in Azure ausgeführt werden. Weitere Informationen finden Sie unter [Sammeln von Protokolldaten mithilfe von Azure-Diagnose](http://go.microsoft.com/fwlink/p/?LinkId=400450).

- Verwenden Sie Visual Studio Enterprise sind Rollen, die auf.NET Framework 4 oder.NET Framework 4.5 ausgerichtet schreiben, können IntelliTrace Zeitpunkt Sie die Bereitstellung eines Azure-Cloud-Dienstes von Visual Studio. IntelliTrace stellt ein Protokoll, mit denen Sie mit Visual Studio zum Debuggen Ihrer Anwendung in Azure ausgeführt werden. Weitere Informationen finden Sie in der [veröffentlichten Clouddienst Visual Studio mit IntelliTrace Debuggen]( http://go.microsoft.com/fwlink/p/?LinkId=623016).

- Sie können das Remotedebuggen auf Ihre Clouddienste gleichzeitig als Cloud-Dienst von Visual Studio bereitgestellt. Möchten Sie Remotedebugging für eine Bereitstellung aktivieren, werden remote debugging-Diensten auf virtuellen Computern installiert, die jede Instanz der Rolle ausgeführt. Diese Dienste wie msvsmon.exe nicht beeinträchtigen oder zusätzliche Kosten. Weitere Informationen finden Sie unter [Debuggen einen Cloud-Dienst in Azure](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure).



