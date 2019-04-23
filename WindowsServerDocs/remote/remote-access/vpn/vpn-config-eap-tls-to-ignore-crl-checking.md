---
title: Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL)
description: Un client de EAP-TLS ne peut pas se connecter, sauf si le serveur NPS effectue une vérification de révocation de la chaîne de certificats (y compris le certificat racine) du client et vérifie que les certificats ont été révoqués.
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: ac59c554c69a6138a106a648c3fab3ed4fe05b7b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836420"
---
# <a name="step-71-configure-eap-tls-to-ignore-certificate-revocation-list-crl-checking"></a>Étape 7.1. Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL)

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

&#171;  [**Précédent :** Étape 7. (Facultatif) Accès conditionnel pour la connectivité VPN à l’aide d’Azure AD](ad-ca-vpn-connectivity-windows10.md)<br>
&#187;[ **Suivant :** Étape 7.2. Créer des certificats racines pour l’authentification VPN auprès d’Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

>[!IMPORTANT]
>Pour implémenter cette modification du Registre provoquera des connexions IKEv2 à l’aide de certificats de cloud avec PEAP échec, mais les connexions IKEv2 à l’aide de certificats d’authentification Client émis à partir de l’autorité de certification sur site continuera de fonctionner.

Dans cette étape, vous pouvez ajouter **IgnoreNoRevocationCheck** et configurez-le pour permettre l’authentification des clients lorsque le certificat n’inclut pas de points de distribution CRL. Par défaut, IgnoreNoRevocationCheck est définie sur 0 (désactivé).

>[!NOTE]
>Si un serveur d’accès distant (RRAS) et de routage de Windows utilise NPS pour proxy RADIUS appels à un deuxième serveur NPS, vous devez définir **IgnoreNoRevocationCheck = 1** sur les deux serveurs.

Un client de EAP-TLS ne peut pas se connecter, sauf si le serveur NPS effectue une vérification de révocation de la chaîne de certificats (y compris le certificat racine). Certificats de cloud émis à l’utilisateur par Azure AD n’ont pas d’une liste de révocation, car elles sont de courte durée de vie certificats avec une durée de vie d’une heure. EAP sur le serveur NPS doit être configuré pour ignorer l’absence d’une liste de révocation. Par défaut, IgnoreNoRevocationCheck est définie sur 0 (désactivé). Ajouter IgnoreNoRevocationCheck et affectez-lui la valeur 1 pour permettre l’authentification des clients lorsque le certificat n’inclut pas de points de distribution CRL. 

Dans la mesure où la méthode d’authentification EAP-TLS, cette valeur de Registre est uniquement nécessaire sous EAP\13. Si d’autres méthodes d’authentification EAP sont utilisés, puis la valeur de Registre doit être ajoutée sous ceux-ci également. 

**Procédure**

1. Ouvrez **regedit.exe** sur le serveur NPS.

2. Accédez à **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13**.

3. Cliquez sur **Modifier > New** et sélectionnez **valeur DWORD (32-bit)** et type **IgnoreNoRevocationCheck**.

4. Double-cliquez sur **IgnoreNoRevocationCheck** et attribuez la valeur données **1**.

5. Cliquez sur **OK** et redémarrez le serveur. Redémarrer les services RRAS et NPS ne suffit pas.

Pour plus d’informations, consultez [comment activer ou désactiver la vérification de révocation de certificats (CRL) sur les Clients](https://technet.microsoft.com/library/bb680540.aspx).


|Chemin d’accès au registre  |Extension EAP  |
|---------|---------|
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13     |EAP-TLS         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\25     |PEAP         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\26     |EAP-MSCHAP v2         |

## <a name="next-step"></a>Étape suivante

[Étape 7.2. Créer des certificats racines pour l’authentification VPN auprès d’Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md): Dans cette étape, vous configurez des certificats racine de l’accès conditionnel pour l’authentification VPN auprès d’Azure AD, ce qui crée automatiquement une application de cloud serveur VPN dans le client. 

---