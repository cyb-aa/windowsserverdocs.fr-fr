---
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: Examiner le rôle du serveur proxy de fédération du partenaire de ressource
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 31e285e863e4316a8e0a65f9b68c27442290927d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862890"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>Examiner le rôle du serveur proxy de fédération du partenaire de ressource

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un serveur proxy de fédération dans Active Directory Federation Services \(AD FS\) peut fonctionner dans un ou plusieurs des rôles suivants, selon la façon dont vous configurez le serveur pour répondre aux besoins de l’organisation partenaire de ressource :  
  
-   **Découverte du partenaire de compte** : un ordinateur client Internet doit identifier le compte partenaire chargé de son authentification. Le client trouve le partenaire de compte à l’aide d’un partenaire de compte formulaire Web de découverte \(discoverclientrealm.aspx\), qui est stocké sur le serveur proxy de fédération du partenaire de ressource. Si plus d’un partenaire de compte est configuré dans le composant logiciel enfichable Gestion AD FS\-dans un dépôt\-menu déroulant s’affiche au client avec tous les partenaires de compte disponibles qui sont visibles pour les ordinateurs clients Internet qui accèdent à du partenaire de compte formulaire Web de découverte. Vous pouvez modifier la manière dont le formulaire web de découverte de partenaire de compte est présenté aux ordinateurs clients en personnalisant le fichier discoverclientrealm.aspx.  
  
-   **Redirection du jeton de sécurité** : Le serveur proxy de fédération du partenaire de compte envoie les jetons de sécurité au partenaire de ressource. Le serveur proxy de fédération de ressources accepte ces jetons et les transmet au serveur de fédération du partenaire de ressource. Le serveur de fédération de ressources émet ensuite un jeton de sécurité qui est lié pour un serveur Web de ressource spécifique. Le serveur proxy de fédération de ressources redirige ensuite le jeton au client.  
  
Pour résumer, un serveur proxy de fédération de ressources facilite le processus d’ouverture de session fédérée en redirigeant les ordinateurs clients vers un serveur de fédération qui peut authentifier les clients. Un serveur proxy de fédération de ressources agit également comme un proxy pour les jetons de sécurité client aux serveurs de fédération de ressources.  
  
> [!NOTE]  
> Lorsqu’il est nécessaire afin de réduire la quantité de matériel et le nombre de certificats requis, le serveur proxy de fédération permettre se trouver sur le même ordinateur que le serveur Web.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

