---
title: Fonctionnalités avancées de VPN Toujours actif (AlwaysOn)
description: Au-delà du scénario de déploiement fourni dans ce déploiement, vous pouvez ajouter d’autres fonctionnalités avancées de VPN pour améliorer la sécurité et la disponibilité de la connexion VPN.
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: a544ac3c1a121874170a2fc78a34bd401b8bebe1
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067184"
---
# Fonctionnalités avancées de toujours actif (AlwaysOn)

>S’applique à: Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Précédent:** obtenir des informations sur la technologie toujours actif (AlwaysOn)](../always-on-vpn-technology-overview.md)<br>
& #187; [ **Suivant:** commencer à planifier le déploiement de toujours actif (AlwaysOn)](always-on-vpn-deploy-planning.md)

Au-delà des scénarios de déploiement fournies, vous pouvez ajouter d’autres fonctionnalités avancées de VPN pour améliorer la sécurité et la disponibilité de la connexion VPN. Par exemple, ces composants peuvent aider à garantir que le client qui se connecte est sain avant d’autoriser une connexion.


## Haute disponibilité

Vous trouverez ci-après des options supplémentaires pour une haute disponibilité.


|Option  |Description  |
|---------|---------|
|Résilience des serveurs et l’équilibrage de charge     |Dans les environnements nécessitant une haute disponibilité ou la prise en charge grand nombre de demandes, vous pouvez augmenter les performances et la résilience d’accès à distance à l’aide d’équilibrage de charge entre plusieurs serveurs qui exécutent le serveur NPS (Network Policy Server) et l’activation à distance Serveur d’accès clustering.<p>Documents connexes:<ul><li>[Équilibrage de charge du serveur Proxy à NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[Déployer l’accès à distance dans un cluster](https://docs.microsoft.com/windows-server/remote/remote-access/ras/cluster/deploy-remote-access-in-cluster)</li></ul>        |
|Résilience de site géographique     |Pour géolocalisation basée sur l’adresse IP, vous pouvez utiliser Global Traffic Manager avec le système DNS dans Windows Server 2016. Pour plus robuste géographique l’équilibrage de charge, vous pouvez utiliser des solutions Global Server l’équilibrage de charge, par exemple, Microsoft Azure Traffic Manager.<p>Documents connexes:<ul><li>[Vue d’ensemble du Gestionnaire de trafic](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)</li><li>[Microsoft Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

---

## Authentification avancée

Vous trouverez ci-après des options supplémentaires pour l’authentification.


|Option  |Description  |
|---------|---------|
|WindowsHelloEntreprise     |Dans Windows10, WindowsHello Entreprise remplace les mots de passe par l’authentification forte à 2facteurs sur lesPC et appareils mobiles. Cette authentification se compose d’un nouveau type d’informations d’identification utilisateur qui sont liée à un appareil et utilise un identificateur biométrique ou le numéro d’Identification personnel (PIN).<p>Le client VPN Windows 10 est compatible avec Windows Hello entreprise. Une fois que l’utilisateur se connecte avec un mouvement, la connexion VPN utilise Windows Hello pour le certificat d’entreprise pour l’authentification basée sur le certificat.<p>Documents connexes:<ul><li>[WindowsHello Entreprise](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>Étude de cas technique: [l’activation de l’accès à distance avec Windows Hello entreprise dans Windows 10](https://msdn.microsoft.com/library/mt728163.aspx)</li></ul>         |
|Azure multi-Factor Authentication (MFA)     |Azure MFA a cloud et locaux versions que vous pouvez intégrer avec le mécanisme d’authentification VPN Windows.<p>Pour plus d’informations sur le fonctionne de ce mécanisme, consultez [rayon d’intégrer l’authentification avec le serveur Azure multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius).         |

---

## Fonctionnalités avancées de VPN

Vous trouverez ci-après des options supplémentaires pour les fonctionnalités avancées.


|Option  |Description  |
|---------|---------|
|Le filtrage de trafic     |Si vous avez besoin de mettre en œuvre les applications VPN clients puissent y accéder, vous pouvez activer des filtres de trafic VPN.<p>Pour plus d’informations, voir [les fonctionnalités de sécurité VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features).         |
|VPN déclenché par l’application     |Vous pouvez configurer les profils VPN pour se connecter automatiquement lorsque certaines applications ou les types d’applications de démarrage.<p>Pour plus d’informations à ce sujet et d’autres options de déclenchement, reportez-vous à la section [options de profil à déclenchement automatique VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile).         |
|Accès conditionnel VPN   |Conformité de dispositif et accès conditionnelle peut nécessiter des appareils gérés pour répondre aux normes avant de se connecter au VPN. Une des fonctionnalités avancées pour l’accès conditionnel VPN vous permet de limiter les connexions VPN aux seuls où le certificat d’authentification client contient «Des AAD l’accès conditionnel OID de «1.3.6.1.4.1.311.87».<p>Pour limiter les connexions VPN, vous devez:<ol><li>Sur le serveur NPS, ouvrez le composant logiciel enfichable **Network Policy Server** .</li><li>Développez **stratégies** > **stratégies réseau**.</li><li>Avec le bouton droit de la stratégie de réseau de **connexions réseau privé virtuel (VPN)** , puis sélectionnez **Propriétés**.</li><li>Sélectionnez l’onglet **paramètres** .</li><li>**Fournisseur spécifique** , cliquez sur **Ajouter**.</li><li>Sélectionnez l’option **Autorisées-certificat-OID** , puis cliquez sur **Ajouter**.</li><li>Coller OID accès conditionnel AAD de **1.3.6.1.4.1.311.87** comme valeur d’attribut, puis cliquez sur **OK** .</li><li>Cliquez sur **Fermer** , puis sur **Appliquer**.<p>Désormais, lorsque les clients VPN tentent de vous connecter à l’aide de n’importe quel certificat autres que le certificat de courte durée cloud, la connexion échouera.</li></ol>Pour plus d’informations sur l’accès conditionnel, consultez [VPN et accès conditionnel](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access).   |

---


## Protection supplémentaire

**Attestation de clé Trusted Platform Module (TPM)**<p>
Un certificat de l’utilisateur avec une clé attestée de module de plateforme sécurisée fournit une garantie de sécurité supérieur, sauvegardée par non exportabilité, anti-martèlement et l’isolement de clés fournies par le module de plateforme sécurisée.

Pour plus d’informations sur l’attestation de clé de module de plateforme sécurisée dans Windows 10, voir [Attestation de clé de module de plateforme sécurisée](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation).

## Étape suivante
[Commencer à planifier le déploiement de toujours actif (AlwaysOn)](always-on-vpn-deploy-planning.md): avant d’installer le rôle de serveur d’accès à distance sur l’ordinateur que vous avez l’intention d’utiliser comme serveur VPN, effectuez les tâches suivantes. Après une planification appropriée, vous pouvez déployer VPN Toujours actif (AlwaysOn) et éventuellement configurer l’accès conditionnel pour une connectivité VPN à l’aide d’Azure AD.  

---

## Rubriquesconnexes
- [Équilibrage de charge de serveur Proxy NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md): clients RADIUS Remote Authentication Dial-In User Service (), qui sont des serveurs d’accès réseau tels que les serveurs de réseau privé virtuel (VPN) et points d’accès sans fil, créer des demandes de connexion et les envoyer à RADIUS serveurs tels que NPS. Dans certains cas, un serveur NPS peut recevoir un trop grand nombre de demandes de connexion en même temps, ce qui entraîne une dégradation des performances ou une surcharge.

- [Vue d’ensemble du Gestionnaire de trafic](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview): cette rubrique fournit une vue d’ensemble de Azure Traffic Manager, ce qui vous permet de contrôler la distribution du trafic d’utilisateur pour les points de terminaison de service. Traffic Manager utilise le système DNS (Domain Name) pour diriger les demandes de client vers le point de terminaison la plus appropriée basée sur une méthode de routage du trafic et l’intégrité des points de terminaison. 

- [Windows Hello entreprise](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification): cette rubrique présente les conditions préalables, telles que les déploiements de cloud uniquement et les déploiements hybrides.  Cette rubrique répertorie également les questions fréquemment posées sur Windows Hello entreprise.

- [Étude de cas technique: activation de l’accès à distance de l’avec Windows Hello entreprise dans Windows 10](https://msdn.microsoft.com/library/mt728163.aspx): dans cette étude de cas technique vous découvrez comment Microsoft implémente un accès à distance avec Windows Hello entreprise.  Windows Hello entreprise est une clé privée/publique ou une approche de l’authentification basée sur les certificats pour les organisations et les consommateurs qui va au-delà de mots de passe. Ce type d’authentification repose sur les informations d’identification de la paire de clés qui peuvent remplacer les mots de passe et sont résistant aux violations, vols et anti-hameçonnage. 

- [Rayon d’intégrer l’authentification avec le serveur Azure multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius): cette rubrique vous guide par le biais d’ajout et configuration d’une authentification de client RADIUS avec serveur Azure multi-Factor Authentication. RADIUS est un protocole standard pour accepter les demandes d’authentification et pour traiter les demandes. Le serveur Azure multi-Factor Authentication peut se comporter comme un serveur RADIUS. 

- [Fonctionnalités de sécurité VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features): cette rubrique vous fournit des recommandations en matière de sécurité VPN pour le VPN LockDown, intégration de Protection des informations Windows (WIP) avec un VPN, et de filtres de trafic. 

- [Options de profil à déclenchement automatique VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile): cette rubrique vous fournit des options de profil à déclenchement automatique VPN, comme déclencheur d’application, déclencheur basé sur le nom et toujours actif.

- [VPN et accès conditionnel](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): cette rubrique vous donne un aperçu de la plateforme d’accès conditionnel basé sur le cloud pour fournir une option de conformité de l’appareil pour les clients distants. L’accès conditionnel est un moteur d’évaluation basé sur les stratégies qui vous permet de créer des règles d’accès pour n’importe quelle application connectée Azure Active Directory (Azure AD). 

- [Attestation de clé de module de plateforme sécurisée](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation): cette rubrique vous fournit une vue d’ensemble du Module de plateforme sécurisée (TPM) et les étapes nécessaires pour déployer le module de plateforme sécurisée de clé d’attestation. Vous trouverez également des informations et des étapes pour résoudre les problèmes de résolution des problèmes. 

---