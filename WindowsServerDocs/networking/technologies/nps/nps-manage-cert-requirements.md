---
title: Configurer des modèles de certificats pour PEAP et EAP exigences
description: Cette rubrique fournit des informations sur l’utilisation de certificats avec le serveur de stratégie réseau et d’accès à distance dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e597a65982aeead907c9a41f17f1785a0bf81016
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>Configurer des modèles de certificats pour PEAP et EAP exigences

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Tous les certificats qui sont utilisés pour l’authentification d’accès réseau avec Extensible Authentication Protocol\-Transport Layer Security \(EAP\-TLS\), Protected Extensible Authentication Protocol\-Transport Layer Security \(PEAP\-TLS\) et PEAP\-MicrosoftChallenge Handshake Authentication Protocol version2 \ (v2\ MS\-CHAP) doivent respecter la configuration requise pour les certificats X.509 et fonctionnent pour les connexions qui utilisent Secure Socket Layer/Transport Level Security (SSL/TLS). Les certificats de client et le serveur ont des exigences supplémentaires.

>[!IMPORTANT]
>Cette rubrique fournit des instructions pour la configuration de modèles de certificats. Pour utiliser ces instructions, il est nécessaire que vous avez déployé votre propre \(PKI\) Infrastructure à clé publique avec les Services de certificats ActiveDirectory \(ADCS\).

## <a name="minimum-server-certificate-requirements"></a>Exigences de certificat de serveur minimale

Avec PEAP\-MS\-CHAP v2, PEAP\-TLS ou EAP\-TLS comme méthode d’authentification, le serveur NPS doit utiliser un certificat de serveur qui respecte les exigences de certificat de serveur minimale. 

Les ordinateurs clients peuvent être configurés pour valider les certificats de serveur à l’aide de la **valider le certificat du serveur** option sur l’ordinateur client ou dans la stratégie de groupe. 

L’ordinateur client accepte la tentative d’authentification du serveur lorsque le certificat de serveur respecte les exigences suivantes:

- Le nom d’objet contient une valeur. Si vous émettez un certificat à votre serveur exécutant le serveur NPS (Network Policy) qui a un nom d’objet vide, le certificat n’est pas disponible pour authentifier votre serveur NPS. Pour configurer le modèle de certificat avec un nom d’objet:

    1. Ouvrez les modèles de certificats.
    2. Dans le volet d’informations, cliquez sur le modèle de certificat que vous souhaitez modifier, puis cliquez sur **propriétés**.
    3. Cliquez sur le **nom de l’objet** onglet, puis cliquez sur **construire à partir de ces informations ActiveDirectory**.
    4. Dans **format du nom du sujet**, sélectionnez une valeur autre que **aucun**.

- Le certificat d’ordinateur sur les chaînes de serveur pour une autorité de certification racine de confiance (CA) et ne pas les cas d’échec des vérifications effectuées par CryptoAPI et qui sont spécifiés dans la stratégie d’accès à distance ou réseau.

- Le certificat d’ordinateur pour le serveur VPN ou un serveur NPS est configuré avec le rôle de l’authentification du serveur dans les extensions de l’étendue de l’utilisation de clé (EKU). (L’identificateur d’objet pour l’authentification serveur est 1.3.6.1.5.5.7.3.1.)

- Le certificat de serveur est configuré avec une valeur d’algorithme requis **RSA**. Pour configurer le paramètre de chiffrement requis:

    1. Ouvrez les modèles de certificats.
    2. Dans le volet d’informations, cliquez sur le modèle de certificat que vous souhaitez modifier, puis cliquez sur **propriétés**.
    3. Cliquez sur le **chiffrement** onglet. Dans **nom de l’algorithme**, cliquez sur **RSA**. Vérifiez que **taille de clé minimale** est défini sur **2048**.

