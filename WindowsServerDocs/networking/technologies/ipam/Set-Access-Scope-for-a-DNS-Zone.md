---
title: Définir l’étendue d’accès pour une zone DNS
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a211dde-80eb-4888-b5bb-4e28fe8dc7df
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 67e6e16d361d6d975c4cf900dc9c6b9e7abd3f20
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282204"
---
# <a name="set-access-scope-for-a-dns-zone"></a>Définir l’étendue d’accès pour une zone DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour définir l’étendue d’accès pour une zone DNS à l’aide de la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-set-the-access-scope-for-a-dns-zone"></a>Pour définir l’étendue d’accès pour une zone DNS  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, cliquez sur **Zones DNS**. Dans le volet d’informations, cliquez sur la zone DNS pour lequel vous souhaitez modifier l’étendue d’accès., puis cliquez sur **définir l’étendue d’accès**.  
  
    ![Définir l’étendue d’accès](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_02.jpg)  
  
3.  Le **définir l’étendue d’accès** boîte de dialogue s’ouvre. Si nécessaire pour votre déploiement, cliquez pour désélectionner **hériter l’étendue d’accès du parent**. Dans **sélectionner l’étendue d’accès**, sélectionnez un élément, puis cliquez sur **OK**.  
  
    ![Sélectionnez l’étendue d’accès](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_03.jpg)  
  
4.  Dans le volet Affichage de console du client IPAM, vérifiez que l’étendue d’accès pour la zone est modifiée.  
  
    ![Vérifiez que l’étendue d’accès pour la zone est modifiée.](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_04.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Contrôle d’accès en fonction du rôle](Role-based-Access-Control.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


