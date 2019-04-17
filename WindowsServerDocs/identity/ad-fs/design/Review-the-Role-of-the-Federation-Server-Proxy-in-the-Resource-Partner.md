---
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: "Passez en revue le rôle de serveur Proxy de fédération du partenaire de ressource"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 31e285e863e4316a8e0a65f9b68c27442290927d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>Passez en revue le rôle de serveur Proxy de fédération du partenaire de ressource

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Un serveur proxy de fédération dans ActiveDirectory Federation Services \(ADFS\) peut fonctionner dans un ou plusieurs des rôles suivants, selon la configuration du serveur pour répondre aux besoins de l’organisation partenaire de ressource:  
  
-   **Découverte du partenaire de compte**: un ordinateur client Internet doit identifier le compte partenaire chargé de son authentification. Le client trouve le partenaire de compte à l’aide d’un \(discoverclientrealm.aspx\) compte partenaire découverte Web formulaire, qui est stocké sur le serveur proxy de fédération du partenaire de ressource. Si plus d’un partenaire de compte est configuré dans la gestion ADFS enfichable, qu'un menu déroulantes s’affiche sur le client à tous les partenaires de compte disponibles visibles sur les ordinateurs clients Internet qui accèdent à la découverte du partenaire de compte formulaire Web. Vous pouvez modifier la présentation de la découverte du partenaire de compte formulaire Web sur les ordinateurs clients en personnalisant le fichier discoverclientrealm.aspx.  
  
-   **Redirection du jeton de sécurité**: le serveur proxy de fédération du partenaire de compte envoie les jetons de sécurité au partenaire de ressource. Le serveur proxy de fédération de ressources accepte ces jetons et les transmet au serveur de fédération du partenaire de ressource. Le serveur de fédération de ressources émet ensuite un jeton de sécurité qui est lié à un serveur Web de ressource spécifique. Le serveur proxy de fédération de ressources redirige ensuite le jeton pour le client.  
  
Pour résumer, un serveur proxy de fédération de ressources facilite le processus d’ouverture de session fédérée en redirigeant les ordinateurs clients vers un serveur de fédération qui peut authentifier les clients. Un serveur proxy de fédération de ressources agit également en tant que proxy pour les jetons de sécurité client pour les serveurs de fédération de ressources.  
  
> [!NOTE]  
> Lorsqu’il est nécessaire aider à réduire la quantité de matériel et le nombre de certificats requis, le serveur proxy de fédération peut se trouver sur le même ordinateur que le serveur Web.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

