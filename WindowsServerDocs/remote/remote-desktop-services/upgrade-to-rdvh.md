---
title: Mise à niveau de votre hôte de virtualisation des services Bureau à distance vers Windows Server 2016
description: Cet article explique comment mettre à niveau vos déploiements existants des services Bureau à distance vers Windows Server 2016.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 08/01/2016
ms.topic: article
ms.assetid: 5aed8ba7-f541-4416-b01c-4d3b1712e2b1
author: spatnaik
manager: scottman
ms.openlocfilehash: 7bbf5f6a81a18303d4f9f4b02a1b8dead3c9a53a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857112"
---
# <a name="upgrading-your-remote-desktop-virtualization-host-to-windows-server-2016"></a>Mise à niveau de votre hôte de virtualisation des services Bureau à distance vers Windows Server 2016

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Mises à niveau des systèmes d’exploitation pris en charge avec le rôle Services Bureau à distance installé
Les mises à niveau vers Windows Server 2016 sont uniquement prises en charge à partir de Windows Server 2012 R2 et Windows Server 2016 TP5.

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-locally"></a>Serveurs Hôte de virtualisation des services Bureau à distance dans le déploiement où les machines virtuelles sont stockées localement
Ces serveurs doivent être mis à niveau tous en même temps. Effectuez les étapes suivantes pour mettre à niveau :

1. Déconnectez tous les utilisateurs.
1. Désactivez ou enregistrez toutes les machines virtuelles sur chaque hôte. 
1. Mettez à niveau les serveurs vers Windows Server 2016. 
1. Toutes les collections doivent être disponibles et opérationnelles à l’issue des mises à niveau.      

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-in-cluster-shared-volumes-csv"></a>Serveurs Hôte de virtualisation des services Bureau à distance dans le déploiement où les machines virtuelles sont stockées dans des volumes partagés de cluster (CSV) 

1. Déterminez une stratégie de mise à niveau dans laquelle certains serveurs Hôte de virtualisation des services Bureau à distance (RDVH) sont mis à niveau tandis que d’autres continuent d’héberger des machines virtuelles sur Windows Server 2012 R2.  
2. Isolez un ou plusieurs serveurs RDVH prévus pour la première série de mise à niveau. Pour cela, procédez à une migration de toutes les machines virtuelles vers les autres serveurs RDVH à ne pas encore mettre à niveau et qui feront toujours partie du cluster 2012 R2 d’origine.
    1. Ouvrez le Gestionnaire du cluster de basculement. 
    1. Cliquez sur **Rôles**. 
    1. Sélectionnez une ou plusieurs machines virtuelles. Cliquez avec le bouton droit pour ouvrir le menu contextuel. 
    1. Cliquez sur **Déplacer** et choisissez **Migration rapide** ou **Dynamique** pour déplacer les machines virtuelles vers un ou plusieurs serveurs Hôte de virtualisation des services Bureau à distance qui ne font pas partie de la mise à niveau initiale. Utilisez la migration **Dynamique** ou **Rapide** en fonction de facteurs, tels que la compatibilité matérielle ou les exigences en ligne. 
3. Supprimez les serveurs RDVH, préparés pour la mise à niveau, à partir du cluster d’origine. 
4. Mettez à niveau les serveurs RDVH isolés. 
5. Lorsque les serveurs RDVH ciblés ont été correctement mis à niveau, créez sur un tout autre volume SAN un cluster avec un volume partagé de cluster (CSV).
6. Regroupez sur le nouveau cluster tous les serveurs RDVH mis à niveau. 
7. Créez dans le nouveau CSV une structure de dossiers reproduisant la structure de dossiers actuelle dans le CSV existant. Les dossiers de collection et les sous-dossiers de niveau supérieur de chaque machine virtuelle seront inclus. 
8. À partir des différents dossiers de collection des machines virtuelles situés sur le CSV d’origine, copiez le dossier /IMGS et son contenu dans les nouveaux dossiers de collection, aux mêmes emplacements sur le nouveau volume partagé de cluster. 
9. Sur la machine RDVH source, supprimez la configuration de la machine virtuelle pour la haute disponibilité en utilisant le Gestionnaire du cluster :
    1. Lancez le Gestionnaire du cluster. 
    1. Cliquez sur **Rôles**. 
    1. Cliquez avec le bouton droit sur les objets de machine virtuelle, puis cliquez sur **Supprimer**. 
10. Sur l’un des serveurs RDVH non mis à niveau, utilisez le Gestionnaire Hyper-V pour déplacer toutes les machines virtuelles sur un des serveurs RDVH mis à niveau et le nouveau volume partagé de cluster :
    1. Ouvrez le Gestionnaire Hyper-V. 
    2. Sélectionnez un des serveurs RDVH non mis à niveau. 
    3. Cliquez avec le bouton droit sur une des machines virtuelles à déplacer, puis cliquez sur **Déplacer**. 
    4. Choisissez **Déplacer la machine virtuelle**, puis cliquez sur **Suivant**. 
    5. Indiquez dans la page **Spécifier la machine de destination** le nom du serveur RDVH mis à niveau qui est ciblé, puis cliquez sur **Suivant**. 
    6. Choisissez **Déplacer les données de la machine virtuelle vers un seul emplacement**, puis cliquez sur **Suivant**. 
    7. Accédez à l’emplacement de destination. 
       > [!IMPORTANT]
       > Vérifiez que ce chemin mène à un dossier vide pour la machine virtuelle donnée. 

       > [!NOTE]
       > Comme cela a été précisé, vous devrez préalablement avoir créé un nouveau sous-dossier de destination avant d’aborder cette étape. La boîte de dialogue Sélectionner un dossier ne vous permet pas de créer un sous-dossier dans cette étape. 
    
       Cliquez sur **Suivant**, puis sur **Terminé**. 
11. Une fois les machines virtuelles déplacées, ajoutez-les en tant qu’objets **Haute disponibilité** de cluster :
     1. Ouvrez le Gestionnaire du cluster de basculement sur un serveur Hôte de virtualisation des services Bureau à distance mis à niveau. 
     1. Cliquez avec le bouton droit sur le nœud **Rôles**, puis cliquez sur **Configurer un rôle**. Cliquez sur **Suivant** dans la page de **Démarrage** de l’Assistant Haute disponibilité. 
     1. Choisissez **Machine virtuelle** dans la liste des rôles disponibles, puis cliquez sur **Suivant**. Une liste de machines virtuelles non configurées s’affiche. 
     1. Sélectionnez toutes les machines virtuelles. Cliquez sur **Suivant**, puis à nouveau sur **Suivant** dans la page de confirmation pour démarrer la tâche de configuration.  
12. Lorsque vous avez déplacé toutes les machines virtuelles, mettez à niveau les serveurs RDVH restants. Utilisez les étapes ci-dessus pour équilibrer les emplacements de machine virtuelle, si nécessaire.

> [!NOTE]  
> Les serveurs Hyper-V hétérogènes dans un cluster ne sont pas pris en charge. 
