---
title: Paramètres de sécurité de la machine virtuelle de génération 1 pour Hyper-V
description: Décrit les paramètres de sécurité disponibles dans le Gestionnaire Hyper-V pour les ordinateurs virtuels de génération 1
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f8f8c569-8b74-4c19-876e-1c7d00cce308
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: ceb3c2628546815f9b0af35946e173f4276130d2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392788"
---
# <a name="generation-1-virtual-machine-security-settings"></a>Paramètres de sécurité de la machine virtuelle de génération 1

>S'applique à : Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Utilisez les paramètres de sécurité de la machine virtuelle de génération 1 dans le Gestionnaire Hyper-V pour protéger les données et l’état d’une machine virtuelle.

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Paramètres de prise en charge du chiffrement dans le Gestionnaire Hyper-V

Vous pouvez protéger les données et l’état de la machine virtuelle en sélectionnant l’option de prise en charge de chiffrement suivante.

- **Chiffrer l’État et le trafic de migration de l’ordinateur virtuel** : chiffre l’état enregistré de la machine virtuelle lorsqu’il est écrit sur le disque et le trafic de migration dynamique.

Pour activer cette option, vous devez ajouter un lecteur de stockage de clés pour la machine virtuelle.

## <a name="key-storage-drive-in-hyper-v-manager"></a>Lecteur de stockage de clés dans le Gestionnaire Hyper-V

Un lecteur de stockage de clés fournit un petit lecteur à la machine virtuelle pour stocker une clé BitLocker. Cela permet à l’ordinateur virtuel de chiffrer son disque de système d’exploitation sans nécessiter de processeur de Module de plateforme sécurisée (TPM) (TPM) virtualisé. Le contenu du lecteur de stockage de clés est chiffré à l’aide d’un protecteur de clé. Le protecteur de clé autorise l’hôte Hyper-V à exécuter l’ordinateur virtuel. Le contenu du lecteur de stockage de clés et le protecteur de clé sont stockés dans le cadre de l’état d’exécution de la machine virtuelle.

Pour déchiffrer le contenu du lecteur de stockage de clés et démarrer l’ordinateur virtuel, l’ordinateur hôte Hyper-V doit être :

- Une partie d’une infrastructure protégée autorisée pour cette machine virtuelle, ou
- Disposer de la clé privée de l’un des gardiens de la machine virtuelle.

Pour en savoir plus sur les infrastructures protégées, consultez la section Présentation des machines virtuelles dotées d’une protection maximale dans [sécurité et assurance](../../../security/Security-and-Assurance.md).

Vous pouvez ajouter un lecteur de stockage de clés à un emplacement vide sur l’un des contrôleurs IDE de l’ordinateur virtuel. Pour ce faire, cliquez sur **Ajouter un lecteur de stockage de clés** pour ajouter un lecteur de stockage de clés au premier emplacement du contrôleur IDE gratuit de cet ordinateur virtuel.

## <a name="see-also"></a>Voir aussi

- [Paramètres de sécurité de l’ordinateur virtuel de génération 2 dans le Gestionnaire Hyper-V](Generation-2-virtual-machine-security-settings-for-hyper-v.md)
- [Sécurité et assurance](../../../security/Security-and-Assurance.md)