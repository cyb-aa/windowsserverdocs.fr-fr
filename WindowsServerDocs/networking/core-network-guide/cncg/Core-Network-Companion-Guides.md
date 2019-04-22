---
title: Guides d’accompagnement du réseau de base
description: Cette rubrique fournit une vue d’ensemble de guides d’accompagnement pour le Guide du réseau Windows Server 2016 Core
manager: brianlic
ms.technology: networking
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d57af0bd-9301-4f62-9888-f528cd10451d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b757e1914ee263a041f39e9767d3cb8af38403dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816800"
---
# <a name="core-network-companion-guidance"></a>Guide complémentaire du réseau principal

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Bien que Windows Server 2016 [Guide du réseau](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) fournit des instructions sur la façon de déployer un nouvel annuaire Active Directory&reg; forêt avec un nouveau domaine racine et l’infrastructure réseau, et les Guides d’accompagnement vous fournir avec la possibilité d’ajouter des fonctionnalités à votre réseau.

Chaque guide d’accompagnement est consacré à la mise en œuvre d’un objectif particulier après le déploiement de votre réseau de base. Dans certains cas, il existe plusieurs guides d’accompagnement qui, lorsqu’ils sont déployés ensemble et dans l’ordre approprié, vous permettent de mener à bien des objectifs très complexes selon des méthodes mesurables, rentables et fiables.

Si vous avez déployé votre domaine Active Directory et votre réseau de base sans vous aider du Guide du réseau de base, vous pouvez quand même utiliser les guides d’accompagnement pour ajouter des fonctionnalités à votre réseau. Dans ce cas, servez-vous du Guide du réseau de base pour dresser la liste des conditions préalables que votre réseau doit remplir si vous prévoyez de déployer des fonctionnalités supplémentaires avec les guides d’accompagnement.

## <a name="core-network-companion-guide-deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Guide d'accompagnement du réseau de base : Déployer des certificats de serveur pour des déploiements câblés et sans fil 802.1X 

Ce guide explique comment reposent sur le réseau de base en déployant des certificats de serveur pour les ordinateurs qui exécutent NPS \(NPS\), Remote Access Service \(RAS\), ou les deux.

Certificats de serveur sont requis lorsque vous déployez des méthodes d’authentification par certificat avec Extensible Authentication Protocol \(EAP\) et EAP protégé \(PEAP\) pour l’authentification d’accès réseau. Déploiement des certificats de serveur avec les Services de certificats Active Directory \(AD CS\) pour l’authentification basée sur certificat EAP et PEAP méthodes offre les avantages suivants :

- Liaison de l’identité du serveur NPS ou RAS à une clé privée
- Une méthode sécurisée et économique pour inscrire automatiquement des certificats sur les serveurs NPS et RAS du membre domaine
- Méthode efficace pour gérer les certificats et les autorités de certification
- Sécurité fournie par l’authentification basée sur des certificats
- Possibilité d’étendre l’utilisation de certificats à d’autres usages
  
Pour obtenir des instructions sur la façon de déployer des certificats de serveur, consultez [déployer des certificats de serveur pour les déploiements de sans fil et câblé à 802.1 X](server-certs/Deploy-Server-Certificates-for-802.1X-Wired-and-Wireless-Deployments.md).  
## <a name="core-network-companion-guide-deploy-password-based-8021x-authenticated-wireless-access"></a>Guide d'accompagnement du réseau de base : Déployer un accès sans fil authentifié 802.1X basé sur des mots de passe

Ce guide explique comment reposent sur le réseau de base en fournissant des instructions sur le déploiement Institute of Electrical and Electronics Engineers \(IEEE\) 802. 1 X\-authentifié sans fil IEEE 802.11 accéder à l’aide Protected Extensible Authentication Protocol\ – Microsoft Challenge Handshake Authentication Protocol version 2 \(PEAP\-MS\-CHAP v2\).

La méthode d’authentification PEAP\-MS\-CHAP v2 nécessite que l’authentification des serveurs exécutant le serveur NPS \(NPS\) présenter les clients sans fil avec un certificat de serveur pour prouver l’identité de serveur NPS à le client, mais l’authentification utilisateur n’est pas effectuée à l’aide d’un certificat - au lieu de cela, les utilisateurs fournissent leur nom d’utilisateur de domaine et le mot de passe.

Étant donné que PEAP\-MS\-CHAP v2 requiert que les utilisateurs fournissent des informations d’identification basées sur mot de passe plutôt qu’un certificat pendant le processus d’authentification, il est généralement plus facile et moins coûteux à déployer que EAP\-TLS ou PEAP \-TLS.

Avant d’utiliser ce guide pour déployer l’accès sans fil avec le PEAP\-MS\-méthode d’authentification CHAP v2, vous devez procédez comme suit :

1. Suivez les instructions dans le Guide du réseau pour déployer votre infrastructure de réseau de base, ou avaient déjà les technologies présentées dans ce guide déployé sur votre réseau.
2. Suivez les instructions dans les Core Network Companion Guide déployer certificats de serveur pour les déploiements de sans fil et câblé à 802.1 X, ou avaient déjà les technologies présentées dans ce guide déployé sur votre réseau.

Pour obtenir des instructions sur la façon de déployer l’accès sans fil avec PEAP\-MS\-CHAP v2, consultez [par mot de passe déployer accès 802. 1 X authentifié sans fil](wireless/a-deploy-8021X-wireless-access.md).

## <a name="core-network-companion-guide-deploy-branchcache-hosted-cache-mode"></a>Guide d'accompagnement du réseau de base : Déployer le mode de cache hébergé de BranchCache

Ce guide explique comment déployer BranchCache en Mode de Cache hébergé dans un ou plusieurs succursales.

BranchCache est une technologie d’optimisation étendu réseaux (étendus WAN) de la bande passante qui est incluse dans certaines éditions des systèmes d’exploitation Windows Server 2016 et Windows 10, ainsi que dans les versions antérieures de Windows et Windows Server.

Lorsque vous déployez BranchCache en mode de cache hébergé, le cache de contenu dans la filiale est hébergé sur un ou plusieurs ordinateurs serveurs, lesquels sont appelés « serveurs de cache hébergé ». Serveurs de cache hébergé peuvent exécuter des charges de travail en plus de l’hébergement du cache, ce qui vous permet d’utiliser le serveur à des fins multiples dans la succursale.

Le mode de cache hébergé de BranchCache augmente l’efficacité du cache, car le contenu est disponible même si le client qui avait demandé et mis en cache les données est hors connexion. Étant donné que le serveur de cache hébergé est toujours disponible, davantage de contenu est mis en cache, ce qui économise davantage la bande passante du réseau étendu et améliore l’efficacité de BranchCache.

Lorsque vous déployez le mode de cache hébergé, tous les clients dans une succursale de plusieurs sous-réseaux peuvent accéder à un cache unique, qui est stocké sur le serveur de cache hébergé, même si les clients se trouvent sur des sous-réseaux différents.

Pour obtenir des instructions sur la façon de déployer BranchCache en Mode de Cache hébergé, consultez [déployer le Mode de Cache hébergé BranchCache](bc-hcm/1-Deploy-Bc-Hcm.md).
