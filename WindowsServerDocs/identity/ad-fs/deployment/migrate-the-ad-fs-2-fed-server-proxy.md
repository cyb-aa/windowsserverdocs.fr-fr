---
title: Migrer le serveur proxy de fédération AD FS 2,0
description: Fournit des informations sur la migration du serveur proxy de fédération AD FS vers Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e97a1b3b66d7c12c96570022b7e69caaf0e92f65
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408228"
---
# <a name="migrate-the-ad-fs-20-federation-server-proxy"></a>Migrer le serveur proxy de fédération AD FS 2,0
Ce document fournit des informations détaillées sur la migration d’un serveur proxy de fédération AD FS 2,0 vers Windows Server 2012.

## <a name="migrate-the-proxy"></a>Migrer le proxy

Pour migrer un proxy de serveur de fédération AD FS 2,0 vers Windows Server 2012, procédez comme suit :  
  
1.  Pour chaque serveur proxy de Fédération que vous envisagez de migrer vers Windows Server 2012, examinez et exécutez les procédures décrites dans [préparer la migration du serveur proxy de fédération AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md).  
  
2.  Supprimez un serveur proxy de fédération de l’équilibrage de charge.  
  
3.  Effectuez une mise à niveau sur place du système d’exploitation sur ce serveur de Windows Server 2008 R2 ou Windows Server 2008 vers Windows Server 2012. Pour plus d’informations, voir [Installation de Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  La mise à niveau du système d’exploitation entraîne la perte de la configuration du proxy AD FS sur ce serveur et la suppression du rôle serveur AD FS 2.0. Le rôle serveur AD FS Windows Server 2012 est installé à la place, mais il n’est pas configuré. Vous devez manuellement créer la configuration du proxy AD FS d’origine et restaurer les paramètres du proxy AD FS restants pour terminer la migration du serveur proxy de fédération.  
  
4. Créez la configuration du proxy AD FS d’origine, à l’aide de l’ **Assistant Configuration du serveur proxy de fédération AD FS**. Pour plus d'informations, voir [Configure a Computer for the Federation Server Proxy Role](configure-a-computer-for-the-federation-server-proxy-role.md). Lorsque vous exécutez l’Assistant, utilisez les informations collectées dans la préparation à la migration du serveur proxy de fédération AD FS 2.0 comme suit :  
  
 
|**Option d’entrée de l’Assistant proxy de serveur de Fédération**|**Utilisez la valeur suivante**|
|-----|-----|  
|**Nom de l’service FS (Federation Service)**|Entrez la valeur BaseHostName issue du fichier proxyproperties.txt|  
|Case à cocher**Utiliser un serveur proxy HTTP pour transmettre des demandes à ce service de fédération**|Cochez cette case si votre fichier proxyproperties.txt contient une valeur pour la propriété ForwardProxyUrl|  
|**Adresse du serveur proxy HTTP**|Entrez la valeur ForwardProxyUrl issue du fichier proxyproperties.txt|  
|Demande d'informations d'identification|Entrez les informations d’identification d’un compte qui est soit un administrateur du serveur de fédération AD FS ou le compte de service sous lequel s’exécute le service de fédération AD FS.|  
  
5. Mettez à jour vos pages web AD FS sur ce serveur. Si vous avez sauvegardé vos pages Web de proxy AD FS personnalisées lors de la préparation de votre serveur proxy de Fédération pour la migration, utilisez vos données de sauvegarde pour remplacer les pages Web par défaut AD FS qui ont été créées par défaut dans le répertoire **%systemdrive%\inetpub\adfs\ls** à la suite de la configuration du proxy AD FS dans Windows Server 2012.  
  
6. Ajoutez ce serveur à l’équilibrage de charge.  
  
7. Si vous avez d’autres serveurs proxy de fédération AD FS 2.0 à migrer, répétez les étapes 2 à 6 pour les autres ordinateurs du serveur proxy de fédération.  
  
  
## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur de fédération AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration du serveur proxy de fédération AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur de fédération AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur proxy de fédération AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les agents web AD FS 1.1](migrate-the-ad-fs-web-agent.md)