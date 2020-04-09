---
title: Vue d’ensemble de TLS/SSL (SSP Schannel)
description: Sécurité de Windows Server
ms.prod: windows-server
ms.technology: security-tls-ssl
ms.topic: article
ms.assetid: 1b7b0432-1bef-4912-8c9a-8989d47a4da9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/16/2018
ms.openlocfilehash: e0045edf1d04feabac2354d47132b86e6d27ad2d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853622"
---
# <a name="tlsssl-overview-schannel-ssp"></a>Vue d’ensemble de TLS/SSL (SSP Schannel)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

Cette rubrique destinée aux professionnels de l’informatique présente les implémentations de TLS et SSL dans Windows à l’aide du fournisseur de services de sécurité (SSP) Schannel en décrivant des applications pratiques, les modifications apportées à l’implémentation de Microsoft et la configuration logicielle requise, ainsi que des ressources supplémentaires pour Windows Server 2012 et Windows 8.

## <a name="description"></a><a name="BKMK_OVER"></a>Descriptive
Schannel est un fournisseur de service de sécurité qui implémente les protocoles d’authentification standard Internet SSL (Secure Sockets Layer) et TLS (Transport Layer Security).

L’interface SSPI (Security Support Provider Interface) est une API utilisée par les systèmes Windows pour exécuter des fonctions relatives à la sécurité, dont notamment l’authentification. L’interface SSPI fonctionne comme une interface commune à plusieurs SSP, y compris le SSP Schannel.

Les versions TLS 1,0, 1,1 et 1,2, \(les versions SSL 2,0 et 3,0, ainsi que le protocole DTLS\) Protocol version 1,0 et le protocole de transport des communications privées \(le protocole PCT\) sont basés sur le chiffrement à clé publique. La suite de protocoles d’authentification Schannel fournit ces protocoles. Tous les protocoles Schannel utilisent un modèle client/serveur.

## <a name="applications"></a><a name="BKMK_APP"></a>Applications
Lorsque vous administrez un réseau, l’un des problèmes qui se posent concerne la sécurisation des données envoyées entre les applications via un réseau non approuvé. Vous pouvez utiliser TLS et SSL pour authentifier les serveurs et les ordinateurs clients, puis utiliser le protocole pour chiffrer les messages entre les parties authentifiées.

Par exemple, vous pouvez utiliser TLS/SSL pour :

-   les transactions sécurisées par SSL avec un site Web de commerce électronique ;
-   l’accès des clients authentifiés à un site Web sécurisé par SSL ;
-   Accès à distance
-   l’accès SQL ;
-   Message électronique

## <a name="requirements"></a><a name="BKMK_SOFT"></a>Exigences
Les protocoles TLS et SSL utilisent un modèle client/serveur et sont basés sur l’authentification par certificat, ce qui nécessite une infrastructure à clé publique.

## <a name="server-manager-information"></a><a name="BKMK_INSTALL"></a>Informations Gestionnaire de serveur
Aucune étape de configuration n’est nécessaire pour implémenter TLS, SSL ou Schannel.

## <a name="see-also"></a>Voir aussi ##

-   [Le package de sécurité Schannel](https://docs.microsoft.com/windows/desktop/com/schannel)
-   [Canal sécurisé](https://docs.microsoft.com/windows/desktop/SecAuthN/secure-channel)
-   [Protocole TLS (Transport Layer Security)](https://docs.microsoft.com/windows/desktop/SecAuthN/transport-layer-security-protocol)
