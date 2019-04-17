---
ms.assetid: e02bb152-d0db-40b0-9942-846dce75f6c7
title: "Configuration requise du déploiement ADDS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7edd7de8077a077245416f859838a6bc55415edc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-deployment-requirements"></a>Configuration requise du déploiement ADDS

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

La structure de votre environnement existant détermine votre stratégie pour le déploiement de Windows Server2008 ActiveDirectory Services de domaine (ADDS). Si vous créez un environnement de domaine ActiveDirectory et que vous ne disposez pas d’une structure de domaine existant, effectuez votre conception de domaine ActiveDirectory avant de commencer la création de votre environnement de domaine ActiveDirectory. Ensuite, vous pouvez déployer un domaine racine de forêt et déployer le reste de votre structure de domaine en fonction de votre conception.  
  
En outre, dans le cadre de votre déploiement ADDS, vous pouvez décider de mettre à niveau et la restructuration de votre environnement. Par exemple, si votre organisation dispose d’une structure de domaine Windows2000 existante, vous posez effectuer une mise à niveau sur place de certains domaines et d’autres restructurer. En outre, vous pouvez décider de réduire la complexité de votre environnement de restructuration de domaines entre forêts ou d’une restructuration de domaines dans une forêt après avoir déployé les services ADDS.  
  
## <a name="deploying-a-windows-server-2008-forest-root-domain"></a>Déploiement d’un domaine racine de forêt Windows Server2008  
Le domaine racine de forêt fournit la base de votre infrastructure de la forêt ADDS. Pour déployer les services ADDS, vous devez tout d’abord déployer un domaine racine de forêt. Pour ce faire, vous devez passer en revue votre conception de domaine ActiveDirectory; configurer le service DNS pour le domaine racine de forêt; créer le domaine racine de forêt, qui est constituée de déploiement de contrôleurs de domaine racine de forêt, la configuration de la topologie de site pour le domaine racine de forêt et la configuration des rôles de maître d’opérations (également appelé opérations à maître uniques flottant ou FSMO); et augmenter les niveaux fonctionnels de forêt et du domaine. L’illustration suivante montre le processus de déploiement d’un domaine racine de forêt.  
  
![Configuration requise de ADDS](media/AD-DS-Deployment-Requirements/033aad0b-25ff-4793-8825-88a6daa01a55.gif)  
  
Pour plus d’informations, voir [déploiement d’un domaine racine de forêt Windows Server2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
## <a name="deploying-windows-server-2008-regional-domains"></a>Déploiement de domaines régionaux Windows Server2008  
Après avoir effectué le déploiement du domaine racine de forêt, vous êtes prêt à déployer de nouveaux domaines régionaux Windows Server2008 qui sont spécifiés par votre conception. Pour ce faire, vous devez déployer des contrôleurs de domaine pour chaque domaine régional. L’illustration suivante montre le processus de déploiement de domaines régionaux.  
  
![Configuration requise de ADDS](media/AD-DS-Deployment-Requirements/89a878c8-9a94-4180-ad43-ca75316a6318.gif)  
  
Pour plus d’informations, voir [déploiement de Windows Server2008domaines régionaux](https://technet.microsoft.com/library/cc755118.aspx).  
  
## <a name="upgrading-active-directory-domains-to-windows-server-2008"></a>La mise à niveau de domaines ActiveDirectory pour Windows Server2008  
Mise à niveau de vos domaines Windows2000 ou Windows Server2003vers des domaines Windows Server2008 est un moyen simple et efficace pour tirer parti des fonctionnalités et des fonctionnalités supplémentaires de Windows Server2008. Vous pouvez mettre à niveau des domaines pour conserver votre configuration réseau et du domaine actuelle tout en améliorant la sécurité, d’extensibilité et la facilité de gestion de votre infrastructure réseau. La mise à niveau à partir de Windows2000 ou Windows Server2003vers Windows Server2008 requiert une configuration réseau minimale. La mise à niveau a également peu d’impact sur les opérations de l’utilisateur. Pour plus d’informations, voir [la mise à niveau de domaines ActiveDirectory pour Windows Server2008 et Windows Server2008R2 domaines ADDS](https://technet.microsoft.com/library/cc731188.aspx).  
  
## <a name="restructuring-ad-ds-domains"></a>Restructuration de domaines ADDS  
Lorsque vous restructurez de domaines entre forêts de Windows Server2008 (restructuration entre forêts), vous pouvez réduire le nombre de domaines dans votre environnement et donc réduire les coûts et la complexité d’administration. Lorsque vous migrez des objets entre forêts dans le cadre de ce processus de restructuration, à la fois les source cible domaine environnements de domaine et existent simultanément. Cela rend vous permet de revenir à l’environnement source pendant la migration, si nécessaire.  
  
Lorsque vous restructurez des domaines Windows Server2008dans une forêt Windows Server2008 (intraforêt restructuration), vous pouvez consolider votre structure de domaine et par conséquent, réduire les coûts et la complexité d’administration. Lorsque vous restructurez des domaines dans une forêt, les comptes migrés n’existent plus dans le domaine source.  
  
Pour plus d’informations sur l’utilisation de l’ActiveDirectory outil de Migration (ADMT) version3.1 (ADMT v3.1) pour la restructuration des domaines, voir [Guide de Migration ADMT v3.1](https://go.microsoft.com/fwlink/?LinkId=93678).  
  


