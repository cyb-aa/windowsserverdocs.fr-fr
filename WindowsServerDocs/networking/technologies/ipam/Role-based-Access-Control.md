---
title: Contrôle d’accès basé sur les rôles
description: Cette rubrique fait partie du Guide de gestion des adresses IP (IPAM) de Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: ecdfc589-fa14-4bb3-ab7e-456ebc719385
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ae5e36e9c0931ca5735df6111f73e87ef012ee5d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860592"
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
  


