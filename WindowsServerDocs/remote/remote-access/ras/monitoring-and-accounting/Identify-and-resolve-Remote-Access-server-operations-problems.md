---
title: Identifier et résoudre les problèmes de fonctionnement du serveur d'accès à distance
description: Cette rubrique fait partie du guide de la surveillance de l’accès à distance et la gestion des comptes dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ce84c9f-fd1f-4463-8fc7-d2f33344a2c9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c58ff97e286f91610a321801d177a2977349fa78
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840460"
---
# <a name="identify-and-resolve-remote-access-server-operations-problems"></a>Identifier et résoudre les problèmes de fonctionnement du serveur d'accès à distance

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

**Remarque :** Windows Server 2012 associe DirectAccess et le service Routage et accès distant (RRAS) dans un rôle Accès à distance unique.  
  
Vous pouvez effectuer les procédures ci-dessous pour identifier les problèmes de fonctionnement du serveur accès à distance, leurs causes racine et la résolution nécessaire pour résoudre les problèmes.  
  
> [!NOTE]  
> Vous devez être connecté en tant que membre du groupe Admins du domaine ou un membre du groupe Administrateurs sur chaque ordinateur pour effectuer les tâches décrites dans cette rubrique. Si vous ne pouvez pas effectuer une tâche pendant que vous êtes connecté avec un compte qui est membre du groupe Administrateurs, essayez d’effectuer la tâche pendant que vous êtes connecté avec un compte qui est membre du groupe Admins du domaine.  
  
Cette rubrique inclut des informations sur les tâches suivantes :  
  
- Simuler un problème d’opérations  
  
- Identifier le problème d’opérations et l’action corrective  
  
- Restaurer le service d’assistance IP  
  
### <a name="BKMK_Simulate"></a>Simuler un problème d’opérations  
  
> [!CAUTION]  
> Étant donné que votre accès à distance serveur est probablement configuré correctement et non rencontre des problèmes, vous pouvez utiliser la procédure suivante pour simuler un problème d’opérations. Si votre serveur traite actuellement des clients dans un environnement de production, il pouvez que vous ne souhaitez pas effectuer ces actions pour l’instant. Au lieu de cela, vous pouvez lire les étapes pour comprendre comment résoudre les problèmes qui peuvent se produire sur votre serveur d’accès à distance à l’avenir.  
  
Application d’assistance IP (IPHlpSvc) hôtes IPv6 transition technologies de service (par exemple, IP-HTTPS, 6to4 ou Teredo), qui est requis pour le serveur DirectAccess fonctionner correctement. Pour illustrer un problème simulé des opérations sur le serveur d’accès à distance, vous devez arrêter le service de réseau (IPHlpSvc).  
  
##### <a name="to-stop-the-ip-helper-service"></a>Pour arrêter le service d’assistance IP  
  
1.  Sur le **Démarrer** écran du serveur d’accès à distance, cliquez sur **outils d’administration**, puis double-cliquez sur **Services**.  
  
2.  Dans la liste des **Services**, faites défiler vers le bas et avec le bouton droit **d’assistance IP**, puis cliquez sur **arrêter**.  
  
### <a name="BKMK_Identify"></a>Identifier le problème d’opérations et l’action corrective  
La désactivation du service d’assistance IP entraîne une erreur grave sur le serveur d’accès à distance. Le tableau de bord de surveillance affiche l’état de fonctionnement du serveur et les détails du problème.  
  
##### <a name="to-identify-the-details-and-take-corrective-action"></a>Pour identifier les détails et l’action corrective  
  
1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **Gestion de l'accès à distance**.  
  
2.  Cliquez sur **TABLEAU DE BORD** pour accéder à **Tableau de bord des accès distants** dans la **Console de gestion de l'accès distant**.  
  
3.  Assurez-vous que votre serveur d’accès à distance est sélectionné dans le volet gauche, puis dans le volet central, cliquez sur **état des opérations**.  
  
4.  Vous verrez la liste des composants avec des icônes vertes ou rouges, qui indiquent leur état de fonctionnement. Cliquez sur le **IP-HTTPS** ligne dans la liste. Lorsque vous avez sélectionné une ligne, les détails de l’opération sont affichés dans le **détails** volet comme suit :  
  
    **Erreur**  
  
    Le service d’assistance IP (IPHlpSvc) s’est arrêté. DirectAccess ne peut pas fonctionner comme prévu. Le service d’assistance IP assure la connectivité de tunnel à l’aide de la plateforme de connectivité, les technologies de transition IPv6 et IP-HTTPS.  
  
    **Causes**  
  
    1.  Le service d’assistance IP s’est arrêté.  
  
    2.  Le service d’assistance IP ne répond pas.  
  
    **Résolution**  
  
    1.  Pour vous assurer que le service est en cours d’exécution, tapez **iphlpsc de Get-Service** à l’invite de Windows PowerShell.  
  
    2.  Pour activer le service, tapez **Start-Service iphlpsvc** à partir d’une invite Windows PowerShell avec élévation de privilèges.  
  
    3.  Pour redémarrer le service, tapez **Restart-Service iphlpsvc** à partir d’une invite Windows PowerShell avec élévation de privilèges.  
  
### <a name="BKMK_Restart"></a>Restaurer le service d’assistance IP  
Pour restaurer le service d’assistance IP sur votre serveur d’accès à distance, vous pouvez suivre les étapes de résolution ci-dessus pour démarrer ou redémarrer le service, ou vous pouvez utiliser la procédure suivante pour inverser la procédure que vous avez utilisé pour simuler la défaillance du service d’assistance IP.  
  
##### <a name="to-restart-the-ip-helper-service-on-the-remote-access-server"></a>Pour redémarrer le service d’assistance IP sur le serveur d’accès à distance  
  
1.  Sur le **Démarrer** , cliquez sur **outils d’administration**, puis double-cliquez sur **Services**.  
  
2.  Dans la liste des **Services**, faites défiler vers le bas et avec le bouton droit **d’assistance IP**, puis cliquez sur **Démarrer**.  
  
![Windows PowerShell](../../../media/Identify-and-resolve-Remote-Access-server-operations-problems/PowerShellLogoSmall.gif)Windows PowerShell équivalente commandes ***  
  
L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
```  
PS> Get-RemoteAccessHealth | Where-Object {$_.Component -eq "IP-HTTPS"} | Format-List -Property *  
```  
  


