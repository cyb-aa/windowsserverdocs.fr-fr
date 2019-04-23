---
title: Installer les compléments
description: Décrit comment utiliser Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833070"
---
# <a name="install-add-ins"></a>Installer les compléments

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous pouvez inclure des compléments sur tous les ordinateurs clients et sur tous les serveurs à condition de les installer avant de créer une image. Ces compléments seront alors automatiquement intégrés aux ordinateurs répliqués à l’aide de cette image. Vous pouvez ajouter un complément en exécutant le fichier .wssx, ou installer les différents fichiers du complément en suivant les instructions fournies dans la [documentation SDK](https://go.microsoft.com/fwlink/?LinkID=248648)pour chaque type de complément. Si vous effectuez l’installation en utilisant un fichier .wssx, le complément peut être désinstallé via le Gestionnaire de compléments. Si vous optez pour la deuxième solution, le Gestionnaire de compléments ne vous sera d’aucune utilité.  
  
> [!NOTE]
>  Tout logiciel installé ou téléchargé sur le serveur (y compris les onglets du tableau de bord et les notifications de santé) doit posséder une interface dont la langue de localisation correspond à celle du système d'exploitation installé sur le serveur.  
  
#### <a name="to-install-an-add-in"></a>Pour installer un complément  
  
1.  (Facultatif) Si vous installez le complément à l’aide d’un fichier .wssx, effectuez les étapes suivantes :  
  
    1.  Enregistrer le < Nomcomplément\>fichier .wssx sur l’ordinateur de référence.  
  
    2.  Double-cliquez sur le fichier .wssx pour ouvrir l’Assistant Installation de compléments.  
  
    3.  Suivez les instructions de l’assistant pour réaliser l’installation :  
  
2.  (Facultatif) Installez les fichiers individuels aux emplacements appropriés comme indiqué dans le Kit de développement logiciel (SDK) pour chaque type de complément.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)