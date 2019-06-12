---
title: Migration dynamique sans Clustering de basculement permet de déplacer un ordinateur virtuel
description: Donne les conditions préalables et les instructions permettant d’effectuer une migration dynamique dans un environnement autonome.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75c32e42-97f7-48df-aac9-1d82d34825e1
author: KBDAzure
ms.author: kathydav
ms.date: 01/17/2017
ms.openlocfilehash: 9be61fbc860e9d8c5cbc020d6dd4082722e32509
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812096"
---
# <a name="use-live-migration-without-failover-clustering-to-move-a-virtual-machine"></a>Migration dynamique sans Clustering de basculement permet de déplacer un ordinateur virtuel

>S'applique à : Windows Server 2016

Cet article vous montre comment déplacer un ordinateur virtuel en effectuant une migration dynamique sans utiliser le Clustering de basculement. Une migration dynamique déplace les ordinateurs virtuels en cours d’exécution entre les hôtes Hyper-V sans temps mort perceptible.   
  
Pour ce faire, vous devez :   

- Un compte d’utilisateur qui est membre du groupe local Administrateurs Hyper-V ou du groupe Administrateurs sur les ordinateurs source et de destination. 
  
- Le rôle Hyper-V dans Windows Server 2016 ou Windows Server 2012 R2 installé sur les serveurs source et de destination et configuré pour les migrations dynamiques. Vous pouvez effectuer une migration dynamique entre les ordinateurs hôtes exécutant Windows Server 2016 et Windows Server 2012 R2, si la machine virtuelle est au moins la version 5.

    Pour obtenir des instructions de mise à niveau de version, consultez [version de mise à niveau machine virtuelle dans Hyper-V sur Windows 10 ou Windows Server 2016](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Pour obtenir des instructions d’installation, consultez [configurer des hôtes pour la migration dynamique](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md).

- Les outils de gestion Hyper-V installés sur un ordinateur exécutant Windows Server 2016 ou Windows 10, à moins que les outils sont installés sur le serveur source ou destination et que vous devez les exécuter à partir de là.  
   
## <a name="use-hyper-v-manager-to-move-a-running-virtual-machine"></a>Utilisez le Gestionnaire Hyper-V pour déplacer une machine virtuelle en cours d’exécution  
  
1.  Ouvrez le Gestionnaire Hyper-V. (Dans le Gestionnaire de serveur, cliquez sur **outils** >>**Gestionnaire Hyper-V**.)  
  
2.  Dans le volet de navigation, sélectionnez un des serveurs. (Si elle n’est pas répertoriée, cliquez sur **Gestionnaire Hyper-V**, cliquez sur **se connecter au serveur**, tapez le nom du serveur, cliquez sur **OK**. Répétez l’opération pour ajouter d’autres serveurs.)  
  
3.  À partir de la **Machines virtuelles** volet, avec le bouton droit de la machine virtuelle, puis cliquez sur **déplacer**. Cette opération ouvre l’Assistant de déplacement. 
  
4.  Utilisez les pages d’Assistant à choisir le type de déplacement et options de serveur de destination.
  
5.  Dans la page **Résumé** , passez vos choix en revue, puis cliquez sur **Terminer**.  

## <a name="use-windows-powershell-to-move-a-running-virtual-machine"></a>Utiliser Windows PowerShell pour déplacer une machine virtuelle en cours d’exécution
  
L’exemple suivant utilise l’applet de commande Move-VM pour déplacer un ordinateur virtuel nommé *LMTest* à un serveur de destination nommé *TestServer02* et déplace les disques durs virtuels et autres fichiers, ces points de contrôle et Intelligente des fichiers d’échange, à la *D:\LMTest* répertoire sur le serveur de destination.  
  
```  
PS C:\> Move-VM LMTest TestServer02 -IncludeStorage -DestinationStoragePath D:\LMTest  
```  
  
## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="failed-to-establish-a-connection"></a>Impossible d’établir une connexion 

Si vous n’avez pas configuré la délégation contrainte, vous devez vous connecter au serveur source avant de pouvoir déplacer une machine virtuelle. Si vous ne le faites pas, la tentative d’authentification échoue, une erreur se produit, et ce message s’affiche :  
  
« Échec de l’opération de migration de machine virtuelle à la Source de la migration.  
Impossible d’établir une connexion avec l’hôte *nom de l’ordinateur*: Aucune information d’identification n’est disponibles dans le package de sécurité 0x8009030E. »
  
 Pour résoudre ce problème, connectez-vous au serveur source et essayez du déplacer à nouveau. Pour éviter d’avoir à se connecter à un serveur source avant de procéder à une migration dynamique, configurer la délégation contrainte. Vous aurez besoin des informations d’identification administrateur de domaine pour configurer la délégation contrainte. Pour obtenir des instructions, consultez [configurer des hôtes pour la migration dynamique](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md). 
 
 ### <a name="failed-because-the-host-hardware-isnt-compatible"></a>A échoué, car le matériel hôte n’est pas compatible
 
 Si une machine virtuelle n’a pas compatibilité du processeur activée et possède un ou plusieurs instantanés, le déplacement échoue si les ordinateurs hôtes ont des versions de processeur différentes. Une erreur se produit, et ce message s’affiche :
 
**La machine virtuelle ne peut pas être déplacée vers l’ordinateur de destination. Le matériel sur l’ordinateur de destination n’est pas compatible avec la configuration matérielle requise de cette machine virtuelle.**
 
 Pour résoudre ce problème, arrêtez la machine virtuelle et activez le paramètre de compatibilité du processeur.
 
1. À partir du Gestionnaire Hyper-V, dans le **Machines virtuelles** volet, avec le bouton droit de la machine virtuelle et cliquez sur paramètres.
2. Dans le volet de navigation, développez **processeurs** et cliquez sur **compatibilité**.
3. Vérifiez **migrer vers un ordinateur avec une autre version de processeur**.
4. Cliquez sur **OK**.
 
   Pour utiliser Windows PowerShell, utilisez le [Set-VMProcessor](https://technet.microsoft.com/library/hh848533.aspx) applet de commande :
 
   ```
   PS C:\> Set-VMProcessor TestVM -CompatibilityForMigrationEnabled $true
   ```