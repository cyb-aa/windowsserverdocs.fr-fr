---
title: "Paramètres de Registre Layer Security (TLS) de transport"
description: "Sécurité de Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 08/07/2017
ms.openlocfilehash: 8ccfacc367a5d32438bebf22798479a07f6cbdfc
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="transport-layer-security-tls-registry-settings"></a>Paramètres de Registre Layer Security (TLS) de transport

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016, Windows Server2008R2, Windows Server2008, Windows10, Windows8.1, Windows8, Windows7, WindowsVista

Cette rubrique de référence destinée aux professionnels de l’informatique contient des informations de paramètre de Registre pris en charge pour l’implémentation Windows du protocole sécurité TLS (Transport Layer) et le protocole Secure Sockets Layer (SSL) par le biais du fournisseur SSP (Security Support Provider) Schannel. Les sous-clés de Registre et les entrées abordées dans cette rubrique vous aident à gérer et dépanner le SSP Schannel, en particulier les protocoles TLS et SSL. 

>[!Caution]
>Ces informations sont fournies en tant que référence à utiliser lors de la résolution de problèmes ou de vérifier que les paramètres requis sont appliquées. Nous recommandons que vous ne modifiez pas directement le Registre à moins qu’aucune autre solution.
>Modifications du Registre ne sont pas validées par l’Éditeur du Registre ou par le système d’exploitation Windows avant de les appliquer. Par conséquent, des valeurs incorrectes peuvent être stockées et cela peut entraîner des erreurs irrécupérables dans le système. Lorsque cela est possible, plutôt que de modifier le Registre directement, utilisez stratégie de groupe ou d’autres outils Windows tels que la Console MMC (MicrosoftManagement Console) pour accomplir des tâches. Si vous devez modifier le Registre, soyez très vigilant. 

## <a name="certificatemappingmethods"></a>CertificateMappingMethods 

Par défaut, cette entrée n’existe pas dans le Registre. La valeur par défaut est que toutes les méthodes de mappage de certificat quatre, répertoriés ci-dessous, sont pris en charge.

Lorsqu’une application serveur requiert l’authentification du client, Schannel tente automatiquement de mapper le certificat fourni par l’ordinateur client à un compte d’utilisateur. Vous pouvez authentifier les utilisateurs qui ouvrent une session avec un certificat client en créant des mappages qui lient les informations de certificat à un compte d’utilisateur Windows. Après avoir créé et activer un mappage de certificat, chaque fois qu’un client présente un certificat client, votre application serveur associe automatiquement cet utilisateur au compte d’utilisateur Windows approprié.

Dans la plupart des cas, un certificat est mappé à un compte d’utilisateur de deux manières: 

- Un seul certificat est mappé à un seul compte d’utilisateur (mappage).
- Plusieurs certificats sont mappés à un compte d’utilisateur (mappage plusieurs-à-un).

Par défaut, le fournisseur Schannel utilise les méthodes de quatre mappage de certificat suivantes, répertoriées par ordre de préférence:

1. Mappage de certificats Kerberos service-for-user (S4U)
2. Mappage du nom principal utilisateur
3. Mappage (également appelé mappage objet/émetteur)
4. Mappage plusieurs-à-un

Versions applicables: comme indiqué dans le **s’applique à** liste qui se trouve au début de cette rubrique.

Chemin d’accès du Registre: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="ciphers"></a>Chiffrements

