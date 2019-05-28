---
ms.assetid: c81b8291-fba5-4b30-a43d-7feb2f4b66be
title: Guide de conception AD FS dans Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2b881553431be873ed9883da67a7989527d7d288
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191266"
---
# <a name="identify-your-ad-fs-deployment-goals"></a>Identifier vos objectifs de déploiement d’AD FS

L’identification correcte de votre Active Directory Federation Services \(AD FS\) objectifs de déploiement est essentielle pour la réussite de votre projet de conception AD FS. Hiérarchisez et, éventuellement, combinez vos objectifs de déploiement afin que vous pouvez concevoir et déployer AD FS à l’aide d’une approche itérative. Vous pouvez tirer parti d’existant, documentés et prédéfinis des objectifs de déploiement AD FS qui sont pertinents pour les conceptions AD FS et de développement une solution à votre situation.  
  
Les versions antérieures d’AD FS ont été déployées plus souvent pour effectuer les opérations suivantes :  
  
-   En fournissant vos employés ou clients et un serveur web\-en, expérience d’authentification unique lors de l’accès aux revendications\-en fonction des applications au sein de votre entreprise.  
  
-   En fournissant vos employés ou clients et un serveur web\-expérience basée sur, l’authentification unique pour accéder aux ressources dans toute organisation partenaire de fédération.  
  
-   En fournissant vos employés ou clients et un serveur Web\-en, expérience d’authentification unique lors de l’accès à distant en interne de services ou sites Web hébergés.  
  
-   En fournissant vos employés ou clients et un serveur web\-en, expérience d’authentification unique lorsqu’ils accèdent à des ressources ou services dans le cloud.  
  
Outre ces options, AD FS dans Windows Server® 2012 R2 ajoute des fonctionnalités qui peuvent vous aider à effectuer les opérations suivantes :  
  
-   Jonction à l’espace de travail des appareils pour SSO et authentification de second facteur transparente. Cela permet aux organisations d’autoriser l’accès à partir d’appareils personnels de l’utilisateur et de gérer les risques liés à la fourniture de cet accès.  
  
-   Gestion des risques avec multi\-factoriser le contrôle d’accès. AD FS fournit un niveau performant d’autorisation qui contrôle quels utilisateurs ont accès à quelles applications. Il peut être basé sur les attributs utilisateur \(UPN, courrier électronique, l’appartenance au groupe de sécurité, force d’authentification, etc.\), attributs de l’appareil \(si l’appareil est rattaché\) ou demander des attributs \(emplacement réseau, adresse IP ou agent utilisateur\).  
  
-   Gestion des risques avec multi supplémentaire\--factor authentication pour les applications sensibles. AD FS permet de contrôler les stratégies pour requérir potentiellement plusieurs\-globalement ou sur chaque application du facteur d’authentification. En outre, AD FS fournit des points d’extensibilité pour n’importe quel multi\-fournisseur facteur à intégrer profondément pour un multi sécurisée et transparente\-tenir compte des utilisateurs finaux.  
  
-   En fournissant des fonctionnalités d’authentification et d’autorisation pour accéder aux ressources web à partir de l’extranet qui sont protégés par le Proxy d’Application Web.  
  
Pour résumer, AD FS dans Windows Server 2012 R2 peut être déployée pour atteindre les objectifs suivants dans votre organisation :  
  
### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>Autoriser les utilisateurs à accéder aux ressources sur leurs appareils personnels depuis n’importe où  
  
-   Jonction à l’espace de travail qui permet aux utilisateurs de joindre leurs appareils personnels à l’application Active Directory de l’entreprise et, par conséquent, d’accéder aux ressources de cette dernière depuis ces appareils en toute transparence.  
  
-   Pre\-l’authentification des ressources au sein du réseau d’entreprise qui sont protégées par le proxy d’Application Web et accessibles à partir d’internet.  
  
-   Modification de mot de passe pour permettre aux utilisateurs de modifier leur mot de passe à partir de n’importe quel appareil d’un espace de travail rattaché à l’expiration de leur mot de passe afin qu’ils puissent continuer à accéder aux ressources.  
  
### <a name="enhance-your-access-control-risk-management-tools"></a>Améliorez vos outils de gestion du risque de contrôle l’accès  
La gestion des risques est un aspect important de la gouvernance et de la conformité de chaque organisation informatique. Il existe un contrôle d’accès nombreuses améliorations de gestion des risques dans AD FS dans Windows Server® 2012 R2, y compris les éléments suivants :  
  
-   Contrôles flexibles basés sur l’emplacement de réseau pour déterminer comment un utilisateur s’authentifie pour accéder à un AD FS\-application sécurisée.  
  
-   Stratégie flexible pour déterminer si un utilisateur doit effectuer plusieurs\-l’authentification multifacteur basée sur les données de l’utilisateur, les données de l’appareil et emplacement réseau.  
  
-   Par\-contrôle d’application pour ignorer l’authentification unique et forcer l’utilisateur à fournir des informations d’identification chaque fois qu’ils accèdent à une application sensible.  
  
-   Flexible par\-stratégie d’accès application basée sur les données utilisateur, données de l’appareil ou l’emplacement réseau.  
  
-   Le verrouillage extranet AD FS permet aux administrateurs de protéger les comptes Active Directory contre les attaques en force brute à partir d’Internet.  
  
-   Révocation d’accès pour n’importe quel appareil membre d’un espace de travail qui est désactivé ou supprimé dans Active Directory.  
  
### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>Utiliser AD FS pour améliorer le signe\-dans l’expérience  
Voici les nouvelles fonctionnalités d’AD FS dans Windows Server® 2012 R2 qui permettent à personnaliser et d’améliorer la connexion administrateur\-dans l’expérience :  
  
-   Personnalisation unifiée du service AD FS, où les modifications sont effectuées une seule fois et propagées automatiquement au reste des serveurs de fédération AD FS dans une batterie de serveurs donnée.  
  
-   Mise à jour de la connexion\-dans les pages dont l’apparence moderne et répondez aux besoins des différents facteurs de forme automatiquement.  
  
-   Prise en charge du secours automatique forms\-en fonction de l’authentification pour les appareils qui ne sont pas joints au domaine d’entreprise, mais sont toujours utilisés génèrent des demandes d’accès à partir du réseau d’entreprise \(intranet\).  
  
-   Contrôles simples pour personnaliser le logo de la société, l’image d’illustration, les liens standard pour la prise en charge de l’informatique, la page d’accueil, la confidentialité, etc.  
  
-   Personnalisation de la description des messages dans la connexion\-dans les pages.  
  
-   Personnalisation des thèmes web.  
  
-   Découverte du domaine d’accueil \(HRD\) basé sur un suffixe d’organisation de l’utilisateur pour une confidentialité améliorée des partenaires d’une société.  
  
-   Filtrage HRD sur un par\-chaque application de choisir automatiquement un domaine basé sur l’application.  
  
-   Un seul\-cliquez sur rapports d’erreurs pour faciliter informatique résolution des problèmes.  
  
-   Personnalisation des messages d’erreur.  
  
-   Choix de l’authentification utilisateur lorsque plusieurs fournisseurs d’authentification sont disponibles.  
  
## <a name="see-also"></a>Voir aussi  
[Guide de conception AD FS dans Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

