---
title: "Gérer BranchCache dans WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-branchcache-in-windows-server-essentials"></a>Gérer BranchCache dans WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

BranchCache peut vous aider à optimiser l’utilisation d’Internet, améliorer les performances d’applications en réseau et réduire le trafic sur votre réseau étendu (WAN) lorsque le serveur WindowsServerEssentials est situé à distance à partir de votre bureau ou lorsque les ordinateurs clients connectés à un serveur local utilisent des ressources basées sur le cloud telles que les bibliothèques SharePoint Online.  
  
 Lorsque BranchCache est activé, le lorsqu’un ordinateur client demande le contenu à partir d’un serveur distant de WindowsServerEssentials, le contenu est mis en cache dans la filiale. Après cela, les autres ordinateurs de la même filiale peuvent obtenir le contenu localement au lieu de télécharger le contenu à partir du serveur sur le réseau étendu. Cela peut améliorer les performances des applications en réseau et réduit la consommation de bande passante sur le réseau étendu.  
  
 Si votre serveur WindowsServerEssentials est local ou distant, BranchCache peut améliorer les temps de réponse pour les dossiers partagés du serveur et le contenu Web hébergé sur le serveur (par exemple, les bibliothèques SharePoint Online).  
  
 Étant donné que BranchCache ne requiert pas de nouveau matériel ou modifications de topologie de réseau, cette fonctionnalité vous offre un moyen simple pour optimiser l’utilisation de la bande passante et améliorer les temps de réponse pour les services et ressources accédés via le réseau étendu.  
  
## <a name="branchcache-scenarios"></a>Scénarios BranchCache  
 Il existe trois principaux scénarios d’utilisation de BranchCache avec un serveur distant:  
  
-   Votre serveur WindowsServerEssentials est hébergé dans MicrosoftAzure.  
  
-   Votre serveur WindowsServerEssentials est hébergé dans le centre de données d’un fournisseur de services tiers.  
  
-   Votre serveur WindowsServerEssentials est dans un autre bureau dans un emplacement physique différent.  
  
## <a name="distributed-cache-mode"></a>Mode de cache distribué  
 Dans WindowsServerEssentials, BranchCache est implémenté dans *mode de cache distribué*, un des deux modes de cache disponibles dans BranchCache. En mode de cache distribué, le cache de contenu dans la filiale est distribué entre les ordinateurs clients. Dans la mesure où aucun matériel supplémentaire ou les modifications de la topologie ne sont requises, ce mode convient mieux aux petites filiales que vous utilisent un serveur distant ou un serveur local pour accéder à des services cloud tels que SharePoint Online. Lorsque vous activez BranchCache dans WindowsServerEssentials, le mode de cache distribué est implémenté.  
  
> [!NOTE]
>  Dans les filiales plus grandes avec plusieurs sous-réseaux ou avec un grand nombre d’employés utilisant les applications en réseau, il peut être utile d’implémentation de BranchCache dans *mode de cache hébergé*. En mode de cache hébergé, le cache de contenu est stocké sur un ou plusieurs serveurs de cache hébergé dans la filiale.
  
## <a name="requirements"></a>Configuration requise  
 Pour utiliser BranchCache dans WindowsServerEssentials, vos ordinateurs clients et le serveur doivent répondre aux exigences suivantes:  
  
-   Le serveur doit exécuter le système d’exploitation WindowsServerEssentials ou Windows Server2012R2 Standard ou système d’exploitation de Windows Server2012R2 Datacenter avec le rôle expérience WindowsServerEssentials.  
  
     Sur un serveur Windows Server2012R2 Standard ou Windows Server2012R2 Datacenter, BranchCache est ajouté lorsque vous ajoutez le rôle expérience WindowsServerEssentials. Pour activer BranchCache, vous devez ouvrir une session le tableau de bord WindowsServerEssentials en tant qu’administrateur de domaine.  
  
-   Les ordinateurs clients doivent exécuter Windows7 entreprise, Windows7 Édition intégrale, Windows8 entreprise ou système d’exploitation Windows8.1 entreprise.  
  
-   Dans le mode de cache distribué, tous les ordinateurs clients doivent être sur le même sous-réseau.  
  
    > [!NOTE]
    >  Si vous connecté les ordinateurs clients à votre serveur WindowsServerEssentials sans les joindre au domaine, ces ordinateurs sont exclus de la mise en cache par défaut. Pour inclure un ordinateur client qui n'est pas joint au domaine dans la mise en cache, exécutez le [Enable-BCDistributed](https://technet.microsoft.com/library/hh848398.aspx) applet de commande Windows PowerShell sur l’ordinateur client. Pour plus d’informations, voir [BranchCache Cmdlets dans Windows PowerShell](https://technet.microsoft.com/library/hh848392.aspx).  
 
  
## <a name="turn-branchcache-on"></a>Activer BranchCache  
 Pour activer BranchCache en mode de cache distribué, cliquez simplement sur un bouton sur le tableau de bord WindowsServerEssentials. La mise en cache commence immédiatement et est totalement transparente.  
  
#### <a name="to-turn-on-branchcache-in-windows-server-essentials"></a>Pour activer BranchCache dans WindowsServerEssentials  
  
1.  Ouvrez une session sur le serveur WindowsServerEssentials avec votre compte d’administrateur.  
  
2.  Dans le tableau de bord WindowsServerEssentials, cliquez sur **paramètres**.  
  
     L’Assistant paramètres s’ouvre.  
  
3.  Cliquez sur **BranchCache**.  
  
4.  Sur le **paramètres BranchCache**, cliquez sur **activer**.  
  
## <a name="use-windows-powershell-to-turn-branchcache-on-or-off"></a>Utiliser Windows PowerShell pour activer ou désactiver BranchCache  
 Vous pouvez utiliser Windows PowerShell pour vérifier l’état de BranchCache (activé ou désactivé) et pour activer ou désactiver BranchCache.  
  
#### <a name="to-turn-branchcache-on-or-off-using-windows-powershell"></a>Pour activer BranchCache ou désactiver à l’aide de Windows PowerShell  
  
1.  Sur le serveur, ouvrez Windows PowerShell en tant qu’administrateur. Sur le **Démarrer** page, avec le bouton droit **Windows PowerShell**, cliquez sur **exécuter en tant qu’administrateur**, puis cliquez sur **Oui**.  
  
2.  Entrez une des applets de commande suivantes à l’invite de commandes.  
  
    -   Pour vérifier l’état de BranchCache (activé ou désactivé), entrez:  
  
        ```powershell  
        Get-WSSBranchCacheStatus  
        ```  
  
    -   Pour activer BranchCache, entrez:  
  
        ```powershell  
        Enable-WSSBranchCache  
        ```  
  
    -   Pour désactiver BranchCache, entrez:  
  
        ```powershell  
        Disable-WSSBranchCache  
        ```  
  
## <a name="see-also"></a>Voir aussi  
    
-   [Vue d’ensemble de BranchCache](https://technet.microsoft.com/library/hh831696.aspx)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)
