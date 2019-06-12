---
title: Résolution des problèmes d’AD FS - intégré à l’authentification Windows
description: Ce document décrit comment résoudre les problèmes de l’authentification windows intégrée
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9703a8652b0e0bbafe48858cbfbcc8aa9aa31ef8
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812047"
---
# <a name="ad-fs-troubleshooting---integrated-windows-authentication"></a>Résolution des problèmes d’AD FS - intégré à l’authentification Windows
L’authentification Windows intégrée permet aux utilisateurs de se connecter avec leurs informations d’identification Windows et l’expérience de l’authentification unique (SSO), à l’aide de Kerberos ou NTLM.

## <a name="reason-integrated-windows-authentication-fails"></a>Échec de l’authentification windows intégrée de raison
Il existe trois principale raison pourquoi l’authentification intégrée windows échoue. Celles-ci sont les suivantes :
    - Configuration incorrecte de principal du Service Name(SPN)
    - Jeton de liaison de canal
    - Configuration d’Internet Explorer

## <a name="spn-misconfiguration"></a>Configuration incorrecte du nom principal de service
Un nom principal de service (SPN) est un identificateur unique d’une instance de service. Noms principaux de service sont utilisées par l’authentification Kerberos pour associer une instance de service avec un compte de connexion de service. Ainsi, une application cliente demander que le service authentifie un compte même si le client n’a pas le nom du compte.

Un exemple d’un SPN est utilisé avec AD FS est comme suit :
1. Un navigateur web interroge Active Directory pour déterminer quel compte de service est en cours d’exécution sts.contoso.com
2. Active Directory indique au navigateur qu’il est le compte de service AD FS.
3. Le navigateur obtiendra un ticket Kerberos pour le compte de service AD FS.

Si le compte de service AD FS possède une configuration incorrecte ou le SPN incorrect cela peut entraîner des problèmes.  En examinant les traces réseau, vous pouvez voir des erreurs telles que KRB erreur : KRB5KDC_ERR_S_PRINCIPAL_UNKNOWN.

À l’aide de traces réseau (tels que Wireshark) vous pouvez déterminer les SPN, le navigateur tente de résoudre et puis à l’aide de la ligne de commande de l’outil, setspn - Q <spn>, vous pouvez effectuer une recherche sur ce SPN.  Il ne peut pas être trouvé, ou il peut être attribué à un autre compte autre que le compte de service AD FS.

![Intégré](media/ad-fs-tshoot-iwa/iwa3.png)

Vous pouvez vérifier le SPN en examinant les propriétés du compte de service AD FS.

![Intégré](media/ad-fs-tshoot-iwa/iwa1.png)

## <a name="channel-binding-token"></a>Jeton de liaison de canal
Actuellement, lorsqu’une application cliente s’authentifie sur le serveur à l’aide de Kerberos, Digest ou NTLM à l’aide de HTTPS, un canal de sécurité de niveau Transport (TLS) est établi au préalable et l’authentification s’effectue à l’aide de ce canal. 

Le jeton de liaison de canal est une propriété du canal externe sécurisé TLS et est utilisé pour lier le canal externe à une conversation sur le canal interne authentifié par le client.

S’il existe une attaque de « man-in-the-middle » qui se produisent et ils sont de-chiffrement et rechiffre le trafic SSL, puis la clé ne correspondront pas.  AD FS détermine qu’il y a quelque chose assis à mi-chemin entre l’élément de navigation web r et elle-même.  Cela entraîne l’échec de l’authentification Kerberos et l’utilisateur sera invité avec une boîte de dialogue 401 au lieu d’une expérience d’authentification unique.

Cela peut être causé par :
 - quoi que ce soit réside entre le navigateur et les services AD FS
 - Fiddler
 - Proxys inverses effectuant le pontage SSL

Par défaut, AD FS a ce paramètre défini sur « Autoriser ».  Vous pouvez modifier ce paramètre à l’aide de l’applet de commande PowerShell `Set-ADFSProperties -ExtendProtectionTokenCheck`

Pour plus d’informations à ce sujet, consultez [meilleures pratiques pour sécuriser la planification et déploiement d’AD FS](../../ad-fs/design/best-practices-for-secure-planning-and-deployment-of-ad-fs.md).

## <a name="internet-explorer-configuration"></a>Configuration d’Internet Explorer
Par défaut, Internet explorer ne peut pas avoir la façon suivante :

1. L’Explorateur Internet reçoit une réponse 401 à partir d’AD FS avec le mot NEGOTIATE dans l’en-tête.
2. Cela indique le navigateur web pour obtenir un ticket Kerberos ou NTLM pour renvoyer à AD FS.
3. Par défaut Internet Explorer tente de faire cette SPNEGO () sans intervention de l’utilisateur si le mot NEGOTIATE est dans l’en-tête.  Il fonctionne uniquement pour les sites intranet.

Il existe 2 principaux éléments qui peuvent empêcher cet happeing.
   - Activer que l’authentification Windows intégrée n’est pas activée dans les propriétés d’Internet Explorer.  Cela situé sous Options Internet -> Avancé -> sécurité.
   
   ![Intégré](media/ad-fs-tshoot-iwa/iwa4.png)
   
   - Zones de sécurité ne sont pas configurés correctement
       - Noms de domaine complets ne sont pas dans la zone intranet
       - URL AD FS n’est pas dans la zone intranet.

      ![Intégré](media/ad-fs-tshoot-iwa/iwa5.png)
## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes AD FS](ad-fs-tshoot-overview.md)
