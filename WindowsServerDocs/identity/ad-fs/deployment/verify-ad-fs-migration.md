---
title: Migration du serveur de fédération 2.0 AD FS
description: Fournit des informations sur la migration d’un serveur AD FS vers Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0a3e8e444f715fe2ae0f0ccd858d90e8664be00c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877770"
---
# <a name="verify-the-ad-fs-20-migration-to-windows-server-2012-r2"></a>Vérifiez l’AD FS 2.0 la migration vers Windows Server 2012 R2

Lorsque vous avez terminé la même migration serveur de votre batterie de serveurs du Service de fédération Active Directory (AD FS) vers Windows Server 2012 R2, vous pouvez utiliser la procédure suivante pour vérifier que les serveurs de fédération dans votre batterie de serveurs sont opérationnels ; Autrement dit, qu’un client sur le même réseau peut accéder à vos serveurs federtation.  
  
Vous devez au minimum appartenir au groupe **Utilisateurs**, **Opérateurs de sauvegarde**, **Utilisateurs avec pouvoir**, **Administrateurs** ou à un groupe équivalent sur l’ordinateur local pour pouvoir suivre cette procédure.
  
### <a name="to-verify-that-a-federation-server-is-operational"></a>Pour vérifier qu’un serveur de fédération est opérationnel  
  
1.  Ouvrez une fenêtre de navigateur et dans la barre d’adresses, tapez le nom de serveurs de fédération et ajoutez-le avec `federationmetadata/2007-06/federationmetadata.xml` pour accéder au point de terminaison de métadonnées de service FS. Par exemple, `https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml` .  
  
Si, dans la fenêtre de navigateur, vous pouvez voir les métadonnées du serveur de fédération sans aucun avertissement ni erreur SSL, le serveur de fédération est opérationnel.  
  
2.  Vous pouvez également accéder à la page de connexion AD FS (nom du service de fédération suivi de `adfs/ls/idpinitiatedsignon.htm`, par exemple `https://fs.contoso.com/adfs/ls/idpinitiatedsignon.htm`).  La page de connexion AD FS s’ouvre dans laquelle vous pouvez vous connecter à l’aide des informations d’identification d’administrateur du domaine.  
  
> [!IMPORTANT]
>  Veillez à configurer les paramètres du navigateur pour approuver le rôle de serveur de fédération en ajoutant le nom de votre service de fédération (par exemple, `https://fs.contoso.com`) à la zone intranet locale du navigateur.  
  
## <a name="next-steps"></a>Étapes suivantes
 [Migrer des Services de rôle Active Directory Federation Services vers Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Préparation à la migration du serveur de fédération AD FS](prepare-migrate-ad-fs-server-r2.md)  
 [Migration du serveur de fédération AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migration de serveur Proxy de fédération AD FS](migrate-fed-server-proxy-r2.md)   
