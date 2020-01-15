---
title: Gérer des licences d’accès client
description: En savoir plus sur l’utilisation des licences d’accès client dans MultiPoint services
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4d809ab1bf2a18dff537bf63620623d576c0b25d
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949881"
---
# <a name="manage-client-access-licenses"></a>Gérer des licences d’accès client
Chaque station qui se connecte à un système MultiPoint services, y compris l’ordinateur exécutant MultiPoint services utilisé comme station, doit avoir une *licence d’accès client (CAL)* Bureau à distance par utilisateur valide.

Si vous utilisez des bureaux virtuels de station plutôt que des stations physiques, vous devez installer une licence d’accès client pour chaque bureau virtuel de station.  
  
1.  Achetez une licence client pour chaque station connectée à votre serveur ou ordinateur MultiPoint services. Pour plus d’informations sur l’achat de licences d’accès client, consultez la documentation relative aux licences Bureau à distance. 

2.  À partir de l’écran d' **Accueil** , ouvrez le **Gestionnaire multipoint**.  
  
3.  Cliquez sur l’onglet dossier de **démarrage** , puis sur **Ajouter des licences d’accès client**.  L’outil de gestion du gestionnaire de licences d’accès client s’ouvre.

## <a name="set-the-licensing-mode-manually"></a>Définir le mode de licence manuellement
Si elle n’est pas configurée correctement, la configuration de MultiPoint services demande une notification indiquant que la période de grâce a expiré. Pour définir le mode de licence, procédez comme suit :

1. Lancez l' **éditeur de stratégie de groupe local** (gpedit. msc).

2. Dans le volet gauche, accédez à **stratégie de l’ordinateur local-> configuration de l’ordinateur-> modèles d’administration-> composants Windows-> Services Bureau à distance-> Bureau à distance hôte de session-> licence**.

3. Dans le volet droit, cliquez avec le bouton droit sur **utiliser les serveurs de licences bureau à distance spécifiés** , puis sélectionnez **modifier**:
   - Dans la boîte de dialogue Éditeur de stratégie de groupe, sélectionnez **activé** .
   - Entrez le nom de l’ordinateur local dans le champ **serveurs de licences à utiliser** .
   - Sélectionnez **OK**
  
4. Dans le volet droit, cliquez avec le bouton droit sur **définir le mode de licence Bureau à distance** , puis sélectionnez **modifier** .
   - Dans la boîte de dialogue Éditeur de stratégie de groupe, sélectionnez **activé** .
   - Définir le **mode de licence** par périphérique/par utilisateur
   - Sélectionnez **OK** 

  
## <a name="see-also"></a>Articles associés  
[Gérer des tâches système à l’aide du Gestionnaire MultiPoint](Manage-System-Tasks-Using-MultiPoint-Manager.md)
