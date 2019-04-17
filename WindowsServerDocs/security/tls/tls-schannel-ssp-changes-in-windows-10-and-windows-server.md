---
title: TLS (SSP Schannel)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: ebd3c40c-b4c0-4f6d-a00c-f90eda4691df
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 6e1a229bfc7063597f8994c71bbaec5637d084a4
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="tls-schannel-ssp-changes-in-windows-10-and-windows-server-2016"></a>Modifications TLS (SSP Schannel) dans Windows10 et Windows Server2016

>S’applique à: Windows Server2016 et Windows10

## <a name="cipher-suite-changes"></a>Modifications de Suite de chiffrement

Windows10, version1511 et Windows Server2016 ajoutent la prise en charge pour la configuration de l’ordre des suites de chiffrement à l’aide de la gestion des périphériques mobiles (GPM).

Pour les modifications d’ordre de priorité de suite de chiffrement, voir [Suites de chiffrement dans Schannel](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx).

Prise en charge supplémentaire pour les suites de chiffrement suivants:

- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (RFC5289) dans Windows10, version1507 et Windows Server2016
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (RFC5289) dans Windows10, version1507 et Windows Server2016

DisabledByDefault modifier pour les suites de chiffrement suivants:

- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256 (RFC5246) dans Windows10, version1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256 (RFC5246) dans Windows10, version1703
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA (RFC5246) dans Windows10, version1703
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA (RFC5246) dans Windows10, version1703
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA (RFC5246) dans Windows10, version1703
- TLS_RSA_WITH_RC4_128_SHA dans Windows10, version1709
- TLS_RSA_WITH_RC4_128_MD5 dans Windows10, version1709

À partir de Windows10, version1507 et Windows Server2016, SHA 512certificats sont pris en charge par défaut.

### <a name="rsa-key-changes"></a>Modifications des clés RSA

Ajoutent des options de configuration du Registre pour des tailles de clé RSA client Windows10, version1507 et Windows Server2016.

