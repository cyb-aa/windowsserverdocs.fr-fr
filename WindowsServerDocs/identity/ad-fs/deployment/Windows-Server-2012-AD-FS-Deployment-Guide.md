---
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: Guide de déploiement des services AD FS Windows Server 2012
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1597230fcfde4fe8a9767f0f14c634bc6fabdceb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408253"
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Guide de déploiement des services AD FS Windows Server 2012


Vous pouvez utiliser Active Directory Services \(de fédération® AD FS\) avec le système d’exploitation Windows Server® 2012 pour créer une solution de gestion des identités fédérées qui étend l’identification distribuée, l’authentification et les services d’autorisation\-aux applications basées sur le Web au-delà des limites de l’organisation et de la plateforme. En déployant AD FS, vous pouvez étendre les fonctionnalités de gestion des identités existantes de votre organisation à Internet.  
  
Vous pouvez déployer les services AD FS pour :  
  
-   Fournissez à vos employés ou clients une\-expérience \(SSO\) d'\-authentification\-unique basée sur le Web lorsqu’ils ont besoin d’un accès à distance à des services ou sites Web hébergés en interne.  
  
-   Fournissez à vos employés ou clients une\-expérience d’authentification unique basée sur le Web\-lorsqu’ils accèdent à des services ou sites Web inter-organisationnels à partir des pare-feu de votre réseau.  
  
-   Fournissez à vos employés ou clients un accès transparent\-aux ressources Web de toute organisation partenaire de Fédération sur Internet sans demander aux employés ou aux clients de se connecter plusieurs fois.  
  
-   Conservez un contrôle total sur les identités de vos employés ou\-clients sans \(utiliser d'\)autres fournisseurs d’authentification Windows Live ID, Liberty Alliance, etc.  
  
## <a name="about-this-guide"></a>À propos de ce guide  
Ce guide est conçu pour les administrateurs système et les ingénieurs système. Il fournit des instructions détaillées pour le déploiement d’une conception de AD FS qui a été présélectionnée par vous ou par un spécialiste d’infrastructure ou un architecte système de votre organisation.  
  
Si aucune conception n’a encore été sélectionnée, nous vous recommandons de suivre les instructions de ce guide jusqu’à ce que vous ayez consulté les options de conception dans le [Guide de conception AD FS dans Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) et que vous avez sélectionné la conception la plus appropriée pour votre dernière. Pour plus d’informations sur l’utilisation de ce guide avec une conception déjà sélectionnée, consultez [implémentation de votre plan de conception de AD FS](Implementing-Your-AD-FS-Design-Plan.md).  
  
Une fois que vous avez sélectionné votre conception dans le Guide de conception et que vous avez rassemblé les informations requises sur les revendications, les types de jetons, les magasins d’attributs et d’autres éléments, vous pouvez utiliser ce guide pour déployer votre AD FS conception dans votre environnement de production. Ce guide fournit les étapes pour le déploiement de l’une des conceptions de AD FS principales suivantes :  
  
-   SSO de web  
  
-   SSO de web fédéré  
  
Utilisez les listes de vérification de l' [implémentation de votre plan de conception AD FS](Implementing-Your-AD-FS-Design-Plan.md) pour déterminer la meilleure façon d’utiliser les instructions de ce guide pour déployer votre conception particulière. Pour plus d’informations sur la configuration matérielle et logicielle requise pour le déploiement [d’AD FS, consultez l’annexe A : Examen des exigences](https://technet.microsoft.com/library/ff678034.aspx) de AD FS dans le AD FS Guide de conception.  
  
### <a name="what-this-guide-does-not-provide"></a>Ce que ce guide ne contient pas  
Ce guide ne contient pas :  
  
-   Aide sur le moment où placer les serveurs de Fédération, les serveurs proxy de Fédération ou les serveurs Web dans votre infrastructure réseau existante. Pour plus d’informations, consultez Planification de l' [emplacement des serveurs de Fédération](https://technet.microsoft.com/library/dd807069.aspx) et planification de l’emplacement du [serveur proxy de Fédération](https://technet.microsoft.com/library/dd807130.aspx) dans le Guide de conception de AD FS.  
  
-   Conseils pour l’utilisation des \(autorités\) de certification des autorités de certification pour configurer AD FS  
  
-   Conseils pour la configuration ou la configuration d’applications\-Web spécifiques  
  
-   Instructions spécifiques à la configuration d’un environnement de laboratoire de test.  
  
-   Informations sur la personnalisation des écrans d’ouverture de session fédérée, les fichiers web.config ou la base de données de configuration.  
  
## <a name="in-this-guide"></a>Dans ce guide  
  
-   [Planification du déploiement d’AD FS](Planning-to-Deploy-AD-FS.md)  
  
-   [Implémentation de votre plan de conception AD FS](Implementing-Your-AD-FS-Design-Plan.md)  
  
-   [Liste de vérification : implémentation d’une conception SSO de web](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Liste de vérification : implémentation d’une conception SSO de web fédéré](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
  
-   [Configuration des organisations partenaires](Configuring-Partner-Organizations.md)  
  
-   [Configuration de règles de revendication](Configuring-Claim-Rules.md)  
  
-   [Déploiement de serveurs de fédération](Deploying-Federation-Servers.md)  
  
-   [Déploiement de serveurs proxy de fédération](Deploying-Federation-Server-Proxies.md)  
  
-   [Interaction avec AD FS 1.x](Interoperating-with-AD-FS-1.x.md)  
