---
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: Examiner le rôle du serveur proxy de fédération du partenaire de ressource
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: df9e291d85ea8899d7f546956276c60582893fe8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359042"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>Examiner le rôle du serveur proxy de fédération du partenaire de ressource

Un serveur proxy de Fédération dans Services ADFS \(AD FS\) peut fonctionner dans un ou plusieurs des rôles suivants, selon la façon dont vous configurez le serveur pour répondre aux besoins de l’organisation partenaire de ressource :  
  
-   **Découverte du partenaire de compte**: un ordinateur client Internet doit identifier le partenaire de compte chargé de son authentification. Le client trouve le partenaire de compte à l’aide d’un formulaire Web de découverte de partenaire de compte \(discoverclientrealm. aspx\), qui est stocké sur le serveur proxy de Fédération du partenaire de ressource. Si plusieurs partenaires de compte sont configurés dans le\-du composant logiciel enfichable Gestion de l’AD FS dans, un menu déroulant\-en aval apparaît au client avec tous les partenaires de compte disponibles visibles par les ordinateurs clients Internet qui accèdent au formulaire Web de découverte de partenaire de compte. Vous pouvez modifier la manière dont le formulaire web de découverte de partenaire de compte est présenté aux ordinateurs clients en personnalisant le fichier discoverclientrealm.aspx.  
  
-   **Redirection des jetons de sécurité**: le serveur proxy de Fédération du partenaire de compte envoie les jetons de sécurité au partenaire de ressource. Le serveur proxy de Fédération de ressources accepte ces jetons et les transmet au serveur de Fédération du partenaire de ressource. Le serveur de Fédération de ressources émet ensuite un jeton de sécurité qui est lié pour un serveur Web de ressources spécifique. Le serveur proxy de Fédération de ressources redirige ensuite le jeton vers le client.  
  
Pour résumer, un proxy de serveur de Fédération de ressources facilite le processus d’ouverture de session fédérée en redirigeant les ordinateurs clients vers un serveur de Fédération qui peut authentifier les clients. Un serveur proxy de Fédération de ressources joue également le rôle de proxy pour les jetons de sécurité client pour les serveurs de Fédération de ressources.  
  
> [!NOTE]  
> Lorsqu’il est nécessaire de réduire la quantité de matériel et le nombre de certificats requis, le serveur proxy de Fédération peut se trouver sur le même ordinateur que le serveur Web.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

