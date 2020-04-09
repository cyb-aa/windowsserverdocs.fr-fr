---
title: Étapes de migration de MultiPoint services
description: Vous guide tout au long des étapes de migration vers MultiPoint services dans Windows Server 2016
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 3ee77efa-7cc5-4ddf-aaff-b5634a717014
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: f2e293fafb8d6f5d84e9ea5a4ad8ef3b7fe2ba7d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858692"
---
# <a name="migrate-to--multipoint-services-in-windows-server-2016"></a>Migrer vers MultiPoint services dans Windows Server 2016

>S’applique à Windows Server 2016

Utilisez les étapes suivantes et les informations que vous avez rassemblées dans la feuille de planification de la migration pour migrer vers MultiPoint services dans Windows Server 2016.

## <a name="transfer-server-settings"></a>Transférer les paramètres du serveur
Sur le serveur de destination, ouvrez le gestionnaire MultiPoint. Cliquez sur **modifier les paramètres du serveur**. Appliquez les paramètres en fonction de la feuille de planification de la migration.

> [!NOTE]
> Si vous devez activer la protection des disques sur le serveur de destination, patientez jusqu’à ce que vous ayez configuré MultiPoint services.

## <a name="transfer-station-settings"></a>Paramètres de station de transfert
Assurez-vous que les stations sont connectées au serveur de destination et toutes mappées avant d’appliquer les paramètres de station. Les stations seront automatiquement détectées. Suivez les instructions de chaque écran de station pour définir le mappage de serveur des stations utilisateur et des périphériques USB connectés. Appliquez vos paramètres de station préférés comme indiqué dans la feuille de planification de la migration.

## <a name="migrate-the-vdi-template"></a>Migrer le modèle VDI

Avant de pouvoir importer le modèle VDI à partir du serveur source, activez les bureaux virtuels sur le serveur de destination à l’aide du gestionnaire MultiPoint :

1. Accédez à l’onglet **bureaux virtuels** dans le gestionnaire multipoint.
2. Cliquez sur **activer les bureaux virtuels**. Le serveur installe le rôle Hyper-V, puis redémarre.
3. Ouvrez le gestionnaire MultiPoint et revenez aux **bureaux virtuels**.
4. Cliquez sur **Importer un modèle de bureau virtuel**. Suivez les instructions pour importer le modèle à partir du serveur source.

> [!NOTE]
> Lorsque vous importez un modèle de bureau virtuel, toute personnalisation appliquée au modèle est réinitialisée. 

## <a name="next-step"></a>Étape suivante
[Validez votre nouveau déploiement de MultiPoint services.](multipoint-services-post-migration-steps.md)