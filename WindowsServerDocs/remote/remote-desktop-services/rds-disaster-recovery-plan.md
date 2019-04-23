---
title: Créer votre plan de récupération d'urgence
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
ms.openlocfilehash: 8ad759a73e4a0ce1dc28f2b8e8d80f4365895430
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879500"
---
# <a name="create-your-disaster-recovery-plan-for-rds"></a>Créer votre plan de récupération d’urgence pour les services Bureau à distance

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez créer un plan de récupération d’urgence dans Azure Site Recovery pour automatiser le processus de basculement. Ajouter toutes les machines virtuelles de composant des services Bureau à distance au plan de récupération.

Procédez comme suit dans Azure pour créer votre plan de récupération :

1. Ouvrez le coffre Azure Site Recovery dans le portail Azure, puis cliquez sur **Plans de récupération**.
2. Cliquez sur **créer** et entrez un nom pour le plan.
3. Sélectionnez votre **Source** et **cible**. La cible est un site des services Bureau à distance secondaire ou Azure.
4. Sélectionnez les machines virtuelles qui hébergent vos composants des services Bureau à distance, puis cliquez sur **OK**.

Les sections suivantes fournissent des informations supplémentaires sur la création de plans de récupération pour les différents types de déploiement des services Bureau à distance.

## <a name="sessions-based-rds-deployment"></a>Déploiement de services Bureau à distance basé sur des sessions

Pour un déploiement basé sur les sessions de services Bureau à distance, regrouper les machines virtuelles afin de leur apparition dans la séquence :

1. Groupe de basculement 1 : machine virtuelle hôte de Session
2. Groupe de basculement 2 : machine virtuelle de connexion Service Broker
3. Groupe de basculement 3 : machine virtuelle de l’accès Web

Votre plan ressemblera à ceci : 

![Un plan de récupération d’urgence pour un déploiement services Bureau à distance basé sur session](media/rds-asr-session-drplan.png)

## <a name="pooled-desktops-rds-deployment"></a>Déploiement des services Bureau à distance de bureaux

Pour un déploiement services Bureau à distance avec des bureaux, regrouper les machines virtuelles afin de leur apparition dans la séquence, ajout de scripts et des étapes manuelles.

1. Groupe de basculement 1 : service Broker pour les machines virtuelles de la connexion services Bureau à distance
2. Groupe 1 action manuelle - mise à jour DNS

   Exécutez PowerShell avec élévation de privilèges sur la machine virtuelle Broker de connexion. Exécutez la commande suivante et attendez quelques minutes sont nécessaires pour garantir que le DNS est mis à jour avec la nouvelle valeur :

   ```
   ipconfig /registerdns
   ```
3. Groupe 1 script - ajouter des hôtes de virtualisation

   Modifiez le script ci-dessous à exécuter pour chaque hôte de virtualisation dans le cloud. En général, une fois que vous ajoutez un hôte de virtualisation pour un agent de connexion, vous devez redémarrer l’ordinateur hôte. Assurez-vous que l’hôte n’a pas un redémarrage en attente avant l’exécution de script, sans quoi il échouera.

   ```
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. Groupe de basculement 2 : machine virtuelle du modèle
5. Groupe 2 script 1 - activer hors tension de la machine virtuelle du modèle
   
   Le modèle de machine virtuelle une fois récupérée sur le site secondaire démarre, mais il est une machine virtuelle de préparée avec Sysprep et ne peut pas démarrer complètement. Services Bureau à distance requiert également que la machine virtuelle soit arrêt pour créer une configuration de machine virtuelle regroupée à partir de celui-ci. Par conséquent, nous devons désactiver cette option. Si vous avez un seul serveur VMM, le nom de machine virtuelle du modèle est le même sur le serveur principal et secondaire. Pour cette raison, nous utilisons l’ID de machine virtuelle, comme spécifié par le *contexte* variable dans le script ci-dessous. Si vous avez plusieurs modèles, les désactiver tout.

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. Groupe 2 script 2 - supprimer des machines virtuelles regroupées existantes

   Vous devez supprimer les machines virtuelles regroupées sur le site principal à partir de l’agent de connexion afin de nouvelles machines virtuelles peuvent être créés sur le site secondaire. Dans ce cas, vous devez spécifier l’hôte exact dans lequel créer la machine virtuelle regroupée. Notez que Ceci supprimera les machines virtuelles dans uniquement à la collection.

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName Win8Desktops; 
   Foreach($vm in $desktops){
      Remove-RDVirtualDesktopFromCollection -CollectionName Win8Desktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   ```
7. Action manuelle du groupe 2 - affecter un nouveau modèle

   Vous devez affecter le nouveau modèle sur le répartiteur de connexion pour la collection afin de créer de nouvelles machines virtuelles regroupées sur le site de récupération. Accédez au répartiteur de connexion des services Bureau à distance et identifier la collection. Modifier les propriétés et spécifiez une nouvelle image de machine virtuelle en tant que son modèle.
