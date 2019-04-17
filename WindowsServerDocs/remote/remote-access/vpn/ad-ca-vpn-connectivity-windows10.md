---
title: Accès conditionnel à la connexion VPN à l'aide d'AzureAD
description: Cette étape facultative, vous pouvez affiner un accès VPN aux utilisateurs comment autorisé vos ressources à l’aide de l’accès conditionnel Azure Active Directory (Azure AD).
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
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067119"
---
# Étape7. (Facultatif) Accès conditionnel pour une connexion VPN à l’aide d’Azure AD

& #171;  [ **Précédente:** étape 6. Configurer le Client Windows 10 toujours sur des connexions VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)<br>
& #187; [ **Suivant:** étape 7.1. Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

Cette étape facultative, vous pouvez affiner la façon dont les utilisateurs VPN accèdent à vos ressources à l’aide de [l’accès conditionnel Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Avec l’accès conditionnel à Azure AD pour la connectivité réseau privé virtuel (VPN), vous pouvez protéger les connexions VPN. L’accès conditionnel est un moteur d’évaluation basé sur les stratégies qui vous permet de créer des règles d’accès pour n’importe quelle application connectée Azure Active Directory (Azure AD). 

## Conditions préalables

Vous êtes familiarisé avec les rubriques suivantes:
- [Accès conditionnel dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
- [VPN et accès conditionnel](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)

Pour configurer l’accès conditionnel Azure Active Directory pour une connexion VPN, vous devez disposer des éléments suivants sont configurés:
- [Infrastructure de serveur](always-on-vpn/deploy/vpn-deploy-server-infrastructure.md)
- [Serveur d’accès à distance pour toujours sur le réseau privé virtuel](always-on-vpn/deploy/vpn-deploy-ras.md)
- [Serveur NPS \(Network Policy Server\)](always-on-vpn/deploy/vpn-deploy-nps.md)
- [DNS et paramètres du pare-feu](always-on-vpn/deploy/vpn-deploy-dns-firewall.md)
- [Client Windows 10 toujours sur des connexions VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)

## [Étape7.1. Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)

Dans cette étape, vous pouvez ajouter **IgnoreNoRevocationCheck** et définir pour autoriser l’authentification de clients lorsque le certificat n’inclut pas les points de distribution. Par défaut, IgnoreNoRevocationCheck est défini sur 0 (désactivé).

Un client EAP-TLS ne peut pas se connecter, sauf si le serveur NPS effectue une vérification de révocation de la chaîne de certificats (y compris le certificat racine). Certificats de cloud délivrés à l’utilisateur par Azure AD n’ont pas d’une liste de révocation, car elles dépendent des certificats de courte durées avec une durée de vie d’une heure. EAP sur le serveur NPS doit être configuré pour ignorer l’absence d’une liste de révocation. Dans la mesure où la méthode d’authentification est EAP-TLS, cette valeur de Registre est uniquement nécessaire sous **EAP\13**. Si d’autres méthodes d’authentification EAP sont utilisés, puis la valeur de Registre doit être ajoutée sous celles également. 




## [Étape7.2. Créer des certificats racine pour l'authentification VPN avec AzureAD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

Dans cette étape, vous configurez des certificats racines pour l’authentification VPN avec Azure AD, qui crée automatiquement une application cloud du serveur VPN dans le client.  

Pour configurer l’accès conditionnel pour une connexion VPN, vous devez:
1. Créer un certificat VPN dans le portail Azure (vous pouvez créer plusieurs certificats).
2. Télécharger le certificat VPN.
3. Déployez le certificat à votre serveur VPN.

## [Étape7.3. Configurer la stratégie d'accès conditionnel](vpn-config-conditional-access-policy.md)

Dans cette étape, vous configurez la stratégie d’accès conditionnel pour une connexion VPN. 

Pour configurer la stratégie d’accès conditionnel, vous devez:
1. Créer une stratégie d’accès conditionnel qui est attribuée aux utilisateurs VPN.
2. Définissez l’application Cloud sur **serveur VPN**.
3. Définir l’octroi (contrôle d’accès) pour **demander une authentification multifacteur**.  Vous pouvez utiliser d’autres contrôles en fonction des besoins.

## [Étape7.4. Déployer des certificats racine de l’accès conditionnel sur site AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

Dans cette étape, vous déployez un certificat racine de confiance pour l’authentification VPN sur votre local AD.

Pour déployer le certificat racine de confiance, vous devez:
1. Ajoutez le certificat téléchargé en tant que *racine de confiance autorité de certification pour l’authentification VPN*.
2. Importer le certificat racine pour le serveur VPN et le client VPN.
3. Vérifiez que les certificats sont présentes et afficheront comme approuvé.


## [Étape7.5. Créer des profils VPNv2 basés sur OMA-DM sur les appareils Windows10](vpn-create-oma-dm-based-vpnv2-profiles.md)

Dans cette étape, vous pouvez créer des OMA-DM profils VPNv2 à l’aide d’Intune pour déployer une stratégie de Configuration de l’appareil VPN. Si vous souhaitez utiliser SCCM ou un PowerShell Script pour créer des profils de VPNv2, reportez-vous à la section [paramètres CSP VPNv2](https://docs.microsoft.com/windows/client-management/mdm/vpnv2-csp) pour plus d’informations. 


## Étape suivante
Étape [7.1. Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md): dans cette étape, vous devez ajouter **IgnoreNoRevocationCheck** et définir pour autoriser l’authentification de clients lorsque le certificat n’inclut pas les points de distribution. Par défaut, IgnoreNoRevocationCheck est défini sur 0 (désactivé).

---

## Rubriquesconnexes
- [Configuration des profils de VPNv2](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): le client VPN est désormais en mesure de s’intégrer à la plateforme d’accès conditionnel basé sur le cloud pour fournir une option de conformité de l’appareil pour les clients distants. Dans cette étape, vous configurez les profils VPNv2 avec **\ < DeviceCompliance > \ < Enabled > true\ < / activée >**. 
 
- [Amélioration de l’accès à distance dans Windows 10 avec un profil VPN automatique](https://www.microsoft.com/itshowcase/Article/Content/894/Enhancing-remote-access-in-Windows-10-with-an-automatic-VPN-profile): Découvrez comment Microsoft implémente l’accès conditionnel pour une connexion VPN. Profils VPN contient toutes les informations que nécessite une connexion au réseau d’entreprise, y compris les méthodes d’authentification qui sont pris en charge et le serveur VPN auquel l’appareil doit se connecter à un appareil. Modifications apportées à Windows 10 mise à jour anniversaire, y compris l’accès conditionnel et l’authentification unique, rend possible pour que nous puissions créer notre profil de connexion VPN sacoche. Nous avons créé le profil de connexion à un domaine et les appareils Microsoft Intune géré à l’aide de la console System Center Configuration Manager. 

- [Instruction conditionnelle accès à Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal): la sécurité est une préoccupation majeure pour les organisations qui utilisent le cloud. Un aspect clé de sécurité du cloud est identité et accès en matière de gestion de vos ressources de cloud. Dans un monde axé sur le mobile, axé, les utilisateurs peuvent accéder à des ressources de votre organisation à l’aide d’un éventail d’appareils et les applications à partir de n’importe quel endroit. Suite à cela, tout en se concentrant sur qui peut accéder à une ressource n’est pas suffisant plus. Pour l’équilibre entre la sécurité et la productivité de maître, professionnels de l’informatique doivent aussi factoriser comment une ressource résidant dans une décision de contrôle d’accès.

- [VPN et accès conditionnel](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): le client VPN est désormais en mesure de s’intégrer à la plateforme d’accès conditionnel basé sur le cloud pour fournir une option de conformité de l’appareil pour les clients distants. L’accès conditionnel est un moteur d’évaluation basé sur les stratégies qui vous permet de créer des règles d’accès pour n’importe quelle application connectée Azure Active Directory (Azure AD). 

---