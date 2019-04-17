---
title: Protocole Datagram Transport Layer Security
description: "Sécurité de Windows Server"
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
ms.date: 10/12/2016
ms.openlocfilehash: 31c1cf1f3218c0511a4407b560be30d0c6f86233
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# Protocole Datagram Transport Layer Security

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique de référence destinée aux professionnels de l’informatique décrit le protocole DTLS Datagram Transport Layer Security (), qui fait partie du fournisseur SSP (Security Support Provider) Schannel.

## <a name="BKMK_DTLS"></a>
Introduite dans le SSP Schannel dans Windows Server2012 et Windows8, le protocole DTLS assure la confidentialité des communications pour les protocoles de datagrammes. Pour plus d’informations sur le protocole DTLS version est prise en charge dans les versions de Windows, voir [protocoles de TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/en-us/library/windows/desktop/mt808159(v=vs.85).aspx). Le protocole permet aux applications serveur et client de communiquer d’une manière qui est conçue pour empêcher l’écoute clandestine, de falsification ou de contrefaçon de message. Le protocole DTLS est basé sur le protocole de sécurité TLS (Transport Layer), et il fournit des garanties de sécurité équivalentes, réduisant le besoin d’utiliser IPsec ou concevant un protocole de sécurité de couche application personnalisée.

Les datagrammes sont communs dans en continu, telles que des jeux ou sécurisé vidéoconférence. Les développeurs peuvent développer des applications à utiliser le protocole DTLS dans le contexte du modèle Interface SSPI (Security Support Provider Interface) de l’authentification Windows pour sécuriser les communications entre les clients et serveurs. Le protocole DTLS s’appuie sur le protocole UDP (User Datagram). DTLS est conçu pour être aussi similaire au protocole TLS en tant que possible afin de réduire les nouveaux termes de sécurité et d’optimiser la quantité de la réutilisation du code et d’infrastructure.

Les suites de chiffrement qui sont disponibles pour la configuration sont répétées après ceux que vous pouvez configurer pour TLS. RC4 n’est pas autorisée. Schannel continue d’utiliser génération CNG (Cryptography Next). Il tire parti de la certification FIPS 140, ce qui a été introduite dans WindowsVista.

## Voir aussi

[IETF RFC4347 Datagram Transport Layer Security](http://tools.ietf.org/html/rfc4347)


