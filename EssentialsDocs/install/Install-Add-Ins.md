---
title: Installer les compléments
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: e62e4f07-c2ba-4c5e-b30c-bdc287cd654e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f8428a0198459e1dc036647a3d47fa1a49c00c01
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820132"
---
# <a name="install-add-ins"></a>Installer les compléments

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous pouvez inclure des compléments sur tous les ordinateurs clients et sur tous les serveurs à condition de les installer avant de créer une image. Ces compléments seront alors automatiquement intégrés aux ordinateurs répliqués à l’aide de cette image. Vous pouvez ajouter un complément en exécutant le fichier .wssx, ou installer les différents fichiers du complément en suivant les instructions fournies dans la [documentation SDK](https://go.microsoft.com/fwlink/?LinkID=248648)pour chaque type de complément. Si vous effectuez l’installation en utilisant un fichier .wssx, le complément peut être désinstallé via le Gestionnaire de compléments. Si vous optez pour la deuxième solution, le Gestionnaire de compléments ne vous sera d’aucune utilité.  
  
> [!NOTE]
>  Tout logiciel installé ou téléchargé sur le serveur (y compris les onglets du tableau de bord et les notifications de santé) doit posséder une interface dont la langue de localisation correspond à celle du système d'exploitation installé sur le serveur.  
  
#### <a name="to-install-an-add-in"></a>Pour installer un complément  
  
1.  (Facultatif) Si vous installez le complément à l’aide d’un fichier .wssx, effectuez les étapes suivantes :  
  
    1.  Enregistrez le fichier < AddinName\>. wssx sur l’ordinateur de référence.  
  
    2.  Double-cliquez sur le fichier .wssx pour ouvrir l’Assistant Installation de compléments.  
  
    3.  Suivez les instructions de l’assistant pour réaliser l’installation :  
  
2.  (Facultatif) Installez les fichiers individuels aux emplacements appropriés comme indiqué dans le Kit de développement logiciel (SDK) pour chaque type de complément.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)