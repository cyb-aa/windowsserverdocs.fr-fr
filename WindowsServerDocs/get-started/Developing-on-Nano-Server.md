---
title: Développement pour Nano Server
description: Communication à distance PowerShell et sessions CIM
ms.prod: windows-server
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.topic: get-started-article
ms.assetid: 57079470-a1c1-4fdc-af15-1950d3381860
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 5933b031260a69bf986d7ca2f7abd832055421fa
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827082"
---
# <a name="developing-for-nano-server"></a>Développement pour Nano Server

>S'applique à : Windows Server 2016

> [!IMPORTANT]
> À compter de Windows Server, version 1709, Nano Server sera uniquement disponible sous forme d’[image de système d’exploitation de base du conteneur](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Pour en savoir plus, consultez [Modifications apportées à Nano Server](nano-in-semi-annual-channel.md). 

Ces rubriques expliquent les différences importantes de PowerShell sur Nano Server et fournissent des conseils pour développer vos propres applets de commande PowerShell pour Nano Server.

- [PowerShell sur Nano Server](PowerShell-on-Nano-Server.md)
- [Développement d’applets de commande PowerShell pour Nano Server](Developing-PowerShell-Cmdlets-for-Nano-Server.md)

## <a name="using-windows-powershell-remoting"></a>Utilisation de la communication à distance Windows PowerShell  
Pour gérer Nano Server avec la communication à distance Windows PowerShell, vous devez ajouter l’adresse IP de l’instance Nano Server à la liste des hôtes approuvés par votre ordinateur de gestion, ajouter le compte que vous utilisez à la liste des administrateurs de Nano Server, et activer CredSSP si vous prévoyez d’utiliser cette fonctionnalité.  

> [!NOTE]
> Si l'instance Nano Server cible et votre ordinateur de gestion se trouvent dans la même forêt AD DS (ou dans des forêts ayant une relation d'approbation), vous n'avez pas besoin d'ajouter l'instance Nano Server à la liste des hôtes approuvés. Il vous suffit de vous connecter à l'instance Nano Server à l'aide de son nom de domaine complet, par exemple : PS C:\> Enter-PSSession -ComputerName nanoserver.contoso.com -Credential (Get-Credential)
  
  
Pour ajouter le serveur Nano Server à la liste des hôtes approuvés, exécutez cette commande à partir d’une invite Windows PowerShell avec élévation de privilèges :  
  
`Set-Item WSMan:\localhost\Client\TrustedHosts <IP address of Nano Server>`  
  
Pour démarrer la session Windows PowerShell à distance, démarrez une session Windows PowerShell locale avec élévation de privilèges, puis exécutez les commandes suivantes :  
  
  
```  
$ip = \<IP address of Nano Server>  
$user = $ip\Administrator  
Enter-PSSession -ComputerName $ip -Credential $user  
```  
  
  
Vous pouvez maintenant exécuter normalement des commandes Windows PowerShell sur le serveur Nano Server.  
  
> [!NOTE]  
> Certaines commandes Windows PowerShell ne sont pas disponibles dans cette version de Nano Server. Pour voir les commandes disponibles, exécutez `Get-Command -CommandType Cmdlet`  
  
Pour arrêter la session à distance, utilisez la commande `Exit-PSSession`  
  
## <a name="using-windows-powershell-cim-sessions-over-winrm"></a>Utilisation de sessions CIM Windows PowerShell sur WinRM  
Vous pouvez utiliser des sessions et instances CIM dans Windows PowerShell pour exécuter des commandes WMI sur le service Gestion à distance de Windows (WinRM).  
  
Pour démarrer la session CIM, exécutez les commandes suivantes dans une invite de commandes Windows PowerShell :  
  
  
```  
$ip = <IP address of the Nano Server\>  
$ip\Administrator  
$cim = New-CimSession -Credential $user -ComputerName $ip  
```  
  
  
Une fois la session établie, vous pouvez exécuter différentes commandes WMI, par exemple :  
  
  
```  
Get-CimInstance -CimSession $cim -ClassName Win32_ComputerSystem | Format-List *  
Get-CimInstance -CimSession $Cim -Query SELECT * from Win32_Process WHERE name LIKE 'p%'  
```  
  
  