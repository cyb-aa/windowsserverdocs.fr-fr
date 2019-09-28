---
title: Migrer le serveur de fédération AD FS 2,0
description: Contient des informations sur la migration d’un serveur AD FS vers Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 024f7a586c58dcf5f0d6f9c3aa291e6bef8d838b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408186"
---
# <a name="verify-the-ad-fs-20-migration-to-windows-server-2012-r2"></a>Vérifier la migration de AD FS 2,0 vers Windows Server 2012 R2

Une fois que vous avez effectué la même migration serveur de votre batterie de serveurs Active Directory service FS (Federation Service) (AD FS) vers Windows Server 2012 R2, vous pouvez utiliser la procédure suivante pour vérifier que les serveurs de Fédération de votre batterie sont opérationnels. autrement dit, tous les clients sur le même réseau peuvent accéder à vos serveurs federtation.  
  
Vous devez au minimum appartenir au groupe **Utilisateurs**, **Opérateurs de sauvegarde**, **Utilisateurs avec pouvoir**, **Administrateurs** ou à un groupe équivalent sur l’ordinateur local pour pouvoir suivre cette procédure.
  
### <a name="to-verify-that-a-federation-server-is-operational"></a>Pour vérifier qu’un serveur de fédération est opérationnel  
  
1.  Ouvrez une fenêtre de navigateur et, dans la barre d’adresse, tapez le nom du serveur de Fédération, `federationmetadata/2007-06/federationmetadata.xml` puis ajoutez-le avec pour accéder au point de terminaison des métadonnées du service de Fédération. Par exemple, `https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml` .  
  
Si, dans la fenêtre de navigateur, vous pouvez voir les métadonnées du serveur de fédération sans aucun avertissement ni erreur SSL, le serveur de fédération est opérationnel.  
  
2. Vous pouvez également accéder à la page de connexion AD FS (nom du service de fédération suivi de `adfs/ls/idpinitiatedsignon.htm`, par exemple `https://fs.contoso.com/adfs/ls/idpinitiatedsignon.htm`).  La page de connexion AD FS s’ouvre dans laquelle vous pouvez vous connecter à l’aide des informations d’identification d’administrateur du domaine.  
  
> [!IMPORTANT]
>  Veillez à configurer les paramètres de votre navigateur pour approuver le rôle de serveur de Fédération en ajoutant le nom de votre `https://fs.contoso.com`service de Fédération (par exemple,) à la zone Intranet local du navigateur.  
  
## <a name="next-steps"></a>Étapes suivantes
 [Migrer Services ADFS services de rôle vers Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Préparation de la migration du serveur de fédération AD FS](prepare-migrate-ad-fs-server-r2.md)  
 [Migration du serveur de fédération AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migration du serveur proxy de fédération AD FS](migrate-fed-server-proxy-r2.md)   
