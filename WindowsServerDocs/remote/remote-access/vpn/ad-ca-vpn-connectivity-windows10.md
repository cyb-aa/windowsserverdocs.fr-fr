---
title: Accès conditionnel à la connexion VPN à l'aide d'Azure AD
description: Dans cette étape facultative, vous pouvez ajuster la façon dont les utilisateurs VPN autorisés accèdent à vos ressources à l’aide de l’accès conditionnel Azure Active Directory (Azure AD).
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 06/28/2019
ms.reviewer: deverette
ms.openlocfilehash: be50c8eaf789b6f0737cbe07cf10d041d25e74f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388202"
---
# <a name="step-7-optional-conditional-access-for-vpn-connectivity-using-azure-ad"></a>Étape 7. Facultatif Accès conditionnel pour la connectivité VPN à l’aide de Azure AD

- [**Premier** Étape 6. Configurer les connexions VPN Toujours actif (AlwaysOn) du client Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [**Situé** Étape 7.1. Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

Dans cette étape facultative, vous pouvez ajuster la façon dont les utilisateurs VPN accèdent à vos ressources à l’aide de l' [accès conditionnel Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Avec Azure AD accès conditionnel pour la connectivité de réseau privé virtuel (VPN), vous pouvez protéger les connexions VPN. L’accès conditionnel est un moteur d’évaluation basé sur les stratégies qui vous permet de créer des règles d’accès pour n’importe quelle application connectée Azure Active Directory (Azure AD).

## <a name="prerequisites"></a>Prérequis

Vous êtes familiarisé avec les rubriques suivantes :

- [Accès conditionnel dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN et accès conditionnel](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Pour configurer Azure Active Directory accès conditionnel pour la connectivité VPN, vous devez configurer les éléments suivants :

- [Infrastructure de serveur](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [Serveur d’accès à distance pour Always On VPN](always-on-vpn/deploy/vpn-deploy-ras.md)
- [NPS (Network Policy Server)](always-on-vpn/deploy/vpn-deploy-nps.md)
- [Paramètres DNS et de pare-feu](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [Connexions VPN Always On client Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checkingvpn-config-eap-tls-to-ignore-crl-checkingmd"></a>[Étape 7.1. Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

Dans cette étape, vous pouvez ajouter **IgnoreNoRevocationCheck** et le configurer pour autoriser l’authentification des clients lorsque le certificat n’inclut pas de points de distribution de liste de révocation de certificats. Par défaut, IgnoreNoRevocationCheck a la valeur 0 (désactivé).

Un client EAP-TLS ne peut se connecter que si le serveur NPS termine une vérification de révocation de la chaîne de certificats (y compris le certificat racine). Les certificats Cloud émis à l’utilisateur par Azure AD n’ont pas de liste de révocation des certificats, car il s’agit de certificats à courte durée de vie d’une heure. EAP sur NPS doit être configuré pour ignorer l’absence d’une liste de révocation de certificats. Étant donné que la méthode d’authentification est EAP-TLS, cette valeur de Registre est uniquement nécessaire sous **EAP\13**. Si d’autres méthodes d’authentification EAP sont utilisées, la valeur de registre doit également être ajoutée sous celles-ci.

## <a name="step-72-create-root-certificates-for-vpn-authentication-with-azure-advpn-create-root-cert-for-vpn-auth-azure-admd"></a>[Étape 7.2. Créer des certificats racine pour l’authentification VPN avec Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

Dans cette étape, vous configurez des certificats racines pour l’authentification VPN avec Azure AD, ce qui crée automatiquement une application Cloud de serveur VPN dans le locataire.  

Pour configurer l’accès conditionnel pour la connectivité VPN, vous devez :

1. Créez un certificat VPN dans le Portail Azure.
2. Téléchargez le certificat VPN.
3. Déployez le certificat sur votre serveur VPN.

> [!IMPORTANT]
> Une fois qu’un certificat VPN a été créé dans le Portail Azure, Azure AD commencera à l’utiliser immédiatement pour émettre des certificats à courte durée de vie vers le client VPN. Il est essentiel de déployer immédiatement le certificat VPN sur le serveur VPN afin d’éviter tout problème avec la validation des informations d’identification du client VPN.

## <a name="step-73-configure-the-conditional-access-policyvpn-config-conditional-access-policymd"></a>[Étape 7.3. Configurer la stratégie d’accès conditionnel](vpn-config-conditional-access-policy.md)

Dans cette étape, vous configurez la stratégie d’accès conditionnel pour la connectivité VPN.

Pour configurer la stratégie d’accès conditionnel, vous devez :

1. Créer une stratégie d’accès conditionnel qui est affectée aux utilisateurs VPN.
2. Définissez l’application Cloud sur le **serveur VPN**.
3. Définissez l’autorisation (contrôle d’accès) pour **exiger une authentification multifacteur**.  Vous pouvez utiliser d’autres contrôles en fonction des besoins.

## <a name="step-74-deploy-conditional-access-root-certificates-to-on-premises-advpn-deploy-cond-access-root-cert-to-on-premise-admd"></a>[Étape 7.4. Déployer des certificats racine d’accès conditionnel vers AD @ no__t-0

Dans cette étape, vous déployez un certificat racine approuvé pour l’authentification VPN sur votre annuaire Active Directory local.

Pour déployer le certificat racine approuvé, vous devez :

1. Ajoutez le certificat téléchargé en tant qu' *autorité de certification racine de confiance pour l’authentification VPN*.
2. Importez le certificat racine sur le serveur VPN et le client VPN.
3. Vérifiez que les certificats sont présents et qu’ils s’affichent comme approuvés.

## <a name="step-75-create-oma-dm-based-vpnv2-profiles-to-windows-10-devicesvpn-create-oma-dm-based-vpnv2-profilesmd"></a>[Étape 7.5. Créer des profils VPNv2 basés sur OMA-DM sur les appareils Windows 10](vpn-create-oma-dm-based-vpnv2-profiles.md)

Au cours de cette étape, vous pouvez créer des profils VPNv2 basés sur OMA-DM à l’aide d’Intune pour déployer une stratégie de configuration d’appareil VPN. Si vous souhaitez utiliser SCCM ou un script PowerShell pour créer des profils VPNv2, consultez [paramètres CSP VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) pour plus d’informations.

## <a name="next-steps"></a>Étapes suivantes

[Étape 7.1. Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL) @ no__t-0 : Dans cette étape, vous devez ajouter **IgnoreNoRevocationCheck** et le configurer pour autoriser l’authentification des clients lorsque le certificat n’inclut pas de points de distribution de liste de révocation de certificats. Par défaut, IgnoreNoRevocationCheck a la valeur 0 (désactivé).

## <a name="related-topics"></a>Rubriques connexes

- [Configurer des profils VPNv2](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Le client VPN est désormais en mesure de s’intégrer à la plateforme d’accès conditionnel basée sur le cloud pour fournir une option de conformité d’appareil pour les clients distants. Dans cette étape, vous configurez les profils VPNv2 avec **\<DeviceCompliance > \<Enabled > true @ no__t-3/Enabled >** .

- [Amélioration de l’accès à distance dans Windows 10 avec un profil VPN automatique](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile): Découvrez comment Microsoft implémente l’accès conditionnel pour la connectivité VPN. Les profils VPN contiennent toutes les informations requises par un appareil pour se connecter au réseau d’entreprise, y compris les méthodes d’authentification prises en charge et le serveur VPN auquel l’appareil doit se connecter. Les modifications apportées à la mise à jour anniversaire Windows 10, y compris l’accès conditionnel et l’authentification unique, nous ont permis de créer notre profil de connexion VPN Always on. Nous avons créé le profil de connexion pour les appareils joints à un domaine et gérés par Microsoft Intune à l’aide de la console System Center Configuration Manager.

- [Accès conditionnel dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal): La sécurité est un problème majeur pour les organisations qui utilisent le Cloud. Un aspect clé de la sécurité du Cloud est l’identité et l’accès à la gestion de vos ressources Cloud. Dans un monde de premier plan destiné aux mobiles et au Cloud, les utilisateurs peuvent accéder aux ressources de votre organisation à l’aide d’un large éventail d’appareils et d’applications, où que vous soyez. Par conséquent, il ne suffit plus de se concentrer sur les personnes qui peuvent accéder à une ressource. Pour maîtriser l’équilibre entre sécurité et productivité, les professionnels de l’informatique doivent également prendre en compte la façon dont une ressource est accessible dans une décision de contrôle d’accès.

- [VPN et accès conditionnel](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Le client VPN est désormais en mesure de s’intégrer à la plateforme d’accès conditionnel basée sur le cloud pour fournir une option de conformité d’appareil pour les clients distants. L’accès conditionnel est un moteur d’évaluation basé sur les stratégies qui vous permet de créer des règles d’accès pour n’importe quelle application connectée Azure Active Directory (Azure AD).
