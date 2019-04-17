---
title: Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL)
description: Un client EAP-TLS ne peut pas se connecter, sauf si le serveur NPS effectue une vérification de révocation de la chaîne de certificats (y compris le certificat racine) du client et vérifie que les certificats ont été révoqués.
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
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067155"
---
# Étape7.1. Configurer EAP-TLS pour ignorer la vérification de la liste de révocation de certificats (CRL)

>S’applique à: Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Précédente:** étape 7. (Facultatif) Accès conditionnel pour une connexion VPN à l’aide d’Azure AD](ad-ca-vpn-connectivity-windows10.md)<br>
& #187; [ **Suivant:** étape 7.2. Créer des certificats racine pour l’authentification VPN avec Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)

>[!IMPORTANT]
>Pour implémenter cette modification du Registre provoquera des connexions IKEv2 à l’aide de certificats de cloud avec le protocole PEAP échec, mais les connexions de protocole IKEv2 à l’aide de certificats de Client Auth émis par l’autorité de certification sur site seraient continuent de fonctionner.

Dans cette étape, vous pouvez ajouter **IgnoreNoRevocationCheck** et définir pour autoriser l’authentification de clients lorsque le certificat n’inclut pas les points de distribution. Par défaut, IgnoreNoRevocationCheck est défini sur 0 (désactivé).

>[!NOTE]
>Si un serveur d’accès à distance (RRAS) et de routage Windows utilise NPS au proxy RADIUS appelle à un second serveur NPS, vous devez définir **IgnoreNoRevocationCheck = 1** sur les deux serveurs.

Un client EAP-TLS ne peut pas se connecter, sauf si le serveur NPS effectue une vérification de révocation de la chaîne de certificats (y compris le certificat racine). Certificats de cloud délivrés à l’utilisateur par Azure AD n’ont pas d’une liste de révocation, car elles dépendent des certificats de courte durées avec une durée de vie d’une heure. EAP sur le serveur NPS doit être configuré pour ignorer l’absence d’une liste de révocation. Par défaut, IgnoreNoRevocationCheck est défini sur 0 (désactivé). Ajoutez IgnoreNoRevocationCheck et définissez-la sur 1 pour permettre l’authentification des clients lorsque le certificat n’inclut pas les points de distribution. 

Dans la mesure où la méthode d’authentification est EAP-TLS, cette valeur de Registre est uniquement nécessaire sous EAP\13. Si d’autres méthodes d’authentification EAP sont utilisés, puis la valeur de Registre doit être ajoutée sous celles également. 

**Procédure**

1. Ouvrez **regedit.exe** sur le serveur NPS.

2. Accédez à **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13**.

3. Cliquez sur **Modifier > Nouveau** et **valeur DWORD (32 bits)** , tapez **IgnoreNoRevocationCheck**.

4. Double-cliquez sur **IgnoreNoRevocationCheck** et définissez les données de valeur sur **1**.

5. Cliquez sur **OK** et redémarrer le serveur. Redémarrer les services RRAS et NPS ne suffit pas.

Pour plus d’informations, voir [comment activer ou désactiver la vérification de révocation de certificats (CRL) sur les Clients](https://technet.microsoft.com/library/bb680540.aspx).


|Chemin d’accès au registre  |Extension de protocole EAP  |
|---------|---------|
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\13     |EAP-TLS         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\25     |PEAP         |
|HKLM\SYSTEM\CurrentControlSet\Services\RasMan\PPP\EAP\26     |EAP-MSCHAP v2         |

## Étape suivante

Étape [7.2. Créer des certificats racine pour l’authentification VPN avec Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md): dans cette étape, vous configurez des certificats racine de l’accès conditionnel pour l’authentification VPN avec Azure AD, qui crée automatiquement une application de cloud serveur VPN dans le client. 

---