---
title: Paramètres de sécurité de machine virtuelle de génération 1 pour Hyper-V
description: Décrit les paramètres de sécurité disponibles dans le Gestionnaire Hyper-V pour les machines virtuelles de génération 1
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f8f8c569-8b74-4c19-876e-1c7d00cce308
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 73cc2e45367d448aa736644e4a3bc02d3670fc6c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447910"
---
# <a name="generation-1-virtual-machine-security-settings"></a>Paramètres de sécurité de machine virtuelle de génération 1

>S'applique à : Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Utilisez les paramètres de sécurité de machine virtuelle de génération 1 dans le Gestionnaire Hyper-V pour aider à protéger les données et l’état d’une machine virtuelle.

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Paramètres de prise en charge de chiffrement dans le Gestionnaire Hyper-V

Vous pouvez protéger les données et l’état de la machine virtuelle en sélectionnant l’option de prise en charge de chiffrement suivantes.

- **Chiffrer le trafic de migration d’état et de la machine virtuelle** -chiffre l’état enregistré de l’ordinateur virtuel lorsqu’elle est écrite sur disque et le trafic de migration en direct.

Pour activer cette option, vous devez ajouter un lecteur de stockage de clés pour la machine virtuelle.

## <a name="key-storage-drive-in-hyper-v-manager"></a>Lecteur de stockage de clés dans le Gestionnaire Hyper-V

Un lecteur de stockage de clés fournit un lecteur de petite à la machine virtuelle pour une clé BitLocker à stocker. Ainsi, la machine virtuelle chiffrer le disque de système d’exploitation sans nécessiter une puce de Module de plateforme sécurisée (TPM) virtualisé. Le contenu du lecteur de stockage de clés est chiffré à l’aide d’un protecteur de clé. L’hôte de protecteur de clé authories Hyper-V pour exécuter la machine virtuelle. Les deux le contenu du lecteur de stockage de clés et le protecteur de clé est stocké dans le cadre de l’état d’exécution de la machine virtuelle.

Pour déchiffrer le contenu du lecteur de stockage de clés et démarrer l’ordinateur virtuel, l’hôte Hyper-V doit être soit :

- Partie d’une infrastructure protégée autorisée pour cette machine virtuelle, ou
- A la clé privée à partir d’un des gardiens de la machine virtuelle.

Pour en savoir plus sur les infrastructures protégées, consultez la section Présentation de machines virtuelles protégées dans [sécurité et Assurance](../../../security/Security-and-Assurance.md).

Vous pouvez ajouter un lecteur de stockage de clés vers un emplacement vide sur l’un des contrôleurs IDE de l’ordinateur virtuel. Pour ce faire, cliquez sur **ajouter le disque de stockage de clé** pour ajouter un lecteur de stockage de clés dans le premier emplacement de contrôleur IDE gratuit de cet ordinateur virtuel.

## <a name="see-also"></a>Voir aussi

- [Paramètres de sécurité de machine virtuelle de génération 2 dans le Gestionnaire Hyper-V](Generation-2-virtual-machine-security-settings-for-hyper-v.md)
- [Sécurité et assurance](../../../security/Security-and-Assurance.md)