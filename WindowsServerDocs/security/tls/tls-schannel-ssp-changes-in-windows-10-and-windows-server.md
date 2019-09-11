---
title: TLS (SSP Schannel)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 05/16/2018
ms.openlocfilehash: a8928d89c42d4462dbcabc756adcfdc5678c7a9d
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870231"
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>Modifications du protocole TLS (SSP Schannel) dans Windows 10 et Windows Server 2016

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016 et Windows 10

## <a name="cipher-suite-changes"></a>Modifications de la suite de chiffrement

Windows 10, version 1511 et Windows Server 2016 prennent en charge la configuration de l’ordre de la suite de chiffrement à l’aide de la gestion des appareils mobiles (MDM).

Pour connaître les modifications de l’ordre de priorité des suites de chiffrement, consultez [suites de chiffrement dans Schannel](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx).

Ajout de la prise en charge des suites de chiffrement suivantes :

- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (RFC 5289) dans Windows 10, version 1507 et Windows Server 2016
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (RFC 5289) dans Windows 10, version 1507 et Windows Server 2016

DisabledByDefault modification pour les suites de chiffrement suivantes :

- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256 (RFC 5246) dans Windows 10, version 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256 (RFC 5246) dans Windows 10, version 1703
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA (RFC 5246) dans Windows 10, version 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA (RFC 5246) dans Windows 10, version 1703
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA (RFC 5246) dans Windows 10, version 1703
- TLS_RSA_WITH_RC4_128_SHA dans Windows 10, version 1709
- TLS_RSA_WITH_RC4_128_MD5 dans Windows 10, version 1709

À compter de Windows 10, version 1507 et Windows Server 2016, les certificats SHA 512 sont pris en charge par défaut.

### <a name="rsa-key-changes"></a>Modifications de la clé RSA

Windows 10, version 1507 et Windows Server 2016 ajoutent des options de configuration du Registre pour les tailles de clé RSA client.

Pour plus d’informations, consultez [tailles de clé RSA KeyExchangeAlgorithm-client](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes).

### <a name="diffie-hellman-key-changes"></a>Modifications de la clé Diffie-Hellman

Windows 10, version 1507 et Windows Server 2016 ajoutent des options de configuration du Registre pour les tailles de clé Diffie-Hellman.

Pour plus d’informations, consultez [tailles de clé KeyExchangeAlgorithm-Diffie-Hellman](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes).

### <a name="sch_use_strong_crypto-option-changes"></a>Modifications de l’option SCH_USE_STRONG_CRYPTO

