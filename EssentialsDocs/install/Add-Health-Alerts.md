---
title: "Ajouter des alertes d’intégrité"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 270e0aac-dc42-46f3-a20b-a68ffbded06d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cf0c062b92c687f5f7b33b419eafdca2dd3bbbfc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="add-health-alerts"></a>Ajouter des alertes d’intégrité

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Un complément d’intégrité fournit les définitions des alertes, les vérifications d’intégrité et les réparations pour des problèmes de réseau. Un complément d’intégrité se compose des fichiers xml prévus pour annoter le code ou des données qui sont utilisées pour évaluer les informations d’intégrité pour une fonctionnalité spécifique. Compléments sont créés par les développeurs et installés sur le serveur et les ordinateurs clients par l’administrateur.  
  
 Reportez-vous à la [SDK WindowsServerSolutions](https://go.microsoft.com/fwlink/?LinkID=248648) pour plus d’informations sur la création d’un complément d’intégrité.  
  
## <a name="installing-health-add-in-files"></a>L’installation des fichiers du complément d’intégrité  
 Une fois que le développeur a créé les fichiers xml, vous devez placer une copie des fichiers dans l’emplacement approprié sur le serveur et les ordinateurs clients.  
  
#### <a name="to-install-the-xml-files-on-the-server"></a>Pour installer les fichiers xml sur le serveur  
  
1.  Dans le **%ProgramFiles%\Windows Server\Bin\Feature Definitions** dossier, créez un dossier nommé **MyHealthAddIn**. Vous pouvez donner à ce dossier n’importe quel nom. Il est recommandé que le nom du dossier être le même que le nom de la fonctionnalité.  
  
2.  Copiez le Definition.xml et les fichiers Definition.xml vers le nouveau dossier.  
  
3.  Si vous avez créé les fichiers binaires pour des conditions ou des actions, vous devez également copier ces fichiers à **%ProgramFiles%\Windows Server\Bin**.  
  
 Ordinateurs clients exécutent une tâche planifiée toutes les 6heures qui extrait les fichiers XML vers l’emplacement approprié. Vous pouvez forcer la synchronisation entre l’ordinateur client et le serveur en exécutant manuellement la tâche.  
  
#### <a name="to-install-the-xml-files-on-the-client-computer"></a>Pour installer les fichiers xml sur l’ordinateur client  
  
1.  Ouvrez le Planificateur de tâches.  
  
2.  Exécutez le **Healthdefinitionupdatetask** dans le Planificateur de tâches.  
  
    > [!NOTE]
    >  Cette tâche n’installe pas les fichiers binaires. Vous devez copier manuellement les fichiers binaires pour le **%ProgramFiles%\Windows Server\Bin** dossier sur l’ordinateur client.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)