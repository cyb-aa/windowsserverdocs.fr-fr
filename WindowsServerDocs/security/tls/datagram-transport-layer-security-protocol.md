---
title: Protocole de la couche de transport de datagrammes
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57b8873a-ad9c-4f2c-93e0-a2af352c6965
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/16/2018
ms.openlocfilehash: b32ebafff5e41d5c3140f008f6f391852f2f3474
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870790"
---
# <a name="datagram-transport-layer-security-protocol"></a>Protocole de la couche de transport de datagrammes

Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

Cette rubrique de référence pour les professionnels de l’informatique décrit le protocole DTLS Datagram Transport Layer Security (), qui fait partie du fournisseur SSP (Security Support Provider) Schannel.

## <a name="BKMK_DTLS"></a>
Introduite dans le SSP Schannel dans Windows Server 2012 et Windows 8, le protocole DTLS assure la confidentialité de communication pour les protocoles de datagramme. Pour plus d’informations sur le protocole DTLS version est prise en charge dans les versions de Windows, consultez [protocoles dans TLS/SSL (Schannel SSP)](https://msdn.microsoft.com/en-us/library/windows/desktop/mt808159(v=vs.85).aspx). Le protocole permet aux applications client et serveur de communiquer d’une manière conçue pour empêcher toute tentative d’écoute clandestine, de falsification ou de contrefaçon de message. Le protocole DTLS s’appuie sur le protocole TLS (Transport Layer Security) et fournit des garanties de sécurité équivalentes, réduisant le besoin d’utiliser IPsec ou concevant un protocole personnalisé de sécurité de la couche Application.

Les datagrammes sont communs de diffusion en continu de médias, tels que des jeux ou sécurisée vidéoconférence. Les développeurs peuvent développer des applications pour utiliser le protocole DTLS dans le contexte du modèle de prise en charge Interface SSPI (Security Provider) de l’authentification Windows pour sécuriser la communication entre les clients et serveurs. Le protocole DTLS repose sur le protocole UDP (User Datagram). DTLS est conçu pour être aussi similaire au protocole TLS que possible pour réduire de nouveau en termes de sécurité et pour optimiser la quantité de réutilisation de code et l’infrastructure.

Les suites de chiffrement qui sont disponibles pour la configuration sont modélisés d’après ceux que vous pouvez configurer pour TLS. RC4 n’est pas autorisée. Schannel continue à utiliser l’infrastructure de Diagnostics Windows (CNG, Cryptography Next Generation). Il tire parti de la certification FIPS 140, ce qui a été introduite dans Windows Vista.

## <a name="see-also"></a>Voir aussi

[IETF RFC 4347 Datagram Transport Layer Security](http://tools.ietf.org/html/rfc4347)


