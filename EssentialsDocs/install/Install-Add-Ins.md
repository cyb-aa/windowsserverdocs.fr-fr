---
title: Installer Add-Ins
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e62e4f07-c2ba-4c5e-b30c-bdc287cd654e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d00cb6886e812ee2b780ad79e1fba44442e279ad
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="install-add-ins"></a>Installer Add-Ins

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Vous pouvez inclure des compléments sur tous les ordinateurs serveur ou client en installant les avant de créer une image. Ces compléments seront alors automatiquement intégrés sur tous les ordinateurs répliqués à l’aide de cette image. Vous pouvez installer un complément en exécutant le fichier .wssx, ou vous pouvez installer des fichiers individuels complément en suivant les instructions dans le [documentation SDK](https://go.microsoft.com/fwlink/?LinkID=248648)pour chaque type de complément. Si vous installez à l’aide d’un fichier .wssx, le complément peut être désinstallé via le Gestionnaire d’Add-In. Si vous installez les fichiers individuels, le complément n’est pas géré à partir du gestionnaire Add-In.  
  
> [!NOTE]
>  Tout logiciel qui est installé ou téléchargé sur le serveur (y compris les onglets du tableau de bord et les notifications de santé) doit avoir une interface localisée qui correspond à la langue du système d’exploitation qui est installé sur le serveur.  
  
#### <a name="to-install-an-add-in"></a>Pour installer un complément  
  
1.  (Facultatif) Si vous installez le complément à l’aide d’un fichier .wssx, procédez comme suit:  
  
    1.  Enregistrez le fichier .wssx < addinName\ > sur l’ordinateur de référence.  
  
    2.  Double-cliquez sur le fichier .wssx pour ouvrir l’Assistant Installation Add-in.  
  
    3.  Suivez les instructions de l’Assistant pour terminer l’installation.  
  
2.  (Facultatif) Installer les différents fichiers aux emplacements appropriés comme défini dans le Kit de développement pour chaque type de complément.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)