Chiffrement TLS/SSL doit être contrôlés par la configuration de l’ordre des suites de chiffrement. Pour plus d’informations, voir [ordre des suites de chiffrement TLS configuration](manage-tls.md#configuring-tls-cipher-suite-order).

Pour plus d’informations sur l’ordre des suites de chiffrement par défaut qui sont utilisés par le SSP Schannel, consultez [Suites de chiffrement de TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 

##<a name="ciphersuites"></a>Suites de chiffrement

La configuration des suites de chiffrement TLS/SSL doit être effectuée à l’aide de la stratégie de groupe, GPM ou PowerShell, consultez [ordre des suites de chiffrement TLS configuration](manage-tls.md#configuring-tls-cipher-suite-order) pour plus d’informations.

Pour plus d’informations sur l’ordre des suites de chiffrement par défaut qui sont utilisés par le SSP Schannel, consultez [Suites de chiffrement de TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 


## <a name="clientcachetime"></a>ClientCacheTime

Cette entrée contrôle la quantité de temps utilisé par le système d’exploitation en millisecondes le point d’expirer les entrées de cache côté client. La valeur 0 désactive la mise en cache d’une connexion sécurisée. Par défaut, cette entrée n’existe pas dans le Registre. 

La première fois qu’un client se connecte à un serveur via le SSP Schannel, complète de la négociation TLS/SSL est effectuée. Lorsque cette opération terminée, le secret principal, suite de chiffrement et les certificats sont stockés dans le cache de session sur le serveur et client respectif.

À partir de Windows Server2008 et WindowsVista, la durée du cache du client par défaut est de 10heures.

Chemin d’accès du Registre: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Durée de cache du client par défaut

## <a name="fipsalgorithmpolicy"></a>FIPSAlgorithmPolicy

Cette entrée contrôle la conformité de traitement FIPS (Federal Information). La valeur par défaut est 0.

Versions applicables: comme indiqué dans le **s’applique à** liste qui se trouve au début de cette rubrique. 

Chemin d’accès du Registre: HKLM SYSTEM\CurrentControlSet\Control\LSA

Suites de chiffrement WindowsServerFIPS: voir [Suites de chiffrement pris en charge et des protocoles dans le SSP Schannel](https://technet.microsoft.com/library/dn786419.aspx).

## <a name="hashes"></a>Hachages

Les algorithmes de hachage TLS/SSL doivent être contrôlés par la configuration de l’ordre des suites de chiffrement. Voir [ordre des suites de chiffrement TLS configuration](manage-tls.md#configuring-tls-cipher-suite-order) pour plus d’informations.

## <a name="issuercachesize"></a>IssuerCacheSize

Cette entrée contrôle la taille du cache de l’émetteur, et il est utilisé avec le mappage de l’émetteur. Le SSP Schannel tente de mapper tous les émetteurs de chaîne de certificats du client, pas seulement l’émetteur direct du certificat client. Lorsque les émetteurs ne correspondent pas à un compte, qui est le cas par défaut, le serveur peut tenter de mapper le même nom d’émetteur à plusieurs reprises, des centaines de fois par seconde. 

Pour éviter cela, le serveur a un cache négatif, donc si un nom d’émetteur ne correspond pas à un compte, il est ajouté au cache et le SSP Schannel ne tente pas de mapper le nom d’émetteur à nouveau tant que l’entrée de cache expire. Cette entrée de Registre spécifie la taille du cache. Par défaut, cette entrée n’existe pas dans le Registre. La valeur par défaut est 100. 

Versions applicables: comme indiqué dans le **s’applique à** liste qui se trouve au début de cette rubrique.

Chemin d’accès du Registre: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="issuercachetime"></a>IssuerCacheTime

Cette entrée contrôle la durée de l’intervalle de délai d’expiration du cache en millisecondes. Le SSP Schannel tente de mapper tous les émetteurs de chaîne de certificats du client, pas seulement l’émetteur direct du certificat client. Dans le cas où les émetteurs ne correspondent pas à un compte, qui est le cas par défaut, le serveur peut tenter de mapper le même nom d’émetteur à plusieurs reprises, des centaines de fois par seconde.

Pour éviter cela, le serveur a un cache négatif, donc si un nom d’émetteur ne correspond pas à un compte, il est ajouté au cache et le SSP Schannel ne tente pas de mapper le nom d’émetteur à nouveau tant que l’entrée de cache expire. Ce cache est conservé pour des raisons de performances, afin que le système ne continue pas tente de mapper les mêmes émetteurs. Par défaut, cette entrée n’existe pas dans le Registre. La valeur par défaut est 10minutes.

Versions applicables: comme indiqué dans le **s’applique à** liste qui se trouve au début de cette rubrique.

Chemin d’accès du Registre: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="keyexchangealgorithm---client-rsa-key-sizes"></a>KeyExchangeAlgorithm - taille de la clé RSA de Client

Cette entrée contrôle la taille de clé RSA client. 

Utilisation des algorithmes d’échange de clés doit être contrôlée par la configuration de l’ordre des suites de chiffrement.

Ajouté dans Windows10, version1507 et Windows Server2016.

Chemin d’accès du Registre: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\PKCS

Pour spécifier une longueur de clé bits minimal pris en charge plage de RSA pour le client TLS, créer un **ClientMinKeyBitLength** entrée. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD pour la longueur en bits souhaitée. Si non configuré, 1024bits sera au minimum. 

Pour spécifier une longueur en bits de clé maximale prise en charge plage de RSA pour le client TLS, créer un **ClientMaxKeyBitLength** entrée. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD pour la longueur en bits souhaitée. Si non configuré, un maximum n’est pas appliqué.

## <a name="keyexchangealgorithm---diffie-hellman-key-sizes"></a>KeyExchangeAlgorithm - tailles de clé Diffie-Hellman

Cette entrée contrôle la taille de clé Diffie-Hellman. 

Utilisation des algorithmes d’échange de clés doit être contrôlée par la configuration de l’ordre des suites de chiffrement.

Ajouté dans Windows10, version1507 et Windows Server2016.

Chemin d’accès du Registre: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\Diffie-Hellman

Pour spécifier une longueur de clé bits minimal pris en charge plage de Diffie-Helman pour le client TLS, créer un **ClientMinKeyBitLength** entrée. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD pour la longueur en bits souhaitée. Si non configuré, 1024bits sera au minimum. 
 
Pour spécifier une longueur en bits de clé maximale prise en charge plage de Diffie-Helman pour le client TLS, créer un **ClientMaxKeyBitLength** entrée. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD pour la longueur en bits souhaitée. Si non configuré, un maximum n’est pas appliqué. 
 
Pour spécifier la longueur de clé bits Diffie-Helman pour la valeur par défaut du serveur TLS, créez un **ServerMinKeyBitLength** entrée. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD pour la longueur en bits souhaitée. Si non configuré, 2048bits est la valeur par défaut. 

## <a name="maximumcachesize"></a>Nombre

Cette entrée contrôle le nombre maximal d’éléments du cache. En définissant le nombre de 0 désactive le cache de session côté serveur et empêche les reconnexions. Augmentation du nombre ci-dessus les valeurs par défaut provoque Lsass.exe consomme plus de mémoire. Chaque élément du cache de session nécessite généralement 2 à 4Ko de mémoire. Par défaut, cette entrée n’existe pas dans le Registre. La valeur par défaut est 20000éléments. 

Versions applicables: comme indiqué dans le **s’applique à** liste qui se trouve au début de cette rubrique.

Chemin d’accès du Registre: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="messaging--fragment-parsing"></a>Messagerie: l’analyse de fragments

________________________________________
Cette entrée contrôle la taille maximale autorisée de messages de négociation TLS fragmentés qui vont être acceptées. Messages dépasse la taille autorisée ne seront pas acceptés et la négociation TLS échouera. Ces entrées n’existent pas dans le Registre par défaut. 

Lorsque vous définissez la valeur sur 0 x 0, messages fragmentés ne sont pas traités et entraîne la négociation TLS échec. Cela rend TLS clients ou des serveurs sur l’ordinateur actuel non compatibles avec les normes RFC TLS. 

Le nombre maximal autorisé de taille peut être augmentée jusqu'à 2 ^ 24-1octets. Autoriser un client ou un serveur lire et stocker de grandes quantités de données non vérifiées à partir du réseau n’est pas une bonne idée et consomme davantage de mémoire pour chaque contexte de sécurité. 

Ajouté dans Windows7 et Windows Server2008R2.
Une mise à jour qui permet à Internet Explorer dans WindowsXP, WindowsVista, ou dans Windows Server2008 pour analyser les messages de négociation TLS/SSL fragmentés est disponible.

Chemin d’accès du Registre: HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Messaging

Pour spécifier une taille maximale autorisée de messages de négociation TLS fragmentés qui accepte le client TLS, créez un **MessageLimitClient** entrée. Après avoir créé l’entrée, remplacez la valeur DWORD pour la longueur en bits souhaitée. Si non configuré, la valeur par défaut sera 0 x 8000octets. 

Pour spécifier une taille maximale autorisée de messages de négociation TLS fragmentés qui accepte le serveur TLS lorsqu’il n’existe aucune authentification client, créez un **MessageLimitServer** entrée. Après avoir créé l’entrée, remplacez la valeur DWORD pour la longueur en bits souhaitée. Si non configuré, la valeur par défaut sera 0 x 4000octets. 

Pour spécifier une taille maximale autorisée de messages de négociation TLS fragmentés qui accepte le serveur TLS lorsqu’il est l’authentification du client, créez un **MessageLimitServerClientAuth** entrée. Après avoir créé l’entrée, remplacez la valeur DWORD pour la longueur en bits souhaitée. Si non configuré, la valeur par défaut sera 0 x 8000octets. 

## <a name="sendtrustedissuerlist"></a>SendTrustedIssuerList

Cette entrée contrôle l’indicateur qui est utilisé lors de la liste des émetteurs approuvés est envoyée. Dans le cas des serveurs qui font confiance des centaines d’autorités de certification pour l’authentification du client, il existe un trop grand nombre d’émetteurs pour le serveur pour être en mesure de les envoyer tous à l’ordinateur client lors de la demande d’authentification du client. Dans ce cas, cette clé de Registre peut être définie, et au lieu d’envoyer une liste partielle, le SSP Schannel n’envoie pas n’importe quelle liste au client.

Ne pas envoyer une liste des émetteurs approuvés peut avoir un impact sur ce que le client envoie lorsqu’il est demandé à un certificat client. Par exemple, lorsqu’Internet Explorer reçoit une demande d’authentification du client, il affiche uniquement les certificats de client qui sont liés à une des autorités de certification qui est envoyé par le serveur. Si le serveur n’a pas envoyé de liste, Internet Explorer affiche tous les certificats du client qui sont installés sur le client. 

Ce comportement peut être souhaitable. Par exemple, lorsque des environnements PKI incluent des certificats croisés, les certificats de serveur et le client aura pas la même racine Autorité de certification; par conséquent, Internet Explorer ne peut pas choisir un certificat qui est lié à une des autorités de certification du serveur. En configurant le serveur de ne pas envoyer une liste d’émetteurs approuvés, Internet Explorer envoie tous ses certificats.

Par défaut, cette entrée n’existe pas dans le Registre.

Comportement d’envoyer la liste des émetteurs approuvés par défaut

| Version de Windows | Heure |
|-----------------|------|
| Windows Server2012 et Windows8 et versions ultérieures | FALSE (FAUX) |
| Windows Server2008R2 et Windows7 et versions antérieures | TRUE |

Versions applicables: comme indiqué dans le **s’applique à** liste qui se trouve au début de cette rubrique.

Chemin d’accès du Registre: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="servercachetime"></a>ServerCacheTime

Cette entrée contrôle la durée en millisecondes pendant lequel le système d’exploitation prend le point d’expirer les entrées de cache côté serveur. La valeur 0 désactive le cache de session côté serveur et empêche les reconnexions. Augmentation ServerCacheTime au-dessus des valeurs par défaut provoque Lsass.exe consomme plus de mémoire. Chaque élément du cache de session nécessite généralement 2 à 4Ko de mémoire. Par défaut, cette entrée n’existe pas dans le Registre. 

Versions applicables: comme indiqué dans le **s’applique à** liste qui se trouve au début de cette rubrique.

Chemin d’accès du Registre: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Heure du cache du serveur par défaut: 10heures

## <a name="ssl-20"></a>SSL 2.0

Cette sous-clé contrôle l’utilisation de SSL 2.0.

À partir de Windows10, version1607 et Windows Server2016, SSL 2.0 a été supprimé et n’est plus pris en charge.
Pour un paramètres par défaut de SSL 2.0, voir [protocoles de TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Chemin d’accès du Registre: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour activer le protocole SSL 2.0, créez un **activé** entrée dans la sous-clé appropriée. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD par 1. Pour désactiver le protocole, remplacez la valeur DWORD 0.

Tableau des sous-clés SSL 2.0

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de SSL 2.0 sur le client SSL. |
| Serveur | Contrôle l’utilisation de SSL 2.0 sur le serveur SSL. |
| DisabledByDefault | Indicateur pour désactiver SSL 2.0 par défaut. |

## <a name="ssl-30"></a>SSL 3.0

Cette sous-clé contrôle l’utilisation de SSL 3.0.

À partir de Windows10, version1607 et Windows Server2016, SSL 3.0 a été désactivée par défaut. Pour les paramètres par défaut de SSL 3.0, voir [protocoles de TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Chemin d’accès du Registre: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour activer le protocole SSL 3.0, créez une entrée activé dans la sous-clé appropriée. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD par 1. Pour désactiver le protocole, remplacez la valeur DWORD 0.

Tableau des sous-clés SSL 3.0

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de SSL 3.0 sur le client SSL. |
| Serveur | Contrôle l’utilisation de SSL 3.0 sur le serveur SSL. |
| DisabledByDefault | Indicateur pour désactiver SSL 3.0 par défaut. |

## <a name="tls-10"></a>TLS 1.0

Cette sous-clé contrôle l’utilisation de TLS 1.0.

Pour les paramètres par défaut de TLS 1.0, consultez Voir [protocoles de TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Chemin d’accès du Registre: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour désactiver le protocole TLS 1.0, créez un **activé** entrée dans la sous-clé appropriée. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD 0. Pour activer le protocole, remplacez la valeur DWORD par 1.

Tableau des sous-clés TLS 1.0

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de TLS 1.0 sur le client TLS. |
| Serveur | Contrôle l’utilisation de TLS 1.0 sur le serveur TLS. |
| DisabledByDefault | Indicateur pour désactiver TLS 1.0 par défaut. |

## <a name="tls-11"></a>TLS 1.1

Cette sous-clé contrôle l’utilisation de TLS 1.1.

Pour les paramètres par défaut de TLS 1.1, voir [protocoles de TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

>[!Note] 
>TLS 1.1 soit activé et négocié sur des serveurs qui exécutent Windows Server2008R2, vous devez créer le **DisabledByDefault** entrée dans la sous-clé appropriée (Client, serveur) et la valeur «0». L’entrée ne sera pas visible dans le Registre et il est défini sur «1» par défaut.

Chemin d’accès du Registre: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour désactiver le protocole TLS 1.1, créez un **activé** entrée dans la sous-clé appropriée. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD 0. Pour activer le protocole, remplacez la valeur DWORD par 1.

Tableau des sous-clés TLS 1.1

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de TLS 1.1 sur le client TLS. |
| Serveur | Contrôle l’utilisation de TLS 1.1 sur le serveur TLS. |
| DisabledByDefault | Indicateur pour désactiver TLS 1.1 par défaut. |

## <a name="tls-12"></a>TLS 1.2

Cette sous-clé contrôle l’utilisation de TLS 1.2.

Pour les paramètres par défaut de TLS 1.2, voir [protocoles de TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

>[!Note] 
>Pour que TLS 1.2 soit activé et négocié sur des serveurs qui exécutent Windows Server2008R2, vous devez créer le **DisabledByDefault** entrée dans la sous-clé appropriée (Client, serveur) et la valeur «0». L’entrée ne sera pas visible dans le Registre et il est défini sur «1» par défaut.

Chemin d’accès du Registre: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour désactiver le protocole TLS 1.2, créez un **activé** entrée dans la sous-clé appropriée. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD 0. Pour activer le protocole, remplacez la valeur DWORD par 1.

Tableau des sous-clés TLS 1.2

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de TLS 1.2 sur le client TLS. |
| Serveur | Contrôle l’utilisation de TLS 1.2 sur le serveur TLS. |
| DisabledByDefault | Indicateur pour désactiver TLS 1.2 par défaut. |

## <a name="dtls-10"></a>DTLS 1.0

Cette sous-clé contrôle l’utilisation de DTLS 1.0.

Pour les paramètres par défaut de DTLS 1.0, voir [protocoles de TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Chemin d’accès du Registre: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour désactiver le protocole DTLS 1.0, créez un **activé** entrée dans la sous-clé appropriée. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD 0. Pour activer le protocole, remplacez la valeur DWORD par 1.

Tableau des sous-clés DTLS 1.0

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de DTLS 1.0 sur le client DTLS. |
| Serveur | Contrôle l’utilisation de DTLS 1.0 sur le serveur DTLS. |
| DisabledByDefault | Indicateur pour désactiver le protocole DTLS 1.0 par défaut. |

## <a name="dtls-12"></a>DTLS 1.2

Cette sous-clé contrôle l’utilisation de DTLS 1.2.

Pour les paramètres par défaut de DTLS 1.2, voir [protocoles de TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Chemin d’accès du Registre: HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour désactiver le protocole DTLS 1.2, créez un **activé** entrée dans la sous-clé appropriée. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD 0. Pour activer le protocole, remplacez la valeur DWORD par 1.

Tableau des sous-clés DTLS 1.2

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de DTLS 1.2 sur le client DTLS. |
| Serveur | Contrôle l’utilisation de DTLS 1.2 sur le serveur DTLS. |
| DisabledByDefault | Indicateur pour désactiver le protocole DTLS 1.2 par défaut. |

