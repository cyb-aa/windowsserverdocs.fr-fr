---
title: Modifier une zone DNS
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a35164e1-11ad-47c8-9843-580d30c70d07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d688790828cb58b9fca2a17c95212c064532bb22
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282167"
---
# <a name="edit-a-dns-zone"></a>Modifier une zone DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour modifier une zone DNS dans la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-edit-a-dns-zone"></a>Pour modifier une zone DNS  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, dans **surveiller et gérer**, cliquez sur **Zones DNS**. Le volet de navigation divise en un volet de navigation supérieur et un volet de navigation inférieur.  
  
3.  Dans le volet de navigation inférieur, apportez l’une des options suivantes :  
  
    -   Recherche directe  
  
    -   Recherche inversée IPv4  
  
    -   Recherche inversée IPv6  
  
4.  Par exemple, sélectionnez la recherche inversée IPv4.  
  
    ![Sélectionnez un type de zone](../../media/Edit-a-DNS-Zone/ipam_EditZone_01.jpg)  
  
5.  Dans le volet d’informations, cliquez sur la zone que vous souhaitez modifier, puis cliquez sur **modification DNS Zone**.  
  
    ![Zone de modification DNS](../../media/Edit-a-DNS-Zone/ipam_EditZone_02.jpg)  
  
6.  Le **modification DNS Zone** boîte de dialogue s’ouvre avec le **général** page sélectionnée. Si nécessaire, modifiez les propriétés générales : **Serveur DNS**, **catégorie de la Zone**, et **type de Zone**, puis cliquez sur **appliquer** ou, si vos modifications sont terminées, **OK**.  
  
    ![Modifier les propriétés de la zone et enregistrer](../../media/Edit-a-DNS-Zone/ipam_EditZone_03a.jpg)  
  
7.  Dans le **modification DNS Zone** boîte de dialogue, cliquez sur **avancé**. Le **avancé** ouvrir la page Propriétés de zone. Si nécessaire, modifiez les propriétés que vous souhaitez modifier, puis cliquez sur **appliquer** ou, si vos modifications sont terminées, **OK**.  
  
    ![Modifier les propriétés avancées de la zone](../../media/Edit-a-DNS-Zone/ipam_EditZone_04a.jpg)  
  
8.  Si nécessaire, sélectionnez les noms de page de propriétés de zone supplémentaire (serveurs de noms, SOA, les transferts de Zone), apportez vos modifications, puis cliquez sur **appliquer** ou **OK**. Pour passer en revue toutes vos modifications de la zone, cliquez sur **Résumé**, puis cliquez sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  
[Gestion des zones DNS](DNS-Zone-Management.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


