---
title: Créer une Zone DNS
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server2016.
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
ms.openlocfilehash: e532837e6c98694fa040a6d47a8e536eecb4c3da
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-dns-zone"></a>Créer une Zone DNS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour créer une zone DNS à l’aide de la console client IPAM.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.  
  
### <a name="to-create-a-dns-zone"></a>Pour créer une zone DNS  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation dans **surveiller et gérer**, cliquez sur **serveurs DNS et DHCP **. Dans le volet d’informations, cliquez sur **Type de serveur**, puis cliquez sur **DNS **. Tous les serveurs DNS qui sont gérés par IPAM sont répertoriés dans les résultats de recherche.  
  
3.  Trouver le serveur où vous souhaitez ajouter une zone et cliquez sur le serveur.  Cliquez sur **zone DNS créer **.  
  
    ![Créer la zone DNS](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_01a.jpg)  
  
4.  Le **création de Zone DNS** boîte de dialogue s’ouvre. Dans **propriétés générales**, sélectionnez une catégorie de zone, un type de zone et entrez un nom dans **nom de la Zone **. Sélectionnez également les valeurs appropriées pour votre déploiement dans **propriétés avancées**, puis cliquez sur **OK **.  
  
    ![Propriétés avancées](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_02a.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Gestion des zones DNS](DNS-Zone-Management.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


