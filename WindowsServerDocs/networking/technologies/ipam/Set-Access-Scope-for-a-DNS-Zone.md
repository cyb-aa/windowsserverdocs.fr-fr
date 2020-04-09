---
title: Définir l’étendue d’accès pour une zone DNS
description: Cette rubrique fait partie du Guide de gestion des adresses IP (IPAM) de Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: 6a211dde-80eb-4888-b5bb-4e28fe8dc7df
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ca1899a4d49639b047b672c42b9743fb27df8a67
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860602"
---
# <a name="set-access-scope-for-a-dns-zone"></a>Définir l’étendue d’accès pour une zone DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour définir l’étendue d’accès d’une zone DNS à l’aide de la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-set-the-access-scope-for-a-dns-zone"></a>Pour définir l’étendue d’accès d’une zone DNS  
  
1.  Dans Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, cliquez sur **zones DNS**. Dans le volet d’affichage, cliquez avec le bouton droit sur la zone DNS pour laquelle vous souhaitez modifier l’étendue d’accès, puis cliquez sur **définir l’étendue d’accès**.  
  
    ![Définir l’étendue d’accès](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_02.jpg)  
  
3.  La boîte de dialogue **définir l’étendue d’accès** s’ouvre. Si nécessaire pour votre déploiement, cliquez pour désélectionner **hériter l’étendue d’accès du parent**. Dans **Sélectionner l’étendue d’accès**, sélectionnez un élément, puis cliquez sur **OK**.  
  
    ![Sélectionner l’étendue d’accès](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_03.jpg)  
  
4.  Dans le volet d’affichage de la console client IPAM, vérifiez que l’étendue d’accès de la zone a été modifiée.  
  
    ![Vérifier que l’étendue d’accès de la zone a été modifiée](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_04.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Access Control basée sur les rôles](Role-based-Access-Control.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


