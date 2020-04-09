---
title: Ajout de catégories de niveau supérieur à la zone de lancement (système d'exploitation Macintosh)
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: ee2173c3-e464-4001-9f43-6d926a575092
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c8a15a831a89afc55d20db4e1c1195173d466b3c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817512"
---
# <a name="add-top-level-categories-to-the-launchpad-macintosh-operating-system"></a>Ajout de catégories de niveau supérieur à la zone de lancement (système d'exploitation Macintosh)

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous pouvez ajouter des catégories de niveau supérieur au Launchpad sur un ordinateur fonctionnant avec le système d'exploitation Macintosh. Pour créer un complément Launchpad qui ajoute des catégories de niveau supérieur, vous pouvez utiliser une combinaison d’informations à partir de cette page et de la rubrique intitulée Comment ajouter des tâches et des catégories au launchpad. dans le [Kit de développement logiciel (SDK) des solutions Windows Server](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
 L'exemple suivant montre comment définir votre entrée Launchpad en tant que catégorie de niveau supérieur dans le fichier .launchpad :  
  
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
  
 Pour configurer l'entrée comme catégorie de niveau supérieur, l'attribut ID de l'élément Category doit être « Microsoft.Launchpad.HomeCategory ».  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)