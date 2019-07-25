---
title: Migrer le serveur proxy AD FS 2.0 de fédération
description: Fournit des informations sur la migration du serveur proxy de fédération AD FS vers Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: eddb0d3432c69ecff4542ff1b8f2204b96ce0820
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445663"
---
# <a name="migrate-the-ad-fs-20-federation-server-proxy"></a>Migrer le serveur proxy AD FS 2.0 de fédération
Ce document fournit des informations détaillées sur la migration d’un serveur de proxy 2.0 de fédération AD FS vers Windows Server 2012.

## <a name="migrate-the-proxy"></a>Migration du proxy

Pour migrer un serveur proxy de fédération 2.0 AD FS vers Windows Server 2012, procédez comme suit :  
  
1.  Pour chaque serveur proxy de fédération que vous projetez de migrer vers Windows Server 2012, consultez et effectuez les procédures décrites dans [préparer la migration d’AD FS 2.0 serveur Proxy de fédération](prepare-to-migrate-ad-fs-fed-proxy.md).  
  
2.  Supprimez un serveur proxy de fédération de l’équilibrage de charge.  
  
3.  Effectuer une mise à niveau sur place du système d’exploitation sur ce serveur à partir de Windows Server 2008 R2 ou Windows Server 2008 vers Windows Server 2012. Pour plus d’informations, voir [Installation de Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  La mise à niveau du système d’exploitation entraîne la perte de la configuration du proxy AD FS sur ce serveur et la suppression du rôle serveur AD FS 2.0. Le rôle de serveur AD FS de Windows Server 2012 est installé à la place, mais il n’est pas configuré. Vous devez manuellement créer la configuration du proxy AD FS d’origine et restaurer les paramètres du proxy AD FS restants pour terminer la migration du serveur proxy de fédération.  
  
4. Créez la configuration du proxy AD FS d’origine, à l’aide de l’ **Assistant Configuration du serveur proxy de fédération AD FS**. Pour plus d’informations, consultez [configurer un ordinateur pour le rôle de Proxy de serveur de fédération](configure-a-computer-for-the-federation-server-proxy-role.md). Lorsque vous exécutez l’Assistant, utilisez les informations collectées dans la préparation à la migration du serveur proxy de fédération AD FS 2.0 comme suit :  
  
 
|**Option de saisie de l’Assistant serveur Proxy de fédération**|**Utilisez la valeur suivante**|
|-----|-----|  
|**Nom de Service de fédération**|Entrez la valeur BaseHostName issue du fichier proxyproperties.txt|  
|Case à cocher**Utiliser un serveur proxy HTTP pour transmettre des demandes à ce service de fédération**|Cochez cette case si votre fichier proxyproperties.txt contient une valeur pour la propriété ForwardProxyUrl|  
|**Adresse du serveur proxy HTTP**|Entrez la valeur ForwardProxyUrl issue du fichier proxyproperties.txt|  
|Demande d'informations d'identification|Entrez les informations d’identification d’un compte qui est soit un administrateur du serveur de fédération AD FS ou le compte de service sous lequel s’exécute le service de fédération AD FS.|  
  
5. Mettez à jour vos pages web AD FS sur ce serveur. Si vous avez sauvegardé vos pages Web de proxy AD FS personnalisées pendant la préparation de votre serveur proxy de fédération de la migration, utiliser vos données de sauvegarde pour remplacer les pages Web AD FS qui ont été créés par défaut dans le **%systemdrive%\inetpub\adfs\ls** répertoire à la suite de la configuration du proxy AD FS dans Windows Server 2012.  
  
6. Ajoutez ce serveur à l’équilibrage de charge.  
  
7. Si vous avez d’autres serveurs proxy de fédération AD FS 2.0 à migrer, répétez les étapes 2 à 6 pour les autres ordinateurs du serveur proxy de fédération.  
  
  
## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration du serveur Proxy pour AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur AD FS 2.0 de fédération](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur Proxy AD FS 2.0 de fédération](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les agents web AD FS 1.1](migrate-the-ad-fs-web-agent.md)