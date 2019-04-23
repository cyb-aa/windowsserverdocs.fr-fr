---
title: Accès conditionnel à la connexion VPN à l'aide d'Azure AD
description: Dans cette étape facultative, vous pouvez affiner l’accès d’utilisateurs VPN autorisé vos ressources à l’aide de l’accès conditionnel Azure Active Directory (Azure AD).
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ''
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 07/13/2018
ms.reviewer: deverette
ms.openlocfilehash: c9104a5d9ae3069e753b8b771270502c4264db96
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885270"
---
# <a name="step-7-optional-conditional-access-for-vpn-connectivity-using-azure-ad"></a>Étape 7. (Facultatif) Accès conditionnel pour la connectivité VPN à l’aide d’Azure AD

&#171;  [**Précédent :** Étape 6. Configurer les clients Windows 10 toujours sur les connexions VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)<br>
&#187;[ **Suivant :** Étape 7.1. Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

Dans cette étape facultative, vous pouvez affiner comment les utilisateurs VPN accéder à vos ressources en utilisant [accès conditionnel Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Accès conditionnel Azure AD pour la connectivité de réseau privé virtuel (VPN), vous pouvez protéger les connexions VPN. L’accès conditionnel est un moteur d’évaluation basé sur les stratégies qui vous permet de créer des règles d’accès pour n’importe quelle application connectée Azure Active Directory (Azure AD). 

## <a name="prerequisites"></a>Prérequis

Vous êtes familiarisé avec les rubriques suivantes :
- [Accès conditionnel dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN et l’accès conditionnel](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Pour configurer l’accès conditionnel Azure Active Directory pour la connectivité VPN, vous devez avoir configuré ce qui suit :
- [Infrastructure de serveur](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [Serveur d’accès à distance pour VPN Always On](always-on-vpn/deploy/vpn-deploy-ras.md)
- [Serveur de stratégie réseau](always-on-vpn/deploy/vpn-deploy-nps.md)
- [DNS et les paramètres du pare-feu](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [Client Windows 10 toujours sur les connexions VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checkingvpn-config-eap-tls-to-ignore-crl-checkingmd"></a>[Étape 7.1. Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

Dans cette étape, vous pouvez ajouter **IgnoreNoRevocationCheck** et configurez-le pour permettre l’authentification des clients lorsque le certificat n’inclut pas de points de distribution CRL. Par défaut, IgnoreNoRevocationCheck est définie sur 0 (désactivé).

Un client de EAP-TLS ne peut pas se connecter, sauf si le serveur NPS effectue une vérification de révocation de la chaîne de certificats (y compris le certificat racine). Certificats de cloud émis à l’utilisateur par Azure AD n’ont pas d’une liste de révocation, car elles sont de courte durée de vie certificats avec une durée de vie d’une heure. EAP sur le serveur NPS doit être configuré pour ignorer l’absence d’une liste de révocation. Dans la mesure où la méthode d’authentification EAP-TLS, cette valeur de Registre est uniquement nécessaire sous **EAP\13**. Si d’autres méthodes d’authentification EAP sont utilisés, puis la valeur de Registre doit être ajoutée sous ceux-ci également. 




## <a name="step-72-create-root-certificates-for-vpn-authentication-with-azure-advpn-create-root-cert-for-vpn-auth-azure-admd"></a>[Étape 7.2. Créer des certificats racines pour l’authentification VPN auprès d’Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

Dans cette étape, vous configurez des certificats racines pour l’authentification VPN auprès d’Azure AD, ce qui crée automatiquement une application de cloud de serveur VPN dans le client.  

Pour configurer l’accès conditionnel pour la connectivité VPN, vous devez :
1. Créer un certificat VPN dans le portail Azure (vous pouvez créer plusieurs certificats).
2. Télécharger le certificat VPN.
3. Déployer le certificat sur votre serveur VPN.

## <a name="step-73-configure-the-conditional-access-policyvpn-config-conditional-access-policymd"></a>[Étape 7.3. Configurer la stratégie d’accès conditionnel](vpn-config-conditional-access-policy.md)

Dans cette étape, vous configurez la stratégie d’accès conditionnel pour la connectivité VPN. 

Pour configurer la stratégie d’accès conditionnel, vous devez :
1. Créer une stratégie d’accès conditionnel est attribuée aux utilisateurs VPN.
2. La valeur est l’application Cloud **serveur VPN**.
3. La valeur est l’allocation (contrôle d’accès) **exiger une authentification multifacteur**.  Vous pouvez utiliser d’autres contrôles en fonction des besoins.

## <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-advpn-deploy-cond-access-root-cert-to-on-premise-admd"></a>[Étape 7.4. Déployer des certificats racine de l’accès conditionnel en local AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

Dans cette étape, vous déployez un certificat racine approuvé pour l’authentification VPN à votre site sur AD.

Pour déployer le certificat racine approuvé, vous devez :
1. Ajouter le certificat téléchargé en tant qu’un *AC racine de confiance pour l’authentification VPN*.
2. Importez le certificat racine pour le serveur VPN et le client VPN.
3. Vérifiez que les certificats sont présents et afficheront comme étant fiables.


## <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devicesvpn-create-oma-dm-based-vpnv2-profilesmd"></a>[Étape 7.5. Créer OMA-DM en fonction des profils de VPNv2 pour les appareils Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

Dans cette étape, vous pouvez créer OMA-DM profils VPNv2 à l’aide d’Intune pour déployer une stratégie de Configuration de l’appareil VPN. Si vous souhaitez utiliser SCCM ou un PowerShell Script pour créer des profils de VPNv2, consultez [paramètres VPNv2 CSP](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) pour plus d’informations. 


## <a name="next-step"></a>Étape suivante
[Étape 7.1. Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md): Dans cette étape, vous devez ajouter **IgnoreNoRevocationCheck** et configurez-le pour permettre l’authentification des clients lorsque le certificat n’inclut pas de points de distribution CRL. Par défaut, IgnoreNoRevocationCheck est définie sur 0 (désactivé).

---

## <a name="related-topics"></a>Rubriques connexes
- [Configurer des profils de VPNv2](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Le client VPN est désormais en mesure de s’intégrer à la plateforme d’accès conditionnel basée sur le cloud pour fournir une option de conformité d’appareil pour les clients distants. Dans cette étape, vous configurez les profils de VPNv2 avec  **\<DeviceCompliance > \<activé > true\</activé >**. 
 
- [Amélioration de l’accès à distance dans Windows 10 avec un profil VPN automatique](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile): Découvrez comment Microsoft a implémenté l’accès conditionnel pour la connectivité VPN. Profils VPN contiennent toutes les informations que nécessite une connexion au réseau d’entreprise, y compris les méthodes d’authentification qui sont prises en charge et le serveur VPN auquel l’appareil doit se connecter à un appareil. Modifications dans Windows 10 mise à jour anniversaire, y compris l’accès conditionnel et l’authentification unique, désormais la possibilité de créer notre profil de connexion VPN d’Always-On. Nous avons créé le profil de connexion pour joints au domaine et des périphériques Microsoft gérés par Intune à l’aide de la console System Center Configuration Manager. 

- [Accès conditionnel dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal): La sécurité est une priorité pour les organisations qui utilisent le cloud. Un aspect clé de sécurité du cloud est des identités et des accès lorsqu’il s’agit de la gestion de vos ressources de cloud. Dans un monde mobile-first, cloud-first, les utilisateurs peuvent accéder des ressources de votre organisation à l’aide d’une variété de périphériques et applications à partir de n’importe quel endroit. Par conséquent, nous focaliser uniquement sur qui peut accéder à une ressource ne suffit plus. Afin de maîtriser l’équilibre entre sécurité et productivité, les professionnels de l’informatique doivent aussi prendre la façon dont une ressource est accédée dans une décision de contrôle d’accès.

- [VPN et l’accès conditionnel](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Le client VPN est désormais en mesure de s’intégrer à la plateforme d’accès conditionnel basée sur le cloud pour fournir une option de conformité d’appareil pour les clients distants. L’accès conditionnel est un moteur d’évaluation basé sur les stratégies qui vous permet de créer des règles d’accès pour n’importe quelle application connectée Azure Active Directory (Azure AD). 

---