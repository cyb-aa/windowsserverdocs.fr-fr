---
title: Guides d’accompagnement du réseau principal
description: Cette rubrique fournit une vue d’ensemble des guides d’accompagnement du Guide du réseau Windows Server2016 Core
manager: brianlic
ms.technology: networking
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d57af0bd-9301-4f62-9888-f528cd10451d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3c272c51cc69017b75e50e79e58186c0ea7c6391
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="core-network-companion-guides"></a>Guides d’accompagnement du réseau principal

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Alors que le serveur Windows Server2016 [Guide réseau de base](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) fournit des instructions sur la façon de déployer un nouveau ActiveDirectory&reg; forêt avec un nouveau domaine racine et l’infrastructure réseau, et les Guides d’accompagnement vous offrir la possibilité d’ajouter des fonctionnalités à votre réseau.

Chaque guide d’accompagnement vous permet d’accomplir un objectif particulier après que vous avez déployé votre réseau de base. Dans certains cas, il existe que plusieurs guides d’accompagnement qui, lorsque déployés ensemble et dans l’ordre approprié, permettent à atteindre des objectifs très complexes de manière mesurables, rentable et raisonnable.

Si vous avez déployé votre réseau de base et de domaine ActiveDirectory avant de rencontrer du Guide du réseau principal, vous pouvez toujours utiliser les Guides d’accompagnement pour ajouter des fonctionnalités à votre réseau. Simplement utiliser le Guide réseau de base sous forme de liste de conditions préalables et savoir que, pour déployer des fonctionnalités supplémentaires avec les Guides d’accompagnement, votre réseau doit respecter les conditions préalables qui sont fournis par le Guide du réseau principal.

## <a name="core-network-companion-guide-deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>Le Guide d’accompagnement du réseau de base: Déployer des certificats de serveur pour 802. 1 X câblés et sans fil des déploiements 

Ce guide explique comment créer sur le réseau de base en déployant des certificats de serveur pour les ordinateurs qui exécutent le serveur de stratégie réseau \(NPS\), \(RAS\) Service d’accès à distance ou les deux.

Certificats de serveur sont requis lorsque vous déployez des méthodes de l’authentification basée sur les certificats avec Extensible Authentication Protocol \(EAP\) et \(PEAP\) EAP protégé pour l’authentification d’accès réseau. Déploiement de certificats de serveur avec les Services de certificats ActiveDirectory \(ADCS\) pour les méthodes d’authentification par certificat EAP et PEAP offre les avantages suivants:

- La liaison à une clé privée de l’identité du serveur NPS ou RAS
- Une méthode rentable et sûre pour inscrire automatiquement des certificats sur les serveurs NPS et RAS du membre du domaine
- Méthode efficace pour gérer les certificats et les autorités de certification
- Sécurité fournie par l’authentification basée sur les certificats
- La possibilité d’étendre l’utilisation de certificats à d’autres usages
  
Pour obtenir des instructions sur la façon de déployer des certificats de serveur, voir [déployer des certificats de serveur pour 802.1 X câblé et sans fil déploiements](server-certs/Deploy-Server-Certificates-for-802.1X-Wired-and-Wireless-Deployments.md).  
## <a name="core-network-companion-guide-deploy-password-based-8021x-authenticated-wireless-access"></a>Le Guide d’accompagnement du réseau de base: Déploiement de mot de passe-802. 1 X accès sans fil authentifié

Ce guide explique comment développer le réseau de base en fournissant des instructions sur le déploiement Institute de Electrical and Electronics Engineers \(IEEE\) 802.1X\-accès sans fil IEEE 802.11 à l’aide Protected Extensible Authentication Protocol\ – MicrosoftChallenge Handshake Authentication Protocol version2 authentifié \ (v2\ PEAP\-MS\-CHAP).

La méthode d’authentification PEAP\-MS\-CHAP v2 requiert que l’authentification des serveurs exécutant NPS \(NPS\) présents clients sans fil avec un certificat de serveur pour prouver l’identité du serveur NPS pour le client, toutefois l’authentification utilisateur n’est pas effectuée à l’aide d’un certificat, au lieu de cela, les utilisateurs fournissent leur nom d’utilisateur de domaine et le mot de passe.

Étant donné que PEAP\-MS\-CHAP v2 requiert que les utilisateurs fournissent des informations d’identification de mot de passe au lieu d’un certificat pendant le processus d’authentification, il est généralement plus facile et moins coûteux à déployer que EAP\-TLS ou PEAP\-TLS.

Avant d’utiliser ce guide pour déployer l’accès sans fil avec la méthode d’authentification PEAP\-MS\-CHAP v2, vous devez procédez comme suit:

1. Suivez les instructions dans le Guide de réseau de base pour déployer votre infrastructure réseau de base, ou avaient déjà les technologies présentées dans ce guide déployé sur votre réseau.
2. Suivez les instructions de la base réseau du dispositif complémentaire Guide déployer des certificats de serveur pour 802.1 X câblé et sans fil déploiements ou ont déjà les technologies présentées dans ce guide déployé sur votre réseau.

Pour obtenir des instructions sur la façon de déployer l’accès sans fil avec PEAP\-MS\-CHAP v2, voir [déployer le mot de passe accès 802. 1 X authentifié sans fil](wireless/a-deploy-8021X-wireless-access.md).

## <a name="core-network-companion-guide-deploy-branchcache-hosted-cache-mode"></a>Le Guide d’accompagnement du réseau de base: Déploiement du Mode de Cache hébergé BranchCache

Ce guide explique comment déployer BranchCache en Mode de Cache hébergé dans un ou plusieurs succursales.

BranchCache est une technologie d’optimisation étendu réseau la bande passante (WAN) incluse dans certaines éditions des systèmes d’exploitation Windows Server2016 et Windows10, ainsi que dans les versions antérieures de Windows et Windows Server.

Lorsque vous déployez BranchCache en mode de cache hébergé, le cache de contenu dans une filiale est hébergé sur un ou plusieurs ordinateurs serveurs, qui sont appelés des serveurs de cache hébergé. Serveurs de cache hébergé peuvent exécuter des charges de travail en plus de l’hébergement du cache, qui vous permet d’utiliser le serveur pour couvrir plusieurs dans la filiale.

Le mode de cache hébergé de BranchCache augmente l’efficacité du cache, car le contenu est disponible même si le client qui avait demandé et mis en cache les données est hors connexion. Étant donné que le serveur de cache hébergé est toujours disponible, davantage de contenu est mis en cache, en fournissant des économies de bande passante réseau étendu supérieures, et l’efficacité de BranchCache est améliorée.

Lorsque vous déployez le mode de cache hébergé, tous les clients dans une succursale de plusieurs sous-réseaux peuvent accéder à un seul et même cache, qui est stocké sur le serveur de cache hébergé, même si les clients se trouvent sur des sous-réseaux différents.

Pour obtenir des instructions sur la façon de déployer BranchCache en Mode de Cache hébergé, consultez [déployer le Mode de Cache hébergé BranchCache](bc-hcm/1-Deploy-Bc-Hcm.md).
