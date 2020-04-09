---
title: Limiter l’accès au Web
description: Découvrez comment limiter l’accès utilisateur à Internet dans MultiPoint services
ms.date: 07/08/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 044f2fd5-5b87-42bb-ba0d-c06516ac48c8
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 485c82284df4d77eea075d092fa08c820567f6c3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853682"
---
# <a name="limit-web-access"></a>Limiter l’accès au Web
En plus de surveiller les activités des utilisateurs sur des postes de travail individuels, vous, en tant qu’utilisateur administratif, pouvez limiter l’accès des utilisateurs aux sites Web spécifiés en indiquant les sites Web autorisés et les sites Web auxquels vous souhaitez bloquer l’accès des utilisateurs.  
  
## <a name="to-limit-web-access-on-a-station"></a>Pour limiter l’accès au Web sur une station  
  
1. Dans le tableau de bord MultiPoint, sous l’onglet **limitation du Web** , cliquez sur **configurer**. La page de **configuration de la limitation de l’accès au Web** s’ouvre. Les sites auxquels l’utilisateur peut accéder sont répertoriés.  
  
2. Cliquez sur l’image miniature de la station d’utilisateur sur laquelle vous souhaitez limiter l’accès au Web.  
  
3. Sous **Selected Item Tasks (Tâches d’élément sélectionné)** , cliquez sur **Limit web access on this station (Limiter l’accès au Web sur cette station)** . La page de **configuration de la limitation de l’accès au Web** s’ouvre. Les sites auxquels l’utilisateur peut accéder sont répertoriés.  
  
4. Pour ajouter un site autorisé, tapez l’adresse web, puis cliquez sur **Add (Ajouter)** .  
  
   > [!NOTE]
   > Par exemple, la saisie de « Contoso.com » autorise ou bloque les sites qui sont relatifs à www\.contoso.com (par exemple, www\.newpage.contoso.com). Le fait d’entrer « contoso » autorise ou limite tous les sites liés à contoso (y compris contoso.com, contoso.uk, etc.).  
  
5. Pour supprimer une adresse Web de la liste des sites autorisés, cliquez sur l’adresse Web dont vous souhaitez supprimer l’accès, puis cliquez sur **Remove (Supprimer)** .  
  
## <a name="to-limit-web-access-on-all-stations"></a>Pour limiter l’accès au Web sur toutes les stations  
  
1. Dans le tableau de bord MultiPoint, sous l’onglet **limitation du Web** , cliquez sur le menu déroulant\-de démarrage, puis sur **limiter les accès Web sur tous les postes de travail**.  
  
   La page de **configuration de la limitation de l’accès au Web** s’ouvre. Les sites auxquels l’utilisateur peut accéder sont répertoriés. Effectuez l'une des opérations suivantes :  
  
2. Pour ajouter un site autorisé, cliquez sur **Allow only these sites (Autoriser uniquement ces sites)** , tapez l’adresse Web autorisée, puis cliquez sur **Add (Ajouter)** .  
  
   Pour ajouter un site que les utilisateurs ne doivent pas visiter, cliquez sur **interdire uniquement ces sites**, tapez l’adresse Web que les utilisateurs ne doivent pas visiter, puis cliquez sur **Ajouter**.  
  
   > [!NOTE]
   > Par exemple, la saisie de « Contoso.com » autorise ou bloque les sites qui sont relatifs à www.contoso.com (par exemple, www\.newpage.contoso.com). Le fait d’entrer « contoso » autorise ou limite tous les sites liés à contoso (y compris contoso.com, contoso.uk, etc.).  
  
3. Pour supprimer une adresse Web de la liste des sites autorisés ou non autorisés, sélectionnez l’adresse Web, puis cliquez sur **Remove (Supprimer)** .  
  
## <a name="see-also"></a>Voir aussi  
[Gérer les postes de travail des utilisateurs](manage-user-desktops-using-multipoint-dashboard.md)  
