---
title: Utiliser la migration dynamique sans clustering de basculement pour déplacer un ordinateur virtuel
description: Indique les conditions préalables et les instructions permettant d’exécuter une migration dynamique dans un environnement autonome.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75c32e42-97f7-48df-aac9-1d82d34825e1
author: KBDAzure
ms.author: kathydav
ms.date: 01/17/2017
ms.openlocfilehash: 55c96ff4696871e4013c3abd6247209d0d4517c0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392558"
---
# <a name="use-live-migration-without-failover-clustering-to-move-a-virtual-machine"></a>Utiliser la migration dynamique sans clustering de basculement pour déplacer un ordinateur virtuel

>S'applique à : Windows Server 2016

Cet article explique comment déplacer un ordinateur virtuel en procédant à une migration dynamique sans utiliser le clustering de basculement. Une migration dynamique déplace les ordinateurs virtuels en cours d’exécution entre les hôtes Hyper-V sans temps d’arrêt perceptible.   
  
Pour pouvoir effectuer cette opération, vous avez besoin des éléments suivants :   

- Un compte d’utilisateur qui est membre du groupe administrateurs Hyper-V local ou du groupe Administrateurs sur les ordinateurs source et de destination. 
  
- Le rôle Hyper-V dans Windows Server 2016 ou Windows Server 2012 R2 est installé sur les serveurs source et de destination et configuré pour les migrations dynamiques. Vous pouvez effectuer une migration dynamique entre des ordinateurs hôtes exécutant Windows Server 2016 et Windows Server 2012 R2 si la machine virtuelle est au moins la version 5.

    Pour obtenir des instructions de mise à niveau de version, consultez [mettre à niveau la version de machine virtuelle dans Hyper-V sur Windows 10 ou Windows Server 2016](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Pour obtenir des instructions d’installation, consultez [configurer des hôtes pour la migration dynamique](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md).

- Les outils de gestion Hyper-V installés sur un ordinateur exécutant Windows Server 2016 ou Windows 10, à moins que les outils ne soient installés sur le serveur source ou de destination et que vous les exécutiez à partir de là.  
   
## <a name="use-hyper-v-manager-to-move-a-running-virtual-machine"></a>Utiliser le Gestionnaire Hyper-V pour déplacer un ordinateur virtuel en cours d’exécution  
  
1.  Ouvrez le Gestionnaire Hyper-V. (À partir de Gestionnaire de serveur, cliquez sur **outils** >>**Gestionnaire Hyper-V**.)  
  
2.  Dans le volet de navigation, sélectionnez l’un des serveurs. (S’il n’est pas listé, cliquez avec le bouton droit sur **Gestionnaire Hyper-V**, cliquez sur **se connecter au serveur**, tapez le nom du serveur, puis cliquez sur **OK**. Répétez l’opération pour ajouter d’autres serveurs.)  
  
3.  Dans le volet **machines virtuelles** , cliquez avec le bouton droit sur l’ordinateur virtuel, puis cliquez sur **déplacer**. L’Assistant déplacement s’ouvre. 
  
4.  Utilisez les pages de l’Assistant pour choisir le type de déplacement, le serveur de destination et les options.
  
5.  Dans la page **Résumé** , passez vos choix en revue, puis cliquez sur **Terminer**.  

## <a name="use-windows-powershell-to-move-a-running-virtual-machine"></a>Utiliser Windows PowerShell pour déplacer un ordinateur virtuel en cours d’exécution
  
L’exemple suivant utilise l’applet de commande Move-VM pour déplacer un ordinateur virtuel nommé *LMTest* vers un serveur de destination nommé *TestServer02* et déplace les disques durs virtuels et autre fichier, tels que les points de contrôle et les fichiers de pagination intelligente, vers le *D:\LMTest* dans le répertoire du serveur de destination.  
  
```  
PS C:\> Move-VM LMTest TestServer02 -IncludeStorage -DestinationStoragePath D:\LMTest  
```  
  
## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="failed-to-establish-a-connection"></a>Échec de l’établissement d’une connexion 

Si vous n’avez pas configuré la délégation avec restriction, vous devez vous connecter au serveur source avant de pouvoir déplacer un ordinateur virtuel. Si vous ne le faites pas, la tentative d’authentification échoue, une erreur se produit et le message suivant s’affiche :  
  
«Échec de l’opération de migration d’ordinateur virtuel au niveau de la source de migration.  
Échec de l’établissement d’une connexion avec le nom de l' *ordinateur*hôte : Aucune information d’identification n’est disponible dans le package de sécurité 0x8009030E.»
  
 Pour résoudre ce problème, connectez-vous au serveur source et réessayez l’opération de déplacement. Pour éviter d’avoir à vous connecter à un serveur source avant d’effectuer une migration dynamique, configurez la délégation avec restriction. Vous aurez besoin d’informations d’identification d’administrateur de domaine pour configurer la délégation avec restriction. Pour obtenir des instructions, consultez [configurer des hôtes pour la migration dynamique](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md). 
 
 ### <a name="failed-because-the-host-hardware-isnt-compatible"></a>Échec, car le matériel hôte n’est pas compatible
 
 Si une machine virtuelle n’a pas de compatibilité avec le processeur activée et possède un ou plusieurs instantanés, le déplacement échoue si les ordinateurs hôtes ont des versions de processeur différentes. Une erreur se produit et le message suivant s’affiche :
 
la machine virtuelle @no__t 0The ne peut pas être déplacée vers l’ordinateur de destination. Le matériel sur l’ordinateur de destination n’est pas compatible avec la configuration matérielle requise de cet ordinateur virtuel. **
 
 Pour résoudre ce problème, arrêtez la machine virtuelle et activez le paramètre de compatibilité du processeur.
 
1. Dans le Gestionnaire Hyper-V, dans le volet **ordinateurs virtuels** , cliquez avec le bouton droit sur l’ordinateur virtuel, puis cliquez sur paramètres.
2. Dans le volet de navigation, développez **processeurs** , puis cliquez sur **compatibilité**.
3. Cochez la case **migrer vers un ordinateur avec une version de processeur différente**.
4. Cliquez sur **OK**.
 
   Pour utiliser Windows PowerShell, utilisez l’applet de commande [Set-VMProcessor](https://technet.microsoft.com/library/hh848533.aspx) :
 
   ```
   PS C:\> Set-VMProcessor TestVM -CompatibilityForMigrationEnabled $true
   ```