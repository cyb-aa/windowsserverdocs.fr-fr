---
title: Configurer des modèles de certificats pour répondre aux besoins PEAP et EAP
description: Cette rubrique fournit des informations sur l’utilisation de certificats avec le serveur de stratégie réseau et l’accès à distance dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c2593c2739728704a59cde169de28ed0ae979419
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316087"
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>Configurer des modèles de certificats pour répondre aux besoins PEAP et EAP

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Tous les certificats utilisés pour l’authentification d’accès réseau avec le protocole EAP (Extensible Authentication Protocol\-Transport Layer Security \(EAP\-TLS\), Protected Extensible Authentication Protocol\-Transport Layer Security \(PEAP\-TLS\)et PEAP\-Microsoft Challenge Handshake Authentication Protocol version 2 \(MS\-CHAP v2\) doivent satisfaire les exigences pour les certificats X. 509 et travailler pour les connexions qui utilisent Sécurité SSL/TLS (Secure Socket Layer/Transport Level Security). Les certificats du client et du serveur ont des exigences supplémentaires.

>[!IMPORTANT]
>Cette rubrique fournit des instructions sur la configuration des modèles de certificats. Pour utiliser ces instructions, vous devez avoir déployé votre propre infrastructure de clé publique \(PKI\) avec Active Directory Services de certificats \(AD CS\).

## <a name="minimum-server-certificate-requirements"></a>Configuration minimale requise pour les certificats de serveur

Avec PEAP\-MS\-CHAP v2, PEAP\-TLS ou EAP\-TLS comme méthode d’authentification, le serveur NPS doit utiliser un certificat de serveur qui répond à la configuration minimale requise pour les certificats de serveur. 

Les ordinateurs clients peuvent être configurés pour valider les certificats de serveur à l’aide de l’option **valider le certificat du serveur** sur l’ordinateur client ou dans stratégie de groupe. 

L’ordinateur client accepte la tentative d’authentification du serveur lorsque le certificat de serveur remplit les conditions suivantes :

- Le nom d’objet contient une valeur. Si vous émettez un certificat sur votre serveur exécutant le serveur NPS (Network Policy Server) dont le nom d’objet est vide, le certificat n’est pas disponible pour authentifier votre serveur NPS. Pour configurer le modèle de certificat avec un nom d’objet :

    1. Ouvrez Modèles de certificats.
    2. Dans le volet d’informations, cliquez avec le bouton droit sur le modèle de certificat que vous souhaitez modifier, puis cliquez sur **Propriétés** .
    3. Cliquez sur l’onglet nom de l' **objet** , puis sur **construire à partir de ces informations de Active Directory**.
    4. Dans **format du nom**de l’objet, sélectionnez une valeur autre que **aucun**.

- Le certificat d’ordinateur sur le serveur est lié à une autorité de certification racine de confiance et n’entraîne pas l’échec des vérifications effectuées par CryptoAPI et spécifiées dans la stratégie d’accès à distance ou la stratégie réseau.

- Le certificat d’ordinateur pour le serveur NPS ou VPN est configuré avec l’objectif d’authentification du serveur dans les extensions d’utilisation améliorée de la clé. (L’identificateur d’objet pour l’authentification du serveur est 1.3.6.1.5.5.7.3.1.)

- Configurez le certificat de serveur avec le paramètre de chiffrement requis :

    1. Ouvrez Modèles de certificats.
    2. Dans le volet d’informations, cliquez avec le bouton droit sur le modèle de certificat que vous souhaitez modifier, puis cliquez sur **Propriétés**.
    3. Cliquez sur l’onglet **chiffrement** et veillez à configurer les éléments suivants :
       - **Catégorie de fournisseur :** Fournisseur de stockage de clés
       - **Nom de l’algorithme :** DOTÉ
       - **Fournisseurs :** Fournisseur de chiffrement de plateforme Microsoft
       - **Taille de clé minimale :** 2048
       - **Algorithme de hachage :** SHA2
    4. Cliquez sur **Suivant**.

- L’extension de l’autre nom de l’objet (SubjectAltName), si elle est utilisée, doit contenir le nom DNS du serveur. Pour configurer le modèle de certificat avec le nom DNS (Domain Name System) du serveur d’inscription : 

    1. Ouvrez Modèles de certificats.
    2. Dans le volet d’informations, cliquez avec le bouton droit sur le modèle de certificat que vous souhaitez modifier, puis cliquez sur **Propriétés** .
    3. Cliquez sur l’onglet nom de l' **objet** , puis sur **construire à partir de ces informations de Active Directory**.
    4. Dans **inclure ces informations dans le nom de substitution du sujet**, sélectionnez **nom DNS**.

Lorsque vous utilisez PEAP et EAP-TLS, NPSs affiche la liste de tous les certificats installés dans le magasin de certificats de l’ordinateur, avec les exceptions suivantes :

- Les certificats qui ne contiennent pas le rôle d’authentification du serveur dans les extensions EKU ne sont pas affichés.

- Les certificats qui ne contiennent pas de nom de sujet ne sont pas affichés.

- Les certificats basés sur le registre et les certificats de connexion par carte à puce ne sont pas affichés.

Pour plus d’informations, consultez [déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="minimum-client-certificate-requirements"></a>Configuration minimale requise pour les certificats clients

Avec EAP-TLS ou PEAP-TLS, le serveur accepte la tentative d’authentification du client lorsque le certificat remplit les conditions suivantes :

- Le certificat client est émis par une autorité de certification d’entreprise ou mappé à un compte d’utilisateur ou d’ordinateur dans Active Directory Domain Services \(AD DS\).

- Le certificat de l’utilisateur ou de l’ordinateur sur le client est lié à une autorité de certification racine de confiance, y compris l’authentification du client dans les extensions EKU \(l’identificateur d’objet pour l’authentification du client est 1.3.6.1.5.5.7.3.2\)et ne parvient pas à effectuer les vérifications effectuées par CryptoAPI et qui sont spécifiées dans la stratégie d’accès à distance ou la stratégie réseau, ni dans les vérifications d'

- Le client 802.1 X n’utilise pas de certificats basés sur le Registre qui sont des certificats de connexion par carte à puce ou protégés par mot de passe.

- Pour les certificats utilisateur, l’autre nom de l’objet \(extension SubjectAltName\) dans le certificat contient le nom d’utilisateur principal \(\)UPN. Pour configurer l’UPN dans un modèle de certificat :

    1. Ouvrez Modèles de certificats.
    2. Dans le volet d’informations, cliquez avec le bouton droit sur le modèle de certificat que vous souhaitez modifier, puis cliquez sur **Propriétés**.
    3. Cliquez sur l’onglet nom de l' **objet** , puis sur **construire à partir de ces informations de Active Directory**.
    4. Dans **inclure ces informations dans le nom de substitution du sujet**, sélectionnez **nom d’utilisateur principal \(UPN\)** .

- Pour les certificats d’ordinateur, l’autre nom de l’objet \(extension SubjectAltName\) dans le certificat doit contenir le nom de domaine complet \(nom de domaine complet\) du client, également appelé *nom DNS*. Pour configurer ce nom dans le modèle de certificat :

    1. Ouvrez Modèles de certificats.
    2. Dans le volet d’informations, cliquez avec le bouton droit sur le modèle de certificat que vous souhaitez modifier, puis cliquez sur **Propriétés**.
    3. Cliquez sur l’onglet nom de l' **objet** , puis sur **construire à partir de ces informations de Active Directory**.
    4. Dans **inclure ces informations dans le nom de substitution du sujet**, sélectionnez **nom DNS**.

Avec PEAP\-TLS et EAP\-TLS, les clients affichent une liste de tous les certificats installés dans le composant logiciel enfichable Certificats, avec les exceptions suivantes :

- Les clients sans fil n’affichent pas les certificats basés sur le registre et les connexions de carte à puce. 

- Les clients sans fil et les clients VPN n’affichent pas les certificats protégés par mot de passe. 

- Les certificats qui ne contiennent pas le rôle d’authentification du client dans les extensions EKU ne sont pas affichés.


Pour plus d’informations sur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
