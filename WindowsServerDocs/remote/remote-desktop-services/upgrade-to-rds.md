---
title: La mise à niveau de vos déploiements de Services Bureau à distance vers Windows Server 2016
description: Cet article décrit comment mettre à niveau vos déploiements de Services Bureau à distance existants vers Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f683a7d9346494e7f1fb6faf716ca9c90cfef8d3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875750"
---
# <a name="upgrading-your-remote-desktop-services-deployments-to-windows-server-2016"></a>La mise à niveau de vos déploiements de Services Bureau à distance vers Windows Server 2016

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Prise en charge des mises à niveau du système d’exploitation avec le rôle Services Bureau à distance est installé
Mises à niveau vers Windows Server 2016 sont pris en charge uniquement à partir de Windows Server 2012 R2 et Windows Server 2016.

## <a name="flow-for-deployment-upgrades"></a>Flux des mises à niveau du déploiement
Afin de réduire le temps d’arrêt au minimum, il est préférable de suivre les étapes ci-dessous :

1. **Serveurs de service Broker pour les connexions Bureau à distance** doit être le premier à être mis à niveau. S’il existe le programme d’installation actif/actif dans le déploiement, supprimer un seul serveur du déploiement et effectuer une mise à niveau sur place. Effectuer des mises à niveau sur les autres serveurs du service Broker pour les connexions Bureau à distance en mode hors connexion et puis les ajouter au déploiement. Le déploiement ne sera pas disponible pendant la mise à niveau des serveurs de service Broker pour les connexions Bureau à distance.

   > [!NOTE] 
   > Il est obligatoire pour mettre à niveau les serveurs de service Broker pour les connexions Bureau à distance. Nous ne gèrent pas les serveurs Windows Server 2012 R2 RD Connection Broker dans un déploiement mixte avec les serveurs Windows Server 2016. Une fois que le service Broker pour les connexions Bureau à distance ou les serveurs exécutent Windows Server 2016 le déploiement sera fonctionnel, même si le reste des serveurs dans le déploiement sont en cours d’exécution Windows Server 2012 R2.

2. **Serveurs de licences des services Bureau à distance** doit être mis à niveau avant que vous mettez à niveau vos serveurs hôte de Session Bureau à distance.
   > [!NOTE] 
   > Les serveurs de licences Windows Server 2012 et 2012 R2 RD fonctionnera avec les déploiements de Windows Server 2016, mais ils ne peuvent traiter que les licences d’accès client à partir de Windows Server 2012 R2 et versions antérieures. Ils ne peuvent pas utiliser les licences d’accès client de Windows Server 2016. Consultez [la licence de votre déploiement des services Bureau à distance avec des licences d’accès client (CAL)](rds-client-access-license.md) pour plus d’informations sur les serveurs de licences bureau à distance.

3. **Serveurs hôtes de Session Bureau à distance** peuvent être mis à niveau ensuite. Pour éviter les temps au cours de la mise à niveau de l’administrateur peut se diviser les serveurs de mise à niveau en 2 étapes comme indiqué ci-dessous. Tout sera opérationnel une fois la mise à niveau. Pour mettre à niveau, utilisez les étapes décrites dans [serveurs de la mise à niveau de hôte de Session Bureau à distance vers Windows Server 2016](upgrade-to-rdsh.md).

4. **Serveurs hôtes de virtualisation des services Bureau à distance** peuvent être mis à niveau ensuite. Pour mettre à niveau, utilisez les étapes décrites dans [serveurs de la mise à niveau de hôte de virtualisation Bureau à distance vers Windows Server 2016](upgrade-to-rdvh.md).

5. **Serveurs d’accès Web de bureau à distance** peuvent être mis à niveau à tout moment.
   > [!NOTE]
   > La mise à niveau Web Bureau à distance peut réinitialiser les propriétés IIS (par exemple, des fichiers de configuration). Ne pas de perdre vos modifications, créer des notes ou des copies des personnalisations effectuées sur le site Web du Bureau à distance dans IIS.

   > [!NOTE] 
   > Windows Server 2012 et les serveurs d’accès Web de bureau à distance R2 2012 fonctionneront avec les déploiements de Windows Server 2016.

6. **Serveurs de passerelle Bureau à distance** peuvent être mis à niveau à tout moment.
   > [!NOTE]
   > Windows Server 2016 n’inclut pas les stratégies de Protection d’accès réseau (NAP) : ils devront être supprimées. Pour supprimer les stratégies appropriées, le plus simple consiste en exécutant l’Assistant Mise à niveau. Si vous devez supprimer toutes les stratégies NAP n’y, la mise à niveau bloquera et créez un fichier texte sur le bureau qui inclut les stratégies spécifiques. Pour gérer les stratégies de protection d’accès réseau, ouvrez l’outil serveur NPS. Après avoir supprimé les, cliquez sur **Actualiser** dans l’outil d’installation pour poursuivre le processus de mise à niveau. 

   > [!NOTE] 
   > Windows Server 2012 et les serveurs de passerelle Bureau à distance de R2 2012 fonctionneront avec les déploiements de Windows Server 2016.

## <a name="vdi-deployment--supported-guest-os-upgrade"></a>Déploiement VDI – mise à niveau du système d’exploitation invité de prise en charge
Les administrateurs ont les options suivantes pour mettre à niveau des collections de la machine virtuelle :

### <a name="upgrade-managed-shared-vm-collections"></a>Mettre à niveau des collections de machine virtuelle partagée managé 
Les administrateurs doivent créer des modèles de machine virtuelle avec la version du système d’exploitation souhaitée et l’utiliser pour corriger toutes les machines virtuelles dans le pool. 

Nous prenons en charge les scénarios de mise à jour corrective suivants :
- Windows 7 SP1 peut être corrigé pour Windows 8 ou Windows 8.1
- Windows 8 peut être corrigé pour Windows 8.1
- Windows 8.1 peut être corrigé pour Windows 10

### <a name="upgrade-unmanaged-shared-vm-collections"></a>Mettre à niveau des collections de machine virtuelle partagées non gérés 
Les utilisateurs finaux ne peut pas mettre à niveau leurs bureaux personnels. Les administrateurs doivent effectuer la mise à niveau. Les étapes exactes sont toujours à déterminer.