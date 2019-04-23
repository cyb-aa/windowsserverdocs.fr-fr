---
title: Fonctionnalités avancées de VPN Toujours actif (AlwaysOn)
description: Au-delà du scénario de déploiement fourni dans ce déploiement, vous pouvez ajouter d’autres fonctionnalités avancées de VPN pour améliorer la sécurité et la disponibilité de votre connexion VPN.
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885080"
---
# <a name="advanced-features-of-always-on-vpn"></a>Fonctionnalités avancées de VPN Always On

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

&#171;  [**Précédent :** En savoir plus sur la technologie VPN Always On](../always-on-vpn-technology-overview.md)<br>
&#187;[ **Suivant :** Commencez à planifier le déploiement VPN Always On](always-on-vpn-deploy-planning.md)

Au-delà des scénarios de déploiement fournis, vous pouvez ajouter d’autres fonctionnalités avancées de VPN pour améliorer la sécurité et la disponibilité de votre connexion VPN. Par exemple, ces composants peuvent aider à vous assurer que le client qui se connecte est sain avant d’autoriser une connexion.


## <a name="high-availability"></a>Haute disponibilité

Voici les options supplémentaires pour la haute disponibilité.


|Option  |Description  |
|---------|---------|
|Résilience des serveurs et équilibrage de charge     |Dans les environnements qui requièrent une haute disponibilité ou la prise en charge grand nombre de demandes, vous pouvez augmenter les performances et la résilience d’accès à distance à l’aide d’équilibrage de charge entre plusieurs serveurs qui exécutent le serveur NPS (Network Policy Server) et activation à distance Serveur d’accès au clustering.<p>Documents associés :<ul><li>[Équilibrage de charge de serveur de Proxy de serveur NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[Déployer l’accès à distance dans un Cluster](https://docs.microsoft.com/windows-server/remote/remote-access/ras/cluster/deploy-remote-access-in-cluster)</li></ul>        |
|Résilience de site géographique     |Pour la géolocalisation basé sur IP, vous pouvez utiliser Global Traffic Manager avec DNS dans Windows Server 2016. Pour l’équilibrage de charge géographique plus robuste, vous pouvez utiliser des solutions globales Server l’équilibrage de charge, tels que Microsoft Azure Traffic Manager.<p>Documents associés :<ul><li>[Vue d’ensemble de Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)</li><li>[Microsoft Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

---

## <a name="advanced-authentication"></a>Authentification avancée

Voici les options supplémentaires pour l’authentification.


|Option  |Description  |
|---------|---------|
|Windows Hello Entreprise     |Dans Windows 10, Windows Hello Entreprise remplace les mots de passe par l’authentification forte à 2 facteurs sur les PC et appareils mobiles. Cette authentification se compose d’un nouveau type d’informations d’identification qui sont liée à un appareil et utilise un biométrique ou numéro d’Identification personnel (PIN).<p>Le client VPN Windows 10 est compatible avec Windows Hello entreprise. Une fois que l’utilisateur se connecte avec un mouvement, la connexion VPN utilise le Windows Hello pour le certificat d’entreprise pour l’authentification basée sur le certificat.<p>Documents associés :<ul><li>[Windows Hello entreprise](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>Étude de cas technique : [L’activation de l’accès à distance avec Windows Hello for Business dans Windows 10](https://msdn.microsoft.com/library/mt728163.aspx)</li></ul>         |
|Authentification multifacteur Azure (MFA)     |Azure MFA a cloud locales et dans les versions que vous pouvez intégrer avec le mécanisme d’authentification VPN de Windows.<p>Pour plus d’informations sur le fonctionne de ce mécanisme, consultez [l’authentification RADIUS s’intègrent avec le serveur Azure multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius).         |

---

## <a name="advanced-vpn-features"></a>Fonctionnalités VPN avancées

Voici les options supplémentaires pour des fonctionnalités avancées.


|Option  |Description  |
|---------|---------|
|Filtrage du trafic     |Si vous avez besoin appliquer les applications VPN clients peuvent accéder à, vous pouvez activer les filtres de trafic VPN.<p>Pour plus d’informations, consultez [les fonctionnalités de sécurité VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features).         |
|VPN déclenché par les applications     |Vous pouvez configurer des profils VPN pour se connecter automatiquement lorsque vous démarrent de certaines applications ou certains types d’applications.<p>Pour plus d’informations sur cela et d’autres options de déclenchement, consultez [options déclenché automatiquement le profil VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile).         |
|Accès conditionnel de VPN   |Conformité de dispositif et d’accès conditionnelle peut nécessiter des appareils gérés afin de répondre aux normes avant de se connecter au VPN. L’une des fonctionnalités avancées pour l’accès conditionnel VPN vous permet de restreindre les connexions VPN à ceux où le certificat d’authentification client contient « Des AAD l’accès conditionnel OID de « 1.3.6.1.4.1.311.87 ».<p>Pour restreindre les connexions VPN, vous devez :<ol><li>Sur le serveur NPS, ouvrez le **Network Policy Server** enfichable.</li><li>Développez **stratégies** > **stratégies réseau**.</li><li>Avec le bouton droit le **des connexions de réseau privé virtuel (VPN, Virtual Private Network)** stratégie de réseau, puis sélectionnez **propriétés**.</li><li>Sélectionnez le **paramètres** onglet.</li><li>Sélectionnez **fournisseur spécifique** et cliquez sur **ajouter**.</li><li>Sélectionnez le **OID de certificat autorisées** , puis cliquez sur **ajouter**.</li><li>Collez l’OID d’accès conditionnel AAD de **1.3.6.1.4.1.311.87** comme la valeur d’attribut, puis cliquez sur **OK** à deux reprises.</li><li>Cliquez sur **fermer** , puis **appliquer**.<p>Désormais, lorsque les clients VPN tentent de vous connecter à l’aide de n’importe quel certificat autre que le certificat de cloud de courte durée, la connexion échoue.</li></ol>Pour plus d’informations sur l’accès conditionnel, consultez [VPN et l’accès conditionnel](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access).   |

---


## <a name="additional-protection"></a>Protection supplémentaire

**Attestation de clé Trusted Platform Module (TPM)**<p>
Un certificat de l’utilisateur avec une clé attesté par le module de plateforme sécurisée fournit la garantie de sécurité plus élevé, sauvegardé par non-exportabilité anti-hameçonnage, isolation et de clés fournies par le module de plateforme sécurisée.

Pour plus d’informations sur l’attestation de clé TPM dans Windows 10, consultez [Attestation de clé TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation).

## <a name="next-step"></a>Étape suivante
[Commencez à planifier le déploiement VPN Always On](always-on-vpn-deploy-planning.md): Avant d’installer le rôle de serveur d’accès à distance sur l’ordinateur que vous avez l’intention d’utiliser comme un serveur VPN, procédez comme suit. Après une planification appropriée, vous pouvez déployer VPN Toujours actif (AlwaysOn) et éventuellement configurer l’accès conditionnel pour une connectivité VPN à l’aide d’Azure AD.  

---

## <a name="related-topics"></a>Rubriques connexes
- [L’équilibrage de charge du serveur NPS Proxy](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md): Clients distants d’authentification Dial-In Service RADIUS (User), qui sont des serveurs d’accès réseau comme les serveurs de réseau privé virtuel (VPN) et des points d’accès sans fil, créer des demandes de connexion et les envoient aux serveurs RADIUS tels que NPS. Dans certains cas, un serveur NPS peut recevoir trop de demandes de connexion en même temps, ce qui entraîne une dégradation des performances ou une surcharge.

- [Vue d’ensemble de Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview): Cette rubrique fournit une vue d’ensemble d’Azure Traffic Manager, ce qui vous permet de contrôler la distribution du trafic utilisateur pour les points de terminaison de service. Traffic Manager utilise le système DNS (Domain Name) pour diriger les demandes des clients vers le point de terminaison approprié selon une méthode de routage du trafic et l’intégrité des points de terminaison. 

- [Windows Hello entreprise](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification): Cette rubrique fournit les conditions préalables, telles que les déploiements cloud uniquement et les déploiements hybrides.  Cette rubrique répertorie également les questions fréquemment posées sur Windows Hello entreprise.

- [Étude de cas technique : L’activation de l’accès à distance avec Windows Hello for Business dans Windows 10](https://msdn.microsoft.com/library/mt728163.aspx): Dans cette étude de cas technique, vous découvrez comment Microsoft a implémenté l’accès à distance avec Windows Hello for Business.  Windows Hello entreprise est une clé publique/privée ou une approche d’authentification par certificat pour les organisations et les consommateurs qui vont au-delà des mots de passe. Cette forme d’authentification s’appuie sur les informations d’identification de la paire de clés qui peuvent remplacer des mots de passe et sont résistantes aux failles, aux vols et au phishing. 

- [Intégrer l’authentification RADIUS avec le serveur Azure multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius): Cette rubrique vous présente Ajout et configuration d’une authentification de client RADIUS avec le serveur Azure multi-Factor Authentication. RADIUS est un protocole standard qui permet d’accepter les demandes d’authentification et de traiter ces demandes. Le serveur Azure multi-Factor Authentication peut agir comme un serveur RADIUS. 

- [Fonctionnalités de sécurité VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features): Cette rubrique fournit des instructions de sécurité VPN de vous pour LockDown VPN, l’intégration de Protection des informations Windows (WIP) avec un VPN et les filtres de trafic. 

- [Options de VPN déclenché automatiquement le profil](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile): Cette rubrique vous fournit des options de profil à déclenchement automatique VPN, comme déclencheur d’application, en fonction du nom de déclencheur et Always On.

- [VPN et l’accès conditionnel](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Cette rubrique fournit une vue d’ensemble de la plateforme d’accès conditionnel basé sur le cloud pour fournir une option de conformité d’appareil pour les clients distants. L’accès conditionnel est un moteur d’évaluation basé sur les stratégies qui vous permet de créer des règles d’accès pour n’importe quelle application connectée Azure Active Directory (Azure AD). 

- [Attestation de clé TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation): Cette rubrique fournit une vue d’ensemble du Module de plateforme sécurisée (TPM) et les étapes pour déployer l’attestation de clé TPM. Vous trouverez également des informations et les étapes pour résoudre les problèmes de dépannage. 

---