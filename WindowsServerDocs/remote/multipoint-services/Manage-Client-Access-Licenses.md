---
title: Gérer des licences d’accès client
description: Découvrez comment travailler avec des licences d’accès client dans MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 675e089e-d841-401e-bba7-69f3929ef609
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: f5f78d3d2387d3b95177a6a8a40fb9b16d8ed8e2
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446122"
---
# <a name="manage-client-access-licenses"></a>Gérer des licences d’accès client
Chaque station qui se connecte à un système MultiPoint Services, y compris l’ordinateur qui exécute MultiPoint Services qui est utilisé comme une station, doit avoir un bureau à distance par utilisateur valide *licence d’accès client (CAL)* .

Si vous utilisez des bureaux virtuels station au lieu de stations physiques, vous devez installer une licence d’accès client pour chaque bureau virtuel station.  
  
1.  Achetez une licence de client pour chaque station est connectée à votre serveur ou ordinateur MultiPoint Services. Pour plus d’informations sur l’achat de licences d’accès client, consultez la documentation pour les licences bureau à distance. <!--@Liza: add link to RDS licensing here-->

2.  À partir de la **Démarrer** écran, ouvrez **Gestionnaire MultiPoint**.  
  
3.  Cliquez sur le **accueil** onglet, puis cliquez sur **ajouter des licences d’accès client**.  L’outil de gestion de licences de gestion des licences d’accès client s’ouvre.

# <a name="set-the-licensing-mode-manually"></a>Définir le mode de licence manuellement
Si pas configuré correctement la configuration de MultiPoint Services invite avec une notification concernant la période de grâce en cours a expiré. Suivez ces étapes pour définir le mode de licence :

1. Lancez **éditeur de stratégie de groupe locale** (gpedit.msc).

2. Dans le volet gauche, accédez à **stratégie ordinateur Local -> Configuration ordinateur - > modèles d’administration -> Windows composants -> Services Bureau à distance - > hôte de Session Bureau à distance -> Gestionnaire de licences**.

3. Dans le volet droit, avec le bouton droit sur **utiliser les serveurs de licences bureau à distance spécifiés** et sélectionnez **modifier**:
   - Dans le dialogue de l’éditeur de stratégie de groupe, sélectionnez **activé**
   - Entrez le nom de l’ordinateur local dans le **aux serveurs à utiliser de licences** champ.
   - Sélectionnez **OK**
  
4. Dans le volet droit, avec le bouton droit sur **définir le mode de licence du Bureau à distance** et sélectionnez **modifier**
   - Dans le dialogue de l’éditeur de stratégie de groupe, sélectionnez **activé**
   - Définir le **mode de licence** à par périphérique / par utilisateur
   - Sélectionnez **OK** 

  
## <a name="see-also"></a>Voir aussi  
[Gérer des tâches système à l’aide du Gestionnaire MultiPoint](Manage-System-Tasks-Using-MultiPoint-Manager.md)
