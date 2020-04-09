---
title: Protocole de la couche de transport de datagrammes
description: Sécurité de Windows Server
ms.prod: windows-server
ms.technology: security-tls-ssl
ms.topic: article
ms.assetid: 57b8873a-ad9c-4f2c-93e0-a2af352c6965
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/16/2018
ms.openlocfilehash: d0c066b063cbfc8def54c2e0d02cbb0eaf7f1d40
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852922"
---
# <a name="datagram-transport-layer-security-protocol"></a>Protocole de la couche de transport de datagrammes

Windows Server (Canal semi-annuel), Windows Server 2016, Windows 10

Cette rubrique de référence destinée aux professionnels de l’informatique décrit le protocole DTLS (Datagram Transport Layer Security), qui fait partie du fournisseur SSP (Security Support Provider) Schannel.

## <a name="BKMK_DTLS"></a>
Introduite dans le SSP Schannel dans Windows Server 2012 et Windows 8, le protocole DTLS fournit la confidentialité des communications pour les protocoles de datagramme. Pour plus d’informations sur la version de DTLS prise en charge dans les versions de Windows, consultez [protocoles dans TLS/SSL (SSP Schannel)](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx). Le protocole permet aux applications client et serveur de communiquer d’une manière conçue pour empêcher toute tentative d’écoute clandestine, de falsification ou de contrefaçon de message. Le protocole DTLS s’appuie sur le protocole TLS (Transport Layer Security) et fournit des garanties de sécurité équivalentes, réduisant le besoin d’utiliser IPsec ou concevant un protocole personnalisé de sécurité de la couche Application.

Les datagrammes sont courants en diffusion multimédia en continu, comme les jeux ou la vidéoconférence sécurisée. Les développeurs peuvent développer des applications pour utiliser le protocole DTLS dans le contexte du modèle SSPI (Authentication Security Support Provider Interface) de Windows pour sécuriser la communication entre les clients et les serveurs. Le protocole DTLS est basé sur le protocole UDP (User Datagram Protocol). DTLS est conçu pour être aussi similaire au TLS que possible pour réduire l’invention de la sécurité et maximiser la quantité de code et la réutilisation de l’infrastructure.

Les suites de chiffrement disponibles pour la configuration sont répétées après celles que vous pouvez configurer pour TLS. RC4 n’est pas autorisé. Schannel continue à utiliser CNG (Cryptography Next Generation). Cela tire parti de la certification FIPS 140, qui a été introduite dans Windows Vista.

## <a name="see-also"></a>Voir aussi

[IETF RFC 4347, datagramme Transport Layer Security](http://tools.ietf.org/html/rfc4347)


                                        