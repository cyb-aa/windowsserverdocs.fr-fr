---
title: Gérer BranchCache dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6e05aec-d07c-4e0b-94ab-f20279e9ffd1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 13d1d439eb9eeb60de9779d783e36405aee3ddfc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828800"
---
# <a name="manage-branchcache-in-windows-server-essentials"></a>Gérer BranchCache dans Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

BranchCache peut vous aider à optimiser l’utilisation d’Internet, améliorer les performances des applications en réseau et réduire le trafic sur votre réseau étendu (WAN) lorsque le serveur Windows Server Essentials se trouve à distance à partir de votre bureau, ou lorsque les ordinateurs clients connecté à un serveur local utilisez ressources cloud telles que les bibliothèques SharePoint Online.  
  
 Avec BranchCache activé, quand un ordinateur client demande du contenu à partir d’un serveur Windows Server Essentials à distance, le contenu est mis en cache la filiale. Par la suite, les autres ordinateurs de la même filiale peuvent obtenir le contenu localement plutôt que de le télécharger à partir des serveurs via le réseau étendu. Ce mode de fonctionnement peut améliorer les performances d’applications en réseau et réduire la consommation de bande passante sur le réseau étendu.  
  
 Si votre serveur Windows Server Essentials est local ou distant, BranchCache peut améliorer les temps de réponse pour les dossiers partagés du serveur et du contenu Web qui est hébergé sur le serveur (par exemple, les bibliothèques SharePoint Online).  
  
 Étant donné que BranchCache ne requiert pas de nouveau matériel ou de modifications de topologie de réseau, c’est une excellente solution pour optimiser l’utilisation de la bande passante et améliorer les temps de réponse pour les services et ressources accessibles via le réseau étendu.  
  
## <a name="branchcache-scenarios"></a>Scénarios BranchCache  
 Il existe trois principaux scénarios d’utilisation de BranchCache avec un serveur distant :  
  
-   Votre serveur Windows Server Essentials est hébergé dans Microsoft Azure.  
  
-   Votre serveur Windows Server Essentials est hébergé dans le centre de données d’un fournisseur de service tiers.  
  
-   Votre serveur Windows Server Essentials est dans un autre bureau dans un emplacement physique différent.  
  
## <a name="distributed-cache-mode"></a>Mode de cache distribué  
 Dans Windows Server Essentials, BranchCache est implémenté dans *mode de cache distribué*, un des deux modes de cache disponibles dans BranchCache. En mode de cache distribué, le cache de contenu dans la filiale est distribué entre les ordinateurs clients. Comme aucun matériel supplémentaire ni aucune modification de topologie ne sont nécessaires, ce mode convient mieux aux petites filiales qui utilisent un serveur distant ou qui utilisent un serveur local pour accéder à des services cloud tels que SharePoint Online. Lorsque vous activez BranchCache dans Windows Server Essentials, le mode de cache distribué est implémenté.  
  
> [!NOTE]
>  Dans les filiales plus grandes disposant de plusieurs sous-réseaux ou d’un nombre important d’employés utilisant les applications en réseau, l’implémentation de BranchCache en *mode de cache hébergé*a de nombreux avantages. En mode de cache hébergé, le cache de contenu est stocké dans un ou plusieurs serveurs de cache hébergés au sein de la filiale.
  
## <a name="requirements"></a>Configuration requise  
 Pour utiliser BranchCache dans Windows Server Essentials, vos ordinateurs serveurs et clients doivent remplir les conditions suivantes :  
  
-   Le serveur doit être en cours d’exécution du système d’exploitation Windows Server Essentials ou Windows Server 2012 R2 Standard ou système d’exploitation de Windows Server 2012 R2 Datacenter avec le rôle expérience Windows Server Essentials.  
  
     Sur un serveur Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter, BranchCache est ajouté lorsque vous ajoutez le rôle expérience Windows Server Essentials. Pour activer BranchCache, vous devez vous connecter à bord Windows Server Essentials en tant qu’administrateur de domaine.  
  
-   Ordinateurs clients doivent exécuter Windows 7 entreprise, Windows 7 Édition intégrale, Windows 8 entreprise ou système d’exploitation Windows 8.1 Enterprise.  
  
-   En mode de cache distribué, tous les ordinateurs clients doivent être situés sur le même sous-réseau.  
  
    > [!NOTE]
    >  Si vous avez connecté vos ordinateurs clients au serveur Windows Server Essentials sans les joindre au domaine, ces ordinateurs sont exclus de la mise en cache par défaut. Pour inclure un ordinateur client qui n’est pas joint au domaine dans la mise en cache, exécutez l’applet de commande [Enable-BCDistributed](https://technet.microsoft.com/library/hh848398.aspx) Windows PowerShell sur l’ordinateur client. Pour plus d’informations, consultez [Applets de commande BranchCache dans Windows PowerShell](https://technet.microsoft.com/library/hh848392.aspx).  
 
  
## <a name="turn-branchcache-on"></a>Activer BranchCache  
 Pour activer BranchCache en mode de cache distribué, cliquez simplement sur un bouton sur le tableau de bord Windows Server Essentials. La mise en cache commence immédiatement et est totalement transparente.  
  
#### <a name="to-turn-on-branchcache-in-windows-server-essentials"></a>Pour activer BranchCache dans Windows Server Essentials  
  
1.  Connectez-vous sur le serveur Windows Server Essentials avec votre compte d’administrateur.  
  
2.  Dans le tableau de bord Windows Server Essentials, cliquez sur **paramètres**.  
  
     L’Assistant Paramètres s’ouvre.  
  
3.  Cliquez sur **BranchCache**.  
  
4.  Dans la page **Paramètres BranchCache** , cliquez sur **Activer**.  
  
## <a name="use-windows-powershell-to-turn-branchcache-on-or-off"></a>Utiliser Windows PowerShell pour activer ou désactiver BranchCache  
 Vous pouvez utiliser Windows PowerShell pour vérifier l’état de BranchCache (activé ou désactivé) et pour activer ou désactiver BranchCache.  
  
#### <a name="to-turn-branchcache-on-or-off-using-windows-powershell"></a>Pour activer ou désactiver BranchCache à l’aide de Windows PowerShell  
  
1.  Sur le serveur, ouvrez Windows PowerShell en tant qu’administrateur. Sur la page **d’accueil**, cliquez avec le bouton droit sur **Windows PowerShell**, cliquez sur **Exécuter en tant qu’administrateur**, puis sur **Oui**.  
  
2.  Entrez l’une des applets de commande suivantes à l’invite de commandes.  
  
    -   Pour vérifier l’état de BranchCache (activé ou désactivé), entrez :  
  
        ```powershell  
        Get-WSSBranchCacheStatus  
        ```  
  
    -   Pour activer BranchCache, entrez :  
  
        ```powershell  
        Enable-WSSBranchCache  
        ```  
  
    -   Pour désactiver BranchCache, entrez :  
  
        ```powershell  
        Disable-WSSBranchCache  
        ```  
  
## <a name="see-also"></a>Voir aussi  
    
-   [Vue d’ensemble de BranchCache](https://technet.microsoft.com/library/hh831696.aspx)  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)
