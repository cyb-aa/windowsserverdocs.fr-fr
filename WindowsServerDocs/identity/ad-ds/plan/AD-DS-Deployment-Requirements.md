---
ms.assetid: e02bb152-d0db-40b0-9942-846dce75f6c7
title: Exigences en matière de déploiement des services AD DS
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 4ab00fe1fa3a40511ba60234025202e9303455f0
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624427"
---
# <a name="ad-ds-deployment-requirements"></a>Exigences en matière de déploiement des services AD DS

> S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La structure de votre environnement existant détermine votre stratégie de déploiement de Windows Server 2008 Active Directory Domain Services (AD DS). Si vous créez un environnement de AD DS et que vous ne disposez pas d’une structure de domaine existante, terminez la conception de votre AD DS avant de commencer à créer votre environnement AD DS. Ensuite, vous pouvez déployer un nouveau domaine racine de forêt et déployer le reste de votre structure de domaine en fonction de votre conception.

En outre, dans le cadre de votre déploiement AD DS, vous pouvez décider de mettre à niveau et de restructurer votre environnement. Par exemple, si votre organisation possède une structure de domaine Windows 2000 existante, vous pouvez effectuer une mise à niveau sur place de certains domaines et restructurer les autres. En outre, vous pouvez décider de réduire la complexité de votre environnement en restructurant les domaines entre les forêts ou en restructurant les domaines au sein d’une forêt après avoir déployé AD DS.

## <a name="deploying-a-windows-server-2008-forest-root-domain"></a>Déploiement d’un domaine racine de forêt Windows Server 2008
Le domaine racine de forêt constitue la base de votre infrastructure de forêt AD DS. Pour déployer AD DS, vous devez d’abord déployer un domaine racine de forêt. Pour ce faire, vous devez passer en revue la conception de votre AD DS. Configurez le service DNS pour le domaine racine de la forêt ; Créez le domaine racine de forêt, qui consiste à déployer des contrôleurs de domaine racine de forêt, à configurer la topologie de site pour le domaine racine de forêt et à configurer des rôles de maître d’opérations (également appelés opérations à maître unique flottant ou FSMO). et augmentent les niveaux fonctionnels de forêt et de domaine. L’illustration suivante montre le processus global de déploiement d’un domaine racine de forêt.

![Configuration requise par les services de domaine Active Directory](media/AD-DS-Deployment-Requirements/033aad0b-25ff-4793-8825-88a6daa01a55.gif)

Pour plus d’informations, consultez [déploiement d’un domaine racine de forêt Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731174(v=ws.10)).

## <a name="deploying-windows-server-2008-regional-domains"></a>Déploiement de domaines régionaux Windows Server 2008
Une fois le déploiement du domaine racine de la forêt terminé, vous êtes prêt à déployer les nouveaux domaines régionaux Windows Server 2008 spécifiés par votre conception. Pour ce faire, vous devez déployer des contrôleurs de domaine pour chaque domaine régional. L’illustration suivante montre le processus de déploiement de domaines régionaux.

![Configuration requise par les services de domaine Active Directory](media/AD-DS-Deployment-Requirements/89a878c8-9a94-4180-ad43-ca75316a6318.gif)

Pour plus d’informations, consultez [déploiement de domaines régionaux Windows Server 2008](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755118(v=ws.10)).

## <a name="upgrading-active-directory-domains-to-windows-server-2008"></a>Mise à niveau des domaines de Active Directory vers Windows Server 2008
La mise à niveau de vos domaines Windows 2000 ou Windows Server 2003 vers des domaines Windows Server 2008 est un moyen efficace et direct de tirer parti des fonctionnalités et fonctionnalités supplémentaires de Windows Server 2008. Vous pouvez mettre à niveau des domaines pour maintenir votre configuration réseau et de domaine actuelle tout en améliorant la sécurité, l’évolutivité et la facilité de gestion de votre infrastructure réseau. La mise à niveau de Windows 2000 ou Windows Server 2003 vers Windows Server 2008 requiert une configuration réseau minimale. La mise à niveau a également un impact minime sur les opérations de l’utilisateur. Pour plus d’informations, consultez [mise à niveau des domaines de Active Directory vers Windows server 2008 et Windows server 2008 R2 AD DS domaines](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731188(v=ws.10)).

## <a name="restructuring-ad-ds-domains"></a>Restructuration de domaines AD DS
Lorsque vous restructurez des domaines entre des forêts Windows Server 2008 (restructuration interforêt), vous pouvez réduire le nombre de domaines dans votre environnement et réduire ainsi la complexité administrative et la surcharge. Lorsque vous migrez des objets entre des forêts dans le cadre de ce processus de restructuration, le domaine source et les environnements de domaine cible existent tous les deux simultanément. Cela vous permet de restaurer l’environnement source au cours de la migration, si nécessaire.

Lorsque vous restructurez des domaines Windows Server 2008 au sein d’une forêt (restructuration) Windows Server 2008, vous pouvez consolider votre structure de domaine et réduire ainsi la complexité administrative et la surcharge. Lorsque vous restructurez des domaines au sein d’une forêt, les comptes migrés n’existent plus dans le domaine source.

Pour plus d’informations sur l’utilisation de l’outil de migration Active Directory (ADMT) version 3,1 (ADMT v 3.1) pour restructurer des domaines, consultez Guide de l' [ADMT : migration et restructuration de Active Directory domaines](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc974332(v=ws.10)).
