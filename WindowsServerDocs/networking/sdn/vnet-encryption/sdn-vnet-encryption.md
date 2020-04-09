---
title: Chiffrement de réseau virtuel
description: Le chiffrement de réseau virtuel permet le chiffrement du trafic réseau virtuel entre les machines virtuelles qui communiquent entre elles au sein des sous-réseaux marqués comme « chiffrement activé ».
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/08/2018
ms.openlocfilehash: 63daea02ec00593504383ce071d3f9454a37956b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853572"
---
# <a name="virtual-network-encryption"></a>Chiffrement de réseau virtuel

>S’applique à : Windows Server

Le chiffrement de réseau virtuel permet le chiffrement du trafic réseau virtuel entre les machines virtuelles qui communiquent entre elles au sein des sous-réseaux marqués comme « chiffrement activé ». Il utilise aussi le protocole DTLS (Datagram Transport Layer Security) sur le sous-réseau virtuel pour chiffrer les paquets. DTLS protège contre les écoutes clandestines, l'altération et la falsification par toute personne ayant accès au réseau physique.

Le chiffrement de réseau virtuel requiert :
- Certificats de chiffrement installés sur chacun des hôtes Hyper-V compatibles SDN.
- Objet d’informations d’identification dans le contrôleur de réseau qui référence l’empreinte numérique de ce certificat.
- La configuration sur chaque réseau virtuel contient des sous-réseaux qui nécessitent un chiffrement.

Une fois que vous activez le chiffrement sur un sous-réseau, tout le trafic réseau au sein de ce sous-réseau est automatiquement chiffré, en plus du chiffrement au niveau de l’application qui peut également avoir lieu.  Le trafic qui traverse des sous-réseaux, même s’il est marqué comme étant chiffré, est envoyé automatiquement non chiffré. Tout trafic qui traverse la limite du réseau virtuel est également envoyé non chiffré.

>[!TIP]
>Si vous devez limiter les applications pour qu’elles communiquent uniquement sur le sous-réseau chiffré, vous pouvez utiliser des listes de Access Control (ACL) uniquement pour autoriser la communication au sein du sous-réseau actuel. Pour plus d’informations, consultez [utiliser des listes de Access Control (ACL) pour gérer le flux de trafic réseau du centre de](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow)données.

### <a name="next-steps"></a>Étapes suivantes :

[Configurer le chiffrement pour un réseau virtuel](https://docs.microsoft.com/windows-server/networking/sdn/vnet-encryption/sdn-config-vnet-encryption)