- Si utilisé, l’extension (SubjectAltName) de Subject Alternative Name, doit contenir le nom DNS du serveur. Pour configurer le modèle de certificat avec le nom du système DNS (Domain Name) du serveur d’inscription: 

    1. Ouvrez les modèles de certificats.
    2. Dans le volet d’informations, cliquez sur le modèle de certificat que vous souhaitez modifier, puis cliquez sur **propriétés**.
    3. Cliquez sur le **nom de l’objet** onglet, puis cliquez sur **construire à partir de ces informations ActiveDirectory**.
    4. Dans **inclure cette information dans le nom de substitution du sujet**, sélectionnez **nom DNS**.

Lorsque vous utilisez PEAP et EAP-TLS, les serveurs NPS affichent une liste de tous les certificats installés dans le magasin de certificats d’ordinateur, avec les exceptions suivantes:

- Les certificats qui ne contiennent pas le rôle de l’authentification du serveur dans les extensions EKU ne sont pas affichés.

- Les certificats qui ne contiennent pas d’un nom d’objet ne sont pas affichés.

- Basé sur Registre et les certificats de carte à puce d’ouverture de session ne sont pas affichés.

Pour plus d’informations, voir [déployer des certificats de serveur pour 802.1 X câblé et sans fil déploiements](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="minimum-client-certificate-requirements"></a>Configuration requise du certificat client minimale

Avec EAP-TLS ou PEAP-TLS, le serveur accepte la tentative d’authentification client lorsque le certificat remplit les conditions suivantes:

- Le certificat client est émis par une autorité de certification d’entreprise ou mappé à un compte d’utilisateur ou d’ordinateur dans les Services de domaine ActiveDirectory \(ADDS\).

- Le certificat utilisateur ou d’ordinateur sur les chaînes de client à une autorité de certification racine de confiance inclut l’objectif de l’authentification du Client dans les extensions EKU \ (l’identificateur d’objet pour l’authentification Client est 1.3.6.1.5.5.7.3.2\) et ne parvient pas les vérifications d’identificateur d’objet certificat qui sont spécifiées dans la stratégie réseau NPS ni les vérifications effectuées par CryptoAPI et qui sont spécifiés dans la stratégie d’accès à distance ou réseau.

- Le client 802. 1 X n’utilise pas les certificats basés sur le Registre qui sont la connexion par carte à puce soit ou certificats protégés par mot de passe.

- Pour les certificats utilisateur, l’extension de \(SubjectAltName\) autre nom du sujet du certificat contient l’UPN \(UPN\) de nom. Pour configurer le nom UPN dans un modèle de certificat:

    1. Ouvrez les modèles de certificats.
    2. Dans le volet d’informations, cliquez sur le modèle de certificat que vous souhaitez modifier, puis cliquez sur **propriétés**.
    3. Cliquez sur le **nom de l’objet** onglet, puis cliquez sur **construire à partir de ces informations ActiveDirectory**.
    4. Dans **inclure cette information dans le nom de substitution du sujet**, sélectionnez **UPN nom \(UPN\)**.

- Certificats de l’ordinateur, l’extension \(SubjectAltName\) Subject Alternative Name dans le certificat doit contenir le nom de domaine complet nom \(FQDN\) du client, qui est également appelé le *nom DNS*. Pour configurer ce nom dans le modèle de certificat:

    1. Ouvrez les modèles de certificats.
    2. Dans le volet d’informations, cliquez sur le modèle de certificat que vous souhaitez modifier, puis cliquez sur **propriétés**.
    3. Cliquez sur le **nom de l’objet** onglet, puis cliquez sur **construire à partir de ces informations ActiveDirectory**.
    4. Dans **inclure cette information dans le nom de substitution du sujet**, sélectionnez **nom DNS**.

Avec PEAP\-TLS et EAP\-TLS, les clients affichent une liste de tous les certificats installés dans le composant logiciel enfichable Certificats, avec les exceptions suivantes:

- Les clients sans fil ne s’affichent pas basé sur Registre et les certificats de carte à puce d’ouverture de session. 

- Les clients sans fil et VPN n’affichent pas les certificats protégés par mot de passe. 

- Les certificats qui ne contiennent pas le rôle de l’authentification du Client dans les extensions EKU ne sont pas affichés.


Pour plus d’informations sur le serveur NPS, voir [serveur NPS (Network Policy)](nps-top.md).
