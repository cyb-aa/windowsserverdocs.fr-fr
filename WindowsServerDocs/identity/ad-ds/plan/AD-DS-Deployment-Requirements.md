---
ms.assetid: e02bb152-d0db-40b0-9942-846dce75f6c7
title: Exigences en matière de déploiement des services AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d21491f5ce9c15ecc514e4be24a91de28b0fd0ce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890970"
---
# <a name="ad-ds-deployment-requirements"></a>Exigences en matière de déploiement des services AD DS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La structure de votre environnement existant détermine votre stratégie de déploiement de Windows Server 2008 Active Directory domaine Services (AD DS). Si vous créez un environnement AD DS et que vous n’avez pas d’une structure de domaine existante, terminer votre conception d’AD DS avant de commencer la création de votre environnement AD DS. Ensuite, vous pouvez déployer un domaine racine de forêt et déployer le reste de votre structure de domaine en fonction de votre conception.  
  
En outre, dans le cadre de votre déploiement AD DS, vous pouvez décider à mettre à niveau et restructurer votre environnement. Par exemple, si votre organisation dispose d’une structure de domaine Windows 2000 existante, vous pourrez effectuer une mise à niveau in situ de certains domaines et restructurer d’autres. En outre, vous pouvez décider de réduire la complexité de votre environnement par la restructuration de domaines entre forêts ou de restructuration de domaines dans une forêt après avoir déployé les services AD DS.  
  
## <a name="deploying-a-windows-server-2008-forest-root-domain"></a>Déploiement d’un domaine racine de forêt Windows Server 2008  
Le domaine racine de forêt fournit la base de votre infrastructure de forêt AD DS. Pour déployer les services AD DS, vous devez d’abord déployer un domaine racine de forêt. Pour ce faire, vous devez passer en revue votre conception des services AD DS ; configurer le service DNS pour le domaine racine de forêt ; créer le domaine racine, qui se compose d’un déploiement de contrôleurs de domaine racine de forêt, la configuration de la topologie de site pour le domaine racine de forêt et la configuration des rôles de maître d’opérations (également appelés opérations à maître uniques flottant ou FSMO) ; et augmenter les niveaux fonctionnels de forêt et du domaine. L’illustration suivante montre le processus de déploiement d’un domaine racine de forêt.  
  
![Configuration requise par les services de domaine Active Directory](media/AD-DS-Deployment-Requirements/033aad0b-25ff-4793-8825-88a6daa01a55.gif)  
  
Pour plus d’informations, consultez [déploiement d’un domaine racine de forêt Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
## <a name="deploying-windows-server-2008-regional-domains"></a>Déploiement de domaines régionaux de Windows Server 2008  
Après avoir terminé le déploiement du domaine racine de forêt, vous êtes prêt à déployer de nouveaux domaines régionaux Windows Server 2008 qui sont spécifiées par votre conception. Pour ce faire, vous devez déployer des contrôleurs de domaine pour chaque domaine régional. L’illustration suivante montre le processus de déploiement de domaines régionaux.  
  
![Configuration requise par les services de domaine Active Directory](media/AD-DS-Deployment-Requirements/89a878c8-9a94-4180-ad43-ca75316a6318.gif)  
  
Pour plus d’informations, consultez [déploiement de Windows Server 2008 domaines régionaux](https://technet.microsoft.com/library/cc755118.aspx).  
  
## <a name="upgrading-active-directory-domains-to-windows-server-2008"></a>La mise à niveau de domaines Active Directory pour Windows Server 2008  
La mise à niveau de vos domaines Windows 2000 ou Windows Server 2003 à des domaines Windows Server 2008 est un moyen simple et efficace pour tirer parti des fonctionnalités et des fonctionnalités supplémentaires de Windows Server 2008. Vous pouvez mettre à niveau des domaines pour maintenir la configuration actuelle de votre réseau et domaine tout en améliorant la sécurité, d’évolutivité et la facilité de gestion de votre infrastructure réseau. La mise à niveau à partir de Windows 2000 ou Windows Server 2003 vers Windows Server 2008 nécessite une configuration réseau minimale. La mise à niveau a également un impact limité sur les opérations de l’utilisateur. Pour plus d’informations, consultez [la mise à niveau de domaines Active Directory pour Windows Server 2008 et Windows Server 2008 R2 domaines AD DS](https://technet.microsoft.com/library/cc731188.aspx).  
  
## <a name="restructuring-ad-ds-domains"></a>Restructuration de domaines AD DS  
Lorsque vous Restructurez les domaines entre forêts Windows Server 2008 (restructuration entre forêts), vous pouvez réduire le nombre de domaines dans votre environnement et par conséquent réduire la complexité d’administration et charge. Lorsque vous migrez des objets entre forêts dans le cadre de ce processus de restructuration, à la fois les source cible domaine environnements de domaine et existent simultanément. Cela rend possible pour vous permettre de revenir à l’environnement source pendant la migration, si nécessaire.  
  
Lorsque vous Restructurez les domaines de Windows Server 2008 dans une forêt Windows Server 2008 (intraforêt restructuration), vous pouvez consolider votre structure de domaine et par conséquent réduire la complexité d’administration et charge. Lorsque vous Restructurez les domaines au sein d’une forêt, les comptes migrés n’existent plus dans le domaine source.  
  
Pour plus d’informations sur l’utilisation de l’Active Directory Migration outil (ADMT) version 3.1 (ADMT v3.1) de restructurer des domaines, consultez [Migration Guide ADMT v3.1](https://go.microsoft.com/fwlink/?LinkId=93678).  
  


