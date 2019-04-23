---
title: Configurer des modèles de certificats pour répondre aux besoins PEAP et EAP
description: Cette rubrique fournit des informations sur l’utilisation de certificats avec le serveur de stratégie réseau et accès à distance dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 41f6f88705fa3d58be695fd825d960e7df21cd24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885880"
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>Configurer des modèles de certificats pour répondre aux besoins PEAP et EAP

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Tous les certificats qui sont utilisés pour l’authentification de l’accès réseau avec le protocole d’authentification Extensible\-Transport Layer Security \(EAP\-TLS\), Protected Extensible Authentication Protocol\- Transport Layer Security \(PEAP\-TLS\), PEAP et\-Microsoft Challenge Handshake Authentication Protocol version 2 \(MS\-CHAP v2\) doit la configuration requise pour les certificats X.509 et fonctionner pour les connexions qui utilisent Secure Socket Layer/Transport Level Security (SSL/TLS). Les certificats de client et le serveur ont des exigences supplémentaires.

>[!IMPORTANT]
>Cette rubrique fournit des instructions sur la configuration des modèles de certificats. Pour utiliser ces instructions, il est nécessaire que vous avez déployé votre propre Infrastructure à clé publique \(PKI\) avec Active Directory Certificate Services \(AD CS\).

## <a name="minimum-server-certificate-requirements"></a>Configuration requise des certificats minimale du serveur

Avec PEAP\-MS\-CHAP v2, PEAP\-TLS ou EAP\-TLS comme méthode d’authentification, le serveur NPS doit utiliser un certificat de serveur qui répond aux exigences de certificat de serveur minimale. 

Ordinateurs clients peuvent être configurés pour valider les certificats de serveur à l’aide de la **valider le certificat du serveur** option sur l’ordinateur client ou dans la stratégie de groupe. 

L’ordinateur client accepte la tentative d’authentification du serveur lorsque le certificat de serveur remplit les conditions suivantes :

- Le nom du sujet contient une valeur. Si vous exécutez un certificat à votre serveur exécutant le serveur NPS (Network Policy) qui a un nom de sujet vide, le certificat n’est pas disponible pour authentifier le serveur NPS. Pour configurer le modèle de certificat avec un nom de sujet :

    1. Ouvrez Modèles de certificats.
    2. Dans le volet de détails, cliquez sur le modèle de certificat que vous souhaitez modifier, puis cliquez sur **propriétés** .
    3. Cliquez sur le **nom de l’objet** onglet, puis cliquez sur **construire à partir de ces informations Active Directory**.
    4. Dans **format du nom du sujet**, sélectionnez une valeur autre que **aucun**.

- Le certificat d’ordinateur sur les chaînes de serveur à une autorité de certification (CA) racine approuvée et ne pas les cas d’échec des vérifications effectuées par CryptoAPI et qui sont spécifiés dans la stratégie d’accès à distance ou réseau.

- Le certificat d’ordinateur pour le serveur NPS ou VPN est configuré avec le rôle de l’authentification du serveur dans les extensions de l’étendue de l’utilisation de clé (EKU). (L’identificateur d’objet pour l’authentification du serveur est 1.3.6.1.5.5.7.3.1.)

- Configurez le certificat de serveur avec le paramètre de chiffrement requis :

    1. Ouvrez Modèles de certificats.
    2. Dans le volet de détails, cliquez sur le modèle de certificat que vous souhaitez modifier, puis cliquez sur **propriétés**.
    3. Cliquez sur le **cryptographie** onglet et veillez à configurer les éléments suivants :
       - **Catégorie de fournisseur :** Key Storage Provider
       - **Nom de l’algorithme :** RSA
       - **Fournisseurs :** Fournisseur de chiffrement de plateforme Microsoft
       - **Taille de clé minimale :** 2 048
       - **Algorithme de hachage :** SHA2
    4. Cliquez sur **Suivant**.

