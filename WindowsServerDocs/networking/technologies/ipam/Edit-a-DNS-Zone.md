---
title: Modifier une zone DNS
description: Cette rubrique fait partie du Guide de gestion des adresses IP (IPAM) de Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a35164e1-11ad-47c8-9843-580d30c70d07
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b2fa8ff6742dc393a5a2cc962c7f849049b5ef31
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312422"
---
# <a name="edit-a-dns-zone"></a>Modifier une zone DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour modifier une zone DNS dans la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-edit-a-dns-zone"></a>Pour modifier une zone DNS  
  
1.  Dans Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, sous **surveiller et gérer**, cliquez sur **zones DNS**. Le volet de navigation est divisé en un volet de navigation supérieur et un volet de navigation inférieur.  
  
3.  Dans le volet de navigation inférieur, effectuez l’une des sélections suivantes :  
  
    -   Recherche directe  
  
    -   Recherche inversée IPv4  
  
    -   Recherche inversée IPv6  
  
4.  Par exemple, sélectionnez recherche inversée IPv4.  
  
    ![Sélectionner un type de zone](../../media/Edit-a-DNS-Zone/ipam_EditZone_01.jpg)  
  
5.  Dans le volet d’affichage, cliquez avec le bouton droit sur la zone que vous souhaitez modifier, puis cliquez sur **modifier la zone DNS**.  
  
    ![Modifier la zone DNS](../../media/Edit-a-DNS-Zone/ipam_EditZone_02.jpg)  
  
6.  La boîte de dialogue **modifier la zone DNS** s’ouvre et affiche la page **général** sélectionnée. Si nécessaire, modifiez les propriétés de la zone générale : **serveur DNS**, **catégorie de zone**et **type de zone**, puis cliquez sur **appliquer** ou, si vos modifications sont terminées, cliquez sur **OK**.  
  
    ![Modifier les propriétés de zone et enregistrer](../../media/Edit-a-DNS-Zone/ipam_EditZone_03a.jpg)  
  
7.  Dans la boîte de dialogue **modifier la zone DNS** , cliquez sur **avancé**. La page Propriétés de la zone **avancée** s’ouvre. Si nécessaire, modifiez les propriétés que vous souhaitez modifier, puis cliquez sur **appliquer** ou, si vos modifications sont terminées, cliquez sur **OK**.  
  
    ![Modifier les propriétés d’une zone avancée](../../media/Edit-a-DNS-Zone/ipam_EditZone_04a.jpg)  
  
8.  Si nécessaire, sélectionnez les noms des pages de propriétés de la zone supplémentaires (serveurs de noms, SOA, transferts de zone), apportez vos modifications, puis cliquez sur **appliquer** ou **OK**. Pour passer en revue toutes les modifications de zone, cliquez sur **Résumé**, puis sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  
[Gestion des zones DNS](DNS-Zone-Management.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


