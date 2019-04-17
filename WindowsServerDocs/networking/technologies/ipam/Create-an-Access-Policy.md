---
title: Créer une stratégie d’accès
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
ms.assetid: 854bd064-2f86-4678-a940-a04b3e48ae10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ac8229952c7e038f9af8fc4f9287b1821db887ac
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="create-an-access-policy"></a>Créer une stratégie d’accès

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour créer une stratégie d’accès dans la console client IPAM.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.  
  
> [!NOTE]  
> Vous pouvez créer une stratégie d’accès pour un utilisateur spécifique ou pour un groupe d’utilisateurs dans ActiveDirectory. Lorsque vous créez une stratégie d’accès, vous devez sélectionner un rôle IPAM intégré ou un rôle personnalisé que vous avez créé. Pour plus d’informations sur les rôles personnalisés, voir [créer un rôle d’utilisateur pour le contrôle d’accès](../../technologies/ipam/Create-a-User-Role-for-Access-Control.md).  
  
### <a name="to-create-an-access-policy"></a>Pour créer une stratégie d’accès  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, cliquez sur **le contrôle d’accès**. Dans le volet de navigation inférieur, cliquez sur **stratégies d’accès**, puis cliquez sur **ajouter une stratégie d’accès**.  
  
    ![Ajouter une stratégie d’accès](../../media/Create-an-Access-Policy/ipam_CreateAP_01.jpg)  
  
3.  Le **ajouter une stratégie d’accès** boîte de dialogue s’ouvre. Dans **paramètres utilisateur**, cliquez sur **ajouter**.  
  
    ![Ajouter une stratégie d’accès](../../media/Create-an-Access-Policy/ipam_CreateAP_02.jpg)  
  
4.  Le **sélectionner utilisateur ou groupe** boîte de dialogue s’ouvre. Cliquez sur **emplacements**.  
  
    ![Emplacements d’utilisateur ou un groupe](../../media/Create-an-Access-Policy/ipam_CreateAP_03.jpg)  
  
5.  Le **emplacements** boîte de dialogue s’ouvre. Accédez à l’emplacement qui contient le compte d’utilisateur, sélectionnez l’emplacement, puis cliquez sur **OK**. Le **emplacements** boîte de dialogue se ferme.  
  
    ![Sélectionner l’emplacement](../../media/Create-an-Access-Policy/ipam_CreateAP_04.jpg)  
  
6.  Dans le **sélectionner utilisateur ou groupe** la boîte de dialogue dans **permet d’entrer le nom d’objet à sélectionner**, tapez le nom de compte d’utilisateur pour lequel vous souhaitez créer une stratégie d’accès. Cliquez sur **OK**.  
  
7.  Dans **ajouter une stratégie d’accès**, dans **paramètres utilisateur**, **alias utilisateur** contient désormais le compte d’utilisateur auquel s’applique la stratégie. Dans **paramètres d’accès**, cliquez sur **New**.  
  
    ![Nouveau paramètre d’accès](../../media/Create-an-Access-Policy/ipam_CreateAP_05.jpg)  
  
8.  Dans **ajouter une stratégie d’accès**, **paramètres d’accès** devient **nouveau paramètre**.  
  
    ![Nom de la boîte de dialogue Modifier à nouveau paramètre](../../media/Create-an-Access-Policy/ipam_CreateAP_06.jpg)  
  
9. Cliquez sur **sélectionner un rôle** pour développer la liste des rôles. Sélectionnez l’un des rôles intégrés ou, si vous avez créé des nouveaux rôles, sélectionnez l’un des rôles que vous avez créé. Par exemple, si vous avez créé le rôle IPAMSrv à appliquer à l’utilisateur, cliquez sur **IPAMSrv**.  
  
    ![Sélectionnez le rôle](../../media/Create-an-Access-Policy/ipam_CreateAP_07.jpg)  
  
10. Cliquez sur **ajouter le paramètre**.  
  
    ![Ajouter un nouveau paramètre](../../media/Create-an-Access-Policy/ipam_CreateAP_08.jpg)  
  
11. Le rôle est ajouté à la stratégie d’accès. Pour créer des stratégies d’accès supplémentaires, cliquez sur **appliquer**, puis répétez ces étapes pour chaque stratégie que vous voulez créer. Si vous ne souhaitez pas créer des stratégies supplémentaires, cliquez sur **OK**.  
  
    ![Cliquez sur Appliquer ou OK](../../media/Create-an-Access-Policy/ipam_CreateAP_09.jpg)  
  
12. Dans le volet d’informations console client IPAM, vérifiez que la nouvelle stratégie d’accès est créée.  
  
    ![Afficher la nouvelle stratégie d’accès](../../media/Create-an-Access-Policy/ipam_CreateAP_09a.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Contrôle d’accès basé sur les rôles](Role-based-Access-Control.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


