---
title: Paramètres de Registre Layer Security (TLS) de transport
description: Sécurité de Windows Server
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
ms.date: 02/28/2019
ms.openlocfilehash: 32068319aae7545675e126eed6e1ab4c914bcbcf
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812636"
---
# <a name="transport-layer-security-tls-registry-settings"></a>Paramètres de Registre Layer Security (TLS) de transport

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows 10

Cette rubrique de référence pour les professionnels de l’informatique contient des informations de paramètre de Registre pris en charge pour l’implémentation Windows du protocole Transport Layer Security (TLS) et le protocole Secure Sockets Layer (SSL) via la prise en charge de sécurité Schannel Fournisseur (SSP). Les sous-clés de Registre et les entrées traitées dans cette rubrique vous aident à administrez et dépanner le SSP Schannel, en particulier les protocoles TLS et SSL. 

> [!CAUTION]
> Ces informations sont fournies à titre de référence et peuvent être utilisées dans le cadre de la résolution de problèmes ou de la vérification de l’application des paramètres requis.
> Nous vous recommandons de ne pas modifier directement le Registre, sauf s’il n’y a pas d’autre solution.
> Les modifications apportées au Registre ne sont pas validées par l’Éditeur du Registre ni par le système d’exploitation Windows avant d’être appliquées.
> Par conséquent, des valeurs incorrectes peuvent être stockées et cela peut générer des erreurs irrécupérables dans le système.
> Si possible, plutôt que de modifier le Registre directement, utilisez la stratégie de groupe ou d’autres outils Windows tels que la console MMC (Microsoft Management) pour exécuter des opérations.
> Si vous devez modifier le Registre, soyez très vigilant.

## <a name="certificatemappingmethods"></a>CertificateMappingMethods 

Par défaut, cette entrée n’existe pas dans le Registre. La valeur par défaut est que les quatre méthodes de mappage des certificats, répertoriées ci-dessous, sont prises en charge.

Lorsqu’une application du serveur requiert l’authentification du client, Schannel tente automatiquement de mapper le certificat fourni par le client avec un compte d’utilisateur. Vous pouvez authentifier les utilisateurs qui se connectent avec un certificat client en créant des mappages qui lient les informations de certificat à un compte d’utilisateur Windows. Après avoir créé et activé un mappage de certificat, chaque fois qu’un client présente un certificat client, votre application serveur associe automatiquement cet utilisateur au compte d’utilisateur Windows approprié.

Dans la plupart des cas, un certificat est mappé avec un compte d’utilisateur de deux manières : 

- Un seul certificat est mappé avec un compte d’utilisateur (mappage un-à-un).
- Plusieurs certificats sont mappés avec un compte d’utilisateur (mappage plusieurs-à-un).

Par défaut, le fournisseur Schannel utilise les quatre méthodes de mappage de certificat suivantes, répertoriées par ordre de préférence :

1. Mappage de certificats Kerberos service-for-user (S4U)
2. Mappage du nom principal de l’utilisateur
3. Mappage un-à-un (également appelé mappage objet/émetteur)
4. Mappage plusieurs-à-un

Versions applicables : Comme indiqué dans la liste **S’applique à** qui se trouve au début de cette rubrique.

Chemin d’accès du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="ciphers"></a>Chiffrements

