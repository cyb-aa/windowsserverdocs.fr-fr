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
ms.openlocfilehash: b4fde1ae50b9422d3a2f35035ab549114e193a67
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867775"
---
# <a name="identify-your-ad-fs-deployment-goals"></a>Identifier vos objectifs de déploiement d’AD FS

L’identification correcte de \(votre\) services ADFS AD FS objectifs du déploiement est essentiel pour la réussite de votre projet de conception de AD FS. Hiérarchisez et, éventuellement, combinez vos objectifs de déploiement afin de pouvoir concevoir et déployer des AD FS à l’aide d’une approche itérative. Vous pouvez tirer parti des objectifs de déploiement AD FS existants, documentés et prédéfinis qui sont pertinents pour les AD FS conceptions et développer une solution de travail adaptée à votre situation.  
  
Les versions antérieures de AD FS étaient le plus souvent déployées pour obtenir les éléments suivants :  
  
-   Fournir à vos employés ou clients une expérience\-d’authentification unique basée sur le Web lors\-de l’accès à des applications basées sur les revendications au sein de votre entreprise.  
  
-   Fournir à vos employés ou clients une expérience\-d’authentification unique basée sur le Web pour accéder aux ressources dans toute organisation partenaire de Fédération.  
  
-   Fournir à vos employés ou clients une expérience\-d’authentification unique basée sur le Web lors de l’accès à distance à des services ou sites Web hébergés en interne.  
  
-   Fournir à vos employés ou clients une expérience\-d’authentification unique basée sur le Web lors de l’accès à des ressources ou des services dans le Cloud.  
  
En outre, AD FS dans Windows Server® 2012 R2 ajoute des fonctionnalités qui peuvent vous aider à atteindre les objectifs suivants :  
  
-   Jonction à l’espace de travail des appareils pour SSO et authentification de second facteur transparente. Cela permet aux organisations d’autoriser l’accès à partir des appareils personnels de l’utilisateur et de gérer les risques lors de la fourniture de cet accès.  
  
-   Gestion des risques avec\-le contrôle d’accès multifacteur. AD FS fournit un niveau performant d’autorisation qui contrôle quels utilisateurs ont accès à quelles applications. Cela peut être basé sur les attributs \(utilisateur UPN, courrier électronique, appartenance au groupe de sécurité, niveau\)d’authentification, \(etc., attributs d’appareil\) si l’appareil est joint à un espace de travail ou attribut \(dedemandel’emplacement réseau, l’adresse IP ou l'\)agent utilisateur.  
  
-   Gestion des risques avec une\-authentification multifacteur supplémentaire pour les applications sensibles. AD FS vous permet de contrôler les stratégies pour qu’elles\-requièrent l’authentification multifacteur globalement ou sur la base de chaque application. En outre, AD FS fournit des points d’extensibilité pour\-tout fournisseur multifacteur afin qu’il s’intègre profondément pour\-une expérience multifacteur sécurisée et transparente pour les utilisateurs finaux.  
  
-   Fournir des fonctionnalités d’authentification et d’autorisation pour accéder aux ressources Web à partir de l’extranet qui sont protégées par le proxy d’application Web.  
  
Pour résumer, AD FS dans Windows Server 2012 R2 peut être déployé pour atteindre les objectifs suivants dans votre organisation :  
  
### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>Autoriser vos utilisateurs à accéder aux ressources sur leurs appareils personnels depuis n’importe où  
  
-   Jonction à l’espace de travail qui permet aux utilisateurs de joindre leurs appareils personnels à l’application Active Directory de l’entreprise et, par conséquent, d’accéder aux ressources de cette dernière depuis ces appareils en toute transparence.  
  
-   Pré\--authentification des ressources au sein du réseau d’entreprise qui sont protégées par le proxy d’application Web et accessibles depuis Internet.  
  
-   Modification de mot de passe pour permettre aux utilisateurs de modifier leur mot de passe à partir de n’importe quel appareil d’un espace de travail rattaché à l’expiration de leur mot de passe afin qu’ils puissent continuer à accéder aux ressources.  
  
### <a name="enhance-your-access-control-risk-management-tools"></a>Améliorez vos outils de gestion des risques de contrôle d’accès  
La gestion des risques est un aspect important de la gouvernance et de la conformité de chaque organisation informatique. Il existe de nombreuses améliorations de la gestion des risques de contrôle d’accès dans AD FS dans Windows Server® 2012 R2, y compris les éléments suivants :  
  
-   Contrôles flexibles basés sur l’emplacement réseau pour régir la manière dont un utilisateur s’authentifie\-pour accéder à une application sécurisée AD FS.  
  
-   Stratégie flexible pour déterminer si un utilisateur doit effectuer\-une authentification multifacteur en fonction des données de l’utilisateur, des données de l’appareil et de l’emplacement réseau.  
  
-   Par\-contrôle d’application pour ignorer l’authentification unique et forcer l’utilisateur à fournir des informations d’identification chaque fois qu’il accède à une application sensible.  
  
-   Flexibilité par\-stratégie d’accès aux applications basée sur les données utilisateur, les données de l’appareil ou l’emplacement réseau.  
  
-   Le verrouillage extranet AD FS permet aux administrateurs de protéger les comptes Active Directory contre les attaques en force brute à partir d’Internet.  
  
-   Révocation d’accès pour n’importe quel appareil membre d’un espace de travail qui est désactivé ou supprimé dans Active Directory.  
  
### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>Utiliser AD FS pour améliorer l’expérience\-de connexion  
Les nouvelles fonctionnalités de AD FS de Windows Server® 2012 R2 qui permettent à l’administrateur de personnaliser et d’améliorer\-l’expérience de connexion sont les suivantes :  
  
-   Personnalisation unifiée du service AD FS, où les modifications sont effectuées une seule fois et propagées automatiquement au reste des serveurs de fédération AD FS dans une batterie de serveurs donnée.  
  
-   Mise à\-jour des pages de connexion qui paraissent modernes et qui répondent à différents facteurs de forme automatiquement.  
  
-   Prise en charge du secours automatique\-de l’authentification basée sur les formulaires pour les appareils qui ne sont pas joints au domaine de l’entreprise, mais qui sont \(toujours\)utilisés pour générer des demandes d’accès à partir de l’intranet du réseau d’entreprise.  
  
-   Contrôles simples pour personnaliser le logo de la société, l’image d’illustration, les liens standard pour la prise en charge de l’informatique, la page d’accueil, la confidentialité, etc.  
  
-   Personnalisation des messages de description dans les pages\-de connexion.  
  
-   Personnalisation des thèmes web.  
  
-   Découverte \(de domaine d'\) découverte du domaine basée sur le suffixe d’organisation de l’utilisateur pour la confidentialité améliorée des partenaires d’une société.  
  
-   Découverte du domaine filtrage par\-application pour choisir automatiquement un domaine basé sur l’application.  
  
-   Un\-rapport d’erreurs de clic pour faciliter le dépannage informatique.  
  
-   Personnalisation des messages d’erreur.  
  
-   Choix de l’authentification utilisateur lorsque plusieurs fournisseurs d’authentification sont disponibles.  
  
## <a name="see-also"></a>Voir aussi  
[Guide de conception AD FS dans Windows Server 2012 R2](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

