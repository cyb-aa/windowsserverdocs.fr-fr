---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: "Déploiement de serveurs de fédération"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 225e6b52ed46eef6f2ccacb5b0d9c6e6d880f475
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-federation-servers"></a>Déploiement de serveurs de fédération

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Pour déployer des serveurs de fédération ActiveDirectory Federation Services \(ADFS\), effectuer chacune des tâches de [liste de vérification: Setting Up a Federation Server](Checklist--Setting-Up-a-Federation-Server.md).  
  
> [!NOTE]  
> Lorsque vous utilisez cette liste de vérification, nous recommandons de lire les références à la planification d’un serveur de fédération dans le [Guide de conception ADFS dans Windows Server2012](https://technet.microsoft.com/library/dd807036.aspx) avant de commencer les procédures de configuration des serveurs. Suivant la liste de vérification de cette façon offre une meilleure compréhension du processus de conception et de déploiement pour les serveurs de fédération.  
  
## <a name="about-federation-servers"></a>Sur les serveurs de fédération  
Les serveurs de fédération sont des ordinateurs exécutant Windows Server2008 avec le logiciel ADFS installé qui ont été configurés pour jouer le rôle de serveur de fédération. Les serveurs de fédération authentifient ou acheminer les demandes à partir des comptes d’utilisateurs dans d’autres organisations et des ordinateurs clients qui peuvent se trouver n’importe où sur Internet.  
  
Le fait de l’installation du logiciel ADFS sur un ordinateur et à l’aide de l’Assistant Configuration du serveur de fédération ADFS pour le configurer pour le rôle de serveur de fédération rend cet ordinateur à un serveur de fédération. Il rend également la gestion ADFS enfichable disponibles sur cet ordinateur dans le **Start\\Administrative Tools\\** menu afin que vous pouvez spécifier les éléments suivants:  
  
-   Le nom d’hôte ADFS envoient dans lequel les applications et les organisations partenaires de demandes de jeton et les réponses  
  
-   L’identificateur d’ADFS qui organisations et les applications du partenaire utilisera pour identifier le nom unique ou l’emplacement de votre organisation  
  
-   Le certificat de signature token\ par tous les serveurs de fédération dans une batterie de serveurs pour le problème et signe les jetons  
  
-   L’emplacement de ASP.NET personnalisée des pages Web pour la découverte du partenaire de compte afin d’améliorer l’expérience du client d’ouverture de session client et la fermeture de session  
  
    > [!NOTE]  
    > La plupart de ces paramètres \(UI\) core l’interface utilisateur sont contenus dans le fichier web.config sur chaque serveur de fédération. Nom d’hôte ADFS et valeurs de l’identificateur ADFS ne sont pas spécifiés dans le fichier web.config.  
  
Serveurs de fédération hébergent un moteur d’émission des revendications qui émet des jetons basés sur les informations d’identification \ (par exemple, nom d’utilisateur et password\) qui sont présentées. Un jeton de sécurité est une unité de données signées par chiffrement qui exprime une ou plusieurs revendications. Une revendication est une instruction qui rend un serveur \ (par exemple, nom, identité, clé, groupe, privilège ou capability\) sur un client. Une fois les informations d’identification sont vérifiées sur le serveur de fédération \ (via la process\ d’ouverture de session utilisateur), les revendications pour l’utilisateur sont collectées par l’examen des attributs utilisateur qui sont stockés dans le magasin d’attributs spécifié.  
  
Dans \(SSO\) Single\-Sign\-On de Web fédéré conceptions \ (conceptions ADFS dans lequel les deux organisations sont involved\), revendications peuvent être modifiées par les règles de revendication pour une partie de confiance spécifique. Les revendications sont intégrées à un jeton qui est envoyé à un serveur de fédération dans l’organisation partenaire de ressource. Une fois un serveur de fédération du partenaire de ressource reçoit les revendications en tant que revendications entrantes, il exécute le moteur d’émission des revendications pour exécuter un ensemble de règles de revendication pour filtrer, passer ou transformer les revendications. Les revendications sont ensuite intégrées dans un nouveau jeton qui est envoyé au serveur Web du partenaire de ressource.  
  
Dans la conception SSO de Web \ (une conception de ADFS dans laquelle une seule organisation est involved\), un serveur de fédération unique peut être utilisé afin que les employés peuvent se connecter une seule fois et toujours accéder à plusieurs applications.  
  
