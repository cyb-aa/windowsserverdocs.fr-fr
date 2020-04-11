---
title: Paramètres de sécurité des machines virtuelles de génération 1 pour Hyper-V
description: Décrit les paramètres de sécurité disponibles dans le Gestionnaire Hyper-V pour les machines virtuelles de génération 1
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: f8f8c569-8b74-4c19-876e-1c7d00cce308
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: f86c4fe9222f08b3ef3719080deeb4fbda6edd33
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860812"
---
# <a name="generation-1-virtual-machine-security-settings"></a>Paramètres de sécurité des machines virtuelles de génération 1

>S'applique à : Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Utilisez les paramètres de sécurité des machines virtuelles de génération 1 dans le Gestionnaire Hyper-V pour protéger les données et l’état d’une machine virtuelle.

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Paramètres de prise en charge du chiffrement dans le Gestionnaire Hyper-V

Vous pouvez protéger les données et l’état de la machine virtuelle en sélectionnant l’option de prise en charge du chiffrement suivante.

- **Chiffrer l’état et le trafic de migration de la machine virtuelle** - Chiffre l’état enregistré quand il est écrit sur le disque et le trafic de migration dynamique de la machine virtuelle.

Pour activer cette option, vous devez ajouter un lecteur de stockage de clé pour la machine virtuelle.

## <a name="key-storage-drive-in-hyper-v-manager"></a>Lecteur de stockage de clé dans le Gestionnaire Hyper-V

Un lecteur de stockage de clé fournit un petit lecteur à la machine virtuelle pour y stocker une clé BitLocker. Ceci permet à la machine virtuelle de chiffrer son disque de système d’exploitation sans nécessiter une puce Module de plateforme sécurisée (TPM) virtualisée. Le contenu du lecteur de stockage de clé est chiffré en utilisant un protecteur de clé. Le protecteur de clé autorise l’hôte Hyper-V à exécuter la machine virtuelle. Le contenu du lecteur de stockage de clé et le protecteur de clé sont stockés dans le cadre de l’état d’exécution de la machine virtuelle.

Pour déchiffrer le contenu du lecteur de stockage de clé et démarrer la machine virtuelle, l’hôte Hyper-V doit :

- Faire partie d’une structure fabric protégée autorisée pour cette machine virtuelle, ou
- Disposer de la clé privée d’un des gardiens de la machine virtuelle.

Pour découvrir plus d’informations sur les structures fabric protégées, consultez la section Présentation des machines virtuelles dotées d’une protection maximale dans [Sécurité et assurance](../../../security/Security-and-Assurance.md).

Vous pouvez ajouter un lecteur de stockage de clé à un emplacement vide sur un des contrôleurs IDE de la machine virtuelle. Pour cela, cliquez sur **Ajouter un lecteur de stockage de clé** pour ajouter un lecteur de stockage de clé au premier emplacement de contrôleur IDE disponible de cette machine virtuelle.

## <a name="see-also"></a>Voir aussi

- [Paramètres de sécurité des machines virtuelles de génération 2 dans le Gestionnaire Hyper-V](Generation-2-virtual-machine-security-settings-for-hyper-v.md)
- [Sécurité et assurance](../../../security/Security-and-Assurance.md)