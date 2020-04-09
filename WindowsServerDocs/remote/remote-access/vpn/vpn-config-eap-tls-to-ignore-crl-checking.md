---
title: Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL)
description: Un client EAP-TLS ne peut se connecter que si le serveur NPS termine une vérification de révocation de la chaîne de certificats (y compris le certificat racine) du client et vérifie que les certificats ont été révoqués.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.date: 07/13/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: db85d71ed1b7d8d5b3c14ac8ea603789422ea2cb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818882"
---
# <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checking"></a>Étape 7.1. Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Précédent :** Étape 7. Facultatif Accès conditionnel pour la connectivité VPN à l’aide de Azure AD](ad-ca-vpn-connectivity-windows10.md)
- [**Ensuite :** Étape 7,2. Créer des certificats racines pour l’authentification VPN avec Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

>[!IMPORTANT]
>Si vous n’implémentez pas cette modification du Registre, les connexions IKEv2 utilisant des certificats Cloud avec PEAP échouent, mais les connexions IKEv2 utilisant les certificats d’authentification client émis par l’autorité de certification locale continuent de fonctionner.

Dans cette étape, vous pouvez ajouter **IgnoreNoRevocationCheck** et le configurer pour autoriser l’authentification des clients lorsque le certificat n’inclut pas de points de distribution de liste de révocation de certificats. Par défaut, IgnoreNoRevocationCheck a la valeur 0 (désactivé).

>[!NOTE]
>Si un serveur de routage et d’accès à distance Windows (RRAS) utilise des appels RADIUS NPS pour proxy vers un deuxième serveur NPS, vous devez définir **IgnoreNoRevocationCheck = 1** sur les deux serveurs.

Un client EAP-TLS ne peut se connecter que si le serveur NPS termine une vérification de révocation de la chaîne de certificats (y compris le certificat racine). Les certificats Cloud émis à l’utilisateur par Azure AD n’ont pas de liste de révocation des certificats, car il s’agit de certificats à courte durée de vie d’une heure. EAP sur NPS doit être configuré pour ignorer l’absence d’une liste de révocation de certificats. Par défaut, IgnoreNoRevocationCheck a la valeur 0 (désactivé). Ajoutez IgnoreNoRevocationCheck et affectez-lui la valeur 1 pour autoriser l’authentification des clients lorsque le certificat n’inclut pas de points de distribution de liste de révocation de certificats. 

Étant donné que la méthode d’authentification est EAP-TLS, cette valeur de Registre est uniquement nécessaire sous EAP\13. Si d’autres méthodes d’authentification EAP sont utilisées, la valeur de registre doit également être ajoutée sous celles-ci. 

**Procédures**

1. Ouvrez **regedit. exe** sur le serveur NPS.

2. Accédez à **HKEY_LOCAL_MACHINE \system\currentcontrolset\services\rasman\ppp\eap\13**.

3. Sélectionnez **modifier > nouveau** , puis sélectionnez **valeur DWORD (32 bits)** et entrez **IgnoreNoRevocationCheck**.

4. Double-cliquez sur **IgnoreNoRevocationCheck** et définissez les données de la valeur sur **1**.

5. Sélectionnez **OK** et redémarrez le serveur. Le redémarrage des services RRAS et NPS ne suffit pas.

Pour plus d’informations, consultez [Comment activer ou désactiver la vérification de la révocation des certificats sur les clients](https://technet.microsoft.com/library/bb680540.aspx).


|Chemin d’accès au registre  |Extension EAP  |
|---------|---------|
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13     |EAP-TLS         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\25     |PEAP         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\26     |EAP-MSCHAP v2         |

## <a name="next-steps"></a>Étapes suivantes :

[Étape 7,2. Créer des certificats racines pour l’authentification VPN avec Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md): dans cette étape, vous configurez des certificats racines d’accès conditionnel pour l’authentification VPN avec Azure ad, ce qui crée automatiquement une application Cloud de serveur VPN dans le locataire.