Pour plus d’informations, voir [KeyExchangeAlgorithm - tailles de clé RSA Client](tls-registry-settings.md#keyexchangealgorithm---client-rsa-key-sizes).

### <a name="diffie-hellman-key-changes"></a>Modifications des clés de Diffie-Hellman

Ajoutent des options de configuration du Registre pour des tailles de clé Diffie-Hellman Windows10, version1507 et Windows Server2016.

Pour plus d’informations, voir [KeyExchangeAlgorithm - tailles de clé Diffie-Hellman](tls-registry-settings.md#keyexchangealgorithm---diffie-hellman-key-sizes).

### <a name="schusestrongcrypto-option-changes"></a>Modifications de l’option SCH_USE_STRONG_CRYPTO

Avec Windows10, version1507 et Windows Server2016, [SCH_USE_STRONG_CRYPTO](https://msdn.microsoft.com/library/windows/desktop/aa379810.aspx) option maintenant désactive NULL, MD5, DES et exporter des chiffrements.

## <a name="elliptical-curve-changes"></a>Modifications de courbe elliptiques

Windows10, version1507 et Windows Server2016 ajoutent la configuration de stratégie de groupe pour des courbes elliptiques sous Configuration ordinateur > modèles d’administration > réseau > paramètres de Configuration de SSL. La liste de commande de courbe ECC Spécifie l’ordre dans lequel des courbes elliptiques sont privilégiés ainsi permet des courbes pris en charge qui ne sont pas activées. 
 
Ajout de la prise en charge des courbes elliptiques suivantes:

- BrainpoolP256r1 (RFC7027) dans Windows10, version1507 et Windows Server2016
- BrainpoolP384r1 (RFC7027) dans Windows10, version1507 et Windows Server2016 
- BrainpoolP512r1 (RFC7027) dans Windows10, version1507 et Windows Server2016
- Curve25519 (RFC brouillon-ietf-tls-curve25519) dans Windows10, version1607 et Windows Server2016

## <a name="dispatch-level-support-for-sealmessage--unsealmessage"></a>Prise en charge au niveau de répartition de SealMessage et UnsealMessage

Windows10, version1507 et Windows Server2016 ajoutent la prise en charge de SealMessage/UnsealMessage au niveau de la répartition.

## <a name="dtls-12"></a>DTLS 1.2

Windows10, version1607 et Windows Server2016 ajoutent la prise en charge de DTLS 1.2 (RFC6347).

## <a name="httpsys-thread-pool"></a>HTTP.SYS 

Windows10, version1607 et Windows Server2016 ajoutent la configuration du Registre de la taille du pool de threads utilisé pour gérer les négociations TLS pour HTTP.SYS.

Chemin d’accès du Registre: 

HKLM\System\CurrentControlSet\Control\LSA

Pour spécifier une taille de pool maximal de threads par cœur de processeur, créez un **MaxAsyncWorkerThreadsPerCpu** entrée. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD à la taille souhaitée. Si non configuré, la valeur maximale est 2threads par cœur de processeur.

## <a name="next-protocol-negotiation-npn-support"></a>Prise en charge de la négociation de protocole (NPN) suivant

À compter de Windows10 version1703, la négociation de protocole suivant (NPN) a été supprimé et n’est plus pris en charge.

## <a name="pre-shared-key-psk"></a>Clé partagée (PSK)

Windows10, version1607 et Windows Server2016 ajoutent la prise en charge de l’algorithme d’échange de clé PSK (RFC4279).

Prise en charge supplémentaire pour les suites de chiffrement PSK suivantes:

- TLS_PSK_WITH_AES_128_CBC_SHA256 (RFC5487) dans Windows10, version1607 et Windows Server2016
- TLS_PSK_WITH_AES_256_CBC_SHA384(RFC5487) dans Windows10, version1607 et Windows Server2016
- TLS_PSK_WITH_NULL_SHA256 (RFC5487) dans Windows10, version1607 et Windows Server2016
- TLS_PSK_WITH_NULL_SHA384 (RFC5487) dans Windows10, version1607 et Windows Server2016
- TLS_PSK_WITH_AES_128_GCM_SHA256 (RFC5487) dans Windows10, version1607 et Windows Server2016
- TLS_PSK_WITH_AES_256_GCM_SHA384 (RFC5487) dans Windows10, version1607 et Windows Server2016

## <a name="session-resumption-without-server-side-state-server-side-performance-improvements"></a>Reprise de session sans les améliorations de performances côté serveur état côté serveur

Windows10, version1507 et Windows Server2016 fournissent resumptions de session de plus de 30% par seconde des tickets de session par rapport à Windows Server2012.

## <a name="session-hash-and-extended-master-secret-extension"></a>Hachage de la session et l’Extension de Secret maître étendu

Windows10, version1507 et Windows Server2016 ajouter la prise en charge de RFC7627: hachage de Session de sécurité TLS (Transport Layer) et étendu maître Secret Extension.

En raison de cette modification, Windows10 et Windows Server2016 nécessite 3ème partie [fournisseur CNG SSL](https://msdn.microsoft.com/library/windows/desktop/ff468652.aspx) mises à jour pour prendre en charge NCRYPT_SSL_INTERFACE_VERSION_3 et pour décrire cette nouvelle interface.


## <a name="ssl-support"></a>Prise en charge SSL

À partir de Windows10, version1607 et Windows Server2016, le client TLS et SSL 3.0 est désactivé par défaut. Cela signifie que, sauf si l’application ou le service requiert SSL 3.0 via l’interface SSPI, le client ne sera jamais proposer ou accepter SSL 3.0 et le serveur ne sera jamais sélectionnez SSL 3.0.

À partir de Windows10 version1607 et Windows Server2016, SSL 2.0 a été supprimé et n’est plus pris en charge.

## <a name="changes-to-windows-tls-adherence-to-tls-12-requirements-for-connections-with-non-compliant-tls-clients"></a>Modifications apportées au respect de TLS Windows de TLS 1.2 Configuration requise pour les connexions avec les clients non conformes TLS

Dans le protocole TLS 1.2, le client utilise le [extension «signature_algorithms»](https://tools.ietf.org/html/rfc5246#section-7.4.1.4.1) pour indiquer au serveur les paires d’algorithme de signature/hachage peuvent être utilisées dans les signatures numériques (par exemple, les certificats de serveur et échange de clés de serveur). Le protocole TLS 1.2 RFC requiert également que le message de certificat de serveur respecte extension «signature_algorithms»:

«Si le client a fourni une extension «signature_algorithms», puis tous les certificats fournis par le serveur doivent être signés par une paire d’algorithme de hachage/signature qui s’affiche dans cette extension.»

Dans la pratique, certains clients TLS tiers ne pas répondent avec le protocole TLS 1.2 RFC et à inclure la signature de toutes les paires qu’ils sont prêts à accepter dans l’extension «signature_algorithms» de l’algorithme de hachage ou omettre l’extension complètement (ce dernier indique au serveur que le client prend uniquement en charge SHA1 avec RSA, DSA et ECDSA).

Un serveur TLS a souvent qu’un certificat configuré par le point de terminaison, ce qui signifie que le serveur ne peut pas toujours fournir un certificat qui répond aux exigences du client.

Avant Windows10 et Windows Server2016, la pile Windows TLS strictement respectée pour le protocole TLS 1.2exigences RFC, ce qui entraîne des échecs de connexion avec les clients TLS RFC non conformes et les problèmes d’interopérabilité. Dans Windows10 et Windows Server2016, les contraintes sont souple ou le serveur peut envoyer un certificat qui n’est pas compatible avec le protocole TLS 1.2 RFC, si c’est la seule option disponible du serveur. Le client peut ensuite continuer ou mettre fin à la négociation.

Lors de la validation des certificats serveur et client, la pile Windows TLS strictement conforme avec le protocole TLS 1.2 RFC et autorise uniquement la signature négociée et les algorithmes de hachage dans les certificats de serveur et client.


