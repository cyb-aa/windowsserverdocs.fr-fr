---
title: "TLS - vue d’ensemble du protocole SSL (SSP Schannel)"
description: "Sécurité de Windows Server"
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
ms.date: 10/12/2016
ms.openlocfilehash: afd0b70264dba1e720f95e40d3d201c2c5bf1c64
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="tls---ssl-schannel-ssp-overview"></a>TLS - vue d’ensemble du protocole SSL (SSP Schannel)

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique destinée aux professionnels de l’informatique présente l’implémentation de TLS/SSL dans Windows en utilisant le fournisseur SSP (Security Service Provider) Schannel en décrivant des applications pratiques, les modifications dans l’implémentation par Microsoft et configuration logicielle requise, ainsi que des ressources supplémentaires pour Windows Server2012 et Windows8.

**Vouliez-vous dire:**

-   [Le package de sécurité Schannel](https://msdn.microsoft.com/library/ms678421.aspx)

-   [Canal sécurisé](https://msdn.microsoft.com/library/windows/desktop/aa380123.aspx)

-   [Protocole de sécurité de couche de transport](https://msdn.microsoft.com/library/windows/desktop/aa380516.aspx)

## <a name="BKMK_OVER"></a>Description de \(Schannel\) TLS/SSL
Schannel est un \(SSP\) Security Support Provider qui implémente le \(SSL\) Secure Sockets Layer et Transport Layer Security \(TLS\) protocoles d’authentification standard Internet.

Le \(SSPI\) Security Support Provider Interface est une API utilisée par les systèmes Windows pour effectuer des fonctions sécurité\, y compris l’authentification. L’interface SSPI fonctionne comme une interface commune à plusieurs \(SSPs\) fournisseurs de sécurité, y compris le SSP. Schannel

Les versions du protocole de Transport Layer Security \(TLS\) 1.0, 1.1 et 1.2, protocole \(SSL\) protocole SSL (Secure Sockets Layer) versions 2.0 et 3.0, \(DTLS\) Datagram Transport Layer Security version1.0 et Private Communications Transport \(PCT\) protocole sont basées sur le chiffrement à clé publique. La suite de protocoles d’authentification de canal de sécurité \(Schannel\) fournit ces protocoles. Tous les protocoles Schannel utilisent un modèle client/serveur.

## <a name="BKMK_APP"></a>Applications pratiques
Un problème lorsque vous administrez un réseau concerne la sécurisation des données envoyées entre les applications via un réseau non approuvé. Vous pouvez utiliser TLS/SSL pour authentifier les serveurs et les ordinateurs clients, puis utilisez le protocole pour crypter les messages entre les parties authentifiées.

Par exemple, vous pouvez utiliser TLS/SSL pour:

-   Transactions sécurisées par SSL\ avec un site Web de commerce e\

-   Accès des clients authentifiés à un site Web sécurisé à l’aide de SSL\

-   Accès à distance

-   Accès à SQL

-   Messagerie E\

## <a name="BKMK_SOFT"></a>Configuration logicielle requise
Le protocole TLS/SSL utilisent un modèle client\server et sont basées sur l’authentification par certificat, ce qui nécessite une Infrastructure à clé publique.

## <a name="BKMK_INSTALL"></a>Informations du Gestionnaire de serveur
Aucune étape de configuration ne sont nécessaires pour implémenter TLS, SSL ou Schannel.

