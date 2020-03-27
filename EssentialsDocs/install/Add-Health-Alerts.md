---
title: Ajout d’alertes d’intégrité
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 270e0aac-dc42-46f3-a20b-a68ffbded06d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4166d65d0008f3427947322b285221e7b0090029
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310255"
---
# <a name="add-health-alerts"></a>Ajout d’alertes d’intégrité

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Un complément d’intégrité fournit les définitions des alertes, assure les contrôles d’intégrité et procède aux réparations applicables en cas de problèmes du réseau. Il est constitué de fichiers XML prévus pour annoter le code ou les données servant à évaluer les informations d’intégrité pour une fonctionnalité spécifique. Ces compléments sont créés par les développeurs et installés sur le serveur et les ordinateurs clients par l’administrateur.  
  
 Pour plus d’informations sur la création d’un complément d’intégrité, consultez le [Kit de développement logiciel (SDK) Windows Server Solutions](https://go.microsoft.com/fwlink/?LinkID=248648) .  
  
## <a name="installing-health-add-in-files"></a>Installation des fichiers du complément d’intégrité  
 Une fois les fichiers XML préparés par le développeur, il convient de placer une copie de ces fichiers à l’emplacement approprié sur le serveur et sur les ordinateurs clients.  
  
#### <a name="to-install-the-xml-files-on-the-server"></a>Pour installer les fichiers XML sur le serveur  
  
1. Dans le dossier **%ProgramFiles%\Windows Server\Bin\Feature Definitions** , créez un dossier appelé **MyHealthAddIn**. Vous pourriez évidemment lui donner tout autre nom. Nous vous suggérons, cependant, de choisir un nom de dossier identique au nom de la fonctionnalité.  
  
2. Copiez les fichiers Definition.xml et Definition.xml.config dans le nouveau dossier.  
  
3. Si vous avez prévu des fichiers binaires pour des conditions ou des actions, pensez également à copier ces fichiers dans **%ProgramFiles%\Windows Server\Bin**.  
  
   Les ordinateurs clients lancent, toutes les 6 heures, une tâche planifiée ayant pour fonction de transférer les fichiers XML vers l’emplacement approprié. Vous pouvez effectuer une synchronisation forcée entre l’ordinateur client et le serveur en exécutant la tâche en mode manuel.  
  
#### <a name="to-install-the-xml-files-on-the-client-computer"></a>Pour installer les fichiers XML sur l’ordinateur client  
  
1.  Ouvrez le Planificateur de tâches.  
  
2.  Exécutez la tâche **HealthDefinitionUpdateTask** dans le Planificateur de tâches.  
  
    > [!NOTE]
    >  Cette tâche n’installe pas les fichiers binaires. Vous devez copier manuellement les fichiers binaires dans le dossier **%ProgramFiles%\Windows Server\Bin** sur l’ordinateur client.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)