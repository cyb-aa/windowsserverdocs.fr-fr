---
title: Tableau sur la mise à niveau et la migration des rôles de serveur pour Windows Server 2016
description: Indique les rôles de serveur qui peuvent faire l’objet d’une mise à niveau ou d’une migration vers Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 10/05/2016
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7e031a64-b1e6-4cf6-994a-e7c575835f6a
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 9f8310baf659810d5d51587bafcc868c59ace61a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852050"
---
# <a name="server-role-upgrade-and-migration-matrix-for-windows-server-2016"></a>Tableau sur la mise à niveau et la migration des rôles de serveur pour Windows Server 2016

>S'applique à : Windows Server 2016

Le tableau suivant présente les options de mise à niveau et de migration des rôles de serveur pour le passage à Windows Server 2016. Pour accéder à des guides de migration des rôles individuels, rendez-vous sur la page [Migration de rôles et de fonctionnalités dans Windows Server](https://docs.microsoft.com/windows-server/get-started/migrate-roles-and-features). Pour plus d’informations sur l’installation et les mises à niveau, voir [Installation, mise à niveau et migration de Windows Server](https://docs.microsoft.com/windows-server/get-started/installation-and-upgrade).

|Rôle serveur|Mise à niveau possible depuis Windows Server 2012 R2 ?|Mise à niveau possible depuis Windows Server 2012 ?|Migration prise en charge?|Migration possible sans temps d’arrêt?|  
|-------------------|----------|--------------|--------------|----------|  
|Services de certificats Active Directory| Oui|    Oui|    Oui|    Non|
|Services de domaine Active Directory|  Oui|    Oui|    Oui|    Oui|
|Services de fédération Active Directory (AD FS)|  Non| Non| Oui|    Non (de nouveaux nœuds doivent être ajoutés à la batterie de serveurs)|
|Services AD LDS (Active Directory Lightweight Directory Services)|   Oui|    Oui|    Oui|    Oui|
|Services AD RMS (Active Directory Rights Management Services)|   Oui|    Oui|    Oui|    Non|
|Serveur DHCP|   Oui|    Oui|    Oui|    Oui|
|Serveur DNS|    Oui|    Oui|    Oui|    Non|
|Cluster de basculement|Oui, avec le processus de [mise à niveau propagée du système d’exploitation de cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade), qui inclut la mise en pause, le drainage et la suppression de nœuds, la mise à niveau vers Windows Server 2016 et le retour au cluster d’origine. Oui, lorsque le serveur est supprimé par le cluster lors de la mise à niveau, puis ajouté à un autre cluster.|Pas lorsque le serveur fait partie d’un cluster. Oui, lorsque le serveur est supprimé par le cluster lors de la mise à niveau, puis ajouté à un autre cluster.  |Oui|Pas pour les clusters de basculement Windows Server 2012. Oui, pour les clusters de basculement Windows Server 2012 R2 avec des machines virtuelles Hyper-V ou les clusters de basculement Windows Server 2012 R2 exécutant le rôle Serveur de fichiers avec montée en puissance parallèle. Voir [Mise à niveau propagée du système d’exploitation de cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).|
|Services de fichiers et de stockage| Oui|    Oui|    Variable selon la sous-fonctionnalité|  Non|
|Hyper-V| Oui. (Lorsque l’hôte fait partie d’un cluster avec le processus de mise à niveau propagée du système d’exploitation de cluster, qui inclut la mise en pause, le drainage et la suppression de nœuds, la mise à niveau vers Windows Serve 2016 et le retour au cluster d’origine.)|  Non|   Oui|  Pas pour les clusters de basculement Windows Server 2012. Oui, pour les clusters de basculement Windows Server 2012 R2 avec des machines virtuelles Hyper-V ou les clusters de basculement Windows Server 2012 R2 exécutant le rôle Serveur de fichiers avec montée en puissance parallèle. Voir [Mise à niveau propagée du système d’exploitation de cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).| 
|Services d’impression et de télécopie|    Non| Non| Oui (Printbrm.exe)| Non|
|Services Bureau à distance|   Oui, pour tous les sous-rôles, mais la batterie de serveurs en mode mixte n’est pas prise en charge.|   Oui, pour tous les sous-rôles, mais la batterie de serveurs en mode mixte n’est pas prise en charge.|   Oui|    Non|
|Serveur Web (IIS)|  Oui|    Oui|    Oui|    Non|
|Expérience Windows Server Essentials|  Oui|    N/A– Nouvelle fonctionnalité|  Oui|    Non|
|Windows Server Update Services|    Oui|    Oui|    Oui|    Non|
|Dossiers de travail|  Oui|    Oui|    Oui|    Oui, depuis un cluster Windows Server 2012 R2 avec la [mise à niveau propagée du système d’exploitation de cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).|

