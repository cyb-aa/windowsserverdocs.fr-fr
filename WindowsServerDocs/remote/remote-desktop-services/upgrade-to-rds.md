---
title: Mise à niveau des déploiements des services Bureau à distance vers Windows Server 2016
description: Cet article explique comment mettre à niveau vos déploiements existants des services Bureau à distance vers Windows Server 2016.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 03/20/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f7b1f1f6-57c8-40ab-a235-e36240dcc1f8
author: spatnaik
manager: scottman
notes: https://social.technet.microsoft.com/wiki/contents/articles/22069.remote-desktop-services-upgrade-guidelines-for-windows-server-2012-r2.aspx
ms.openlocfilehash: 29648db89b61a9d22aad6d5aa814cfe7f425a970
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403879"
---
# <a name="upgrading-your-remote-desktop-services-deployments-to-windows-server-2016"></a>Mise à niveau des déploiements des services Bureau à distance vers Windows Server 2016

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Mises à niveau des systèmes d’exploitation pris en charge avec le rôle Services Bureau à distance installé
Les mises à niveau vers Windows Server 2016 sont uniquement prises en charge à partir de Windows Server 2012 R2 et Windows Server 2012.

## <a name="flow-for-deployment-upgrades"></a>Flux des mises à niveau de déploiement
Afin de réduire au maximum le temps d’arrêt, il est préférable de suivre les étapes ci-dessous :

1. Les **serveurs du service Broker pour les connexions Bureau à distance** doivent être les premiers à être mis à niveau. Si le programme d’installation actif/actif existe dans le déploiement, supprimez tous les serveurs du déploiement sauf un, et effectuez une mise à niveau sur place. Procédez aux mises à niveau sur les autres serveurs du service Broker pour les connexions Bureau à distance en mode hors connexion, puis rajoutez-les au déploiement. Le déploiement n’est pas disponible pendant la mise à niveau des serveurs du service Broker pour les connexions Bureau à distance.

   > [!NOTE] 
   > Il est obligatoire de mettre à niveau les serveurs du service Broker pour les connexions Bureau à distance. Nous ne prenons pas en charge les serveurs du service Broker pour les connexions Bureau à distance de Windows Server 2012 R2 dans un déploiement mixte constitué également de serveurs Windows Server 2016. Aussitôt que le ou les serveurs du service Broker pour les connexions Bureau à distance exécutent Windows Server 2016, le déploiement est fonctionnel, même si le reste des serveurs du déploiement exécutent toujours Windows Server 2012 R2.

2. Les **serveurs de licences des services Bureau à distance** doivent être mis à niveau avant que vous ne procédiez à la mise à niveau de vos serveurs Hôte de session Bureau à distance.
   > [!NOTE] 
   > Les serveurs de licences des services Bureau à distance de Windows Server 2012 et 2012 R2 fonctionneront avec les déploiements de Windows Server 2016, mais ils peuvent traiter seulement des licences d’accès client à partir de Windows Server 2012 R2 et antérieur. Ils ne peuvent pas utiliser les licences d’accès client de Windows Server 2016. Consultez [Gérer les licences de votre déploiement Services Bureau à distance avec des licences d’accès client (CAL)](rds-client-access-license.md) pour plus d’informations sur les serveurs de licences des services Bureau à distance.

3. Les **serveurs Hôte de session Bureau à distance** peuvent ensuite être mis à niveau. Pour éviter les temps d’arrêt lors de la mise à niveau, l’administrateur peut scinder le groupe des serveurs et effectuer la mise à niveau en 2 étapes, comme expliqué ci-dessous. L’ensemble sera opérationnel une fois la mise à niveau effectuée. Pour la mise à niveau, utilisez les étapes décrites dans [Mettre à niveau les serveurs Hôte de session des services Bureau à distance vers Windows Server 2016](upgrade-to-rdsh.md).

4. Les **serveurs Hôte de virtualisation des services Bureau à distance** peuvent ensuite être mis à niveau. Pour la mise à niveau, utilisez les étapes décrites dans [Mettre à niveau les serveurs Hôte de virtualisation des services Bureau à distance vers Windows Server 2016](upgrade-to-rdvh.md).

5. Les **serveurs d’accès Bureau à distance par le web** peuvent être mis à niveau à tout moment.
   > [!NOTE]
   > La mise à niveau du site web des services Bureau à distance peut réinitialiser les propriétés IIS (comme les fichiers de configuration). Afin de ne pas perdre vos modifications, prenez des notes ou faites des copies des personnalisations réalisées sur le site web des services Bureau à distance dans IIS.

   > [!NOTE] 
   > Les serveurs d’accès Bureau à distance par le web de Windows Server 2012 et 2012 R2 fonctionneront avec les déploiements de Windows Server 2016.

6. Les **serveurs de passerelle Bureau à distance** peuvent être mis à niveau à tout moment.
   > [!NOTE]
   > Windows Server 2016 n’inclut pas les stratégies de protection d’accès réseau (NAP) : elles devront être supprimées. Pour supprimer les stratégies voulues, la solution la plus simple consiste à utiliser l’Assistant Mise à niveau. S’il existe des stratégies NAP à supprimer, la mise à niveau se bloque et crée un fichier texte sur le bureau qui inclut ces stratégies particulières. Pour gérer les stratégies NAP, ouvrez l’outil Serveur NPS (Network Policy Server). Après les avoir supprimées, cliquez sur **Actualiser** dans l’outil d’installation pour poursuivre le processus de mise à niveau. 

   > [!NOTE] 
   > Les serveurs de passerelle Bureau à distance de Windows Server 2012 et 2012 R2 fonctionneront avec les déploiements de Windows Server 2016.

## <a name="vdi-deployment--supported-guest-os-upgrade"></a>Déploiement VDI – Mise à niveau des systèmes d’exploitation invités pris en charge
Les administrateurs disposent des options suivantes pour la mise à niveau des collections de machines virtuelles :

### <a name="upgrade-managed-shared-vm-collections"></a>Mettre à niveau les collections de machines virtuelles partagées managées 
Les administrateurs doivent créer des modèles de machine virtuelle avec la version du système d’exploitation souhaitée, et l’utiliser pour corriger toutes les machines virtuelles dans le pool. 

Nous prenons en charge les scénarios de mise à jour corrective suivants :
- Windows 7 SP1 peut faire l’objet d’un correctif pour Windows 8 ou Windows 8.1
- Windows 8 peut faire l’objet d’un correctif pour Windows 8.1
- Windows 8.1 peut faire l’objet d’un correctif pour Windows 10

### <a name="upgrade-unmanaged-shared-vm-collections"></a>Mettre à niveau les collections de machines virtuelles partagées non managées 
Les utilisateurs finaux ne peuvent pas mettre à niveau leurs bureaux personnels. Les administrateurs doivent procéder à la mise à niveau. Les étapes précises restent à déterminer.