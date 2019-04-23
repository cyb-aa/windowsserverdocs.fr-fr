---
title: TLS (Schannel SSP)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 05/16/2018
ms.openlocfilehash: 030fd81e0c6ba0423f1fa73e680006766cf2b180
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890770"
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>Modifications TLS (Schannel SSP) dans Windows 10 et Windows Server 2016

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016 et Windows 10

## <a name="cipher-suite-changes"></a>Changements de Suite de chiffrement

Windows 10, version 1511 et Windows Server 2016 ajoutent la prise en charge pour la configuration de l’ordre des suites de chiffrement à l’aide de la gestion des appareils mobiles (MDM).

Pour les changements d’ordre de priorité de suite de chiffrement, consultez [Suites de chiffrement dans Schannel](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx).

Prise en charge pour les suites de chiffrement suivantes :

- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (RFC 5289) dans Windows 10, version 1507 et Windows Server 2016
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (RFC 5289) dans Windows 10, version 1507 et Windows Server 2016

DisabledByDefault modifier des suites de chiffrement suivantes :

- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256 (RFC 5246) dans Windows 10, version 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256 (RFC 5246) dans Windows 10, version 1703
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA (RFC 5246) dans Windows 10, version 1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA (RFC 5246) dans Windows 10, version 1703
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA (RFC 5246) dans Windows 10, version 1703
- TLS_RSA_WITH_RC4_128_SHA dans Windows 10, version 1709
- TLS_RSA_WITH_RC4_128_MD5 dans Windows 10, version 1709

À partir de Windows 10, version 1507 et Windows Server 2016, les certificats SHA 512 sont pris en charge par défaut.

### <a name="rsa-key-changes"></a>Changements de clés RSA

Ajouter des options de configuration de Registre pour les tailles de clé RSA client Windows 10, version 1507 et Windows Server 2016.

Pour plus d’informations, consultez [KeyExchangeAlgorithm - tailles de clé RSA de Client](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes).

### <a name="diffie-hellman-key-changes"></a>Changements de clés Diffie-Hellman

Windows 10, version 1507 et Windows Server 2016 ajouter des options de configuration de Registre pour les tailles de clé Diffie-Hellman.

Pour plus d’informations, consultez [KeyExchangeAlgorithm - tailles de clé Diffie-Hellman](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes).

### <a name="schusestrongcrypto-option-changes"></a>Modifications de l’option SCH_USE_STRONG_CRYPTO

