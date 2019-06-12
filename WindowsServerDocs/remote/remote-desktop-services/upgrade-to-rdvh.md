---
title: La mise à niveau de votre hôte de virtualisation des services Bureau à distance vers Windows Server 2016
description: Cet article décrit comment mettre à niveau vos déploiements de Services Bureau à distance existants vers Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5aed8ba7-f541-4416-b01c-4d3b1712e2b1
author: spatnaik
manager: scottman
ms.openlocfilehash: 9e624517e5e7910a32a68d1ebc38b3f8d5ab8459
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805208"
---
# <a name="upgrading-your-remote-desktop-virtualization-host-to-windows-server-2016"></a>La mise à niveau de votre hôte de virtualisation des services Bureau à distance vers Windows Server 2016

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Prise en charge des mises à niveau du système d’exploitation avec le rôle Services Bureau à distance est installé
Mises à niveau vers Windows Server 2016 sont pris en charge uniquement à partir de Windows Server 2012 R2 et Windows Server 2016 TP5.

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-locally"></a>Serveurs hôte de virtualisation Bureau à distance dans le déploiement dans lequel les machines virtuelles sont stockées localement
Ces serveurs doivent être mis à niveau tous en même temps. Suivez les étapes suivantes pour mettre à niveau :

1. Déconnecter tous les utilisateurs.
1. Activer ou désactiver l’enregistrement de toutes les machines virtuelles sur chaque hôte. 
1. Mettre à niveau les serveurs vers Windows Server 2016. 
1. Toutes les collections doivent être disponible et fonctionnelle après que les mises à niveau sont terminées.      

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-in-cluster-shared-volumes-csv"></a>Serveurs hôte de virtualisation Bureau à distance dans le déploiement où les machines virtuelles sont stockées dans des Volumes CSV (Cluster Shared) 

1. Déterminer une stratégie de mise à niveau où certains des serveurs RDVH sont mis à niveau et certaines continuera à héberger des machines virtuelles sur Windows Server 2012 R2.  
2. Isoler un ou plusieurs des serveurs RDVH, ciblés pour l’évaluation initiale de mise à niveau, en effectuant une migration toutes les machines virtuelles à d’autres « ne pas à être encore mis à niveau » serveurs RDVH qui resteront partie du cluster d’origine 2012 R2.
    1. Ouvrez le Gestionnaire du Cluster de basculement. 
    1. Cliquez sur **Rôles**. 
    1. Sélectionnez un ou plusieurs machines virtuelles. Avec le bouton droit pour ouvrir le menu contextuel. 
    1. Cliquez sur **déplacer** et choisissez **Live** ou **Migration rapide** pour déplacer les machines virtuelles à un ou plusieurs des serveurs hôtes de virtualisation Bureau à distance qui ne font pas partie de la mise à niveau initiale. Utilisez **Live** ou **rapide** Migration en fonction de facteurs tels que la compatibilité du matériel ou des exigences en ligne. 
3. Supprimez les serveurs RDVH, préparés pour la mise à niveau à partir du cluster d’origine. 
4. Mettre à niveau les serveurs RDVH isolés. 
5. Une fois que les serveurs RDVH ciblés ont été mis à niveau, créez un nouveau cluster et le CSV, qui doit se trouver sur un volume SAN totalement différent.
6. Joignez tous les serveurs RDVH mis à niveau vers le nouveau cluster. 
7. Créer une structure de dossiers dans le nouveau fichier CSV qui reproduit la structure de dossiers existante dans le fichier CSV existant. Cela inclut les dossiers de collection et sous-dossiers de haut niveau de chaque machine virtuelle. 
8. À partir de différents dossiers de collecte des machines virtuelles sur le fichier CSV d’origine, copier le dossier /IMGS et le contenu sur les nouveaux dossiers de collection dans les mêmes emplacements sur le nouveau volume partagé de cluster. 
9. Sur l’ordinateur RDVH source, utilisez le Gestionnaire du Cluster pour supprimer la configuration de la machine virtuelle pour la haute disponibilité :
    1. Lancez le Gestionnaire de Cluster. 
    1. Cliquez sur **Rôles**. 
    1. Cliquez sur les objets de la machine virtuelle, puis cliquez sur **supprimer**. 
10. Sur l’un des serveurs RDVH non mis à niveau, utilisez le Gestionnaire Hyper-V pour déplacer toutes les machines virtuelles à un des serveurs RDVH mis à niveau et nouvelle Cluster CSV :
    1. Ouvrez le Gestionnaire Hyper-V. 
    2. Sélectionnez un des serveurs RDVH non mis à niveau. 
    3. Cliquez sur un des machines virtuelles à déplacer, puis cliquez sur **déplacer**. 
    4. Choisissez **déplacer l’ordinateur virtuel**, puis cliquez sur **suivant**. 
    5. Nommez le ciblé mis à niveau RDVH du serveur sur le **spécifier l’ordinateur de Destination** page, puis cliquez sur **suivant**. 
    6. Choisissez **déplacer des données de la machine virtuelle vers un emplacement unique**, puis cliquez sur **suivant**. 
    7. Accédez à l’emplacement de destination. 
       > [!IMPORTANT]
       > Vérifiez que ce chemin d’accès est dans un dossier vide pour la machine virtuelle spécifique. 

       > [!NOTE]
       > Comme mentionné, vous devez avoir déjà créé un nouveau dossier de destination sub avant cette étape. La boîte de dialogue Sélectionner un dossier vous autorisera pas à créer un sous-dossier dans cette étape. 
    
       Cliquez sur **Suivant**, puis sur **Terminé**. 
11. Une fois que les machines virtuelles sont déplacées, ajoutez-les en tant que cluster **haute disponibilité** objets :
     1. Ouvrez le Gestionnaire du Cluster de basculement sur un serveur hôte de virtualisation Bureau à distance mis à niveau. 
     1. Cliquez sur le **rôles** nœud, puis cliquez sur **configurer un rôle**. Cliquez sur **suivant** sur le **Démarrer** page de l’Assistant haute disponibilité. 
     1. Choisissez **Machine virtuelle** dans la liste des rôles disponibles, puis cliquez sur **suivant**. Une liste de machines virtuelles qui ne sont pas configurés s’affiche. 
     1. Sélectionnez toutes les machines virtuelles. Cliquez sur **suivant** puis cliquez sur **suivant** à nouveau sur la page de confirmation pour démarrer la tâche de configuration.  
12. Une fois que vous avez déplacé toutes les machines virtuelles, mettre à niveau les serveurs RDVH restants. Utilisez les étapes ci-dessus pour l’équilibrage des emplacements de machine virtuelle comme il convient.

> [!NOTE]  
> Les serveurs Hyper-V hétérogènes dans un cluster ne sont pas pris en charge. 
