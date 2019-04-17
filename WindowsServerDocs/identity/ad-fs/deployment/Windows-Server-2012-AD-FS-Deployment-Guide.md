---
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: "Guide de déploiement de Windows Server 2012 AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3e555d1003878e12320cb8557bd205ac24e1bbb3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Guide de déploiement de Windows Server 2012 AD FS

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Vous pouvez utiliser Active Directory® Federation Services \(AD FS\) avec le système d’exploitation Windows Server® 2012 pour créer une solution de gestion des identités fédérées qui étend identification distribuée, l’authentification et les services d’autorisation pour les applications basées sur la console Web au-delà des limites des plateformes et d’organisation. En déployant AD FS, vous pouvez étendre les fonctionnalités de gestion des identités existantes de votre organisation à Internet.  
  
Vous pouvez déployer AD FS pour:  
  
-   Fournir vos employés ou clients une expérience \(SSO\) console Web, connexion de single\ sur lorsqu’ils ont besoin d’un accès à distance pour les services ou sites Web hébergés en interne.  
  
-   Fournir vos employés ou clients une expérience d’authentification basée sur la console Web, lorsqu’ils accèdent à une nouveauté d’organisation sites Web services ou depuis les pare-feu de votre réseau.  
  
-   Fournir vos employés ou clients un accès transparent aux ressources de console Web dans toute organisation partenaire de fédération sur Internet sans avoir besoin d’employés ou clients à se connecter plusieurs fois.  
  
-   Garder un contrôle total sur les identités de vos employés ou clients sans utiliser d’autres fournisseurs de connexion sur \ (Windows Live ID, Liberty Alliance et others\).  
  
## <a name="about-this-guide"></a>À propos de ce guide  
Ce guide est destiné à être utilisé par les administrateurs système et les ingénieurs système. Il fournit des instructions détaillées pour déployer une conception AD FS qui présélectionnée par vous ou un infrastructure spécialiste ou un architecte système dans votre organisation.  
  
Si une conception n’a pas encore été sélectionnée, nous vous conseillons d’attendre de suivre les instructions de ce guide après avoir consulté les options de conception dans le [Guide de conception AD FS dans Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) et vous avez sélectionné la conception la plus appropriée pour votre organisation. Pour plus d’informations sur l’utilisation de ce guide avec une conception qui a déjà été sélectionnée, voir [implémentation de votre Plan de conception AD FS](Implementing-Your-AD-FS-Design-Plan.md).  
  
Après avoir sélectionné votre conception dans le guide de conception et de collecter les informations requises sur les revendications, les types de jetons, les magasins d’attributs et les autres éléments, vous pouvez utiliser ce guide pour déployer votre conception AD FS dans votre environnement de production. Ce guide fournit les étapes pour le déploiement de modèles AD FS principaux suivants:  
  
-   SSO de Web  
  
-   SSO de Web fédéré  
  
Utilisez les listes de vérification dans [implémentation de votre Plan de conception AD FS](Implementing-Your-AD-FS-Design-Plan.md) pour déterminer la meilleure façon d’utiliser les instructions de ce guide pour déployer votre conception. Pour plus d’informations sur la configuration matérielle et logicielle pour le déploiement d’AD FS, consultez le [annexe a: examen des annonces de la configuration requise pour FS](https://technet.microsoft.com/library/ff678034.aspx) dans le Guide de conception AD FS.  
  
### <a name="what-this-guide-does-not-provide"></a>Ce que ce guide ne fournit pas  
Ce guide ne fournit pas:  
  
-   Conseils en matière de quand et où placer des serveurs de fédération, les serveurs proxy de fédération ou les serveurs Web dans votre infrastructure réseau existante. Pour obtenir ces informations, voir [emplacement des serveurs de fédération planification](https://technet.microsoft.com/library/dd807069.aspx) et [planification du Placement Federation Server Proxy](https://technet.microsoft.com/library/dd807130.aspx) dans le Guide de conception AD FS.  
  
-   Conseils pour l’utilisation de la certification autorités \(CAs\) pour configurer AD FS  
  
-   Conseils pour la configuration des applications de console Web spécifiques  
  
-   Les instructions qui sont spécifiques à la configuration d’un environnement de laboratoire de test d’installation.  
  
-   Informations sur la personnalisation des écrans d’ouverture de session fédérée, les fichiers web.config ou la base de données de configuration.  
  
## <a name="in-this-guide"></a>Dans ce guide  
  
-   [Planification du déploiement d’AD FS](Planning-to-Deploy-AD-FS.md)  
  
-   [Implémentation d’ADFS Plan de conception](Implementing-Your-AD-FS-Design-Plan.md)  
  
-   [Liste de vérification: Implémentation d’une conception SSO de Web](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [Liste de vérification: Implémentation d’une conception SSO de Web fédéré](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
  
-   [Configuration des organisations partenaires](Configuring-Partner-Organizations.md)  
  
-   [Configuration des règles de revendication](Configuring-Claim-Rules.md)  
  
-   [Déploiement de serveurs de fédération](Deploying-Federation-Servers.md)  
  
-   [Déploiement de serveurs proxy de fédération](Deploying-Federation-Server-Proxies.md)  
  
-   [Il interagit avec AD FS 1.x](Interoperating-with-AD-FS-1.x.md)  