Avec Windows 10, version 1507 et Windows Server 2016, l’option [SCH_USE_STRONG_CRYPTO](https://msdn.microsoft.com/library/windows/desktop/aa379810.aspx) désactive désormais les chiffrements null, MD5, des et d’exportation.

## <a name="elliptical-curve-changes"></a>Modifications de la courbe elliptique

Windows 10, version 1507 et Windows Server 2016 ajoutent stratégie de groupe configuration pour les courbes elliptiques sous Configuration ordinateur > Modèles d’administration > réseau > paramètres de configuration SSL. La liste d’ordre des courbes ECC spécifie l’ordre dans lequel les courbes elliptiques sont préférées et active les courbes prises en charge qui ne sont pas activées. 
 
Ajout de la prise en charge des courbes elliptiques suivantes :

- BrainpoolP256r1 (RFC 7027) dans Windows 10, version 1507 et Windows Server 2016
- BrainpoolP384r1 (RFC 7027) dans Windows 10, version 1507 et Windows Server 2016 
- BrainpoolP512r1 (RFC 7027) dans Windows 10, version 1507 et Windows Server 2016
- Curve25519 (RFC draft-ietf-TLS-Curve25519) dans Windows 10, version 1607 et Windows Server 2016

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>Prise en charge du niveau de répartition pour SealMessage & UnsealMessage

Windows 10, version 1507 et Windows Server 2016 prennent en charge SealMessage/UnsealMessage au niveau de la distribution.

## <a name="dtls-12"></a>DTLS 1,2

Windows 10, version 1607 et Windows Server 2016 prennent en charge DTLS 1,2 (RFC 6347).

## <a name="httpsys-thread-pool"></a>Protocoles. Pool de threads SYS 

Windows 10, version 1607 et Windows Server 2016 ajoutent la configuration du registre de la taille du pool de threads utilisé pour gérer les négociations TLS pour HTTP. Table.

Chemin du Registre : 

HKLM\SYSTEM\CurrentControlSet\Control\LSA

Pour spécifier une taille maximale de pool de threads par cœur de processeur, créez une entrée **MaxAsyncWorkerThreadsPerCpu** . Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD par la taille souhaitée. S’il n’est pas configuré, la valeur maximale est 2 threads par cœur de processeur.

## <a name="next-protocol-negotiation-npn-support"></a>Prise en charge de la négociation de protocole suivante (NPN)

À compter de Windows 10 version 1703, la négociation de protocole suivante (NPN) a été supprimée et n’est plus prise en charge.

## <a name="pre-shared-key-psk"></a>Clé prépartagée (PSK)

Windows 10, version 1607 et Windows Server 2016 ajoutent la prise en charge de l’algorithme d’échange de clés PSK (RFC 4279).

Ajout de la prise en charge des suites de chiffrement PSK suivantes :

- TLS_PSK_WITH_AES_128_CBC_SHA256 (RFC 5487) dans Windows 10, version 1607 et Windows Server 2016
- TLS_PSK_WITH_AES_256_CBC_SHA384 (RFC 5487) dans Windows 10, version 1607 et Windows Server 2016
- TLS_PSK_WITH_NULL_SHA256 (RFC 5487) dans Windows 10, version 1607 et Windows Server 2016
- TLS_PSK_WITH_NULL_SHA384 (RFC 5487) dans Windows 10, version 1607 et Windows Server 2016
- TLS_PSK_WITH_AES_128_GCM_SHA256 (RFC 5487) dans Windows 10, version 1607 et Windows Server 2016
- TLS_PSK_WITH_AES_256_GCM_SHA384 (RFC 5487) dans Windows 10, version 1607 et Windows Server 2016

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>Reprise de session sans amélioration des performances côté serveur de l’État côté serveur

Windows 10, version 1507 et Windows Server 2016 offrent 30% de reprises de session supplémentaires par seconde avec des tickets de session par rapport à Windows Server 2012.

## <a name="session-hash-and-extended-master-secret-extension"></a>Hachage de session et extension de secret principal étendue

Windows 10, version 1507 et Windows Server 2016 ajoutent la prise en charge de RFC 7627 : Hachage de session TLS (Transport Layer Security) et extension de secret principal étendue.

En raison de cette modification, Windows 10 et Windows Server 2016 nécessitent des mises à jour tierces du [fournisseur de chiffrement CNG](https://msdn.microsoft.com/library/windows/desktop/ff468652.aspx) pour prendre en charge NCRYPT_SSL_INTERFACE_VERSION_3 et décrire cette nouvelle interface.


## <a name="ssl-support"></a>Prise en charge SSL

À partir de Windows 10, version 1607 et Windows Server 2016, le client TLS et le serveur SSL 3,0 sont désactivés par défaut. Cela signifie que, à moins que l’application ou le service ne demande spécifiquement SSL 3,0 via l’interface SSPI, le client n’offrira jamais ou n’acceptera pas SSL 3,0 et le serveur ne sélectionnera jamais SSL 3,0.

À compter de Windows 10 version 1607 et de Windows Server 2016, SSL 2,0 a été supprimé et n’est plus pris en charge.

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>Modifications apportées à l’adhérence Windows TLS à la configuration TLS 1,2 pour les connexions avec des clients TLS non conformes

Dans TLS 1,2, le client utilise l' [extension « signature_algorithms »](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1) pour indiquer au serveur les paires d’algorithmes de signature/hachage qui peuvent être utilisées dans les signatures numériques (c.-à-d., les certificats de serveur et l’échange de clés serveur). Le RFC 1,2 de TLS requiert également que le message de certificat de serveur respecte l’extension « signature_algorithms » :

« Si le client a fourni une extension «signature_algorithms », tous les certificats fournis par le serveur doivent être signés par une paire algorithme de hachage/signature qui apparaît dans cette extension.»

En pratique, certains clients TLS tiers ne sont pas conformes à la norme TLS 1,2 et ne parviennent pas à inclure toutes les paires d’algorithmes de signature et d’algorithme de hachage qu’ils sont prêtes à accepter dans l’extension « signature_algorithms », ou à omettre complètement l’extension (cette dernière indique à serveur sur lequel le client ne prend en charge que SHA1 avec RSA, DSA ou ECDSA).

Un serveur TLS n’a souvent qu’un seul certificat configuré par point de terminaison, ce qui signifie que le serveur ne peut pas toujours fournir un certificat qui répond aux exigences du client.

Avant Windows 10 et Windows Server 2016, la pile TLS Windows était strictement conforme aux exigences de la RFC TLS 1,2, provoquant des échecs de connexion avec des clients TLS non conformes et des problèmes d’interopérabilité. Dans Windows 10 et Windows Server 2016, les contraintes sont assouplies et le serveur peut envoyer un certificat qui n’est pas conforme à la norme TLS 1,2 RFC, s’il s’agit de l’option de serveur uniquement. Le client peut ensuite continuer ou terminer le protocole de transfert.

Lors de la validation des certificats serveur et client, la pile TLS Windows est strictement conforme à la RFC 1,2 TLS et autorise uniquement la signature négociée et les algorithmes de hachage dans les certificats du serveur et du client.


