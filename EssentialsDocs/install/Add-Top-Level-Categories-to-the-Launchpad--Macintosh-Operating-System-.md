---
title: "Ajout de catégories de niveau supérieur au Launchpad (système d’exploitation Macintosh)"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ee2173c3-e464-4001-9f43-6d926a575092
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ae4eb5943d37b4a9d3b554af28cb425420782cf8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="add-top-level-categories-to-the-launchpad-macintosh-operating-system"></a>Ajout de catégories de niveau supérieur au Launchpad (système d’exploitation Macintosh)

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Vous pouvez ajouter des catégories de niveau supérieur au Launchpad sur un ordinateur exécutant le système d’exploitation Macintosh. Pour créer un complément Launchpad qui ajoute les catégories de niveau supérieur, vous pouvez utiliser une combinaison d’informations à partir de cette page et de la rubrique de procédure: ajouter des tâches et catégories au Launchpad? dans le [WindowsServerSolutionsSDK](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
 L’exemple suivant montre comment vous pouvez spécifier votre entrée Launchpad pour la catégorie de niveau supérieur dans le fichier .launchpad:  
  
```  
  
<?xml version="1.0" encoding="utf-8" ?>  
<LaunchPad resFile="Localizable">  
   <Category id="Microsoft.Launchpad.HomeCategory" name="SampleAddin"  image="SampleImage01.png">  
      <Task id="Microsoft.Launchpad.SampleAddin.Pictures"   
          name="Pictures"       
           src="smb://%ServerAddress%/Pictures"   
         image="SampleImage02.png"/>  
   </Category>  
</LaunchPad>  
```  
  
 Pour l’entrée à la catégorie de niveau supérieur, l’attribut Id de l’élément Category doit être «Microsoft.Launchpad.HomeCategory».  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)