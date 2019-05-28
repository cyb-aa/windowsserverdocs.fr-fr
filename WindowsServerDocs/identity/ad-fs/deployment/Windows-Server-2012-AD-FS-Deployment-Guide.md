---
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: Guide de déploiement des services AD FS Windows Server 2012
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6be56c25cc6f639f73842f57cdf48a6339dccf9c
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191852"
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Guide de déploiement des services AD FS Windows Server 2012


Vous pouvez utiliser Active Directory® Federation Services \(AD FS\) avec le système d’exploitation Windows Server® 2012 pour créer une solution de gestion d’identité fédérée qui étend distribuée d’identification, l’authentification, et services d’autorisation sur le Web\-des applications basées sur au-delà des limites de plate-forme et d’organisation. En déployant les services AD FS, vous pouvez étendre les fonctionnalités de gestion d'identités existantes de votre organisation à Internet.  
  
Vous pouvez déployer les services AD FS pour :  
  
-   Fournissez vos employés ou clients et un serveur Web\-en, unique\-connexion\-sur \(SSO\) expérience lorsqu’ils ont besoin en interne un accès à distance à hosted services ou sites Web.  
  
-   Fournissez vos employés ou clients et un serveur Web\-en, expérience d’authentification unique lorsqu’ils accèdent à croisé\-des sites Web d’organisation ou services dans les pare-feu de votre réseau.  
  
-   Fournir vos employés ou clients un accès transparent à Web\-en fonction des ressources dans toute organisation partenaire de fédération sur Internet sans demander aux employés ou clients pour se connecter plusieurs fois.  
  
-   Garder un contrôle total sur les identités de vos employés ou clients sans utiliser d’autre signe\-sur les fournisseurs \(Windows Live ID, Liberty Alliance et autres\).  
  
## <a name="about-this-guide"></a>À propos de ce guide  
Ce guide est conçu pour les administrateurs système et les ingénieurs système. Il fournit des instructions détaillées pour le déploiement d’une conception AD FS a été choisie par vous ou un infrastructure spécialiste ou un architecte système dans votre organisation.  
  
Si une conception n’a pas encore été sélectionnée, nous vous recommandons d’attendre de suivre les instructions fournies dans ce guide jusqu’après avoir examiné les options de conception dans le [Guide de conception AD FS dans Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) et que vous avez sélectionné le plus conception appropriée pour votre organisation. Pour plus d’informations sur l’utilisation de ce guide avec une conception qui a déjà été sélectionnée, consultez [implémenter votre Plan de conception AD FS](Implementing-Your-AD-FS-Design-Plan.md).  
  
Après avoir sélectionné votre conception dans le guide de conception et rassembler les informations requises sur les revendications, les types de jetons, les magasins d’attributs et les autres éléments, vous pouvez utiliser ce guide pour déployer votre conception AD FS dans votre environnement de production. Ce guide fournit les étapes pour le déploiement des conceptions AD FS principales suivantes :  
  
-   SSO de web  
  
-   SSO de web fédéré  
  
Utilisez les listes de contrôle [implémenter votre Plan de conception AD FS](Implementing-Your-AD-FS-Design-Plan.md) pour déterminer la meilleure façon d’utiliser les instructions de ce guide pour déployer votre conception. Pour plus d’informations sur la configuration requise matérielle et logicielle pour le déploiement d’AD FS, consultez le [annexe a : Examen des exigences en matière de AD FS](https://technet.microsoft.com/library/ff678034.aspx) dans la conception AD FS Guide.  
  
### <a name="what-this-guide-does-not-provide"></a>Ce que ce guide ne contient pas  
Ce guide ne contient pas :  
  
-   Conseils en matière de quand et où placer les serveurs de fédération, les serveurs proxy de fédération ou les serveurs Web dans votre infrastructure réseau existante. Pour obtenir ces informations, consultez [emplacement des serveurs de fédération planification](https://technet.microsoft.com/library/dd807069.aspx) et [planification Federation Server Proxy Placement](https://technet.microsoft.com/library/dd807130.aspx) dans le Guide de conception AD FS.  
  
-   Conseils d’utilisation des autorités de certification \(autorités de certification\) pour configurer AD FS  
  
-   Conseils pour la configuration ou de la configuration Web spécifique\-en fonction des applications  
  
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
