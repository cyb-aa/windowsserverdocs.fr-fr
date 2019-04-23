---
title: Étapes de migration de Services MultiPoint
description: Vous guide à travers les étapes pour migrer vers les Services MultiPoint dans Windows Server 2016
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ee77efa-7cc5-4ddf-aaff-b5634a717014
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: b63ef4bf63ce990aa0b0ba7624905ba8f14dde98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854810"
---
# <a name="migrate-to--multipoint-services-in-windows-server-2016"></a>Migrer vers les Services MultiPoint dans Windows Server 2016

>S'applique à : Windows Server 2016

Utilisez les étapes suivantes ainsi que les informations collectées dans la feuille de calcul de planification de migration pour migrer vers les Services MultiPoint dans Windows Server 2016.

## <a name="transfer-server-settings"></a>Transférer des paramètres de serveur
Sur le serveur de destination, ouvrez le gestionnaire MultiPoint. Cliquez sur **modifier les paramètres serveur**. Appliquez les paramètres en fonction de la feuille de calcul de planification de migration.

> [!NOTE]
> Si vous avez besoin activer la protection de disque sur le serveur de destination, attendre après avoir configuré les Services MultiPoint.

## <a name="transfer-station-settings"></a>Transférer des paramètres de station
Assurez-vous que les stations sont connectées sur le serveur de destination et tous mappés avant d’appliquer les paramètres de station. Les stations seront détectées automatiquement. Suivez les instructions de chaque écran de station pour définir le mappage du serveur de stations d’utilisateur et les périphériques USB connectés. Appliquer les paramètres de station préféré comme indiqué dans la feuille de calcul de planification de migration.

## <a name="migrate-the-vdi-template"></a>Migrer le modèle d’infrastructure VDI

Avant de pouvoir importer le modèle d’infrastructure VDI à partir du serveur source, activé des bureaux virtuels sur le serveur de destination à l’aide du gestionnaire MultiPoint :

1. Accédez à la **bureaux virtuels** onglet dans le gestionnaire MultiPoint.
2. Cliquez sur **activé des bureaux virtuels**. Le serveur de s’installer le rôle Hyper-V et redémarrez.
3. Ouvrez le gestionnaire MultiPoint et revenir à **bureaux virtuels**.
4. Cliquez sur **modèle de bureau virtuel importation**. Suivez les instructions pour importer le modèle à partir du serveur source.

> [!NOTE]
> Lorsque vous importez un modèle de bureau virtuel, les personnalisations appliquées au modèle sont réinitialisées. 

## <a name="next-step"></a>Étape suivante
[Valider votre nouveau déploiement de MultiPoint Services.](multipoint-services-post-migration-steps.md)