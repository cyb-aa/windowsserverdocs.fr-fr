---
title: Vider les données d’utilisation
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45cada9e-69b9-43df-b6f5-6d3942435809
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c41be119099aed4867df1bae1a55e2fbaa5c9064
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="purge-utilization-data"></a>Vider les données d’utilisation

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment supprimer les données d’utilisation de la base de données IPAM.  

Vous devez être membre du **administrateurs IPAM**, l’ordinateur local **administrateurs**, ou équivalent, pour effectuer cette procédure.

## <a name="to-purge-the-ipam-database"></a>Pour supprimer la base de données IPAM  
1. Ouvrez le Gestionnaire de serveur, puis accédez à l’interface du client IPAM.
2. Accédez à un des emplacements suivants: **blocs d’adresses IP**, **inventaire d’adresses IP**, ou **groupes de plages d’adresses IP**.  
3. Cliquez sur **tâches**, puis cliquez sur **purger les données d’utilisation**. Le **purger les données d’utilisation** boîte de dialogue s’ouvre.
4. Dans **vider l’utilisation de toutes les données ou avant**, cliquez sur **sélectionner une date**.
5. Choisissez la date pour lequel vous souhaitez supprimer tous les enregistrements de base de données à la fois et avant cette date.
6. Cliquez sur **OK**. IPAM supprime tous les enregistrements que vous avez spécifiées.
