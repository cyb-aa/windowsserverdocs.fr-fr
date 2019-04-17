---
ms.assetid: c81b8291-fba5-4b30-a43d-7feb2f4b66be
title: Guide de conception ADFS dans Windows Server2012R2
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f618add4c4803142b3bd7278908834a412f30999
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="identify-your-ad-fs-deployment-goals"></a>Identifier vos objectifs de déploiement ADFS

>S’applique à: Windows Server2016, Windows Server2012R2

Identification des objectifs de votre déploiement Active Directory Federation Services \(AD FS\) est essentielle à la réussite de votre projet de conception AD FS. Hiérarchisez et, éventuellement, combinez vos objectifs de déploiement afin que vous pouvez concevoir et déployer AD FS à l’aide d’une approche itérative. Vous pouvez tirer parti d’existant, documentés et prédéfinis des objectifs de déploiement d’AD FS qui sont pertinentes pour les conceptions AD FS et développement une solution adaptée à votre situation.  
  
Les versions antérieures d’AD FS ont été couramment déployées pour effectuer les opérations suivantes:  
  
-   Fournir à vos employés ou clients une console Web-expérience d’authentification unique lors de l’accès aux applications basées sur les claims\ au sein de votre entreprise.  
  
-   Fournir à vos employés ou clients une expérience d’authentification basée sur la console Web, pour accéder aux ressources dans toute organisation partenaire de fédération.  
  
-   Fournir à vos employés ou clients une console Web-expérience d’authentification unique lors de l’accès à distant en interne hébergé services ou sites Web.  
  
-   Fournir à vos employés ou clients une expérience d’authentification basée sur la console Web, lors de l’accès aux ressources ou services dans le cloud.  
  
En plus des, AD FS dans Windows Server® 2012 R2 ajoute des fonctionnalités qui peuvent vous aider à effectuer les opérations suivantes:  
  
-   Jonction à l’appareil pour l’authentification unique et transparente l’authentification facteur. Cela permet aux organisations d’autoriser l’accès à partir d’appareils personnels de l’utilisateur et de gérer les risques lors de la fourniture de cet accès.  
  
-   Gestion des risques avec contrôle d’accès prennent facteurs. AD FS fournit un niveau performant d’autorisation qui contrôle qui a accès à quelles applications. Cela peut être basé sur des attributs utilisateur \ (UPN, courrier électronique, l’appartenance au groupe de sécurité, force de l’authentification, etc. \), attributs de l’appareil \ (si le périphérique est joined\ de l’espace de travail) ou demander des attributs \ (réseau, adresse IP ou agent\ utilisateur).  
  
-   Gestion des risques avec une authentification prennent-facteur supplémentaire pour les applications sensibles. AD FS permet de contrôler les stratégies pour requérir potentiellement authentification prennent facteurs globalement ou sur chaque application. En outre, AD FS fournit des points d’extensibilité pour n’importe quel fournisseur prennent facteurs à intégrer profondément pour une expérience de facteurs prennent sécurisée et transparente pour les utilisateurs finaux.  
  
-   Fournir des fonctionnalités d’authentification et d’autorisation pour accéder aux ressources web à partir de l’extranet qui sont protégées par le Proxy d’Application Web.  
  
Pour résumer, AD FS dans Windows Server 2012 R2 peut être déployé pour atteindre les objectifs suivants dans votre organisation:  
  
### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>Autoriser les utilisateurs à accéder aux ressources sur leurs appareils personnels depuis n’importe où  
  
-   Jonction d’espace de travail qui permet aux utilisateurs de joindre leurs appareils personnels à Active Directory d’entreprise et ainsi accéder et transparence lors de l’accès aux ressources d’entreprise à partir de ces appareils.  
  
-   Authentification Pre\ des ressources au sein du réseau d’entreprise qui sont protégées par le proxy d’Application Web et accessibles à partir d’internet.  
  
-   Modification de mot de passe pour permettre aux utilisateurs de modifier leur mot de passe à partir de n’importe quel appareil joint à un espace de travail lors de leur mot de passe a expiré afin qu’ils puissent continuer à accéder aux ressources.  
  
### <a name="enhance-your-access-control-risk-management-tools"></a>Améliorer vos outils de gestion de l’accès contrôle risque  
La gestion des risques sont un aspect important de gouvernance et de conformité de chaque organisation informatique. Il existe le contrôle d’accès nombreuses améliorations de la gestion des risques dans AD FS dans Windows Server® 2012 R2, y compris les suivantes:  
  
-   Contrôles flexibles basés sur l’emplacement réseau pour déterminer comment un utilisateur s’authentifie pour accéder à une application sécurisée par AD FS\.  
  
-   Stratégies flexibles pour déterminer si un utilisateur a besoin effectuer l’authentification de facteurs prennent basée sur les données de l’utilisateur, des données de l’appareil et emplacement réseau.  
  
-   Contrôle de Per\-application à ignorer l’authentification unique et forcer l’utilisateur à fournir les informations d’identification chaque fois qu’ils accèdent à une application sensible.  
  
-   Stratégie d’accès de per\-application souple basée sur les données utilisateur, les données de l’appareil ou emplacement réseau.  
  
-   Verrouillage Extranet AD FS, ce qui permet aux administrateurs de protéger les comptes Active Directory contre les attaques en force brute à partir d’internet.  
  
-   Révocation d’accès pour n’importe quel appareil joint à un espace de travail qui est désactivé ou supprimé dans Active Directory.  
  
### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>Utiliser les services ADFS pour améliorer l’expérience de connexion  
Nouvelles fonctionnalités AD FS dans Windows Server® 2012 R2 qui permettent à l’administrateur de personnaliser et d’améliorer l’expérience de connexion sont les suivantes:  
  
-   Personnalisation unifiée du service AD FS, où les modifications sont effectuées une seule fois et propagées automatiquement au reste des serveurs de fédération AD FS dans une batterie de serveurs donnée.  
  
-   Pages mises à jour connexion aspect moderne prenant en charge différents facteurs de forme automatiquement.  
  
-   Prise en charge du secours automatique pour l’authentification basée sur les forms\ pour les périphériques qui ne sont pas joints au domaine d’entreprise, mais sont toujours utilisés générer des demandes d’accès à partir du réseau d’entreprise \(intranet\).  
  
-   Contrôles simples pour personnaliser le logo de société, de l’image d’illustration, de liens standard pour la prise en charge de l’informatique, page d’accueil, confidentialité, etc..  
  
-   Personnalisation des messages de description dans les pages de connexion.  
  
-   Personnalisation des thèmes web.  
  
-   Accueil \(HRD\) découverte de domaine basé sur un suffixe d’organisation de l’utilisateur pour une confidentialité améliorée des partenaires de l’entreprise.  
  
-   Filtrage HRD sur une base per\-application à choisir automatiquement un domaine basé sur l’application.  
  
-   Clic One\ rapport d’erreurs pour faciliter informatique résolution des problèmes.  
  
-   Messages d’erreur personnalisables.  
  
-   Choix de l’authentification utilisateur lorsque plusieurs fournisseurs d’authentification n’est disponible.  
  
## <a name="see-also"></a>Voir aussi  
[Guide de conception ADFS dans Windows Server2012R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

