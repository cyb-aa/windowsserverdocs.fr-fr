---
title: Chiffrement du réseau virtuel
description: Chiffrement de réseau virtuel, le chiffrement du trafic de réseau virtuel entre des machines virtuelles qui communiquent entre eux au sein de sous-réseaux marqués comme « Chiffrement Enabled ».
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: f2f50ae3146854e2ef6081b0c400a474b53dcf66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851080"
---
# <a name="virtual-network-encryption"></a>Chiffrement du réseau virtuel

>S’applique à : Windows Server

Chiffrement de réseau virtuel, le chiffrement du trafic de réseau virtuel entre des machines virtuelles qui communiquent entre eux au sein de sous-réseaux marqués comme « Chiffrement Enabled ». Il utilise aussi le protocole DTLS (Datagram Transport Layer Security) sur le sous-réseau virtuel pour chiffrer les paquets. DTLS protège contre les écoutes clandestines, l'altération et la falsification par toute personne ayant accès au réseau physique.

Le chiffrement du réseau virtuel requiert :
- Certificats de chiffrement installés sur chacun des hôtes Hyper-V activé de SDN.
- Un objet d’informations d’identification dans le contrôleur de réseau faisant référence à l’empreinte numérique du certificat.
- Configuration sur chacun des réseaux virtuels contiennent des sous-réseaux qui exigent un chiffrement.

Une fois que vous activez le chiffrement sur un sous-réseau, tout le trafic réseau au sein de ce sous-réseau est chiffré automatiquement, en plus d’un chiffrement au niveau de l’application peut également avoir lieu.  Le trafic entre sous-réseaux, même si marquée sous forme chiffrée, est automatiquement envoyé sans être chiffré. Tout le trafic qui traverse la limite de réseau virtuel obtient envoyé sans être chiffré.

>[!TIP]
>Si vous devez limiter les applications de communiquer uniquement sur le sous-réseau chiffré, vous pouvez utiliser la listes de contrôle d’accès (ACL) uniquement pour permettre la communication au sein du sous-réseau en cours. Pour plus d’informations, consultez [utiliser Access Control Lists (ACL) pour le flux du trafic réseau centre de données gérer](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow).

### <a name="next-steps"></a>Étapes suivantes

[Configurer le chiffrement pour un réseau virtuel](https://docs.microsoft.com/windows-server/networking/sdn/vnet-encryption/sdn-config-vnet-encryption)

