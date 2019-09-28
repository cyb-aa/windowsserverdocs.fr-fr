---
title: Résolution des problèmes de AD FS-authentification Windows intégrée
description: Ce document décrit comment résoudre les problèmes liés à l’authentification Windows intégrée
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 98f6c2e39b2a5eeab76103c1ae477dde785a0e04
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385395"
---
# <a name="ad-fs-troubleshooting---integrated-windows-authentication"></a>Résolution des problèmes de AD FS-authentification Windows intégrée
L’authentification Windows intégrée permet aux utilisateurs de se connecter avec leurs informations d’identification Windows et d’utiliser l’authentification unique (SSO) à l’aide de Kerberos ou NTLM.

## <a name="reason-integrated-windows-authentication-fails"></a>Raison de l’échec de l’authentification Windows intégrée
Trois raisons principales justifient l’échec de l’authentification Windows intégrée. Celles-ci sont les suivantes :
    - Configuration inconfigurée du nom de principal du service (SPN)
    - Jeton de liaison de canal
    - Configuration d’Internet Explorer

## <a name="spn-misconfiguration"></a>Configuration Inde SPN
Un nom de principal du service (SPN) est un identificateur unique d’une instance de service. Les SPN sont utilisés par l’authentification Kerberos pour associer une instance de service à un compte d’ouverture de session de service. Cela permet à une application cliente de demander que le service authentifie un compte même si le client n’a pas le nom du compte.

Voici un exemple d’utilisation d’un nom de principal du service avec AD FS :
1. Un navigateur Web interroge Active Directory pour déterminer quel compte de service exécute sts.contoso.com
2. Active Directory indique au navigateur qu’il s’agit du compte de service AD FS.
3. Le navigateur obtiendra un ticket Kerberos pour le compte de service AD FS.

Si le compte de service AD FS a mal configuré ou si le SPN est incorrect, cela peut entraîner des problèmes.  En examinant les suivis réseau, vous pouvez voir des erreurs telles que KRB Error : KRB5KDC_ERR_S_PRINCIPAL_UNKNOWN.

À l’aide des suivis réseau (tels que Wireshark), vous pouvez déterminer le nom de principal du service que le navigateur tente de résoudre, puis utiliser l’outil en ligne de commande, SetSPN-Q <spn>, vous pouvez effectuer une recherche sur ce SPN.  Il est peut-être introuvable ou il peut être attribué à un autre compte autre que le compte de service AD FS.

![intégrer](media/ad-fs-tshoot-iwa/iwa3.png)

Vous pouvez vérifier le nom de principal du service en examinant les propriétés du compte de service AD FS.

![intégrer](media/ad-fs-tshoot-iwa/iwa1.png)

## <a name="channel-binding-token"></a>Jeton de liaison de canal
Actuellement, lorsqu’une application cliente s’authentifie auprès du serveur à l’aide de Kerberos, de Digest ou de NTLM à l’aide du protocole HTTPs, un canal TLS (Transport Level Security) est d’abord établi et l’authentification a lieu à l’aide de ce canal. 

Le jeton de liaison de canal est une propriété du canal externe sécurisé TLS et est utilisé pour lier le canal externe à une conversation sur le canal interne authentifié par le client.

En cas d’attaque de l’intercepteur (« Man-in-the-Middle ») et qu’il est en train de déchiffrer et de rechiffrer le trafic SSL, la clé ne correspondra pas.  AD FS déterminera qu’il y a un point assis au milieu entre le r Web Browse et lui-même.  Cela entraînera l’échec de l’authentification Kerberos et l’utilisateur recevra une boîte de dialogue 401 au lieu d’une expérience SSO.

Cela peut être dû à :
 - tout ce qui se trouve entre le navigateur et le AD FS
 - Fiddler
 - Proxys inverses effectuant le pontage SSL

Par défaut, AD FS a la valeur « autoriser ».  Vous pouvez modifier ce paramètre à l’aide de la applet PowerShell `Set-ADFSProperties -ExtendProtectionTokenCheck`

Pour plus d’informations, consultez [meilleures pratiques pour la planification et le déploiement sécurisés de AD FS](../../ad-fs/design/best-practices-for-secure-planning-and-deployment-of-ad-fs.md).

## <a name="internet-explorer-configuration"></a>Configuration d’Internet Explorer
Par défaut, Internet Explorer se présente comme suit :

1. Internet Explorer recevra une réponse 401 de AD FS avec le mot NEGOTIATE dans l’en-tête.
2. Cela indique au navigateur Web d’obtenir un ticket Kerberos ou NTLM à renvoyer à AD FS.
3. Par défaut, IE essaiera de le faire (SPNEGO) sans interaction de l’utilisateur si le mot NEGOTIATE se trouve dans l’en-tête.  Il ne fonctionne que pour les sites intranet.

Il y a 2 éléments principaux qui peuvent empêcher cela à partir de happeing.
   - L’option Activer l’authentification Windows intégrée n’est pas activée dans les propriétés d’IE.  Cela se trouve sous Options Internet-> Advanced-> Security.
   
   ![intégrer](media/ad-fs-tshoot-iwa/iwa4.png)
   
   - Les zones de sécurité ne sont pas configurées correctement
       - Les noms de domaine complets ne sont pas dans la zone Intranet
       - AD FS URL n’est pas dans la zone intranet.

      ![intégrer](media/ad-fs-tshoot-iwa/iwa5.png)
## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes AD FS](ad-fs-tshoot-overview.md)
