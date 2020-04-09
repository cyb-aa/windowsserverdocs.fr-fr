---
title: Créer une stratégie d’accès
description: Cette rubrique fait partie du Guide de gestion des adresses IP (IPAM) de Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: 854bd064-2f86-4678-a940-a04b3e48ae10
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 807f02387a64a26ed8b1c58a387b165bece83168
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814812"
---
# <a name="create-an-access-policy"></a>Créer une stratégie d’accès

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour créer une stratégie d’accès dans la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
> [!NOTE]  
> Vous pouvez créer une stratégie d’accès pour un utilisateur spécifique ou un groupe d’utilisateurs dans Active Directory. Lorsque vous créez une stratégie d’accès, vous devez sélectionner un rôle IPAM intégré ou un rôle personnalisé que vous avez créé. Pour plus d’informations sur les rôles personnalisés, consultez [créer un rôle d’utilisateur pour Access Control](../../technologies/ipam/Create-a-User-Role-for-Access-Control.md).  
  
### <a name="to-create-an-access-policy"></a>Pour créer une stratégie d’accès  
  
1.  Dans Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, cliquez sur **contrôle d’accès**. Dans le volet de navigation inférieur, cliquez avec le bouton droit sur **stratégies d’accès**, puis cliquez sur Ajouter une **stratégie d’accès**.  
  
    ![Ajouter une stratégie d’accès](../../media/Create-an-Access-Policy/ipam_CreateAP_01.jpg)  
  
3.  La boîte de dialogue **Ajouter une stratégie d’accès** s’ouvre. Dans **paramètres utilisateur**, cliquez sur **Ajouter**.  
  
    ![Ajouter une stratégie d’accès](../../media/Create-an-Access-Policy/ipam_CreateAP_02.jpg)  
  
4.  La boîte de dialogue **Sélectionner un utilisateur ou un groupe** s’ouvre. Cliquez sur **emplacements**.  
  
    ![Emplacements d’utilisateur ou de groupe](../../media/Create-an-Access-Policy/ipam_CreateAP_03.jpg)  
  
5.  La boîte de dialogue **emplacements** s’ouvre. Accédez à l’emplacement qui contient le compte d’utilisateur, sélectionnez l’emplacement, puis cliquez sur **OK**. La boîte de dialogue **emplacements** se ferme.  
  
    ![Sélectionner un emplacement](../../media/Create-an-Access-Policy/ipam_CreateAP_04.jpg)  
  
6.  Dans la boîte de dialogue **Sélectionner l’utilisateur ou le groupe** , dans **Entrez le nom de l’objet à sélectionner**, tapez le nom du compte d’utilisateur pour lequel vous souhaitez créer une stratégie d’accès. Cliquez sur **OK**.  
  
7.  Dans **Ajouter une stratégie d’accès**, dans **paramètres utilisateur**, l’alias de l' **utilisateur** contient maintenant le compte d’utilisateur auquel la stratégie s’applique. Dans **paramètres d’accès**, cliquez sur **nouveau**.  
  
    ![Nouveau paramètre d’accès](../../media/Create-an-Access-Policy/ipam_CreateAP_05.jpg)  
  
8.  Dans **Ajouter une stratégie d’accès**, les **paramètres d’accès** sont modifiés en **nouveau paramètre**.  
  
    ![Le nom de la boîte de dialogue passe au nouveau paramètre](../../media/Create-an-Access-Policy/ipam_CreateAP_06.jpg)  
  
9. Cliquez sur **Sélectionner un rôle** pour développer la liste des rôles. Sélectionnez l’un des rôles intégrés ou, si vous avez créé de nouveaux rôles, sélectionnez l’un des rôles que vous avez créés. Par exemple, si vous avez créé le rôle IPAMSrv à appliquer à l’utilisateur, cliquez sur **IPAMSrv**.  
  
    ![Sélectionner un rôle](../../media/Create-an-Access-Policy/ipam_CreateAP_07.jpg)  
  
10. Cliquez sur **Ajouter un paramètre**.  
  
    ![Ajouter un nouveau paramètre](../../media/Create-an-Access-Policy/ipam_CreateAP_08.jpg)  
  
11. Le rôle est ajouté à la stratégie d’accès. Pour créer des stratégies d’accès supplémentaires, cliquez sur **appliquer**, puis répétez ces étapes pour chaque stratégie que vous souhaitez créer. Si vous ne souhaitez pas créer de stratégies supplémentaires, cliquez sur **OK**.  
  
    ![Cliquez sur appliquer ou sur OK](../../media/Create-an-Access-Policy/ipam_CreateAP_09.jpg)  
  
12. Dans le volet d’affichage de la console client IPAM, vérifiez que la nouvelle stratégie d’accès est créée.  
  
    ![Afficher la nouvelle stratégie d’accès](../../media/Create-an-Access-Policy/ipam_CreateAP_09a.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Access Control basée sur les rôles](Role-based-Access-Control.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


