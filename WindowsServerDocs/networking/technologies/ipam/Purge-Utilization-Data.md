---
title: Supprimer définitivement les données d’utilisation
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server 2016.
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
ms.openlocfilehash: 7b471417d4c44c22f115443f1f2dcca6f351e6f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827940"
---
# <a name="purge-utilization-data"></a>Supprimer définitivement les données d’utilisation

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour savoir comment supprimer des données d’utilisation de la base de données IPAM.  

Vous devez être membre du **administrateurs IPAM**, l’ordinateur local **administrateurs** , ou équivalent, pour effectuer cette procédure.

## <a name="to-purge-the-ipam-database"></a>Pour purger la base de données IPAM  
1. Ouvrez le Gestionnaire de serveur, puis accédez à l’interface du client IPAM.
2. Accédez à l'un des emplacements suivants : **Blocs d’adresses IP**, **inventaire d’adresses IP**, ou **groupes de plages d’adresses IP**.  
3. Cliquez sur **tâches**, puis cliquez sur **purger les données d’utilisation**. Le **purger les données d’utilisation** boîte de dialogue s’ouvre.
4. Dans **vider l’utilisation de toutes les données ou avant**, cliquez sur **sélectionner une date**.
5. Choisissez la date pour laquelle vous souhaitez supprimer tous les enregistrements de base de données en ligne et avant cette date.
6. Cliquez sur **OK**. IPAM supprime tous les enregistrements que vous avez spécifiées.