Chiffrement TLS/SSL doit être contrôlé en configurant l’ordre des suites de chiffrement. Pour plus d’informations, consultez [configuration ordre des suites de chiffrement TLS](manage-tls.md#configuring-tls-cipher-suite-order).

Pour plus d’informations sur l’ordre de suites de chiffrement par défaut qui sont utilisés par le SSP Schannel, consultez [Suites de chiffrement dans TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 

## <a name="ciphersuites"></a>Suites de chiffrement

Configuration des suites de chiffrement TLS/SSL doit être effectuée à l’aide de la stratégie de groupe, gestion des appareils mobiles ou PowerShell, consultez [configuration ordre des suites de chiffrement TLS](manage-tls.md#configuring-tls-cipher-suite-order) pour plus d’informations.

Pour plus d’informations sur l’ordre de suites de chiffrement par défaut qui sont utilisés par le SSP Schannel, consultez [Suites de chiffrement dans TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 


## <a name="clientcachetime"></a>ClientCacheTime

Cette entrée contrôle la durée en millisecondes nécessaire pour que le système d’exploitation fasse expirer les entrées de cache côté client. La valeur 0 désactive la mise en cache d’une connexion sécurisée. Par défaut, cette entrée n’existe pas dans le Registre. 

La première fois qu’un client se connecte à un serveur via le SSP Schannel, une liaison TLS/SSL complète est établie. Ensuite, le secret principal, la suite de chiffrement et les certificats sont stockés dans le cache de session sur le client et le serveur correspondants.

Depuis Windows Server 2008 et Windows Vista, la durée du cache client par défaut est de 10 heures.

Chemin d’accès du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Heure du cache du client par défaut

## <a name="enableocspstaplingforsni"></a>EnableOcspStaplingForSni

Agrafage de certificat protocole OCSP (Status) en ligne permet à un serveur web, tels que les Services Internet (IIS), pour fournir l’état actuel de la révocation d’un certificat de serveur lorsqu’il envoie le certificat de serveur à un client pendant la négociation TLS. Cette fonctionnalité réduit la charge sur les serveurs de protocole OCSP, car le serveur web peut mettre en cache l’état actuel de OCSP du certificat du serveur et les envoyer à plusieurs clients web. Sans cette fonctionnalité, chaque client web tentait de récupérer l’état actuel de OCSP du certificat du serveur à partir du serveur de protocole OCSP. Cela générerait une charge élevée sur ce serveur OCSP. 

Outre par IIS, les services web sur http.sys peuvent également bénéficier de ce paramètre, y compris Active Directory Federation Services (ADFS) et Proxy d’Application Web (WAP). 

Par défaut, prise en charge du protocole OCSP est activée pour les sites Web IIS qui ont la simple liaison sécurisée (SSL/TLS). Toutefois, cette prise en charge n’est pas activée par défaut si le site Web IIS est à l’aide d’une des deux types de liaisons de sécurisées (SSL/TLS) suivants :
- Exiger l’Indication de nom de serveur
- Utiliser le magasin de certificats centralisés

Dans ce cas, la réponse hello de serveur pendant la négociation TLS n’inclure un état OCSP agrafé par défaut. Ce comportement améliore les performances : L’implémentation d’agrafage de Windows OCSP gérant des centaines de certificats de serveur. Étant donné que SNI et CCS activer IIS à l’échelle à des milliers de sites Web qui ont potentiellement des milliers de certificats de serveur, la définition ce comportement doit être activé par défaut peut provoquer des problèmes de performances.

Versions applicables : Toutes les versions à compter de Windows Server 2012 et Windows 8. 

Chemin d’accès du Registre : [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL]

Ajoutez la clé suivante :

"EnableOcspStaplingForSni"=dword:00000001

Pour désactiver, définissez la valeur DWORD sur 0 :

"EnableOcspStaplingForSni"=dword:00000000

> [!NOTE] 
> L’activation de cette clé de Registre a un impact potentiel sur les performances.

## <a name="fipsalgorithmpolicy"></a>FIPSAlgorithmPolicy

Cette entrée contrôle la conformité aux normes FIPS (Federal Information Processing Standard). La valeur par défaut est 0.

Versions applicables : Toutes les versions à compter de Windows Server 2012 et Windows 8. 

Chemin d’accès du Registre : HKLM SYSTEM\CurrentControlSet\Control\LSA

Windows Server FIPS les suites de chiffrement : Consultez [pris en charge des protocoles et des Suites de chiffrement dans le SSP Schannel](https://technet.microsoft.com/library/dn786419.aspx).

## <a name="hashes"></a>Hachages

Les algorithmes de hachage TLS/SSL doivent être contrôlés en configurant l’ordre des suites de chiffrement. Consultez [configuration ordre des suites de chiffrement TLS](manage-tls.md#configuring-tls-cipher-suite-order) pour plus d’informations.

## <a name="issuercachesize"></a>IssuerCacheSize

Cette entrée contrôle la taille du cache de l’émetteur. Elle est utilisée avec le mappage de l’émetteur. Le SSP Schannel tente de mapper tous les émetteurs de la chaîne de certificats du client, et non pas seulement l’émetteur direct du certificat du client. Lorsque les émetteurs ne correspondent pas à un compte, ce qui est le cas par défaut, le serveur peut tenter de mapper le même nom de l’émetteur à plusieurs reprises, des centaines de fois par seconde. 

Pour éviter ce problème, le serveur a un cache négatif ; ainsi lorsqu’un nom d’émetteur ne correspond pas à un compte, il est ajouté au cache et le SSP Schannel ne tente pas de mapper le nom d’émetteur à nouveau tant que l’entrée de cache n’a pas expiré. Cette entrée de Registre spécifie la taille du cache. Par défaut, cette entrée n’existe pas dans le Registre. La valeur par défaut est 100. 

Versions applicables : Toutes les versions à compter de Windows Server 2008 et Windows Vista.

Chemin d’accès du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="issuercachetime"></a>IssuerCacheTime

Cette entrée contrôle l’intervalle de temps d’expiration du cache en millisecondes. Le SSP Schannel tente de mapper tous les émetteurs de la chaîne de certificats du client, et non pas seulement l’émetteur direct du certificat du client. Lorsque les émetteurs ne correspondent pas à un compte, ce qui est le cas par défaut, le serveur peut tenter de mapper le même nom d’émetteur à plusieurs reprises, des centaines de fois par seconde.

Pour éviter ce problème, le serveur a un cache négatif ; ainsi lorsqu’un nom d’émetteur ne correspond pas à un compte, il est ajouté au cache et le SSP Schannel ne tente pas de mapper le nom d’émetteur à nouveau tant que l’entrée de cache n’a pas expiré. Ce cache est conservé pour des raisons de performances, afin que le système ne continue pas de tente de mapper les mêmes émetteurs. Par défaut, cette entrée n’existe pas dans le Registre. La valeur par défaut est 10 minutes.

Versions applicables : Toutes les versions à compter de Windows Server 2008 et Windows Vista.

Chemin d’accès du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="keyexchangealgorithm---client-rsa-key-sizes"></a>KeyExchangeAlgorithm - tailles de clé RSA de Client

Cette entrée contrôle les tailles de clé RSA client. 

Utilisation des algorithmes d’échange de clés doit être contrôlée en configurant l’ordre des suites de chiffrement.

Ajouté dans Windows 10, version 1507 et Windows Server 2016.

Chemin d’accès du Registre : HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\PKCS

Pour spécifier une longueur de bits pour les clés de plage prise en charge minimale de RSA pour le client TLS, créez un **ClientMinKeyBitLength** entrée. Par défaut, cette entrée n’existe pas dans le Registre. Une fois que vous avez créé l’entrée, remplacez la valeur DWORD à la longueur en bits souhaitée. Si non configuré, 1 024 bits sera la valeur minimale. 

Pour spécifier une longueur en bits de clé maximale prise en charge plage de RSA pour le client TLS, créez un **ClientMaxKeyBitLength** entrée. Par défaut, cette entrée n’existe pas dans le Registre. Une fois que vous avez créé l’entrée, remplacez la valeur DWORD à la longueur en bits souhaitée. Si ne pas configuré, un maximum n’est pas appliqué.

## <a name="keyexchangealgorithm---diffie-hellman-key-sizes"></a>KeyExchangeAlgorithm - tailles de clé Diffie-Hellman

Cette entrée contrôle les tailles de clé Diffie-Hellman. 

Utilisation des algorithmes d’échange de clés doit être contrôlée en configurant l’ordre des suites de chiffrement.

Ajouté dans Windows 10, version 1507 et Windows Server 2016.

Chemin d’accès du Registre : HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\Diffie-Hellman

Pour spécifier une longueur en bits de clé minimale prise en charge plage de Diffie-Helman pour le client TLS, créez un **ClientMinKeyBitLength** entrée. Par défaut, cette entrée n’existe pas dans le Registre. Une fois que vous avez créé l’entrée, remplacez la valeur DWORD à la longueur en bits souhaitée. Si non configuré, 1 024 bits sera la valeur minimale. 
 
Pour spécifier une longueur en bits de clé maximale prise en charge plage de Diffie-Helman pour le client TLS, créez un **ClientMaxKeyBitLength** entrée. Par défaut, cette entrée n’existe pas dans le Registre. Une fois que vous avez créé l’entrée, remplacez la valeur DWORD à la longueur en bits souhaitée. Si ne pas configuré, un maximum n’est pas appliqué. 
 
Pour spécifier la longueur en bits de clé Diffie-Helman pour la valeur par défaut du serveur TLS, créez un **ServerMinKeyBitLength** entrée. Par défaut, cette entrée n’existe pas dans le Registre. Une fois que vous avez créé l’entrée, remplacez la valeur DWORD à la longueur en bits souhaitée. Si non configuré, 2 048 bits sera la valeur par défaut. 

## <a name="maximumcachesize"></a>MaximumCacheSize

Cette entrée contrôle le nombre maximal d’éléments du cache. En définissant le nombre maximal d’éléments du cache sur 0, vous désactivez le cache de session côté serveur et empêchez les reconnexions. Lorsque la valeur de cette entrée est supérieure aux valeurs par défaut, Lsass.exe consomme plus de mémoire. Chaque élément de cache de session nécessite généralement 2 à 4 Ko de mémoire. Par défaut, cette entrée n’existe pas dans le Registre. La valeur par défaut est 20 000 éléments. 

Versions applicables : Toutes les versions à compter de Windows Server 2008 et Windows Vista.

Chemin d’accès du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="messaging--fragment-parsing"></a>Messaging – l’analyse du fragment

________________________________________
Cette entrée contrôle la taille maximale autorisée des messages de négociation TLS fragmentés qui vont être acceptées. Les messages supérieurs à la taille autorisée ne seront pas acceptés et la négociation TLS échouera. Ces entrées n’existent pas dans le Registre par défaut. 

Lorsque vous définissez la valeur sur 0 x 0, messages fragmentés ne sont pas traités et entraînent la négociation TLS à échouer. Cela rend TLS clients ou serveurs sur l’ordinateur actuel non compatible avec les normes RFC TLS. 

Le nombre maximal autorisé de taille peut être augmentée jusqu'à 2 ^ 24-1 octets. Autoriser un client ou un serveur lire et stocker de grandes quantités de données non vérifiées à partir du réseau n’est pas une bonne idée et consomme de la mémoire supplémentaire pour chaque contexte de sécurité. 

Ajouté dans Windows 7 et Windows Server 2008 R2.
Une mise à jour qui permet à Internet Explorer dans Windows XP, Windows Vista, ou dans Windows Server 2008 pour analyser les messages de négociation TLS/SSL fragmentés n’est disponible.

Chemin d’accès du Registre : HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Messaging

Pour spécifier une taille maximale autorisée des messages de négociation TLS fragmentés qui accepte le client TLS, créez un **MessageLimitClient** entrée. Une fois que vous avez créé l’entrée, remplacez la valeur DWORD à la longueur en bits souhaitée. Si non configuré, la valeur par défaut sera 0 x 8000 octets. 

Pour spécifier une taille maximale autorisée des messages de négociation TLS fragmentés que le serveur TLS acceptera lorsqu’il n’existe aucune authentification client, créez un **MessageLimitServer** entrée. Une fois que vous avez créé l’entrée, remplacez la valeur DWORD à la longueur en bits souhaitée. Si non configuré, la valeur par défaut sera 0 x 4000 octets. 

Pour spécifier une taille maximale autorisée des messages de négociation TLS fragmentés que le serveur TLS acceptera en l’absence de l’authentification du client, créez un **MessageLimitServerClientAuth** entrée. Une fois que vous avez créé l’entrée, remplacez la valeur DWORD à la longueur en bits souhaitée. Si non configuré, la valeur par défaut sera 0 x 8000 octets. 

## <a name="sendtrustedissuerlist"></a>SendTrustedIssuerList

Cette entrée contrôle l’indicateur qui est utilisé lors de l’envoi de la liste des émetteurs approuvés. Dans le cas des serveurs qui font confiance à des centaines d’autorités de certification pour l’authentification du client, il y a trop d’émetteurs pour que le serveur puisse tous les envoyer à l’ordinateur client lors de la demande d’authentification du client. Dans ce cas, cette clé de Registre peut être définie, et au lieu d’envoyer une liste partielle, le SSP Schannel n’envoie aucune liste au client.

Ne pas envoyer une liste d’émetteurs approuvés peut avoir un impact sur ce que le client envoie lorsqu’il reçoit une demande de certificat de client. Par exemple, lorsqu’Internet Explorer reçoit une demande d’authentification du client, il affiche uniquement les certificats du client qui sont liés à l’une des autorités de certification qui est envoyée par le serveur. Si le serveur n’a pas envoyé de liste, Internet Explorer affiche tous les certificats du client installés sur le client. 

Ce comportement peut être souhaitable. Par exemple, lorsque des environnements PKI incluent des certificats croisés, les certificats de serveur et de client n’ont pas la même autorité de certification racine. Par conséquent, Internet Explorer ne peut pas choisir un certificat qui est lié à une des autorités de certification du serveur. En configurant le serveur de manière à ce qu’il n’envoie pas de liste d’émetteurs approuvés, Internet Explorer envoie tous ses certificats.

Par défaut, cette entrée n’existe pas dans le Registre.

Comportement d’envoyer la liste des émetteurs approuvés par défaut

| Version de Windows | Time |
|-----------------|------|
| Windows Server 2012 et Windows 8 et versions ultérieures | FALSE |
| Windows Server 2008 R2 et Windows 7 et versions antérieures | TRUE |

Versions applicables : Toutes les versions à compter de Windows Server 2008 et Windows Vista.

Chemin d’accès du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="servercachetime"></a>ServerCacheTime

Cette entrée contrôle la durée en millisecondes nécessaire pour que le système d’exploitation fasse expirer les entrées de cache côté serveur. La valeur 0 désactive le cache de session côté serveur et empêche les reconnexions. Lorsque la valeur de cette entrée est supérieure aux valeurs par défaut, Lsass.exe consomme plus de mémoire. Chaque élément du cache de session nécessite généralement 2 à 4 Ko de mémoire. Par défaut, cette entrée n’existe pas dans le Registre. 

Versions applicables : Toutes les versions à compter de Windows Server 2008 et Windows Vista.

Chemin d’accès du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Durée de cache du serveur par défaut : 10 heures

## <a name="ssl-20"></a>SSL 2.0

Cette sous-clé contrôle l’utilisation de SSL 2.0.

Depuis Windows 10, version 1607 et Windows Server 2016, SSL 2.0 a été supprimé et n’est plus pris en charge.
Pour une valeur par défaut les paramètres SSL 2.0, consultez [protocoles dans TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Chemin d’accès du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour activer le protocole SSL 2.0, créez un **activé** entrée dans la sous-clé Client ou du serveur, comme décrit dans le tableau suivant. Par défaut, cette entrée n’existe pas dans le Registre. Une fois que vous avez créé l’entrée, remplacez la valeur DWORD par 1. 

Tableau des sous-clés SSL 2.0

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de SSL 2.0 sur le client SSL. |
| Server | Contrôle l’utilisation de SSL 2.0 sur le serveur SSL. |

Pour désactiver SSL 2.0 pour le client ou serveur, modifiez la valeur DWORD 0. Si une application SSPI demandes d’utilisation de SSL 2.0, il sera refusé. 

Pour désactiver SSL 2.0 par défaut, créez un **DisabledByDefault** entrée et modifier la valeur DWORD de valeur 1. Si un explicitement d’application SSPI demande à utiliser SSL 2.0, il peut être négociée. 

L’exemple suivant montre le SSL 2.0 est désactivé dans le Registre :

![SSL 2.0 est désactivé](images/ssl-2-registry-setting.png)


## <a name="ssl-30"></a>SSL 3.0

Cette sous-clé contrôle l’utilisation de SSL 3.0.

Depuis Windows 10, version 1607 et Windows Server 2016, SSL 3.0 a été désactivée par défaut. Pour les paramètres par défaut de SSL 3.0, consultez [protocoles dans TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Chemin d’accès du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour activer le protocole SSL 3.0, créez un **activé** entrée dans la sous-clé Client ou du serveur, comme décrit dans le tableau suivant.  
Par défaut, cette entrée n’existe pas dans le Registre. Une fois que vous avez créé l’entrée, remplacez la valeur DWORD par 1. 

Tableau des sous-clés SSL 3.0

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de SSL 3.0 sur le client SSL. |
| Server | Contrôle l’utilisation de SSL 3.0 sur le serveur SSL. |

Pour désactiver SSL 3.0 pour le client ou serveur, modifiez la valeur DWORD 0.
Si une application SSPI demande à utiliser SSL 3.0, il sera refusé. 

Pour désactiver SSL 3.0 par défaut, créez un **DisabledByDefault** entrée et modifier la valeur DWORD de valeur 1. Si une application SSPI demande explicitement à utiliser SSL 3.0, il peut être négociée. 

L’exemple suivant montre le SSL 3.0 est désactivé dans le Registre :

![SSL 3.0 est désactivé](images/ssl-3-registry-setting.png)

## <a name="tls-10"></a>TLS 1.0

Cette sous-clé contrôle l’utilisation de TLS 1.0.

Pour les paramètres par défaut de TLS 1.0, consultez [protocoles dans TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Chemin d’accès du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour activer le protocole TLS 1.0, créez un **activé** entrée dans la sous-clé Client ou du serveur comme décrit dans le tableau suivant. Par défaut, cette entrée n’existe pas dans le Registre. Une fois que vous avez créé l’entrée, remplacez la valeur DWORD par 1. 

Tableau des sous-clés TLS 1.0

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de TLS 1.0 sur le client TLS. |
| Server | Contrôle l’utilisation de TLS 1.0 sur le serveur TLS. |

Pour désactiver TLS 1.0 pour le client ou serveur, modifiez la valeur DWORD 0.
Si une application SSPI demande à utiliser que TLS 1.0, il sera refusé. 

Pour désactiver TLS 1.0 par défaut, créez un **DisabledByDefault** entrée et modifier la valeur DWORD de valeur 1. Si une application SSPI demande explicitement à utiliser TLS 1.0, il peut être négociée. 

L’exemple suivant montre le protocole TLS 1.0 est désactivé dans le Registre :

![TLS 1.0 est désactivé](images/tls-registry-setting.png)

## <a name="tls-11"></a>TLS 1.1

Cette sous-clé contrôle l’utilisation de TLS 1.1.

Pour les paramètres par défaut de TLS 1.1, consultez [protocoles dans TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Chemin d’accès du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour activer le protocole TLS 1.1, créez un **activé** entrée dans la sous-clé Client ou du serveur comme décrit dans le tableau suivant. Par défaut, cette entrée n’existe pas dans le Registre. Une fois que vous avez créé l’entrée, remplacez la valeur DWORD par 1. 

Tableau des sous-clés TLS 1.1

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de TLS 1.1 sur le client TLS. |
| Server | Contrôle l’utilisation de TLS 1.1 sur le serveur TLS. |

Pour désactiver TLS 1.1 pour le client ou serveur, modifiez la valeur DWORD 0.
Si une application SSPI demande pour utiliser TLS 1.1, il sera refusé. 

Pour désactiver TLS 1.1 par défaut, créez un **DisabledByDefault** entrée et modifier la valeur DWORD de valeur 1. Si une application SSPI demande explicitement pour utiliser TLS 1.1, il peut être négociée. 

L’exemple suivant montre le TLS 1.1 est désactivé dans le Registre :

![TLS 1.1 désactivé](images/tls-11-registry-setting.png)

## <a name="tls-12"></a>TLS 1.2

Cette sous-clé contrôle l’utilisation de TLS 1.2.

Pour les paramètres par défaut de TLS 1.2, consultez [protocoles dans TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Chemin d’accès du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour activer le protocole TLS 1.2, créez un **activé** entrée dans la sous-clé Client ou du serveur comme décrit dans le tableau suivant. Par défaut, cette entrée n’existe pas dans le Registre. Une fois que vous avez créé l’entrée, remplacez la valeur DWORD par 1. 

Tableau des sous-clés TLS 1.2

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de TLS 1.2 sur le client TLS. |
| Server | Contrôle l’utilisation de TLS 1.2 sur le serveur TLS. |

Pour désactiver TLS 1.2 pour le client ou serveur, modifiez la valeur DWORD 0.
Si une application SSPI demande pour utiliser TLS 1.2, sera refusée. 

Pour désactiver TLS 1.2 par défaut, créez un **DisabledByDefault** entrée et modifier la valeur DWORD de valeur 1. Si une application SSPI demande explicitement pour utiliser TLS 1.2, il peut être négociée. 

L’exemple suivant montre le TLS 1.2 est désactivé dans le Registre :

![TLS 1.2 est désactivé](images/tls-12-registry-setting.png)

## <a name="dtls-10"></a>DTLS 1.0

Cette sous-clé contrôle l’utilisation de DTLS 1.0.

Pour les paramètres par défaut de DTLS 1.0, consultez [protocoles dans TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Chemin d’accès du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour activer le protocole DTLS 1.0, créez un **activé** entrée dans la sous-clé Client ou du serveur comme décrit dans le tableau suivant. Par défaut, cette entrée n’existe pas dans le Registre. Une fois que vous avez créé l’entrée, remplacez la valeur DWORD par 1. 

Tableau des sous-clés DTLS 1.0

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de DTLS 1.0 sur le client DTLS. |
| Server | Contrôle l’utilisation de DTLS 1.0 sur le serveur DTLS. |

Pour désactiver DTLS 1.0 pour le client ou serveur, modifiez la valeur DWORD 0.
Si une application SSPI demande à utiliser le protocole DTLS 1.0, il sera refusé. 

Pour désactiver le protocole DTLS 1.0 par défaut, créez un **DisabledByDefault** entrée et modifier la valeur DWORD de valeur 1. Si une application SSPI demande explicitement à utiliser DTLS 1.0, il peut être négociée. 

L’exemple suivant montre 1.0 DTLS désactivé dans le Registre :

![1.0 DTLS désactivé](images/dtls-10-registry-setting.png)

## <a name="dtls-12"></a>DTLS 1.2

Cette sous-clé contrôle l’utilisation de DTLS 1.2.

Pour les paramètres par défaut de DTLS 1.2, consultez [protocoles dans TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Chemin d’accès du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour activer le protocole DTLS 1.2, créez un **activé** entrée dans la sous-clé Client ou du serveur comme décrit dans le tableau suivant. Par défaut, cette entrée n’existe pas dans le Registre. Une fois que vous avez créé l’entrée, remplacez la valeur DWORD par 1. 

Tableau des sous-clés DTLS 1.2

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de DTLS 1.2 sur le client DTLS. |
| Server | Contrôle l’utilisation de DTLS 1.2 sur le serveur DTLS. |


Pour désactiver DTLS 1.2 pour le client ou serveur, modifiez la valeur DWORD 0.
Si une application SSPI demande à utiliser le protocole DTLS 1.0, il sera refusé. 

Pour désactiver le protocole DTLS 1.2 par défaut, créez un **DisabledByDefault** entrée et modifier la valeur DWORD de valeur 1. Si une application SSPI demande explicitement à utiliser DTLS 1.2, il peut être négociée. 

L’exemple suivant montre 1.1 DTLS désactivé dans le Registre :

![1.1 DTLS désactivé](images/dtls-11-registry-setting.png)


