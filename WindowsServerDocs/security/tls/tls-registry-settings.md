---
title: Paramètres du Registre TLS (Transport Layer Security)
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
ms.openlocfilehash: 83146bd8a65b90994ed90a6dda29a4bc00a2533a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870296"
---
# <a name="transport-layer-security-tls-registry-settings"></a>Paramètres du Registre TLS (Transport Layer Security)

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows 10

Cette rubrique de référence destinée aux professionnels de l’informatique contient des informations sur les paramètres de Registre pris en charge pour l’implémentation Windows du protocole TLS (Transport Layer Security) et du protocole protocole SSL (SSL) par le biais de la prise en charge de la sécurité Schannel. Fournisseur (SSP). Les sous-clés et les entrées de Registre abordées dans cette rubrique vous aident à administrer et à dépanner le SSP Schannel, en particulier les protocoles TLS et SSL. 

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

Chemin du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="ciphers"></a>Chiffrements

Le chiffrement TLS/SSL doit être contrôlé en configurant l’ordre de la suite de chiffrement. Pour plus d’informations, consultez Configuration de la [suite de chiffrement TLS](manage-tls.md#configuring-tls-cipher-suite-order).

Pour plus d’informations sur l’ordre des suites de chiffrement par défaut qui sont utilisées par le SSP Schannel, voir [suites de chiffrement dans TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 

## <a name="ciphersuites"></a>Suites de chiffrement

La configuration des suites de chiffrement TLS/SSL doit être effectuée à l’aide de la stratégie de groupe, MDM ou PowerShell. pour plus d’informations, consultez Configuration de la [suite de chiffrement TLS](manage-tls.md#configuring-tls-cipher-suite-order) .

Pour plus d’informations sur l’ordre des suites de chiffrement par défaut qui sont utilisées par le SSP Schannel, voir [suites de chiffrement dans TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/aa374757.aspx). 


## <a name="clientcachetime"></a>ClientCacheTime

Cette entrée contrôle la durée en millisecondes nécessaire pour que le système d’exploitation fasse expirer les entrées de cache côté client. La valeur 0 désactive la mise en cache d’une connexion sécurisée. Par défaut, cette entrée n’existe pas dans le Registre. 

La première fois qu’un client se connecte à un serveur via le SSP Schannel, une liaison TLS/SSL complète est établie. Ensuite, le secret principal, la suite de chiffrement et les certificats sont stockés dans le cache de session sur le client et le serveur correspondants.

À partir de Windows Server 2008 et Windows Vista, la durée de mise en cache par défaut du client est de 10 heures.

Chemin du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Heure du cache du client par défaut

## <a name="enableocspstaplingforsni"></a>EnableOcspStaplingForSni

L’agrafage du protocole OCSP (Online Certificate Status Protocol) permet à un serveur Web, tel que Internet Information Services (IIS), de fournir l’état de révocation actuel d’un certificat de serveur lorsqu’il envoie le certificat de serveur à un client pendant la négociation TLS. Cette fonctionnalité réduit la charge sur les serveurs OCSP, car le serveur Web peut mettre en cache l’État OCSP actuel du certificat de serveur et l’envoyer à plusieurs clients Web. Sans cette fonctionnalité, chaque client Web essaiera de récupérer l’État OCSP actuel du certificat de serveur à partir du serveur OCSP. Cela générerait une charge élevée sur ce serveur OCSP. 

Outre IIS, les services Web sur http. sys peuvent également bénéficier de ce paramètre, y compris Services ADFS (AD FS) et le proxy d’application Web (WAP). 

Par défaut, la prise en charge OCSP est activée pour les sites Web IIS qui ont une liaison sécurisée (SSL/TLS) simple. Toutefois, cette prise en charge n’est pas activée par défaut si le site Web IIS utilise l’un des types suivants de liaisons sécurisées (SSL/TLS), ou les deux :
- Exiger Indication du nom du serveur
- Utiliser le magasin de certificats centralisés

Dans ce cas, la réponse Hello du serveur pendant la négociation TLS n’inclut pas l’état d’agrafage OCSP par défaut. Ce comportement améliore les performances : L’implémentation de l’agrafage OCSP Windows s’adapte à des centaines de certificats de serveur. Étant donné que SNI et CCS permettent à IIS de s’adapter à des milliers de sites Web qui ont potentiellement des milliers de certificats de serveur, le fait de définir ce comportement comme étant activé par défaut peut entraîner des problèmes de performances.

Versions applicables : Toutes les versions à partir de Windows Server 2012 et Windows 8. 

Chemin d’accès au registre : [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL]

Ajoutez la clé suivante :

"EnableOcspStaplingForSni" = dword : 00000001

Pour désactiver, définissez la valeur DWORD sur 0 :

"EnableOcspStaplingForSni" = dword : 00000000

> [!NOTE] 
> L’activation de cette clé de registre a un impact potentiel sur les performances.

## <a name="fipsalgorithmpolicy"></a>FIPSAlgorithmPolicy

Cette entrée contrôle la conformité aux normes FIPS (Federal Information Processing Standard). La valeur par défaut est 0.

Versions applicables : Toutes les versions à partir de Windows Server 2012 et Windows 8. 

Chemin du Registre : HKLM SYSTEM\CurrentControlSet\Control\LSA

Suites de chiffrement FIPS de Windows Server : Consultez [les suites et les protocoles de chiffrement pris en charge dans le SSP Schannel](https://technet.microsoft.com/library/dn786419.aspx).

## <a name="hashes"></a>Hachages

Les algorithmes de hachage TLS/SSL doivent être contrôlés en configurant l’ordre de la suite de chiffrement. Pour plus d’informations, consultez Configuration de la [suite de chiffrement TLS](manage-tls.md#configuring-tls-cipher-suite-order) .

## <a name="issuercachesize"></a>IssuerCacheSize

Cette entrée contrôle la taille du cache de l’émetteur. Elle est utilisée avec le mappage de l’émetteur. Le SSP Schannel tente de mapper tous les émetteurs dans la chaîne de certificats du client, et non pas seulement l’émetteur direct du certificat client. Lorsque les émetteurs ne correspondent pas à un compte, ce qui est le cas par défaut, le serveur peut tenter de mapper le même nom de l’émetteur à plusieurs reprises, des centaines de fois par seconde. 

Pour éviter ce problème, le serveur a un cache négatif ; ainsi lorsqu’un nom d’émetteur ne correspond pas à un compte, il est ajouté au cache et le SSP Schannel ne tente pas de mapper le nom d’émetteur à nouveau tant que l’entrée de cache n’a pas expiré. Cette entrée de Registre spécifie la taille du cache. Par défaut, cette entrée n’existe pas dans le Registre. La valeur par défaut est 100. 

Versions applicables : Toutes les versions à partir de Windows Server 2008 et Windows Vista.

Chemin du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="issuercachetime"></a>IssuerCacheTime

Cette entrée contrôle l’intervalle de temps d’expiration du cache en millisecondes. Le SSP Schannel tente de mapper tous les émetteurs dans la chaîne de certificats du client, et non pas seulement l’émetteur direct du certificat client. Lorsque les émetteurs ne correspondent pas à un compte, ce qui est le cas par défaut, le serveur peut tenter de mapper le même nom d’émetteur à plusieurs reprises, des centaines de fois par seconde.

Pour éviter ce problème, le serveur a un cache négatif ; ainsi lorsqu’un nom d’émetteur ne correspond pas à un compte, il est ajouté au cache et le SSP Schannel ne tente pas de mapper le nom d’émetteur à nouveau tant que l’entrée de cache n’a pas expiré. Ce cache est conservé pour des raisons de performances, afin que le système ne continue pas de tente de mapper les mêmes émetteurs. Par défaut, cette entrée n’existe pas dans le Registre. La valeur par défaut est 10 minutes.

Versions applicables : Toutes les versions à partir de Windows Server 2008 et Windows Vista.

Chemin du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="keyexchangealgorithm---client-rsa-key-sizes"></a>KeyExchangeAlgorithm-tailles de clé RSA du client

Cette entrée contrôle les tailles de clé RSA du client. 

L’utilisation des algorithmes d’échange de clés doit être contrôlée en configurant l’ordre de la suite de chiffrement.

Ajouté dans Windows 10, version 1507 et Windows Server 2016.

Chemin du Registre : HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\PKCS

Pour spécifier une plage minimale prise en charge de la longueur en bits de clé RSA pour le client TLS, créez une entrée **ClientMinKeyBitLength** . Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD par la longueur de bit souhaitée. S’il n’est pas configuré, 1024 bits est le minimum. 

Pour spécifier une plage maximale prise en charge de la longueur en bits de clé RSA pour le client TLS, créez une entrée **ClientMaxKeyBitLength** . Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD par la longueur de bit souhaitée. S’il n’est pas configuré, le nombre maximal n’est pas appliqué.

## <a name="keyexchangealgorithm---diffie-hellman-key-sizes"></a>Tailles de clé de Diffie-Hellman KeyExchangeAlgorithm

Cette entrée contrôle les tailles de clé Diffie-Hellman. 

L’utilisation des algorithmes d’échange de clés doit être contrôlée en configurant l’ordre de la suite de chiffrement.

Ajouté dans Windows 10, version 1507 et Windows Server 2016.

Chemin du Registre : HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\KeyExchangeAlgorithms\Diffie-Hellman

Pour spécifier une plage de bits de clé Diffie-Helman minimale prise en charge pour le client TLS, créez une entrée **ClientMinKeyBitLength** . Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD par la longueur de bit souhaitée. S’il n’est pas configuré, 1024 bits est le minimum. 
 
Pour spécifier une plage maximale prise en charge de la longueur en bits de clé Diffie-Helman pour le client TLS, créez une entrée **ClientMaxKeyBitLength** . Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD par la longueur de bit souhaitée. S’il n’est pas configuré, le nombre maximal n’est pas appliqué. 
 
Pour spécifier la longueur en bits de la clé Diffie-Helman pour le serveur TLS par défaut, créez une entrée **ServerMinKeyBitLength** . Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD par la longueur de bit souhaitée. S’il n’est pas configuré, 2048 bits est la valeur par défaut. 

## <a name="maximumcachesize"></a>MaximumCacheSize

Cette entrée contrôle le nombre maximal d’éléments du cache. En définissant le nombre maximal d’éléments du cache sur 0, vous désactivez le cache de session côté serveur et empêchez les reconnexions. Lorsque la valeur de cette entrée est supérieure aux valeurs par défaut, Lsass.exe consomme plus de mémoire. Chaque élément du cache de session nécessite généralement 2 à 4 Ko de mémoire. Par défaut, cette entrée n’existe pas dans le Registre. La valeur par défaut est 20 000 éléments. 

Versions applicables : Toutes les versions à partir de Windows Server 2008 et Windows Vista.

Chemin du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="messaging--fragment-parsing"></a>Messagerie-analyse de fragments

________________________________________
Cette entrée contrôle la taille maximale autorisée des messages de négociation TLS fragmentés qui seront acceptés. Les messages dont la taille est supérieure à la taille autorisée ne sont pas acceptés et la négociation TLS échoue. Par défaut, ces entrées n’existent pas dans le registre. 

Lorsque vous définissez la valeur sur 0x0, les messages fragmentés ne sont pas traités et entraînent l’échec de la négociation TLS. Cela rend les clients TLS ou les serveurs sur l’ordinateur actuel non conformes aux RFC TLS. 

La taille maximale autorisée peut être augmentée jusqu’à 2 ^ 24-1 octets. Autoriser un client ou un serveur à lire et stocker de grandes quantités de données non vérifiées à partir du réseau n’est pas une bonne idée et consommera de la mémoire supplémentaire pour chaque contexte de sécurité. 

Ajouté dans Windows 7 et Windows Server 2008 R2.
Une mise à jour qui permet à Internet Explorer dans Windows XP, Windows Vista ou Windows Server 2008 d’analyser les messages de négociation TLS/SSL fragmentés est disponible.

Chemin du Registre : HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Messaging

Pour spécifier une taille maximale autorisée de messages de négociation TLS fragmentés que le client TLS acceptera, créez une entrée **MessageLimitClient** . Après avoir créé l’entrée, remplacez la valeur DWORD par la longueur de bit souhaitée. S’il n’est pas configuré, la valeur par défaut est 0x8000 octets. 

Pour spécifier une taille maximale autorisée de messages de négociation TLS fragmentés que le serveur TLS acceptera en l’absence d’authentification du client, créez une entrée **MessageLimitServer** . Après avoir créé l’entrée, remplacez la valeur DWORD par la longueur de bit souhaitée. S’il n’est pas configuré, la valeur par défaut est 0x4000 octets. 

Pour spécifier une taille maximale autorisée de messages de négociation TLS fragmentés que le serveur TLS acceptera en cas d’authentification du client, créez une entrée **MessageLimitServerClientAuth** . Après avoir créé l’entrée, remplacez la valeur DWORD par la longueur de bit souhaitée. S’il n’est pas configuré, la valeur par défaut est 0x8000 octets. 

## <a name="sendtrustedissuerlist"></a>SendTrustedIssuerList

Cette entrée contrôle l’indicateur qui est utilisé lors de l’envoi de la liste des émetteurs approuvés. Dans le cas des serveurs qui font confiance à des centaines d’autorités de certification pour l’authentification du client, il y a trop d’émetteurs pour que le serveur puisse tous les envoyer à l’ordinateur client lors de la demande d’authentification du client. Dans ce cas, cette clé de Registre peut être définie, et au lieu d’envoyer une liste partielle, le SSP Schannel n’envoie aucune liste au client.

Ne pas envoyer une liste d’émetteurs approuvés peut avoir un impact sur ce que le client envoie lorsqu’il reçoit une demande de certificat de client. Par exemple, lorsqu’Internet Explorer reçoit une demande d’authentification du client, il affiche uniquement les certificats du client qui sont liés à l’une des autorités de certification qui est envoyée par le serveur. Si le serveur n’a pas envoyé de liste, Internet Explorer affiche tous les certificats du client installés sur le client. 

Ce comportement peut être souhaitable. Par exemple, lorsque des environnements PKI incluent des certificats croisés, les certificats du client et du serveur n’ont pas la même autorité de certification racine ; par conséquent, Internet Explorer ne peut pas choisir un certificat qui est lié à l’une des autorités de certification du serveur. En configurant le serveur de manière à ce qu’il n’envoie pas de liste d’émetteurs approuvés, Internet Explorer envoie tous ses certificats.

Par défaut, cette entrée n’existe pas dans le Registre.

Comportement de la liste envoyer un émetteur approuvé par défaut

| Version Windows | Time |
|-----------------|------|
| Windows Server 2012 et Windows 8 et versions ultérieures | FALSE |
| Windows Server 2008 R2 et Windows 7 et versions antérieures | TRUE |

Versions applicables : Toutes les versions à partir de Windows Server 2008 et Windows Vista.

Chemin du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

## <a name="servercachetime"></a>ServerCacheTime

Cette entrée contrôle la durée en millisecondes nécessaire pour que le système d’exploitation fasse expirer les entrées de cache côté serveur. La valeur 0 désactive le cache de session côté serveur et empêche les reconnexions. Lorsque la valeur de cette entrée est supérieure aux valeurs par défaut, Lsass.exe consomme plus de mémoire. Chaque élément du cache de session nécessite généralement 2 à 4 Ko de mémoire. Par défaut, cette entrée n’existe pas dans le Registre. 

Versions applicables : Toutes les versions à partir de Windows Server 2008 et Windows Vista.

Chemin du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL

Heure du cache du serveur par défaut : 10 heures

## <a name="ssl-20"></a>SSL 2.0

Cette sous-clé contrôle l’utilisation de SSL 2,0.

À partir de Windows 10, version 1607 et Windows Server 2016, SSL 2,0 a été supprimé et n’est plus pris en charge.
Pour obtenir les paramètres par défaut SSL 2,0, consultez [protocoles dans TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Chemin du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour activer le protocole SSL 2,0, créez une entrée **activé** dans la sous-clé client ou serveur, comme décrit dans le tableau suivant. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD par 1. 

Table de sous-clé SSL 2,0

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de SSL 2,0 sur le client SSL. |
| Serveur | Contrôle l’utilisation de SSL 2,0 sur le serveur SSL. |

Pour désactiver SSL 2,0 pour le client ou le serveur, remplacez la valeur DWORD par 0. Si une application SSPI demande l’utilisation de SSL 2,0, elle est refusée. 

Pour désactiver SSL 2,0 par défaut, créez une entrée **DisabledByDefault** et remplacez la valeur DWORD par 1. Si une application SSPI explicitement les demandes d’utilisation du protocole SSL 2,0, elle peut être négociée. 

L’exemple suivant montre le chiffrement SSL 2,0 désactivé dans le registre :

![SSL 2,0 désactivé](images/ssl-2-registry-setting.png)


## <a name="ssl-30"></a>SSL 3.0

Cette sous-clé contrôle l’utilisation de SSL 3,0.

À partir de Windows 10, version 1607 et Windows Server 2016, SSL 3,0 a été désactivé par défaut. Pour les paramètres par défaut SSL 3,0, consultez [protocoles dans TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx). 

Chemin du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour activer le protocole SSL 3,0, créez une entrée **activé** dans la sous-clé client ou serveur, comme décrit dans le tableau suivant.  
Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD par 1. 

Table de sous-clé SSL 3,0

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de SSL 3,0 sur le client SSL. |
| Serveur | Contrôle l’utilisation de SSL 3,0 sur le serveur SSL. |

Pour désactiver SSL 3,0 pour le client ou le serveur, remplacez la valeur DWORD par 0.
Si une application SSPI demande l’utilisation de SSL 3,0, elle est refusée. 

Pour désactiver SSL 3,0 par défaut, créez une entrée **DisabledByDefault** et remplacez la valeur DWORD par 1. Si une application SSPI demande explicitement l’utilisation de SSL 3,0, elle peut être négociée. 

L’exemple suivant montre le chiffrement SSL 3,0 désactivé dans le registre :

![SSL 3,0 désactivé](images/ssl-3-registry-setting.png)

## <a name="tls-10"></a>TLS 1.0

Cette sous-clé contrôle l’utilisation de TLS 1,0.

Pour les paramètres par défaut TLS 1,0, consultez [protocoles dans TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Chemin du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour activer le protocole TLS 1,0, créez une entrée **activé** dans la sous-clé client ou serveur, comme décrit dans le tableau suivant. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD par 1. 

Table de sous-clé TLS 1,0

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de TLS 1,0 sur le client TLS. |
| Serveur | Contrôle l’utilisation de TLS 1,0 sur le serveur TLS. |

Pour désactiver TLS 1,0 pour le client ou le serveur, remplacez la valeur DWORD par 0.
Si une application SSPI demande l’utilisation de TLS 1,0, elle est refusée. 

Pour désactiver TLS 1,0 par défaut, créez une entrée **DisabledByDefault** et remplacez la valeur DWORD par 1. Si une application SSPI demande explicitement l’utilisation de TLS 1,0, elle peut être négociée. 

L’exemple suivant montre le protocole TLS 1,0 désactivé dans le registre :

![TLS 1,0 désactivé](images/tls-registry-setting.png)

## <a name="tls-11"></a>TLS 1.1

Cette sous-clé contrôle l’utilisation de TLS 1,1.

Pour les paramètres par défaut TLS 1,1, consultez [protocoles dans TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Chemin du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour activer le protocole TLS 1,1, créez une entrée **activé** dans la sous-clé client ou serveur, comme décrit dans le tableau suivant. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD par 1. 

Table de sous-clé TLS 1,1

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de TLS 1,1 sur le client TLS. |
| Serveur | Contrôle l’utilisation de TLS 1,1 sur le serveur TLS. |

Pour désactiver TLS 1,1 pour le client ou le serveur, remplacez la valeur DWORD par 0.
Si une application SSPI demande l’utilisation de TLS 1,1, elle est refusée. 

Pour désactiver TLS 1,1 par défaut, créez une entrée **DisabledByDefault** et remplacez la valeur DWORD par 1. Si une application SSPI demande explicitement l’utilisation de TLS 1,1, elle peut être négociée. 

L’exemple suivant montre le protocole TLS 1,1 désactivé dans le registre :

![TLS 1,1 désactivé](images/tls-11-registry-setting.png)

## <a name="tls-12"></a>TLS 1.2

Cette sous-clé contrôle l’utilisation de TLS 1,2.

Pour les paramètres par défaut TLS 1,2, consultez [protocoles dans TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Chemin du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour activer le protocole TLS 1,2, créez une entrée **activé** dans la sous-clé client ou serveur, comme décrit dans le tableau suivant. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD par 1. 

Table de sous-clé TLS 1,2

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de TLS 1,2 sur le client TLS. |
| Serveur | Contrôle l’utilisation de TLS 1,2 sur le serveur TLS. |

Pour désactiver TLS 1,2 pour le client ou le serveur, remplacez la valeur DWORD par 0.
Si une application SSPI demande l’utilisation de TLS 1,2, elle est refusée. 

Pour désactiver TLS 1,2 par défaut, créez une entrée **DisabledByDefault** et remplacez la valeur DWORD par 1. Si une application SSPI demande explicitement l’utilisation de TLS 1,2, elle peut être négociée. 

L’exemple suivant montre le protocole TLS 1,2 désactivé dans le registre :

![TLS 1,2 désactivé](images/tls-12-registry-setting.png)

## <a name="dtls-10"></a>DTLS 1.0

Cette sous-clé contrôle l’utilisation de DTLS 1,0.

Pour les paramètres par défaut de DTLS 1,0, consultez [protocoles dans TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Chemin du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour activer le protocole DTLS 1,0, créez une entrée **activé** dans la sous-clé client ou serveur, comme décrit dans le tableau suivant. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD par 1. 

Table de sous-clé DTLS 1,0

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de DTLS 1,0 sur le client DTLS. |
| Serveur | Contrôle l’utilisation de DTLS 1,0 sur le serveur DTLS. |

Pour désactiver DTLS 1,0 pour le client ou le serveur, remplacez la valeur DWORD par 0.
Si une application SSPI demande l’utilisation de DTLS 1,0, elle est refusée. 

Pour désactiver DTLS 1,0 par défaut, créez une entrée **DisabledByDefault** et remplacez la valeur DWORD par 1. Si une application SSPI demande explicitement l’utilisation de DTLS 1,0, elle peut être négociée. 

L’exemple suivant montre le DTLS 1,0 désactivé dans le registre :

![DTLS 1,0 désactivé](images/dtls-10-registry-setting.png)

## <a name="dtls-12"></a>DTLS 1,2

Cette sous-clé contrôle l’utilisation de DTLS 1,2.

Pour les paramètres par défaut de DTLS 1,2, consultez [protocoles dans TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159.aspx).

Chemin du Registre : HKLM SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols

Pour activer le protocole DTLS 1,2, créez une entrée **activé** dans la sous-clé client ou serveur, comme décrit dans le tableau suivant. Par défaut, cette entrée n’existe pas dans le Registre. Après avoir créé l’entrée, remplacez la valeur DWORD par 1. 

Table de sous-clé DTLS 1,2

| Sous-clé | Description |
|--------|-------------|
| Client | Contrôle l’utilisation de DTLS 1,2 sur le client DTLS. |
| Serveur | Contrôle l’utilisation de DTLS 1,2 sur le serveur DTLS. |


Pour désactiver DTLS 1,2 pour le client ou le serveur, remplacez la valeur DWORD par 0.
Si une application SSPI demande l’utilisation de DTLS 1,0, elle est refusée. 

Pour désactiver DTLS 1,2 par défaut, créez une entrée **DisabledByDefault** et remplacez la valeur DWORD par 1. Si une application SSPI demande explicitement l’utilisation de DTLS 1,2, elle peut être négociée. 

L’exemple suivant montre le DTLS 1,1 désactivé dans le registre :

![DTLS 1,1 désactivé](images/dtls-11-registry-setting.png)


