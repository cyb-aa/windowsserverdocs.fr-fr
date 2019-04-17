---
title: Modifier une Zone DNS
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
ms.assetid: a35164e1-11ad-47c8-9843-580d30c70d07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3e7cc75017c2b59293a042d4af0a677d3eda46c0
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="edit-a-dns-zone"></a>Modifier une Zone DNS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour modifier une zone DNS dans la console client IPAM.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.  
  
### <a name="to-edit-a-dns-zone"></a>Pour modifier une zone DNS  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation dans **surveiller et gérer**, cliquez sur **Zones DNS**. Le volet de navigation se divise en un volet de navigation supérieur et un volet de navigation inférieur.  
  
3.  Dans le volet de navigation inférieur, sélectionnez l’une des options suivantes:  
  
    -   Recherche directe  
  
    -   Recherche inversée IPv4  
  
    -   Recherche inversée IPv6  
  
4.  Par exemple, sélectionnez la recherche inversée IPv4.  
  
    ![Sélectionnez un type de zone](../../media/Edit-a-DNS-Zone/ipam_EditZone_01.jpg)  
  
5.  Dans le volet d’informations, cliquez sur la zone que vous souhaitez modifier, puis cliquez sur **Zone DNS de modifier**.  
  
    ![Zone DNS de modification](../../media/Edit-a-DNS-Zone/ipam_EditZone_02.jpg)  
  
6.  Le **Zone DNS de modifier** boîte de dialogue s’ouvre avec la **général** page sélectionnée. Si nécessaire, modifiez les propriétés de zone Général: **serveur DNS**, **catégorie Zone**, et **type de Zone**, puis cliquez sur **appliquer** ou, si vos modifications sont terminées, **OK**.  
  
    ![Modifier les propriétés de la zone et enregistrer](../../media/Edit-a-DNS-Zone/ipam_EditZone_03a.jpg)  
  
7.  Dans le **Zone DNS de modifier** boîte de dialogue, cliquez sur **avancé**. Le **avancé** ouvre la page des propriétés de zone. Si nécessaire, modifiez les propriétés que vous souhaitez modifier, puis cliquez sur **appliquer** ou, si vos modifications sont terminées, **OK**.  
  
    ![Modifier les propriétés avancées de la zone](../../media/Edit-a-DNS-Zone/ipam_EditZone_04a.jpg)  
  
8.  Si nécessaire, sélectionnez les noms de page de propriétés de zone supplémentaires (serveurs de noms, SOA, les transferts de zones), apportez vos modifications, puis cliquez sur **appliquer** ou **OK**. Pour passer en revue toutes vos modifications de la zone, cliquez sur **Résumé**, puis cliquez sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  
[Gestion des zones DNS](DNS-Zone-Management.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