- Si utilisé, l’extension de l’autre nom (SubjectAltName), doit contenir le nom DNS du serveur. Pour configurer le modèle de certificat avec le nom du système DNS (Domain Name) du serveur de l’inscription : 

    1. Ouvrez Modèles de certificats.
    2. Dans le volet de détails, cliquez sur le modèle de certificat que vous souhaitez modifier, puis cliquez sur **propriétés** .
    3. Cliquez sur le **nom de l’objet** onglet, puis cliquez sur **construire à partir de ces informations Active Directory**.
    4. Dans **inclure cette information dans le nom de substitution du sujet**, sélectionnez **nom DNS**.

Lorsque vous utilisez PEAP et EAP-TLS, NPSs afficher une liste de tous les certificats installés dans le magasin de certificats d’ordinateur, avec les exceptions suivantes :

- Les certificats qui ne contiennent pas le rôle de l’authentification du serveur dans leurs extensions ne sont pas affichés.

- Les certificats qui ne contiennent pas un nom d’objet ne sont pas affichés.

- En fonction du Registre et les certificats de carte à puce d’ouverture de session ne sont pas affichés.

Pour plus d’informations, consultez [déployer des certificats de serveur pour les déploiements de sans fil et câblé à 802.1 X](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="minimum-client-certificate-requirements"></a>Configuration requise des certificats minimale du client

Avec EAP-TLS ou PEAP-TLS, le serveur accepte la tentative d’authentification client lorsque le certificat remplit les conditions suivantes :

- Le certificat client est émis par une autorité de certification d’entreprise ou mappé à un compte d’utilisateur ou d’ordinateur dans Active Directory Domain Services \(AD DS\).

- Le certificat utilisateur ou un ordinateur sur le client est lié à une autorité de certification racine de confiance inclut l’objectif de l’authentification du Client dans les extensions EKU \(l’identificateur d’objet pour l’authentification du Client est 1.3.6.1.5.5.7.3.2\)et échoue ni le vérifications qui sont effectuées par CryptoAPI et qui sont spécifiés dans la stratégie d’accès à distance ou stratégie de réseau ni les vérifications d’identificateur d’objet certificat qui sont spécifiées dans la stratégie de réseau NPS.

- Le client 802. 1 X n’utilise pas les certificats basés sur le Registre qui sont soit une carte à puce-ouverture de session ou certificats protégés par mot de passe.

- Pour les certificats utilisateur, l’autre nom de sujet \(SubjectAltName\) extension dans le certificat contient le nom d’utilisateur principal \(UPN\). Pour configurer l’UPN dans un modèle de certificat :

    1. Ouvrez Modèles de certificats.
    2. Dans le volet de détails, cliquez sur le modèle de certificat que vous souhaitez modifier, puis cliquez sur **propriétés**.
    3. Cliquez sur le **nom de l’objet** onglet, puis cliquez sur **construire à partir de ces informations Active Directory**.
    4. Dans **inclure cette information dans le nom de substitution du sujet**, sélectionnez **nom d’utilisateur principal \(UPN\)**.

- Certificats de l’ordinateur, l’autre nom de sujet \(SubjectAltName\) extension dans le certificat doit contenir le nom de domaine pleinement qualifié \(FQDN\) du client, également appelé le  *Nom DNS*. Pour configurer ce nom dans le modèle de certificat :

    1. Ouvrez Modèles de certificats.
    2. Dans le volet de détails, cliquez sur le modèle de certificat que vous souhaitez modifier, puis cliquez sur **propriétés**.
    3. Cliquez sur le **nom de l’objet** onglet, puis cliquez sur **construire à partir de ces informations Active Directory**.
    4. Dans **inclure cette information dans le nom de substitution du sujet**, sélectionnez **nom DNS**.

Avec PEAP\-TLS et EAP\-TLS, les clients affichent une liste de tous les certificats installés dans le composant logiciel enfichable Certificats, avec les exceptions suivantes :

- Les clients sans fil ne s’affichent pas basée sur le Registre et les certificats de carte à puce d’ouverture de session. 

- Les clients sans fil et VPN n’affichent pas les certificats protégés par mot de passe. 

- Les certificats qui ne contiennent pas le rôle de l’authentification du Client dans leurs extensions ne sont pas affichés.


Pour plus d’informations sur le serveur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
