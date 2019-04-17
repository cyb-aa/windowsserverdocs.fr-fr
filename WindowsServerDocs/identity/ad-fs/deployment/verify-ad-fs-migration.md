---
title: "Migrer le serveur AD FS 2.0 de fédération"
description: "Fournit des informations sur la migration d’un serveur AD FS vers Windows Server 2012 R2."
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0a3e8e444f715fe2ae0f0ccd858d90e8664be00c
ms.sourcegitcommit: 03ce78a1624dbd7f4e6abf2ec1ef185b55de29a1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/12/2017
---
# <a name="verify-the-ad-fs-20-migration-to-windows-server-2012-r2"></a>Vérifier le services AD FS 2.0 la migration vers Windows Server 2012 R2

Une fois que vous effectuez la même migration serveur de votre batterie de serveurs du Service de fédération Active Directory (AD FS) vers Windows Server 2012 R2, vous pouvez utiliser la procédure suivante pour vérifier que les serveurs de fédération dans votre batterie sont opérationnels; Autrement dit, qu’un client sur le même réseau peut accéder à vos serveurs federtation.  
  
L’appartenance au groupe **utilisateurs**, **opérateurs de sauvegarde**, **utilisateurs avec pouvoir**, **administrateurs** ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.
  
### <a name="to-verify-that-a-federation-server-is-operational"></a>Pour vérifier qu’un serveur de fédération est opérationnel  
  
1.  Ouvrez une fenêtre de navigateur et dans la barre d’adresses, tapez le nom de serveurs de fédération et ajoutez `federationmetadata/2007-06/federationmetadata.xml` pour rechercher le point de terminaison du métadonnées du service de fédération. Par exemple, `https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml` .  
  
Si dans votre fenêtre de navigateur, vous pouvez voir les métadonnées du serveur de fédération sans éventuels avertissements ou erreurs SSL, votre serveur de fédération est opérationnel.  
  
2.  Vous pouvez également accéder à la page de connexion AD FS (nom du service FS suivi `adfs/ls/idpinitiatedsignon.htm`, par exemple, `https://fs.contoso.com/adfs/ls/idpinitiatedsignon.htm`).  Cela affiche la page de connexion AD FS où vous pouvez vous connecter avec les informations d’identification d’administrateur de domaine.  
  
> [!IMPORTANT]
>  Veillez à configurer les paramètres de votre navigateur pour approuver le rôle de serveur de fédération en ajoutant le nom de votre service de fédération (par exemple, `https://fs.contoso.com`) à la zone intranet local du navigateur.  
  
## <a name="next-steps"></a>Étapes suivantes
 [Migrer le rôle Services de fédération Active Directory pour Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Préparation de la migration du serveur de fédération AD FS](prepare-migrate-ad-fs-server-r2.md)  
 [Migration du serveur de fédération AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migration du serveur Proxy de fédération AD FS](migrate-fed-server-proxy-r2.md)   