8. Groupe 2 script 3 - de recréer tous les bureaux de machines virtuelles

   Recréez les machines virtuelles regroupées sur le site de récupération via le répartiteur de connexion. Dans ce cas, vous devez spécifier l’hôte exact dans lequel créer la machine virtuelle regroupée.

   Le nom de machine virtuelle regroupé doit être unique, à l’aide du préfixe et suffixe. Si le nom de la machine virtuelle existe déjà, le script échoue. En outre, si le côté principal des machines virtuelles sont numérotées de 1 à 5, la numérotation de site de récupération continuera à partir de 6.

   ```powershell
   ipmo RemoteDesktop; 
   Add-RDVirtualDesktopToCollection -CollectionName Win8Desktops -VirtualDesktopAllocation @{"RDVH1.contoso.com" = 1} 
   ```
9. Groupe de basculement 3 : accès Web et la machine virtuelle du serveur de passerelle

Le plan de récupération doit ressembler à ceci :

![Un plan de récupération d’urgence pour un déploiement services Bureau à distance avec des bureaux](media/rds-asr-pooled-drplan.png)

## <a name="personal-desktops-rds-deployment"></a>Déploiement des services Bureau à distance les bureaux personnels

Pour un déploiement services Bureau à distance avec les bureaux personnels, regrouper les machines virtuelles afin de leur apparition dans la séquence, ajout de scripts et des étapes manuelles.

1. Groupe de basculement 1 : service Broker pour les machines virtuelles de la connexion services Bureau à distance
2. Groupe 1 action manuelle - mise à jour DNS

   Exécutez PowerShell avec élévation de privilèges sur la machine virtuelle Broker de connexion. Exécutez la commande suivante et attendez quelques minutes sont nécessaires pour garantir que le DNS est mis à jour avec la nouvelle valeur :

   ```
   ipconfig /registerdns
   ```
3. Groupe 1 script - ajouter des hôtes de virtualisation
      
   Modifiez le script ci-dessous à exécuter pour chaque hôte de virtualisation dans le cloud. En général, une fois que vous ajoutez un hôte de virtualisation pour un agent de connexion, vous devez redémarrer l’ordinateur hôte. Assurez-vous que l’hôte n’a pas un redémarrage en attente avant l’exécution de script, sans quoi il échouera.

   ```powershell
   Broker - broker.contoso.com
   Virtualization host - VH1.contoso.com

   ipmo RemoteDesktop; 
   add-rdserver –ConnectionBroker broker.contoso.com –Role RDS-VIRTUALIZATION –Server VH1.contoso.com 
   ```
4. Groupe de basculement 2 : machine virtuelle du modèle
5. Groupe 2 script 1 - Turn off modèle de machine virtuelle
   
   Le modèle de machine virtuelle une fois récupérée sur le site secondaire démarre, mais il est une machine virtuelle de préparée avec Sysprep et ne peut pas démarrer complètement. Services Bureau à distance requiert également que la machine virtuelle soit arrêt pour créer une configuration de machine virtuelle regroupée à partir de celui-ci. Par conséquent, nous devons désactiver cette option. Si vous avez un seul serveur VMM, le nom de machine virtuelle du modèle est le même sur le serveur principal et secondaire. Pour cette raison, nous utilisons l’ID de machine virtuelle, comme spécifié par le *contexte* variable dans le script ci-dessous. Si vous avez plusieurs modèles, les désactiver tout.

   ```powershell
   ipmo virtualmachinemanager; 
   Foreach($vm in $VMsAsTemplate)
   {
      Get-SCVirtualMachine -ID $vm | Stop-SCVirtualMachine –Force
   } 
   ```
6. Groupe de basculement 3 - machines virtuelles personnelles
7. Groupe 3 script 1 : supprimer des machines virtuelles personnelles existants et ajoutez-les

   Supprimez les machines virtuelles personnelles sur le site principal de l’agent de connexion afin de nouvelles machines virtuelles peuvent être créés sur le site secondaire. Vous devez extraire les affectations de machines virtuelles et de rajouter les machines virtuelles sur le répartiteur de connexion avec le hachage des affectations. Cela sera uniquement supprimer les machines virtuelles personnelles de la collection et ajoutez-les de nouveau. L’allocation des bureaux personnelle est exportée et importée dans la collection.

   ```powershell
   ipmo RemoteDesktop
   $desktops = Get-RDVirtualDesktop -CollectionName CEODesktops; 
   Export-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 

   Foreach($vm in $desktops){
     Remove-RDVirtualDesktopFromCollection -CollectionName CEODesktops -VirtualDesktopName $vm.VirtualDesktopName –Force
   }
   
   Import-RDPersonalVirtualDesktopAssignment -CollectionName CEODesktops -Path ./Desktopallocations.txt -ConnectionBroker broker.contoso.com 
   ```
8. Groupe de basculement 3 : accès Web et la machine virtuelle du serveur de passerelle

Votre plan ressemblera à ceci : 

![Un plan de récupération d’urgence pour un déploiement de RDS bureaux personnels](media/rds-asr-personal-desktops-drplan.png)
