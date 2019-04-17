---
title: "Migrer le serveur proxy AD FS 2.0 de fédération"
description: "Fournit des informations sur la migration du serveur proxy de fédération AD FS vers Windows Server 2012."
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 98e28c9be808f63ed39a3ac24dd95014b388d001
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-the-ad-fs-20-federation-server-proxy"></a>Migrer le serveur proxy AD FS 2.0 de fédération
Ce document fournit des informations détaillées sur la migration d’un serveur de proxy AD FS 2.0 de fédération vers Windows Server 2012.

## <a name="migrate-the-proxy"></a>Migrer le serveur proxy

Pour migrer un serveur proxy de fédération 2.0 AD FS vers Windows Server 2012, effectuez la procédure suivante:  
  
1.  Pour chaque serveur proxy de fédération que vous projetez de migrer vers Windows Server 2012, consultez et effectuez les procédures de [préparer la migration AD FS 2.0 serveur Proxy de fédération](prepare-to-migrate-ad-fs-fed-proxy.md).  
  
2.  Supprimer un serveur proxy de fédération de l’équilibrage de charge.  
  
3.  Effectuez une mise à niveau sur place du système d’exploitation sur ce serveur à partir de Windows Server 2008 R2 ou Windows Server 2008 vers Windows Server 2012. Pour plus d’informations, voir [l’installation de Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Le résultat de la mise à niveau du système d’exploitation, la configuration du proxy AD FS sur ce serveur est perdue et le rôle de serveur 2.0 AD FS est supprimé. Le rôle de serveur AD FS Windows Server 2012 est installé à la place, mais il n’est pas configuré. Vous devez manuellement créer la configuration du proxy AD FS d’origine et restaurer les paramètres de proxy AD FS restants pour terminer la migration de proxy de serveur de fédération.  
  
4.  Créer la configuration du proxy AD FS d’origine à l’aide de la **Assistant Configuration de Proxy du serveur de fédération AD FS**. Pour plus d’informations, voir [configurer un ordinateur pour le rôle de Proxy de serveur de fédération](configure-a-computer-for-the-federation-server-proxy-role.md). Lorsque vous exécutez l’Assistant, utilisez les informations collectées dans la préparation à la migration AD FS 2.0 serveur Proxy de fédération comme suit:  
  
 
|**Option de saisie de l’Assistant serveur Proxy de fédération**|**Utilisez la valeur suivante**|
|-----|-----|  
|**Nom de Service de fédération**|Entrez la valeur basehostname issue du fichier proxyproperties.txt|  
|**Utiliser un serveur proxy HTTP lors de l’envoi des demandes de fédération de cette** case à cocher Service|Cochez cette case si votre fichier proxyproperties.txt contient une valeur pour la propriété ForwardProxyUrl|  
|**Adresse du serveur proxy HTTP**|Entrez la valeur ForwardProxyUrl du fichier proxyproperties.txt|  
|Informations d’identification|Entrez les informations d’identification d’un compte qui est soit un administrateur du serveur de fédération AD FS ou le compte de service sous lequel s’exécute le service de fédération AD FS.|  
  
5.  Mettre à jour vos pages Web AD FS sur ce serveur. Si vous avez sauvegardé vos pages Web de proxy AD FS personnalisées pendant la préparation de votre serveur proxy de fédération de la migration, utiliser vos données de sauvegarde pour remplacer les pages Web AD FS qui ont été créés par défaut dans le **%systemdrive%\inetpub\adfs\ls** répertoire suite à la configuration du proxy AD FS dans Windows Server 2012.  
  
6.  Ajouter ce serveur à l’équilibrage de charge.  
  
7.  Si vous avez d’autres serveurs proxy AD FS 2.0 de fédération à migrer, répétez les étapes 2 à 6 pour les autres ordinateurs de proxy de serveur de fédération.  
  
  
## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration AD FS 2.0 serveur Proxy de fédération](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur AD FS 2.0 de fédération](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur Proxy AD FS 2.0 de fédération](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les Agents de 1.1 Web AD FS](migrate-the-ad-fs-web-agent.md)