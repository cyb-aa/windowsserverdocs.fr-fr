---
ms.assetid: c4d83dd3-2846-4658-8b9c-93901ee69766
title: Déploiement de serveurs de fédération
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 785dcad4ac8e03cc59730fb533e30a001569dd63
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359631"
---
# <a name="deploying-federation-servers"></a>Déploiement de serveurs de fédération

Pour déployer des serveurs de Fédération dans Services ADFS \(AD FS @ no__t-1, effectuez chacune des tâches dans [Checklist : Configuration d’un serveur de Fédération @ no__t-0.  
  
> [!NOTE]  
> Lorsque vous utilisez cette liste de vérification, nous vous recommandons de lire d’abord les références à la planification du serveur de Fédération dans la [AD FS Guide de conception de Windows server 2012](https://technet.microsoft.com/library/dd807036.aspx) avant de commencer les procédures de configuration des serveurs. Le fait de suivre la liste de vérification de cette façon permet de mieux comprendre le processus de conception et de déploiement pour les serveurs de Fédération.  
  
## <a name="about-federation-servers"></a>À propos des serveurs de Fédération  
Les serveurs de Fédération sont des ordinateurs exécutant Windows Server 2008 avec le logiciel AD FS installé qui a été configuré pour agir dans le rôle de serveur de Fédération. Les serveurs de Fédération authentifient ou acheminent les demandes de comptes d’utilisateurs dans d’autres organisations et à partir d’ordinateurs clients qui peuvent se trouver n’importe où sur Internet.  
  
Le fait d’installer le logiciel AD FS sur un ordinateur et d’utiliser l’Assistant Configuration du serveur de fédération AD FS pour le configurer pour le rôle de serveur de Fédération fait de cet ordinateur un serveur de Fédération. Il rend également le composant logiciel enfichable de gestion AD FS no__t-0in disponible sur cet ordinateur dans le menu **Démarrer @ no__t-2Administrative Tools @ no__t-3** pour vous permettre de spécifier les éléments suivants :  
  
-   Nom d’hôte AD FS dans lequel les organisations partenaires et les applications envoient des demandes et des réponses de jeton.  
  
-   L’identificateur de AD FS que les organisations partenaires et les applications utilisent pour identifier le nom unique ou l’emplacement de votre organisation  
  
-   Le certificat @ no__t-0signing qui est utilisé par tous les serveurs de Fédération d’une batterie de serveurs pour émettre et signer des jetons  
  
-   Emplacement des pages Web ASP.NET personnalisées pour l’ouverture de session du client, la fermeture de session et la découverte de partenaire de compte qui amélioreront l’expérience client  
  
    > [!NOTE]  
    > La plupart de ces paramètres \(UI @ no__t-1 de l’interface utilisateur de base sont contenus dans le fichier Web. config sur chaque serveur de Fédération. Les AD FS nom d’hôte et AD FS valeurs d’identificateur ne sont pas spécifiés dans le fichier Web. config.  
  
Les serveurs de Fédération hébergent un moteur d’émission de revendications qui émet des jetons basés sur les informations d’identification \(for exemple, le nom d’utilisateur et le mot de passe @ no__t-1 qui y sont présentés. Un jeton de sécurité est une unité de données signée par chiffrement qui exprime une ou plusieurs revendications. Une revendication est une instruction qui permet à un serveur d' @no__t 0for, par exemple, le nom, l’identité, la clé, le groupe, le privilège ou la fonctionnalité @ no__t-1 sur un client. Une fois les informations d’identification vérifiées sur le serveur de Fédération \(through le processus d’ouverture de session utilisateur @ no__t-1, les revendications pour l’utilisateur sont collectées par l’examen des attributs utilisateur stockés dans le magasin d’attributs spécifié.  
  
Dans le Web fédéré Single @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3 conçoit des conceptions \(AD FS dans lesquelles deux organisations ou plus sont impliquées @ no__t-5, les revendications peuvent être modifiées par les règles de revendication pour une partie de confiance spécifique. Les revendications sont intégrées dans un jeton qui est envoyé à un serveur de Fédération dans l’organisation du partenaire de ressource. Une fois qu’un serveur de Fédération du partenaire de ressource reçoit les revendications en tant que revendications entrantes, il exécute le moteur d’émission de revendications pour exécuter un ensemble de règles de revendication pour filtrer, transmettre ou transformer ces revendications. Les revendications sont ensuite intégrées dans un nouveau jeton qui est envoyé au serveur Web dans le partenaire de ressource.  
  
Dans la conception de l’authentification unique Web @no__t 0AN AD FS conception dans laquelle une seule organisation est impliquée @ no__t-1, un seul serveur de Fédération peut être utilisé pour permettre aux employés de se connecter une seule fois et d’accéder toujours à plusieurs applications.  
  
