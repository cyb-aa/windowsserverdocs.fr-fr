---
title: Vue d’ensemble TLS/SSL (SSP Schannel)
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b7b0432-1bef-4912-8c9a-8989d47a4da9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/16/2018
ms.openlocfilehash: a6571e5e06e07fd62ad4cf39bab322b45c90a9f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848600"
---
# <a name="tlsssl-overview-schannel-ssp"></a>Vue d’ensemble TLS/SSL (SSP Schannel)

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

Cette rubrique destinée aux professionnels de l’informatique présente les implémentations SSL et TLS dans Windows à l’aide du fournisseur de Service de sécurité (SSP) Schannel en décrivant des applications pratiques, des modifications dans l’implémentation de Microsoft et la configuration logicielle requise, ainsi que ressources supplémentaires pour Windows Server 2012 et Windows 8.

## <a name="BKMK_OVER"></a>Description
Schannel est un fournisseur de service de sécurité qui implémente les protocoles d’authentification standard Internet SSL (Secure Sockets Layer) et TLS (Transport Layer Security).

L’interface SSPI (Security Support Provider Interface) est une API utilisée par les systèmes Windows pour exécuter des fonctions relatives à la sécurité, dont notamment l’authentification. L’interface SSPI fonctionne comme une interface commune à plusieurs fournisseurs de services partagés, y compris le SSP. Schannel

TLS versions 1.0, 1.1 et 1.2, les versions SSL 2.0 et 3.0, ainsi que la sécurité de couche de Transport de datagramme \(DTLS\) protocol version 1.0 et le Transport de Communications privées \(PCT\) protocole reposent sur chiffrement à clé publique. La suite de protocoles d’authentification Schannel fournit ces protocoles. Tous les protocoles Schannel utilisent un modèle client/serveur.

## <a name="BKMK_APP"></a>Applications
Lorsque vous administrez un réseau, l’un des problèmes qui se posent concerne la sécurisation des données envoyées entre les applications via un réseau non approuvé. Vous pouvez utiliser les protocoles TLS et SSL pour authentifier les serveurs et les ordinateurs clients, puis utiliser le protocole pour chiffrer les messages entre les parties authentifiées.

Par exemple, vous pouvez utiliser TLS/SSL pour :

-   les transactions sécurisées par SSL avec un site Web de commerce électronique ;
-   l’accès des clients authentifiés à un site Web sécurisé par SSL ;
-   Accès à distance
-   l’accès SQL ;
-   Courrier électronique

## <a name="BKMK_SOFT"></a>Configuration requise
Les protocoles TLS et SSL utilisent un modèle client/serveur et sont basées sur l’authentification par certificat, ce qui nécessite une infrastructure à clé publique.

## <a name="BKMK_INSTALL"></a>Informations de gestionnaire de serveur
Aucune étape de configuration ne sont nécessaires pour implémenter TLS, SSL ou Schannel.

## <a name="see-also"></a>Voir aussi ##

-   [Le package de sécurité Schannel](https://docs.microsoft.com/windows/desktop/com/schannel)
-   [Canal sécurisé](https://docs.microsoft.com/windows/desktop/SecAuthN/secure-channel)
-   [Protocole de sécurité de couche de transport](https://docs.microsoft.com/windows/desktop/SecAuthN/transport-layer-security-protocol)
