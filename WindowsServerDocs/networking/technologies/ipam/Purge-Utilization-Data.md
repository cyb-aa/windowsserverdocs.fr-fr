---
title: Supprimer définitivement les données d’utilisation
description: Cette rubrique fait partie du Guide de gestion des adresses IP (IPAM) de Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45cada9e-69b9-43df-b6f5-6d3942435809
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a8542e643a9c4d33acad18523fd34eed8926413d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316775"
---
# <a name="purge-utilization-data"></a>Supprimer définitivement les données d’utilisation

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à supprimer des données d’utilisation de la base de données IPAM.  

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs **IPAM**, du groupe **administrateurs** de l’ordinateur local ou d’un groupe équivalent.

## <a name="to-purge-the-ipam-database"></a>Pour purger la base de données IPAM  
1. Ouvrez Gestionnaire de serveur, puis accédez à l’interface client IPAM.
2. Accédez à l’un des emplacements suivants : **blocs d’adresses IP**, **inventaire d’adresses IP**ou **groupes de plages d’adresses IP**.  
3. Cliquez sur **tâches**, puis sur **purger les données d’utilisation**. La boîte de dialogue **purger les données d’utilisation** s’ouvre.
4. Dans **purger toutes les données d’utilisation le ou avant le**, cliquez sur **Sélectionner une date**.
5. Choisissez la date pour laquelle vous souhaitez supprimer tous les enregistrements de base de données à la fois sur et avant cette date.
6. Cliquez sur **OK**. IPAM supprime tous les enregistrements que vous avez spécifiés.
