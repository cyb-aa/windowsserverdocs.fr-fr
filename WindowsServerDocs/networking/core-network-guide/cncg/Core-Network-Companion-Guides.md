---
title: Guides d’accompagnement du réseau de base
description: Cette rubrique fournit une vue d’ensemble des guides d’accompagnement du Guide du réseau de base Windows Server 2016.
manager: brianlic
ms.technology: networking
ms.prod: windows-server
ms.topic: article
ms.assetid: d57af0bd-9301-4f62-9888-f528cd10451d
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5f3ae4f9c22e61a8428a257d9324fe164eeaa04b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319106"
---
# <a name="core-network-companion-guidance"></a>Guide complémentaire du réseau principal

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Le Guide du réseau de [base](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) Windows Server 2016 fournit des instructions sur le déploiement d’un nouveau Active Directory&reg; forêt avec un nouveau domaine racine et l’infrastructure réseau de prise en charge, les guides d’accompagnement vous permettent d’ajouter des fonctionnalités à votre réseau.

Chaque guide d’accompagnement est consacré à la mise en œuvre d’un objectif particulier après le déploiement de votre réseau de base. Dans certains cas, il existe plusieurs guides d’accompagnement qui, lorsqu’ils sont déployés ensemble et dans l’ordre approprié, vous permettent de mener à bien des objectifs très complexes selon des méthodes mesurables, rentables et fiables.

Si vous avez déployé votre domaine Active Directory et votre réseau de base sans vous aider du Guide du réseau de base, vous pouvez quand même utiliser les guides d’accompagnement pour ajouter des fonctionnalités à votre réseau. Dans ce cas, servez-vous du Guide du réseau de base pour dresser la liste des conditions préalables que votre réseau doit remplir si vous prévoyez de déployer des fonctionnalités supplémentaires avec les guides d’accompagnement.

## <a name="core-network-companion-guide-deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Guide d’accompagnement du réseau de base : déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 X 

Ce guide d’accompagnement explique comment créer sur le réseau de base en déployant des certificats de serveur pour les ordinateurs qui exécutent le serveur de stratégie réseau \(\)NPS, le service d’accès à distance \(le\)RAS, ou les deux.

Des certificats de serveur sont requis lorsque vous déployez des méthodes d’authentification basées sur des certificats avec le protocole EAP (Extensible Authentication Protocol) \(EAP\) et PEAP\) Protected EAP \(pour l’authentification d’accès réseau. Le déploiement de certificats de serveur avec les services de certificats Active Directory \(les services AD CS\) pour les méthodes d’authentification basées sur des certificats EAP et PEAP offre les avantages suivants :

- Liaison de l’identité du serveur NPS ou RAS à une clé privée
- Méthode économique et sécurisée pour l’inscription automatique de certificats sur des serveurs NPS et RAS membres du domaine
- Méthode efficace pour gérer les certificats et les autorités de certification
- Sécurité fournie par l’authentification basée sur des certificats
- Possibilité d’étendre l’utilisation de certificats à d’autres usages
  
Pour obtenir des instructions sur le déploiement de certificats de serveur, consultez [déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 x](server-certs/Deploy-Server-Certificates-for-802.1X-Wired-and-Wireless-Deployments.md).  
## <a name="core-network-companion-guide-deploy-password-based-8021x-authenticated-wireless-access"></a>Guide d’accompagnement du réseau de base : déployer un accès sans fil authentifié 802.1 X basé sur un mot de passe

Ce guide d’accompagnement explique comment créer sur le réseau de base en fournissant des instructions sur la façon de déployer des ingénieurs de l’électro-et des appareils électroniques \(IEEE\) 802.1 X\-un accès sans fil IEEE 802,11 authentifié à l’aide du protocole PEAP (Protected Extensible Authentication Protocol) \(PEAP\-MS\-CHAP v2\).

La méthode d’authentification PEAP\-MS\-CHAP v2 exige que les serveurs d’authentification exécutant Network Policy Server \(NPS\) présentent des clients sans fil avec un certificat de serveur pour prouver l’identité du serveur NPS au client, mais que l’authentification utilisateur n’est pas effectuée à l’aide d’un certificat, les utilisateurs fournissent leur nom d’utilisateur de domaine et leur mot de passe.

Étant donné que PEAP\-MS\-CHAP v2 exige que les utilisateurs fournissent des informations d’identification basées sur un mot de passe plutôt qu’un certificat au cours du processus d’authentification, il est généralement plus facile et moins coûteux à déployer que EAP\-TLS ou PEAP\-TLS.

Avant d’utiliser ce guide pour déployer l’accès sans fil avec la méthode d’authentification PEAP\-MS\-CHAP v2, vous devez effectuer les opérations suivantes :

1. Suivez les instructions du Guide du réseau de base pour déployer votre infrastructure réseau de base, ou déjà disposer des technologies présentées dans ce guide sur votre réseau.
2. Suivez les instructions du Guide d’accompagnement du réseau de base déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 X, ou les technologies présentées dans ce guide sont déjà déployées sur votre réseau.

Pour obtenir des instructions sur le déploiement de l’accès sans fil avec PEAP\-MS\-CHAP v2, consultez [déployer un accès sans fil authentifié 802.1 x basé sur un mot de passe](wireless/a-deploy-8021X-wireless-access.md).

## <a name="core-network-companion-guide-deploy-branchcache-hosted-cache-mode"></a>Guide d’accompagnement du réseau de base : déployer le mode de cache hébergé de BranchCache

Ce guide d’accompagnement explique comment déployer BranchCache en mode de cache hébergé dans une ou plusieurs filiales.

BranchCache est une technologie d’optimisation de la bande passante du réseau étendu (WAN), incluse dans certaines éditions des systèmes d’exploitation Windows Server 2016 et Windows 10, ainsi que dans les versions antérieures de Windows et Windows Server.

Lorsque vous déployez BranchCache en mode de cache hébergé, le cache de contenu dans la filiale est hébergé sur un ou plusieurs ordinateurs serveurs, lesquels sont appelés « serveurs de cache hébergé ». Les serveurs de cache hébergé peuvent exécuter des charges de travail en plus de l’hébergement du cache, ce qui vous permet d’utiliser le serveur à plusieurs fins dans la filiale.

Le mode de cache hébergé de BranchCache augmente l’efficacité du cache, car le contenu est disponible même si le client qui a demandé et mis en cache les données à l’origine est hors connexion. Étant donné que le serveur de cache hébergé est toujours disponible, davantage de contenu est mis en cache, ce qui économise davantage la bande passante du réseau étendu et améliore l’efficacité de BranchCache.

Lorsque vous déployez le mode de cache hébergé, tous les clients d’une filiale à plusieurs sous-réseaux peuvent accéder à un seul cache, qui est stocké sur le serveur de cache hébergé, même si les clients se trouvent sur des sous-réseaux différents.

Pour obtenir des instructions sur le déploiement de BranchCache en mode de cache hébergé, consultez [déployer le mode de cache hébergé de BranchCache](bc-hcm/1-Deploy-Bc-Hcm.md).
