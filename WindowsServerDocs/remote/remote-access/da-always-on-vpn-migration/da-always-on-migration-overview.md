---
title: Présentation de la migration toujours sur VPN d’accès à distance
description: VPN Always On traite les précédentes écarts entre les réseaux privés virtuels Windows et DirectAccess et comment migrer à partir de DirectAccess pour VPN Always On.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/29/2018
ms.openlocfilehash: 402d8ff72fe869572c9e6129cdf1aa7e755c354a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845980"
---
# <a name="overview-of-the-directaccess-to-always-on-vpn-migration"></a>Vue d'ensemble de la migration de DirectAccess vers VPN Toujours actif (AlwaysOn) 

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

&#187;[ **Suivant :** Planifier la DirectAccess pour la migration VPN Always On](da-always-on-migration-planning.md)

Dans les versions précédentes de l’architecture VPN de Windows, les limitations de plateforme rendaient difficile pour fournir les fonctionnalités critiques nécessaires pour remplacer DirectAccess, telles que les connexions automatique initiées avant que les utilisateurs se connectent. VPN Toujours actif (AlwaysOn), toutefois, a atténué la plupart de ces limitations ou étendu la fonctionnalité VPN au-delà des capacités de DirectAccess. VPN Always On résout les écarts précédents entre des réseaux privés virtuels Windows et DirectAccess.

DirectAccess – à – toujours sur le processus de migration de VPN se compose de quatre composants principaux et les processus de haut niveau :


1.  **Planifier la migration de VPN Always On.** Planification permet d’identifier les clients cible pour la séparation des phases utilisateur ainsi que les infrastructure et les fonctionnalités.

    1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

    2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

    3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

    4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

2.  **Déployer une infrastructure VPN côte à côte.** Une fois que vous avez déterminé vos phases de migration et les fonctionnalités que vous souhaitez inclure dans votre déploiement, vous déployez l’infrastructure VPN Always On côte à côte avec l’infrastructure DirectAccess existant.  

3.  **Déployer des certificats et configuration aux clients.**  Une fois que l’infrastructure VPN est prêt, vous créez et publiez les certificats requis sur le client. Lorsque les clients ont reçu les certificats, vous déployez le script de configuration VPN_Profile.ps1. Vous pouvez également utiliser Intune pour configurer le client VPN. Utilisez Microsoft System Center Configuration Manager ou Microsoft Intune pour surveiller les déploiements de configuration VPN réussis.

4.  **Supprimer et mettre hors service.** Désactivez correctement l’environnement après avoir migré tout le monde off DirectAccess.

    1.  [!INCLUDE [remove-da-from-client-shortdesc-include](../includes/remove-da-from-client-shortdesc-include.md)]

    2.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]


## <a name="directaccess-deployment-scenario"></a>Scénario de déploiement de DirectAccess

Dans ce scénario de déploiement, vous utilisez un scénario de déploiement de DirectAccess simple comme point de départ pour la migration de que ce guide vous présente. Vous n’avez pas besoin de correspondre à ce scénario de déploiement avant de migrer vers VPN Always On, mais pour de nombreuses organisations, ce paramétrage simple est une représentation exacte de son déploiement de DirectAccess en cours. Le tableau ci-dessous fournit une liste de fonctionnalités de base pour cette installation.

Plusieurs scénarios de déploiement de DirectAccess et les options existent, par conséquent, votre implémentation est susceptible d’être différente de celle décrite ici. Si tel est le cas, reportez-vous à [mappage de fonctionnalité entre DirectAccess et VPN Always On](../vpn/vpn-map-da.md) pour déterminer la fonctionnalité de VPN Always On définie de mappage pour vos ajouts actuels, puis ajouter ces fonctionnalités à votre configuration. En outre, vous pouvez faire référence à la [améliorations de VPN Always On](../vpn/always-on-vpn/always-on-vpn-enhancements.md) pour ajouter des options à votre déploiement de VPN Always On.

>[!NOTE] 
>Pour les appareils joints à un domaine, il existe des considérations supplémentaires, telles que l’inscription de certificats. Pour plus d’informations, consultez [toujours sur VPN déploiement de Windows Server et Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md).

### <a name="deployment-scenario-feature-list"></a>Liste de fonctionnalités de scénario de déploiement

| Fonctionnalité de DirectAccess | Scénario typique |
|-----|----|
| Scénario de déploiement                   | Déployer DirectAccess complet pour l’accès client et la gestion à distance                                               |
| Cartes réseau                      | 2                                                                                                              |
| Authentification utilisateur                   | Informations d’identification Active Directory                                                                                   |
| Utiliser des certificats d’ordinateur             | Oui                                                                                                            |
| Groupes de sécurité                       | Oui                                                                                                            |
| Serveur DirectAccess unique            | Oui                                                                                                            |
| Topologie réseau                      | Traduction d’adresses réseau (NAT) derrière un pare-feu de périmètre avec deux cartes réseau                            |
| Mode d’accès                           | Mettre fin au bord                                                                                                    |
| Tunneling                             | Tunneling fractionné                                                                                                   |
| Authentification                        | Authentification standard infrastructure à clé publique (PKI) avec certificat de l’ordinateur ainsi que Kerberos (pas KerbProxy) |
| Protocoles                             | IP via le protocole HTTPS (IP-HTTPS)                                                                                       |
| Réseau de serveur (NLS) emplacement off-box | Oui                                                                                                            |

## <a name="always-on-vpn-deployment-scenario"></a>Scénario de déploiement VPN Always On

Dans ce scénario de déploiement, vous vous concentrer sur la migration d’un environnement simple a DirectAccess à un simple environnement VPN Always On, ce qui est la solution de remplacement de DirectAccess. Le tableau suivant fournit les fonctionnalités utilisées dans cette solution simple. Pour plus d’informations sur les améliorations supplémentaires pour le client VPN Always On, consultez [améliorations de VPN Always On](../vpn/always-on-vpn/always-on-vpn-enhancements.md).

### <a name="always-on-vpn-features-used-in-the-simple-environment"></a>Fonctionnalités VPN Always On utilisées dans l’environnement simple

| Fonctionnalité VPN | Configuration de scénario de déploiement |
|-----|-----|
| Type de connexion | Natif Internet Key Exchange version 2 (IKEv2) |
| Cartes réseau   | 2        |
| Authentification utilisateur  | Informations d’identification Active Directory            |
| Utiliser des certificats d’ordinateur        | Oui                          |
| Routage | Le Tunneling fractionné |
| Résolution de noms | Liste d’informations de nom de domaine et le suffixe de nom de domaine (DNS) |
| Déclenchement | Détection de réseau Always on et approuvé |
| Authentification  | Protégé Extensible Authentication Protocol-Transport Layer Security (PEAP-TLS) avec les certificats utilisateur protégé par le Module de plateforme sécurisée |

## <a name="next-step"></a>Étape suivante

[Planifier la DirectAccess pour la migration VPN Always On](da-always-on-migration-planning.md). Le principal objectif de la migration est de maintenir la connectivité à distance au bureau pour les utilisateurs tout au long du processus.

---