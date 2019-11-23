---
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: Planification du déploiement d’AD FS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ed2082706975d58a1535aaeb61e6c5283d23306a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359500"
---
# <a name="planning-to-deploy-ad-fs"></a>Planification du déploiement d’AD FS


Une fois que vous avez collecté les informations relatives à votre environnement et que vous décidez d’un Services ADFS \(AD FS conception de\) en suivant les instructions du [Guide de conception AD FS dans Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx), vous pouvez commencer à planifier le déploiement de la conception de AD FS de votre organisation. Une fois la conception terminée et les informations contenues dans cette rubrique, vous pouvez déterminer les tâches à effectuer pour déployer AD FS dans votre organisation.  
  
## <a name="reviewing-your-ad-fs-design"></a>Examen de votre conception AD FS  
Si l’équipe de conception qui a construit la conception de la AD FS d’origine pour votre organisation est différente de celle qui implémente le déploiement, assurez-vous que l’équipe de déploiement examine la conception finale avec l’équipe de conception. Passez en revue les points suivants relatifs à la conception :  
  
-   La stratégie de l'équipe de conception pour déterminer la meilleure topologie physique pour la sélection élective des serveurs de fédération dans votre réseau d'entreprise ou de périmètre. L’équipe de déploiement peut consulter la documentation relative à ce sujet en consultant les rubriques suivantes du Guide de conception de AD FS :  
  
    -   [Rôle de la base de données de configuration AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)  
  
    -   [Planification de la sélection élective du serveur de fédération](https://technet.microsoft.com/library/dd807069.aspx)  
  
    -   [Planification de la sélection élective du serveur proxy de fédération](https://technet.microsoft.com/library/dd807130.aspx)  
  
    L'équipe de conception peut déléguer les tâches de sélection élective du serveur de fédération ou du serveur proxy de fédération à l'équipe de déploiement. L'équipe de déploiement est ensuite chargée de la documentation et de l'implémentation de la topologie physique des serveurs.  
  
-   Les raisons professionnelles motivant la désignation de votre organisation en tant que fournisseur de revendications ou partie de confiance, ou les deux à la fois, dans le cadre de la conception AD FS documentée. Assurez-vous que les membres de l’équipe de déploiement comprennent les raisons pour lesquelles AD FS est déployé et les autres sociétés ou organisations impliquées dans le partenariat de Fédération. Assurez-vous que les membres de l’équipe de déploiement comprennent également les contraintes qui existent pour les autres sociétés ou organisations \(un matériel limité, aucun environnement extranet, et ainsi de suite\) qui peut limiter l’étendue de la conception d’une façon ou d’une autre. Pour plus d'informations sur les organisations partenaires, voir [Planification de votre déploiement](https://technet.microsoft.com/library/dd807083.aspx).  
  
Une fois que les équipes de conception et les équipes de déploiement s’accordent sur ces problèmes, elles peuvent poursuivre le déploiement de la conception de la AD FS. Pour plus d'informations, voir [Implémentation de votre plan de conception AD FS](Implementing-Your-AD-FS-Design-Plan.md).  
