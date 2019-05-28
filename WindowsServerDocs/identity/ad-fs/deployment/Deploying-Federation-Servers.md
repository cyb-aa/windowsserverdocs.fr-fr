---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: Déploiement de serveurs de fédération
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1f3b2e1ce901df1df1a232dfba51c292c8e1c29c
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192180"
---
# <a name="deploying-federation-servers"></a>Déploiement de serveurs de fédération

Pour déployer des serveurs de fédération dans Active Directory Federation Services \(AD FS\), effectuez toutes les tâches dans [liste de vérification : Configuration d’un serveur de fédération](Checklist--Setting-Up-a-Federation-Server.md).  
  
> [!NOTE]  
> Lorsque vous utilisez cette liste de vérification, nous vous recommandons de commencer par lire les références à la planification d’un serveur de fédération dans le [Guide de conception AD FS dans Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) avant de commencer les procédures de configuration des serveurs. Suivant la liste de vérification de cette façon offre une meilleure compréhension du processus de conception et de déploiement pour les serveurs de fédération.  
  
## <a name="about-federation-servers"></a>À propos des serveurs de fédération  
Serveurs de fédération sont des ordinateurs exécutant Windows Server 2008 avec le logiciel AD FS installé et qui ont été configurés pour jouer le rôle de serveur de fédération. Serveurs de fédération authentifieront ou acheminer les requêtes à partir de comptes d’utilisateur dans d’autres organisations et des ordinateurs clients pouvant se trouver n’importe où sur Internet.  
  
Le fait d’installer le logiciel AD FS sur un ordinateur et à l’aide de l’Assistant Configuration du serveur de fédération AD FS pour le configurer pour le rôle de serveur de fédération rend cet ordinateur à un serveur de fédération. En outre, le composant logiciel enfichable Gestion AD FS\-dans disponible sur cet ordinateur dans le **Démarrer\\outils d’administration\\**  menu afin que vous pouvez spécifier les éléments suivants :  
  
-   Le nom d’hôte AD FS envoient dans lequel les applications et les organisations partenaires demandes de jeton et les réponses  
  
-   L’identificateur d’AD FS qui les organisations et les applications du partenaire utilise pour identifier le nom unique ou l’emplacement de votre organisation  
  
-   Le jeton\-certificat que tous les serveurs de fédération dans une batterie de serveurs utilise pour le problème et signe les jetons de signature  
  
-   L’emplacement des pages Web ASP.NET personnalisées pour l’ouverture de session client, de déconnexion et de découverte du partenaire de compte afin d’améliorer l’expérience du client  
  
    > [!NOTE]  
    > Interface utilisateur de base de la majorité de ces \(l’interface utilisateur\) paramètres sont contenus dans le fichier web.config sur chaque serveur de fédération. Le nom d’hôte de AD FS et les valeurs d’identificateur AD FS ne sont pas spécifiés dans le fichier web.config.  
  
Serveurs de fédération hébergent un moteur d’émission des revendications qui émet des jetons basés sur les informations d’identification \(, par exemple, le nom d’utilisateur et mot de passe\) qui sont présentées. Un jeton de sécurité est une unité de données à signature cryptographique exprimant une ou plusieurs revendications. Une revendication est une instruction qui rend un serveur \(, par exemple, nom, identité, clé, groupe, privilège ou fonctionnalité\) sur un client. Une fois les informations d’identification sont vérifiées sur le serveur de fédération \(au long du processus d’ouverture de session utilisateur\), revendications de l’utilisateur sont collectées par l’examen des attributs utilisateur qui sont stockés dans le magasin d’attributs spécifié.  
  
Dans l’unique de Web fédéré\-connexion\-sur \(SSO\) conceptions \(AD FS conçoit dans lequel deux ou plusieurs organisations sont impliquées\), revendications peuvent être modifiées par les règles de revendication pour une partie de confiance spécifique tiers. Les affirmations sont intégrées dans un jeton qui est envoyé à un serveur de fédération dans l’organisation partenaire ressource. Une fois un serveur de fédération du partenaire de ressource reçoit les affirmations comme affirmations entrantes, il exécute le moteur d’émission de revendications pour exécuter un ensemble de règles de revendication à filtrer, de traverser ou de transformer ces revendications. Les revendications sont ensuite intégrées dans un nouveau jeton qui est envoyé au serveur Web du partenaire de ressource.  
  
Dans la conception SSO de Web \(une conception AD FS dans lequel une seule organisation est impliquée\), un serveur de fédération unique peut être utilisé afin que les employés peuvent se connecter une seule fois et toujours accéder à plusieurs applications.  
  
