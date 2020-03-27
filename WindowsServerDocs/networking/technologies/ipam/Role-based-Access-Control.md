---
title: Contrôle d’accès basé sur les rôles
description: Cette rubrique fait partie du Guide de gestion des adresses IP (IPAM) de Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ecdfc589-fa14-4bb3-ab7e-456ebc719385
ms.author: lizross
author: eross-msft
ms.openlocfilehash: dc4dc26a5079a34cdaa3d455e59f6bfb4d1f74c1
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309521"
---
# <a name="role-based-access-control"></a>Contrôle d’accès basé sur les rôles

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique fournit des informations sur l’utilisation du contrôle d’accès en fonction du rôle dans IPAM.  
  
> [!NOTE]  
> En plus de cette rubrique, la documentation de contrôle d’accès IPAM suivante est disponible dans cette section.  
>   
> -   [Gérer les Access Control basés sur des rôles avec Gestionnaire de serveur](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
> -   [Gérer les Access Control basés sur des rôles avec Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
  
Le contrôle d’accès en fonction du rôle vous permet de spécifier des privilèges d’accès à différents niveaux, notamment le serveur DNS, la zone DNS et les niveaux d’enregistrement de ressources DNS.  
En utilisant le contrôle d’accès en fonction du rôle, vous pouvez spécifier qui a un contrôle granulaire sur les opérations pour créer, modifier et supprimer différents types d’enregistrements de ressources DNS.  
  
Vous pouvez configurer le contrôle d’accès afin que les utilisateurs soient limités aux autorisations suivantes.  
  
-   Les utilisateurs ne peuvent modifier que des enregistrements de ressources DNS spécifiques  
  
-   Les utilisateurs peuvent modifier les enregistrements de ressources DNS d’un type spécifique, par exemple PTR ou MX.  
  
-   Les utilisateurs peuvent modifier les enregistrements de ressources DNS pour des zones spécifiques  
  
## <a name="see-also"></a>Voir aussi  
[Gérer les Access Control basés sur des rôles avec Gestionnaire de serveur](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
[Gérer les Access Control basés sur des rôles avec Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


