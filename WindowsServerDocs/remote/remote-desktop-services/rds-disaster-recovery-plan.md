---
title: Créer votre plan de récupération d’urgence
description: Découvrez comment créer un plan de récupération d’urgence pour votre déploiement des services Bureau à distance.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 05/05/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: e7bfe19258662a8e334ea0476689d8e860bfc8e5
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63743886"
---
# <a name="create-your-disaster-recovery-plan-for-rds"></a>Créer votre plan de récupération d’urgence pour les services Bureau à distance

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Vous pouvez créer un plan de récupération d’urgence dans Azure Site Recovery pour automatiser le processus de basculement. Ajoutez toutes les machines virtuelles du composant des services Bureau à distance au plan de récupération.

Procédez comme suit dans Azure pour créer votre plan de récupération :

1. Ouvrez le coffre Azure Site Recovery dans le portail Azure, puis cliquez sur **Plans de récupération**.
2. Cliquez sur **Créer** et entrez le nom du plan.
3. Sélectionnez votre **Source** et votre **Cible**. La cible est soit un site secondaire des services Bureau à distance, soit Azure.
4. Sélectionnez les machines virtuelles qui hébergent vos composants des services Bureau à distance, puis cliquez sur **OK**.

Les sections suivantes fournissent des informations supplémentaires sur la création de plans de récupération pour les différents types de déploiement des services Bureau à distance.

## <a name="sessions-based-rds-deployment"></a>Déploiement de services Bureau à distance basé sur les sessions

Pour un déploiement basé sur les sessions de services Bureau à distance, regroupez les machines virtuelles dans l’ordre :

1. Groupe de basculement 1 : machine virtuelle hôte de la session
2. Groupe de basculement 2 : machine virtuelle de service Broker de connexion
3. Groupe de basculement 3 : machine virtuelle d’accès web

Votre plan doit ressembler à ceci : 

![Un plan de récupération d’urgence pour un déploiement des services Bureau à distance basé sur les sessions](media/rds-asr-session-drplan.png)

## <a name="pooled-desktops-rds-deployment"></a>Déploiement des services Bureau à distance regroupés en pool de postes de travail

Pour un déploiement des services Bureau à distance avec des postes de travail regroupés en pool, regroupez les machines virtuelles dans l’ordre d’apparition, en ajoutant les étapes manuelles et les scripts.

1. Groupe de basculement 1 : machine virtuelle de service Broker de connexion des services Bureau à distance
2. Groupe 1 action manuelle : mise à jour du DNS

   Exécutez PowerShell avec élévation de privilèges sur la machine virtuelle du service Broker de connexion. Exécutez la commande suivante et attendez quelques minutes pour vous assurer que le DNS est mis à jour avec la nouvelle valeur :

   ```
   ipconfig /registerdns
   ```
3. Script Groupe 1 - Ajouter des hôtes de virtualisation

   Modifiez le script ci-dessous à exécuter pour chaque hôte de virtualisation dans le cloud. En général, une fois que vous avez ajouté un hôte de virtualisation à un service Broker de connexion, vous devez redémarrer l’ordinateur hôte. Assurez-vous que l’hôte n’a pas de redémarrage en attente avant l’exécution du script, sans quoi il échouera.

   ```
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. Groupe de basculement 2 : machine virtuelle modèle
5. Script 1 Groupe 2 - Désactiver la machine virtuelle modèle
   
   La machine virtuelle modèle démarre une fois récupérée sur le site secondaire, mais il s’agit d’une machine virtuelle préparée avec Sysprep et elle ne peut pas démarrer complètement. Les services Bureau à distance exigent également que la machine virtuelle soit arrêtée pour pouvoir créer une configuration de pool de machines virtuelles. Par conséquent, nous devons l’arrêter. Si vous avez un seul serveur VMM, le nom de la machine virtuelle modèle est le même sur le serveur principal et sur le serveur secondaire. Pour cette raison, nous utilisons l’ID de machine virtuelle comme spécifié par la variable *Context* dans le script ci-dessous. Si vous avez plusieurs modèles, désactivez-les tous.

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. Script 2 Groupe 2 - Supprimer les machines virtuelles regroupées en pool

   Vous devez supprimer les machines virtuelles regroupées en pool sur le site principal dans le service Broker de connexion afin que de nouvelles machines virtuelles puissent être créées sur le site secondaire. Dans ce cas, vous devez spécifier l’hôte exact sur lequel créer la machine virtuelle regroupée dans le pool. Notez que ceci supprimera les machines virtuelles uniquement dans la collection.

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName Win8Desktops; 
   Foreach($vm in $desktops){
      Remove-RDVirtualDesktopFromCollection -CollectionName Win8Desktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   ```