Avec Windows 10 version 1507 et Windows Server 2016, [SCH_USE_STRONG_CRYPTO](https://msdn.microsoft.com/library/windows/desktop/aa379810.aspx) option maintenant désactive NULL, MD5, DES et exporter des chiffrements.

## <a name="elliptical-curve-changes"></a>Modifications de courbe elliptiques

Windows 10, version 1507 et Windows Server 2016 ajoutent la configuration de stratégie de groupe pour les courbes elliptiques sous Configuration ordinateur > modèles d’administration > réseau > Paramètres de Configuration de SSL. La liste des commandes de courbe ECC Spécifie l’ordre dans lequel les courbes elliptiques sont préférables ainsi permet des courbes pris en charge qui ne sont pas activés. 
 
Prise en charge pour les courbes elliptiques suivantes :

- BrainpoolP256r1 (RFC 7027) dans Windows 10, version 1507 et Windows Server 2016
- BrainpoolP384r1 (RFC 7027) dans Windows 10, version 1507 et Windows Server 2016 
- BrainpoolP512r1 (RFC 7027) dans Windows 10, version 1507 et Windows Server 2016
- Curve25519 (RFC draft-ietf-tls-curve25519) dans Windows 10, version 1607 et Windows Server 2016

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>Prise en charge au niveau de répartition de SealMessage & UnsealMessage

Windows 10, version 1507 et Windows Server 2016 ajouter la prise en charge de SealMessage/UnsealMessage au niveau de répartition.

## <a name="dtls-12"></a>DTLS 1.2

Windows 10, version 1607 et Windows Server 2016 ajoutent la prise en charge pour le protocole DTLS 1.2 (RFC 6347).

## <a name="httpsys-thread-pool"></a>HTTP. Pool de threads SYS 

Windows 10, version 1607 et Windows Server 2016 ajoutent la configuration du Registre de la taille du pool de threads utilisé pour gérer les négociations TLS pour HTTP. SYS.

Chemin d’accès du Registre : 

HKLM\SYSTEM\CurrentControlSet\Control\LSA

Pour spécifier une taille de pool maximale thread par cœur de processeur, créez un **MaxAsyncWorkerThreadsPerCpu** entrée. Par défaut, cette entrée n’existe pas dans le Registre. Une fois que vous avez créé l’entrée, remplacez la valeur DWORD à la taille souhaitée. Si non configuré, le maximum est 2 threads par cœur de processeur.

## <a name="next-protocol-negotiation-npn-support"></a>Prise en charge de la négociation de protocole (NPN) suivant

À compter de Windows 10 version 1703, la négociation de protocole suivant (NPN) a été supprimé et n’est plus pris en charge.

## <a name="pre-shared-key-psk"></a>Clé prépartagée (PSK)

Windows 10, version 1607 et Windows Server 2016 ajoutent la prise en charge de l’algorithme d’échange de clés PSK (RFC 4279).

Prise en charge pour les suites de chiffrement PSK suivantes :

- TLS_PSK_WITH_AES_128_CBC_SHA256 (RFC 5487) dans Windows 10, version 1607 et Windows Server 2016
- TLS_PSK_WITH_AES_256_CBC_SHA384(RFC 5487) dans Windows 10, version 1607 et Windows Server 2016
- TLS_PSK_WITH_NULL_SHA256 (RFC 5487) dans Windows 10, version 1607 et Windows Server 2016
- TLS_PSK_WITH_NULL_SHA384 (RFC 5487) dans Windows 10, version 1607 et Windows Server 2016
- TLS_PSK_WITH_AES_128_GCM_SHA256 (RFC 5487) dans Windows 10, version 1607 et Windows Server 2016
- TLS_PSK_WITH_AES_256_GCM_SHA384 (RFC 5487) dans Windows 10, version 1607 et Windows Server 2016

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>Reprise de session sans les améliorations de performances côté serveur d’état côté serveur

Windows 10, version 1507 et Windows Server 2016 fournissent la reprise de session plus de 30 % par seconde avec des tickets de session par rapport à Windows Server 2012.

## <a name="session-hash-and-extended-master-secret-extension"></a>Hachage de la session et étendue Extension de Secret principal

Windows 10, version 1507 et Windows Server 2016 ajoutent la prise en charge pour RFC 7627 : Transport Layer Security (TLS) Session hachage et étendus d’Extension de Secret principal.

En raison de cette modification, Windows 10 et Windows Server 2016 nécessite 3ème partie [fournisseur CNG SSL](https://msdn.microsoft.com/library/windows/desktop/ff468652.aspx) mises à jour pour prendre en charge NCRYPT_SSL_INTERFACE_VERSION_3 et pour décrire cette nouvelle interface.


## <a name="ssl-support"></a>Prise en charge SSL

Depuis Windows 10, version 1607 et Windows Server 2016, le client TLS et le serveur SSL 3.0 est désactivé par défaut. Cela signifie que, sauf si l’application ou le service requiert SSL 3.0 via l’interface SSPI, le client ne sera jamais offrent ou accepter SSL 3.0 et le serveur sélectionne jamais SSL 3.0.

Depuis Windows 10 version 1607 et Windows Server 2016, SSL 2.0 a été supprimé et n’est plus pris en charge.

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>Modifications apportées au respect des exigences de TLS 1.2 pour les connexions avec les clients non conformes TLS TLS de Windows

Dans TLS 1.2, le client utilise le [« signature_algorithms » extension](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1) pour indiquer au serveur les paires d’algorithme de signature/hachage peuvent être utilisés dans les signatures numériques (par exemple, des certificats de serveur et échange de clés de serveur). Le protocole TLS 1.2 RFC requiert également que le message de certificat de serveur respecte les extension « signature_algorithms » :

« Si le client fourni une extension « signature_algorithms », puis tous les certificats fournis par le serveur doivent être signées par une paire d’algorithme de hachage/signatures qui apparaît dans cette extension. »

Dans la pratique, certains clients TLS par des tiers ne pas respecter les TLS 1.2 RFC et l’échec à inclure la signature de toutes les paires qu’ils sont prêts à accepter dans l’extension « signature_algorithms » de l’algorithme de hachage ou omettre complètement de l’extension (ce dernier indique à le serveur que le client prend uniquement en charge SHA1 avec RSA, DSA ou ECDSA).

Un serveur TLS a souvent qu’un seul certificat configuré pour chaque point de terminaison, ce qui signifie que le serveur ne peut pas toujours fournir un certificat qui répond aux exigences du client.

Avant Windows 10 et Windows Server 2016, la pile Windows TLS conforme strictement au TLS 1.2 exigences de RFC, ce qui entraîne des échecs de connexion avec les clients TLS RFC non conformes et les problèmes d’interopérabilité. Dans Windows 10 et Windows Server 2016, les contraintes sont assouplies et le serveur peut envoyer un certificat qui ne sont pas conformes avec TLS 1.2 RFC, si c’est la seule option du serveur. Le client peut ensuite continuer ou arrêter le protocole de transfert.

Lors de la validation des certificats serveur et client, la pile Windows TLS strictement conforme à la demande de changement TLS 1.2 et autorise uniquement la signature négociée et les algorithmes de hachage dans les certificats de serveur et client.


