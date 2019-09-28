---
title: Protocole TLS (Transport Layer Security)
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: aca2db3ae5bf424dd0f855d24c1ef771039c8b14
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403379"
---
# <a name="transport-layer-security-protocol"></a>Protocole TLS (Transport Layer Security)

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

Cette rubrique destinée aux professionnels de l’informatique décrit le fonctionnement du protocole TLS (Transport Layer Security) et fournit des liens vers les RFC IETF pour TLS 1,0, TLS 1,1 et TLS 1,2.

Les protocoles TLS (et SSL) sont situés entre la couche du protocole d’application et la couche TCP/IP, où ils peuvent sécuriser et envoyer des données d’application à la couche de transport. Étant donné que les protocoles fonctionnent entre la couche application et la couche transport, les protocoles TLS et SSL peuvent prendre en charge plusieurs protocoles de la couche application.

Les protocoles TLS et SSL partent du principe qu’un transport orienté connexion, en général, est en cours d’utilisation. Le protocole permet aux applications client et serveur de détecter les risques de sécurité suivants :

-   Falsification des messages

-   Interception des messages

-   Falsification de message

Les protocoles TLS et SSL peuvent être divisés en deux couches. La première couche se compose du protocole d’application et des trois protocoles de négociation : le protocole de négociation, le protocole de spécification de chiffrement de modification et le protocole d’alerte. La deuxième couche est le protocole d’enregistrement. L’illustration suivante montre les différentes couches et leurs éléments.

**Couches de protocole TLS et SSL**


Le SSP Schannel implémente les protocoles TLS et SSL sans modification. Le protocole SSL est propriétaire, mais l’IETF (Internet Engineering Task Force) produit les spécifications TLS publiques. Pour plus d’informations sur la version TLS ou SSL prise en charge dans les versions de Windows, consultez [protocoles dans TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx). Le tableau suivant répertorie les spécifications de chaque version de TLS. Chaque spécification contient des informations sur les éléments suivants :

-   Protocole d’enregistrement TLS

-   Les protocoles de négociation TLS : \- modifier le protocole d’alerte \- du protocole de spécification du chiffrement

-   Calculs de chiffrement

-   Suites de chiffrement obligatoires

-   Protocole de données d’application

[RFC 5246-le protocole TLS (Transport Layer Security) version 1,2](http://tools.ietf.org/html/rfc5246)

[RFC 4346-le protocole TLS (Transport Layer Security) version 1,1](http://tools.ietf.org/html/rfc4346)

[RFC 2246-le protocole TLS version 1,0](http://tools.ietf.org/html/rfc2246)

## <a name="BKMK_SessionResumption"></a>Reprise de session TLS
Introduite dans Windows Server 2012 R2, le SSP Schannel a implémenté la partie côté serveur de la reprise de session TLS. L’implémentation côté client de RFC 5077 a été ajoutée dans Windows 8.

Les périphériques qui connectent fréquemment TLS aux serveurs doivent se reconnecter. La reprise de session TLS réduit le coût d’établissement des connexions TLS, car la reprise implique une négociation TLS abrégée. Cela facilite les tentatives de reprise en permettant à un groupe de serveurs TLS de reprendre les sessions TLS de l’autre. Cette modification offre les économies suivantes pour tout client TLS qui prend en charge RFC 5077, y compris les appareils Windows Phone et Windows RT :

-   Utilisation des ressources sur le serveur réduite

-   Bande passante réduite, ce qui améliore l’efficacité des connexions client

-   Réduction du temps consacré à la négociation TLS en raison de la reprise de la connexion

Pour plus d’informations sur la reprise de session TLS sans état, consultez le document IETF [RFC 5077](http://www.ietf.org/rfc/rfc5077).

## <a name="BKMK_AppProtocolNego"></a>Négociation de protocole d’application
 Windows Server 2012 R2 et Windows 8.1 introduit la prise en charge qui autorise la négociation du protocole d’application TLS côté client. Les applications peuvent tirer parti des protocoles dans le cadre du développement HTTP 2,0 standard, et les utilisateurs peuvent accéder aux services en ligne tels que Google et Twitter à l’aide d’applications exécutant le protocole SPDY.

Pour plus d’informations sur le fonctionnement de la négociation de protocole d’application, consultez la rubrique [Extension de la négociation de protocole à la couche d’application Transport Layer Security (TLS)](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05).

## <a name="BKMK_SNI"></a>Prise en charge TLS pour les extensions de Indication du nom du serveur
La fonctionnalité d’indication du nom de serveur étend les protocoles SSL et TLS pour permettre l’identification correcte du serveur lorsque de nombreuses images virtuelles s’exécutent sur un serveur unique. Dans un scénario d’hébergement virtuel, plusieurs domaines (chacun disposant de son propre certificat potentiellement distinct) sont hébergés sur un serveur. Dans ce cas, le serveur n’a aucun moyen de savoir à l’avance quel certificat envoyer au client. SNI permet au client d’informer le domaine cible plus tôt dans le protocole et cela permet au serveur de sélectionner correctement le certificat approprié.

Ces fonctionnalités supplémentaires :

-   Vous permet d’héberger plusieurs sites Web SSL sur une combinaison de protocole et de port Internet unique

-   réduisent l’utilisation de la mémoire lorsque plusieurs sites Web SSL sont hébergés sur un serveur Web unique ;

-   Permet à d’autres utilisateurs de se connecter simultanément aux sites Web SSL



