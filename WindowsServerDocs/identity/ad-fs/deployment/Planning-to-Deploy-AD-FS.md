---
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: Planification du déploiement d’AD FS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1459cade5071374ca39d453b9915a68e4bcfe539
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192039"
---
# <a name="planning-to-deploy-ad-fs"></a>Planification du déploiement d’AD FS


Une fois que vous collectez des informations relatives à votre environnement et vous décidez sur un Active Directory Federation Services \(AD FS\) conception en suivant les instructions dans le [Guide de conception AD FS dans Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx), Vous pouvez commencer à planifier le déploiement de la conception d’AD FS de votre organisation. Avec la conception terminée et les informations contenues dans cette rubrique, vous pouvez déterminer les tâches à effectuer pour déployer AD FS dans votre organisation.  
  
## <a name="reviewing-your-ad-fs-design"></a>Examen de votre conception AD FS  
Si l’équipe de conception AD FS d’origine de la construction de conception pour votre organisation est différente de l’équipe de déploiement qui sera réellement mettre en œuvre le déploiement, assurez-vous que l’équipe de déploiement examine la conception finale avec l’équipe de conception. Passez en revue les points suivants relatifs à la conception :  
  
-   La stratégie de l'équipe de conception pour déterminer la meilleure topologie physique pour la sélection élective des serveurs de fédération dans votre réseau d'entreprise ou de périmètre. L’équipe de déploiement peut consulter la documentation sur ce sujet en consultant les rubriques suivantes dans le Guide de conception AD FS :  
  
    -   [Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)  
  
    -   [Planification de la sélection élective du serveur de fédération](https://technet.microsoft.com/library/dd807069.aspx)  
  
    -   [Planification de la sélection élective du serveur proxy de fédération](https://technet.microsoft.com/library/dd807130.aspx)  
  
    L'équipe de conception peut déléguer les tâches de sélection élective du serveur de fédération ou du serveur proxy de fédération à l'équipe de déploiement. L'équipe de déploiement est ensuite chargée de la documentation et de l'implémentation de la topologie physique des serveurs.  
  
-   Les raisons professionnelles motivant la désignation de votre organisation en tant que fournisseur de revendications ou partie de confiance, ou les deux à la fois, dans le cadre de la conception AD FS documentée. Vérifiez que les membres de l’équipe de déploiement comprennent les raisons pourquoi AD FS est déployée et quelles autres sociétés ou organisations sont impliqués dans la relation de fédération. Vérifiez que les membres de l’équipe de déploiement comprennent également les contraintes qui existent pour les autres sociétés ou organisations \(limitée de matériel, aucun environnement extranet et ainsi de suite\) qui peut limiter l’étendue de la conception d’une certaine façon. Pour plus d'informations sur les organisations partenaires, voir [Planification de votre déploiement](https://technet.microsoft.com/library/dd807083.aspx).  
  
Après la conception équipes et des équipes de déploiement d’accord sur ces problèmes, elles peuvent lancer le déploiement de la conception d’AD FS. Pour plus d'informations, voir [Implémentation de votre plan de conception AD FS](Implementing-Your-AD-FS-Design-Plan.md).  