7. Action manuelle du groupe 2 - Affecter un nouveau modèle

   Vous devez affecter le nouveau modèle au service Broker de connexion pour la collection afin de créer de nouvelles machines virtuelles regroupées en pool sur le site de récupération. Accédez au service Broker de connexion des services Bureau à distance et identifiez la collection. Modifiez les propriétés et spécifiez une nouvelle image de machine virtuelle comme modèle.
8. Script 3 Groupe 2 - Recréer toutes les machines virtuelles regroupées en pools

   Recréez les machines virtuelles regroupées en pools sur le site de récupération via le service Broker de connexion. Dans ce cas, vous devez spécifier l’hôte exact sur lequel créer la machine virtuelle regroupée dans le pool.

   Le nom de machine virtuelle dans le pool doit être unique, avec préfixe et suffixe. Si le nom de la machine virtuelle existe déjà, le script échoue. En outre, si les machines virtuelles côté principal sont numérotées de 1 à 5, la numérotation du site de récupération reprendra à partir de 6.

   ```powershell
   ipmo RemoteDesktop; 
   Add-RDVirtualDesktopToCollection -CollectionName Win8Desktops -VirtualDesktopAllocation @{"RDVH1.contoso.com" = 1} 
   ```
9. Groupe de basculement 3 : machine virtuelle d’accès Web et de serveur de passerelle

Le plan de récupération doit ressembler à ceci :

![Un plan de récupération d’urgence pour un déploiement des services Bureau à distance avec des postes de travail regroupés en pools](media/rds-asr-pooled-drplan.png)

## <a name="personal-desktops-rds-deployment"></a>Déploiement des services Bureau à distance sur postes de travail personnels

Pour un déploiement des services Bureau à distance avec des postes de travail personnels, regroupez les machines virtuelles dans l’ordre d’apparition, en ajoutant les étapes manuelles et les scripts.

1. Groupe de basculement 1 : machine virtuelle de service Broker de connexion des services Bureau à distance
2. Groupe 1 action manuelle : mise à jour du DNS

   Exécutez PowerShell avec élévation de privilèges sur la machine virtuelle du service Broker de connexion. Exécutez la commande suivante et attendez quelques minutes pour vous assurer que le DNS est mis à jour avec la nouvelle valeur :

   ```
   ipconfig /registerdns
   ```
3. Script Groupe 1 - Ajouter des hôtes de virtualisation
      
   Modifiez le script ci-dessous à exécuter pour chaque hôte de virtualisation dans le cloud. En général, une fois que vous avez ajouté un hôte de virtualisation à un service Broker de connexion, vous devez redémarrer l’ordinateur hôte. Assurez-vous que l’hôte n’a pas de redémarrage en attente avant l’exécution du script, sans quoi il échouera.

   ```powershell
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. Groupe de basculement 2 : machine virtuelle modèle
5. Script 1 Groupe 2 - Désactiver la machine virtuelle modèle
   
   La machine virtuelle modèle démarre une fois récupérée sur le site secondaire, mais il s’agit d’une machine virtuelle préparée avec Sysprep et elle ne peut pas démarrer complètement. Les services Bureau à distance exigent également que la machine virtuelle soit arrêtée pour pouvoir créer une configuration de pool de machines virtuelles. Par conséquent, nous devons l’arrêter. Si vous avez un seul serveur VMM, le nom de la machine virtuelle modèle est le même sur le serveur principal et sur le serveur secondaire. Pour cette raison, nous utilisons l’ID de machine virtuelle comme spécifié par la variable *Context* dans le script ci-dessous. Si vous avez plusieurs modèles, désactivez-les tous.

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. Groupe de basculement 3 - Machines virtuelles personnelles
7. Script 1 Groupe 3 - Supprimer les machines virtuelles personnelles et les ajouter

   Supprimez les machines virtuelles personnelles du site principal du service Broker de connexion afin que de nouvelles machines virtuelles puissent être créées sur le site secondaire. Vous devez extraire les affectations de machines virtuelles et ajouter à nouveau les machines virtuelles au service Broker de connexion avec le hachage des affectations. Ceci ne fait que supprimer les machines virtuelles personnelles de la collection et les ajouter de nouveau. L’allocation des postes de travail personnels est exportée et réimportée dans la collection.

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName CEODesktops; 
   Export-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 

   Foreach($vm in $desktops){
     Remove-RDVirtualDesktopFromCollection -CollectionName CEODesktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   
   Import-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 
   ```
8. Groupe de basculement 3 : machine virtuelle d’accès Web et de serveur de passerelle

Votre plan doit ressembler à ceci : 

![Un plan de récupération d’urgence pour le déploiement des services Bureau à distance avec des postes personnels](media/rds-asr-personal-desktops-drplan.png)
