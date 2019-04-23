---
title: Créer une zone DNS
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a030ff51-a815-4fc4-b26d-aae41c3e4ce5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 31dbae80882e1375e548d5f6942c6473786f838f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870220"
---
# <a name="create-a-dns-zone"></a>Créer une zone DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour créer une zone DNS à l’aide de la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-create-a-dns-zone"></a>Pour créer une zone DNS  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, dans **surveiller et gérer**, cliquez sur **serveurs DNS et DHCP**. Dans le volet d’informations, cliquez sur **Type de serveur**, puis cliquez sur **DNS**. Tous les serveurs DNS qui sont gérés par IPAM sont répertoriées dans les résultats de recherche.  
  
3.  Recherchez le serveur où vous souhaitez ajouter une zone, puis cliquez sur le serveur.  Cliquez sur **créer une zone DNS**.  
  
    ![Créer une zone DNS](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_01a.jpg)  
  
4.  Le **créer DNS Zone** boîte de dialogue s’ouvre. Dans **propriétés générales**, sélectionnez une catégorie de la zone, un type de zone et entrez un nom dans **nom de la Zone**. Sélectionnez également les valeurs appropriées pour votre déploiement dans **propriétés avancées**, puis cliquez sur **OK**.  
  
    ![Propriétés avancées](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_02a.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Gestion des zones DNS](DNS-Zone-Management.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


