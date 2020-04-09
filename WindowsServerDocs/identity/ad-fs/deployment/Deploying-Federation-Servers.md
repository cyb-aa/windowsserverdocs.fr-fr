---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: Déploiement de serveurs de fédération
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 9e66c513c34658c40152cd7b622a1818e7198e47
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855562"
---
# <a name="deploying-federation-servers"></a>Déploiement de serveurs de fédération

Pour déployer des serveurs de Fédération dans Services ADFS \(AD FS\), effectuez chacune des tâches de la [liste de vérification : configuration d’un serveur de Fédération](Checklist--Setting-Up-a-Federation-Server.md).  
  
> [!NOTE]  
> Lorsque vous utilisez cette liste de vérification, nous vous recommandons de lire d’abord les références à la planification du serveur de Fédération dans la [AD FS Guide de conception de Windows server 2012](https://technet.microsoft.com/library/dd807036.aspx) avant de commencer les procédures de configuration des serveurs. Le fait de suivre la liste de vérification de cette façon permet de mieux comprendre le processus de conception et de déploiement pour les serveurs de Fédération.  
  
## <a name="about-federation-servers"></a>À propos des serveurs de Fédération  
Les serveurs de Fédération sont des ordinateurs exécutant Windows Server 2008 avec le logiciel AD FS installé qui a été configuré pour agir dans le rôle de serveur de Fédération. Les serveurs de Fédération authentifient ou acheminent les demandes de comptes d’utilisateurs dans d’autres organisations et à partir d’ordinateurs clients qui peuvent se trouver n’importe où sur Internet.  
  
Le fait d’installer le logiciel AD FS sur un ordinateur et d’utiliser l’Assistant Configuration du serveur de fédération AD FS pour le configurer pour le rôle de serveur de Fédération fait de cet ordinateur un serveur de Fédération. Elle rend également le\-du composant logiciel enfichable de gestion de AD FS disponible sur cet ordinateur dans le menu **démarrer\\outils d’administration\\** afin que vous puissiez spécifier les éléments suivants :  
  
-   Nom d’hôte AD FS dans lequel les organisations partenaires et les applications envoient des demandes et des réponses de jeton.  
  
-   L’identificateur de AD FS que les organisations partenaires et les applications utilisent pour identifier le nom unique ou l’emplacement de votre organisation  
  
-   Jeton\-certificat de signature que tous les serveurs de Fédération d’une batterie de serveurs utiliseront pour émettre et signer des jetons  
  
-   Emplacement des pages Web ASP.NET personnalisées pour l’ouverture de session du client, la fermeture de session et la découverte de partenaire de compte qui amélioreront l’expérience client  
  
    > [!NOTE]  
    > La majorité de ces paramètres d’interface utilisateur de base \(interface utilisateur\) sont contenus dans le fichier Web. config sur chaque serveur de Fédération. Les AD FS nom d’hôte et AD FS valeurs d’identificateur ne sont pas spécifiés dans le fichier Web. config.  
  
Les serveurs de Fédération hébergent un moteur d’émission de revendications qui émet des jetons en fonction des informations d’identification \(par exemple, le nom d’utilisateur et le mot de passe\) qui y sont présentés. Un jeton de sécurité est une unité de données signée par chiffrement qui exprime une ou plusieurs revendications. Une revendication est une instruction qu’un serveur effectue \(par exemple, nom, identité, clé, groupe, privilège ou fonctionnalité\) sur un client. Une fois les informations d’identification vérifiées sur le serveur de Fédération \(par le biais du processus d’ouverture de session de l’utilisateur\), les revendications pour l’utilisateur sont collectées par l’examen des attributs utilisateur stockés dans le magasin d’attributs spécifié.  
  
Dans les conceptions de\-unique Web fédéré\-sur les conceptions de\) SSO \(\(AD FS conceptions dans lesquelles deux organisations ou plus sont impliquées\), les revendications peuvent être modifiées par les règles de revendication pour une partie de confiance spécifique. Les revendications sont intégrées dans un jeton qui est envoyé à un serveur de Fédération dans l’organisation du partenaire de ressource. Une fois qu’un serveur de Fédération du partenaire de ressource reçoit les revendications en tant que revendications entrantes, il exécute le moteur d’émission de revendications pour exécuter un ensemble de règles de revendication pour filtrer, transmettre ou transformer ces revendications. Les revendications sont ensuite intégrées dans un nouveau jeton qui est envoyé au serveur Web dans le partenaire de ressource.  
  
Dans la conception SSO de Web \(une AD FS conception dans laquelle une seule organisation est impliquée\), un seul serveur de Fédération peut être utilisé pour permettre aux employés de se connecter une seule fois et d’accéder toujours à plusieurs applications.  
  
