---
title: Protocole TLS (Transport Layer Security)
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de510bb0-a9f6-4bbe-8f8a-8dd7473bbae8
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 05/16/2018
ms.openlocfilehash: 0a3b241fe0d2a61361d551b7f515507ad55d71cd
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284239"
---
# <a name="transport-layer-security-protocol"></a>Protocole TLS (Transport Layer Security)

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

Cette rubrique destinée aux professionnels de l’informatique décrit comment le protocole de sécurité TLS (Transport Layer) fonctionne et fournit des liens vers les RFC IETF pour TLS 1.0, TLS 1.1 et TLS 1.2.

Les protocoles (SSL et TLS) sont situés entre la couche de protocole d’application et de la couche TCP/IP, où ils peuvent sécuriser et envoyer des données d’application à la couche de transport. Étant donné que les protocoles fonctionnent entre la couche application et la couche de transport, TLS et SSL peuvent prendre en charge plusieurs protocoles de couche application.

TLS et SSL supposent qu’un transport orienté connexion, en général TCP, est en cours d’utilisation. Le protocole permet aux applications clientes et serveur détecter les risques de sécurité suivants :

-   Toute falsification du message

-   Interception de messages

-   Contrefaçon de message

Les protocoles TLS et SSL peuvent être divisés en deux couches. La première couche se compose de protocole d’application et les trois protocoles de négociation : le protocole de négociation, le changement cipher spec protocole et le protocole d’alerte. Le second niveau est le protocole d’enregistrement. L’image suivante illustre les différentes couches et leurs éléments.

**Couches de protocole TLS et SSL**


Le SSP Schannel implémente les protocoles TLS et SSL sans modification. Le protocole SSL est propriétaire, mais la Internet Engineering Task Force génère les spécifications de TLS publiques. Pour plus d’informations sur les SSL ou TLS version est prise en charge dans les versions de Windows, consultez [protocoles dans TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx). Le tableau suivant répertorie les spécifications pour chaque version TLS. Chaque spécification contient des informations sur :

-   Le protocole d’enregistrement TLS

-   Les protocoles de négociation TLS : \- Modifier le protocole spec chiffrement \- protocole d’alerte

-   Calculs de chiffrement

-   Suites de chiffrement obligatoire

-   Protocole de données d’application

[RFC 5246 : la Version du protocole Transport Layer Security (TLS) 1.2](http://tools.ietf.org/html/rfc5246)

[RFC 4346 : la Version du protocole Transport Layer Security (TLS) 1.1](http://tools.ietf.org/html/rfc4346)

[RFC 2246 : la Version du protocole TLS 1.0](http://tools.ietf.org/html/rfc2246)

## <a name="BKMK_SessionResumption"></a>Reprise de session TLS
Introduite dans Windows Server 2012 R2, le SSP Schannel implémenté la partie côté serveur de reprise de session TLS. L’implémentation côté client de RFC 5077 a été ajoutée dans Windows 8.

Les périphériques qui connectent fréquemment TLS aux serveurs doivent se reconnecter. Reprise de session TLS réduit le coût de l’établissement des connexions TLS car une reprise implique une négociation TLS abrégée. Cela facilite les autres tentatives de reprise en permettant à un groupe de serveurs TLS de reprendre les sessions TLS entre eux. Cette modification apporte les économies suivantes pour n’importe quel client TLS qui prend en charge RFC 5077, y compris les appareils Windows Phone et Windows RT :

-   Utilisation des ressources sur le serveur réduite

-   Bande passante réduite, ce qui améliore l’efficacité des connexions client

-   Réduit le temps passé pour les négociations TLS de la connexion

Pour plus d’informations sur la reprise de session TLS sans état, consultez le document IETF [RFC 5077](http://www.ietf.org/rfc/rfc5077).

## <a name="BKMK_AppProtocolNego"></a>Négociation de protocole d’application
 Windows Server 2012 R2 et Windows 8.1 introduit la prise en charge qui permet la négociation de protocole d’application TLS côté client. Les applications puissent exploiter les protocoles dans le cadre du développement standard HTTP 2.0 et accessibles aux utilisateurs des services en ligne tels que Google et Twitter à l’aide d’applications qui s’exécutent le protocole SPDY.

Pour plus d’informations sur le fonctionnement de la négociation de protocole d’application, consultez la rubrique [Extension de la négociation de protocole à la couche d’application Transport Layer Security (TLS)](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05).

## <a name="BKMK_SNI"></a>Prise en charge TLS pour les extensions d’Indication du nom du serveur
La fonctionnalité d’indication du nom de serveur étend les protocoles SSL et TLS pour permettre l’identification correcte du serveur lorsque de nombreuses images virtuelles s’exécutent sur un serveur unique. Dans un scénario d’hébergement virtuel, plusieurs domaines (chacun avec son propre certificat potentiellement distinct) sont hébergés sur un seul serveur. Dans ce cas, le serveur n’a aucun moyen de savoir au préalable quel certificat pour envoyer au client. SNI permet au client d’informer le domaine cible plus tôt dans le protocole, et cela permet au serveur sélectionner correctement le certificat approprié.

Ces fonctionnalités supplémentaires :

-   Vous permet d’héberger plusieurs sites Web SSL sur une combinaison de port et le protocole Internet unique

-   réduisent l’utilisation de la mémoire lorsque plusieurs sites Web SSL sont hébergés sur un serveur Web unique ;

-   Permet aux utilisateurs de plus à se connecter simultanément à des sites Web SSL



