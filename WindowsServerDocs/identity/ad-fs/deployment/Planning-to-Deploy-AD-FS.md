---
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: "Planification du déploiement d’AD FS"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ca9e53d7d98f3ae5e6b7b329e52d4979e8c10215
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="planning-to-deploy-ad-fs"></a>Planification du déploiement d’AD FS

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012


Une fois que vous collectez des informations relatives à votre environnement et que vous décidez sur une conception d’ActiveDirectory Federation Services \(ADFS\) en suivant les instructions dans le [Guide de conception ADFS dans Windows Server2012](https://technet.microsoft.com/library/dd807036.aspx), vous pouvez commencer à planifier le déploiement de la conception d’ADFS de votre organisation. Avec la conception terminée et les informations contenues dans cette rubrique, vous pouvez déterminer quelles tâches exécuter pour déployer ADFS dans votre organisation.  
  
## <a name="reviewing-your-ad-fs-design"></a>Examen de votre conception ADFS  
Si l’équipe de conception qui construit le ADFS d’origine de conception de votre organisation est différente de l’équipe de déploiement qui sera réellement le déploiement, assurez-vous que l’équipe de déploiement examine la conception finale avec l’équipe de conception. Passez en revue les points suivants relatifs à la conception:  
  
-   Stratégie de l’équipe de conception pour déterminer la meilleure topologie physique pour le positionnement des serveurs de fédération dans votre réseau d’entreprise ou d’un réseau de périmètre. L’équipe de déploiement peut faire référence à la documentation sur ce sujet en consultant les rubriques suivantes dans le Guide de conception ADFS:  
  
    -   [Le rôle de la base de données de Configuration ADFS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)  
  
    -   [Planification du Placement de serveur de fédération](https://technet.microsoft.com/library/dd807069.aspx)  
  
    -   [Planification du positionnement des serveurs Proxy de fédération](https://technet.microsoft.com/library/dd807130.aspx)  
  
    Il est possible que l’équipe de conception puisse laisser du serveur de fédération ou le placement de proxy de serveur de fédération pour l’équipe de déploiement. L’équipe de déploiement est ensuite chargée de documentation et de l’implémentation de la topologie physique des serveurs.  
  
-   Les raisons professionnelles motivant la désignation de votre organisation en tant qu’un fournisseur de revendications, de confiance ou les deux, dans l’étendue de la conception ADFS documentée. Vérifiez que les membres de l’équipe de déploiement comprennent les raisons pourquoi ADFS est déployé et quelles autres sociétés ou organisations sont impliqués dans la relation de fédération. Vérifiez que les membres de l’équipe de déploiement comprennent également les contraintes qui existent pour les autres sociétés ou organisations \ (limitée forth\ tel est le cas, aucun environnement extranet et matériel) qui sont susceptibles de limiter l’étendue de la conception d’une certaine façon. Pour plus d’informations sur les organisations partenaires, voir [planification de votre déploiement](https://technet.microsoft.com/library/dd807083.aspx).  
  
Une fois la conception équipes et des équipes de déploiement sont d’accord sur ces problèmes, elles peuvent lancer le déploiement de la conception ADFS. Pour plus d’informations, voir [implémentation de votre Plan de conception ADFS](Implementing-Your-AD-FS-Design-Plan.md).  
