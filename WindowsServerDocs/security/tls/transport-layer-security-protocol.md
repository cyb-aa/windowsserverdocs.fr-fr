---
title: "Protocole de sécurité de la couche de transport"
description: "Sécurité de Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de510bb0-a9f6-4bbe-8f8a-8dd7473bbae8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 94c81a79e5f3fd8fc22eafde3de8bd3d0bdd73f0
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# Protocole de sécurité de la couche de transport

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique destinée aux professionnels de l’informatique décrit comment le protocole de sécurité TLS (Transport Layer) fonctionne et fournit des liens vers les normes RFC IETF pour TLS 1.0, TLS 1.1 et TLS 1.2.

Les protocoles (SSL et TLS) sont situés entre la couche de protocole d’application et la couche TCP/IP, où ils peuvent sécuriser et envoyer des données d’application à la couche de transport. Étant donné que les protocoles fonctionnent entre la couche d’application et la couche de transport, TLS et SSL peuvent prendre en charge plusieurs protocoles de la couche application.

TLS et SSL partons du principe qu’un transport orientée connexion, en général TCP, est en cours d’utilisation. Le protocole permet aux applications serveur et client détecter les risques de sécurité suivants:

-   Falsification des messages

-   Interception des messages

-   Contrefaçon de message

Les protocoles TLS et SSL peuvent être divisés en deux couches. La première couche comprend le protocole d’application et les protocoles de négociation trois: le protocole de négociation du protocole de spécification de chiffrement modification et le protocole d’alerte. Le second niveau est le protocole d’enregistrement. L’image suivante illustre les différentes couches et leurs éléments.

**Couches du protocole TLS et SSL**


Le SSP Schannel implémente les protocoles TLS et SSL sans modification. Le protocole SSL est propriétaire, mais la Internet Engineering Task Force produit les spécifications du protocole TLS publiques. Pour plus d’informations sur les SSL ou TLS version est prise en charge dans les versions de Windows, voir [protocoles de TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/en-us/library/windows/desktop/mt808159(v=vs.85).aspx). Le tableau suivant répertorie les spécifications pour chaque version TLS. Chaque spécification contient des informations sur:

-   Le protocole d’enregistrement TLS

-   Les protocoles de négociation TLS: \-protocole spécification du chiffrement modification \-protocole d’alerte

-   Calculs de chiffrement

-   Suites de chiffrement obligatoires

-   Protocole de données d’application

[RFC5246: la Version du protocole Transport Layer Security (TLS) 1.2](http://tools.ietf.org/html/rfc5246)

[RFC4346: la Version du protocole Transport Layer Security (TLS) 1.1](http://tools.ietf.org/html/rfc4346)

[RFC2246: la Version du protocole TLS 1.0](http://tools.ietf.org/html/rfc2246)

## <a name="BKMK_SessionResumption"></a>Reprise de session TLS
Introduite dans Windows Server2012R2, le SSP Schannel implémenté la partie côté serveur de reprise de session TLS. L’implémentation côté client de RFC 5077 a été ajoutée dans Windows 8.

Les périphériques qui connectent fréquemment TLS aux serveurs doivent se reconnecter. Reprise de session TLS réduit le coût d’établissement des connexions TLS car une reprise implique une négociation TLS abrégée. Cela facilite plusieurs tentatives de reprise en permettant à un groupe de serveurs TLS pour reprendre les sessions TLS à l’autre. Cette modification fournit le gain suivant pour n’importe quel client TLS qui prend en charge RFC5077, y compris les appareils Windows Phone et WindowsRT:

-   Utilisation des ressources réduites sur le serveur

-   Bande passante réduite, ce qui améliore l’efficacité des connexions client

-   Réduit le temps passé pour les négociations TLS de la connexion

Pour plus d’informations sur la reprise de session TLS sans état, consultez le document IETF [RFC 5077.](http://www.ietf.org/rfc/rfc5077)

## <a name="BKMK_AppProtocolNego"></a>Négociation de protocole d’application
 Windows Server2012R2 et Windows8.1 introduit la prise en charge qui permet la négociation de protocole d’application TLS côté client. Les applications puissent exploiter les protocoles dans le cadre du développement standard HTTP 2.0 et les utilisateurs peuvent accéder à des services en ligne comme Google et Twitter à l’aide des applications qui s’exécutent le protocole SPDY.

Pour plus d’informations sur le fonctionne de la négociation de protocole d’application, consultez [Extension de négociation de protocole de couche d’Application Transport Layer Security (TLS)](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05).

## <a name="BKMK_SNI"></a>Prise en charge TLS pour les extensions d’Indication du nom du serveur
La fonctionnalité d’Indication de nom de serveur (SNI) étend les protocoles SSL et TLS pour permettre l’identification correcte du serveur lorsque de nombreuses images virtuelles s’exécutent sur un serveur unique. Dans un scénario d’hébergement virtuel, plusieurs domaines (chacun avec son propre certificat potentiellement distinct) sont hébergés sur un seul serveur. Dans ce cas, le serveur n’a aucun moyen de savoir au préalable quel certificat pour envoyer au client. La fonctionnalité SNI permet au client d’informer le domaine cible plus tôt dans le protocole et cela permet au serveur de sélectionner correctement le certificat approprié.

Cette fonctionnalité supplémentaire:

-   Vous permet d’héberger plusieurs sites Web SSL sur un seul protocole Internet et la combinaison de port

-   Réduit l’utilisation de la mémoire lorsque plusieurs sites Web SSL sont hébergés sur un serveur web unique

-   Permet à plusieurs utilisateurs de se connecter simultanément à des sites Web SSL



