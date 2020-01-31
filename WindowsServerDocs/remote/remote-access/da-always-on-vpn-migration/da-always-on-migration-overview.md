---
title: Vue d’ensemble de la migration d’accès à distance Always On VPN
description: Always On VPN traite les écarts précédents entre les VPN Windows et DirectAccess, et comment migrer de DirectAccess vers Always On VPN.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/29/2018
ms.openlocfilehash: d3ea6f0e29803b8a709f31811f77678bf03201a8
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822576"
---
# <a name="overview-of-the-directaccess-to-always-on-vpn-migration"></a>Vue d'ensemble de la migration de DirectAccess vers VPN Toujours actif (AlwaysOn) 

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

&#187;[ **Ensuite :** planifier la migration de DIRECTACCESS vers Always on VPN](da-always-on-migration-planning.md)

Dans les versions précédentes de l’architecture VPN Windows, les limites de la plateforme rendaientait difficile de fournir les fonctionnalités critiques nécessaires au remplacement de DirectAccess, telles que les connexions automatiques lancées avant la connexion des utilisateurs. VPN Toujours actif (AlwaysOn), toutefois, a atténué la plupart de ces limitations ou étendu la fonctionnalité VPN au-delà des capacités de DirectAccess. Always On VPN résout les écarts précédents entre les VPN Windows et DirectAccess.

Le processus de migration de DirectAccess-à-Always On VPN se compose de quatre composants principaux et de processus de haut niveau :


1.  **Planifiez la migration Always On VPN.** La planification permet d’identifier les clients cibles pour la séparation des phases utilisateur, ainsi que l’infrastructure et les fonctionnalités.

    1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

    2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

    3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

    4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

2.  **Déployez une infrastructure VPN côte à côte.** Une fois que vous avez déterminé vos phases de migration et les fonctionnalités que vous souhaitez inclure dans votre déploiement, vous déployez l’infrastructure VPN Always On côte à côte avec l’infrastructure DirectAccess existante.  

3.  **Déployez les certificats et la configuration sur les clients.**  Une fois l’infrastructure VPN prête, vous créez et publiez les certificats requis sur le client. Lorsque les clients ont reçu les certificats, vous déployez le script de configuration VPN_Profile. ps1. Vous pouvez également utiliser Intune pour configurer le client VPN. Utilisez le Configuration Manager de point de terminaison Microsoft ou Microsoft Intune pour surveiller les déploiements de configuration VPN réussis.

4.  **Supprimer et désactiver.** Désactivez correctement l’environnement après avoir migré tout le monde hors DirectAccess.

    1.  [!INCLUDE [remove-da-from-client-shortdesc-include](../includes/remove-da-from-client-shortdesc-include.md)]

    2.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]


## <a name="directaccess-deployment-scenario"></a>Scénario de déploiement DirectAccess

Dans ce scénario de déploiement, vous utilisez un scénario de déploiement DirectAccess simple comme point de départ pour la migration que ce guide présente. Vous n’avez pas besoin de faire correspondre ce scénario de déploiement avant de migrer vers Always On VPN, mais pour de nombreuses organisations, cette configuration simple est une représentation exacte de son déploiement DirectAccess actuel. Le tableau ci-dessous fournit une liste des fonctionnalités de base de cette installation.

De nombreux scénarios et options de déploiement DirectAccess existent, de sorte que votre implémentation est susceptible d’être différente de celle décrite ici. Dans ce cas, reportez-vous à la rubrique [mappage des fonctionnalités entre DirectAccess et Always on VPN](../vpn/vpn-map-da.md) pour déterminer le mappage de l’ensemble de fonctionnalités VPN Always on pour vos ajouts actuels, puis ajoutez ces fonctionnalités à votre configuration. Vous pouvez également vous reporter à la [Always on les améliorations du VPN](../vpn/always-on-vpn/always-on-vpn-enhancements.md) pour ajouter des options à votre déploiement Always on VPN.

>[!NOTE] 
>Pour les appareils qui ne sont pas joints à un domaine, des considérations supplémentaires sont à prendre en compte, telles que l’inscription de certificats. Pour plus d’informations, consultez [Always on le déploiement VPN pour Windows Server et Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md).

### <a name="deployment-scenario-feature-list"></a>Liste des fonctionnalités du scénario de déploiement

| Fonctionnalité DirectAccess | Scénario classique |
|-----|----|
| Scénario de déploiement                   | Déployer DirectAccess complet pour l’accès client et l’administration à distance                                               |
| Cartes réseau                      | 2                                                                                                              |
| Authentification utilisateur                   | Informations d’identification Active Directory                                                                                   |
| Utiliser des certificats d’ordinateur             | Oui                                                                                                            |
| Groupes de sécurité                       | Oui                                                                                                            |
| Serveur DirectAccess unique            | Oui                                                                                                            |
| Topologie réseau                      | Traduction d’adresses réseau (NAT) derrière un pare-feu de périmètre avec deux cartes réseau                            |
| Mode d’accès                           | De bout en bout                                                                                                    |
| Tunneling                             | Fractionner le tunnel                                                                                                   |
| Authentification                        | Authentification de l’infrastructure à clé publique (PKI) standard avec certificat d’ordinateur et Kerberos (non KerbProxy) |
| Protocoles                             | IP sur HTTPs (IP-HTTPs)                                                                                       |
| Serveur emplacement réseau (NLS) désactivé | Oui                                                                                                            |

## <a name="always-on-vpn-deployment-scenario"></a>Scénario de déploiement de Always On VPN

Dans ce scénario de déploiement, vous vous concentrez sur la migration d’un environnement DirectAccess simple vers un environnement VPN simple Always On, qui est la solution de remplacement de DirectAccess. Le tableau suivant fournit les fonctionnalités utilisées dans cette solution simple. Pour plus d’informations sur les améliorations supplémentaires apportées au client VPN Always On, consultez [Always on les améliorations du VPN](../vpn/always-on-vpn/always-on-vpn-enhancements.md).

### <a name="always-on-vpn-features-used-in-the-simple-environment"></a>Always On les fonctionnalités VPN utilisées dans l’environnement simple

| Fonctionnalité VPN | Configuration du scénario de déploiement |
|-----|-----|
| Type de connexion | Native protocole IKE (Internet Key Exchange) version 2 (IKEv2) |
| Cartes réseau   | 2        |
| Authentification utilisateur  | Informations d’identification Active Directory            |
| Utiliser des certificats d’ordinateur        | Oui                          |
| Routage | Tunneling fractionné |
| Résolution de noms | Liste d’informations de nom de domaine et suffixe DNS (Domain Name System) |
| Déclenchement | Détection de réseau fiable et Always on |
| Authentification  | PEAP-TLS (Protected Extensible Authentication Protocol-Transport Layer Security) avec des certificats utilisateur protégés par Module de plateforme sécurisée (TPM) |

## <a name="next-step"></a>Étape suivante

[Planifiez DirectAccess pour Always on la migration VPN](da-always-on-migration-planning.md). Le principal objectif de la migration est de maintenir la connectivité à distance au bureau pour les utilisateurs tout au long du processus.

---