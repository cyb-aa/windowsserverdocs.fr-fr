---
title: Automatiser l’installation des compléments lors de la phase d’installation
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2e6ff6e4-8d68-4d49-9e38-8088bc8bf95e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c2345726a17a074fc7022c8c4dc9b2443e9ad384
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433642"
---
# <a name="automate-installation-of-add-ins-during-setup"></a>Automatiser l’installation des compléments lors de la phase d’installation

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_AddIns"></a> Automatiser l’installation des compléments lors de l’installation  
 Pour installer des compléments lors de la phase d’installation, utilisez la méthode PostIC.cmd décrite dans la section [Création du fichier PostIC.cmd pour exécuter les tâches de configuration initiales](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md) de ce document.  
  
 Ajoutez l’entrée suivante à votre fichier PostIC.cmd :  
  
```  
C:\Program Files\Windows Server\bin\Installaddin.exe <full path to wssx file> -q  
```  
  
 Le complément prend maintenant en charge les étapes de préinstallation et de désinstallation personnalisée.  
  
 L’étape de préinstallation est exécutée avant l’installation de tous les fichiers **.msi** spécifiés dans addin.xml. Lorsque le mode interactif est exécuté, la boîte de dialogue de progression est affichée sans que la progression ne soit modifiée. Le bouton d’annulation est désactivé pendant la phase de préinstallation. Pour mettre en œuvre une étape de préinstallation, ajoutez les contenus suivants au fichier addin.xml (directement sous Package) :  
  
> [!NOTE]
>  Le schéma xml doit correspondre exactement à celui montré ci-dessous :  
  
```  
<Package xmlns="https://schemas.microsoft.com/WindowsServerSolutions/2010/03/Addins" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">  
  <Id>...</Id>  
  <Version>...</Version>  
  <Name>...</Name>  
  <Allow32BitOn64BitClients>...</Allow32BitOn64BitClients>  
  <ServerBinary>...</ServerBinary>  
  <ClientBinary32>...</ClientBinary32>  
  <ClientBinary64>...</ClientBinary64>  
  <SupportedSkus>...</SupportUrl>    
  <SupportUrl>...</SupportUrl>  
  <Location>...</Location>    
  <PrivacyStatement>...</PrivacyStatement>  
  <OtherBinaries>...</OtherBinaries>   
  <Preinstall>  
<Executable>exefile</Executable>  
<NormalArgs>args-for-interactive-mode</NormalArgs>  
<SilentArgs>args-for-silent-mode</SilentArgs>  
<IgnoreExitCode>true</IgnoreExitCode>  
  </Preinstall>  
  <UninstallConfirm>...</UninstallConfirm>      
</Package>  
<¦>  
<¦>  
```  
  
 Le « **exefile** » est le fichier exécutable dans le package de compléments qui exécute l’étape de préinstallation et doit être spécifié. **NormalArgs** spécifie des arguments devant être transmis au fichier exécutable dans la ligne de commande en cas d'utilisation du mode interactif. Dans ce mode, le fichier exécutable peut afficher des boîtes de dialogue contextuelles pour l’interaction entre utilisateurs. **SilentArgs** spécifie des arguments devant être transmis au fichier exécutable dans la ligne de commande en cas d’utilisation du mode silencieux (-q est spécifié en cas d'invocation de installaddin.exe). Le fichier exécutable ne doit pas afficher de boîtes de dialogue contextuelles dans ce mode. Si **IgnoreExitCode** est spécifié avec Vrai, l’étape de préinstallation est toujours considérée comme réussie, sinon le code de sortie 0 indique la réussite, 1 indique l’annulation, et d’autres valeurs indiquent un échec. Les balises **NormalArgs**, **SilentArgs**et **IgnoreExitCode** sont toutes optionnelles.  
  
 Une étape de désinstallation personnalisée peut être utilisée dans l’un des cas suivants :  
  
- Remplacer la boîte de dialogue de confirmation intégrée.  
  
- Remplir des boîtes de dialogue personnalisées avant la désinstallation.  
  
- Exécuter certaines tâches avant la désinstallation.  
  
  Pour mettre en œuvre une étape de désinstallation, ajoutez les contenus suivants au fichier addin.xml (directement sous Package) :  
  
```  
<Package xmlns="https://schemas.microsoft.com/WindowsServerSolutions/2010/03/Addins" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">  
  <Id>...</Id>  
  <Version>...</Version>  
  <Name>...</Name>  
  <Allow32BitOn64BitClients>...</Allow32BitOn64BitClients>  
  <ServerBinary>...</ServerBinary>  
  <ClientBinary32>...</ClientBinary32>  
  <ClientBinary64>...</ClientBinary64>  
  <SupportedSkus>...</SupportUrl>    
  <SupportUrl>...</SupportUrl>  
  <Location>...</Location>    
  <PrivacyStatement>...</PrivacyStatement>  
  <OtherBinaries>...</OtherBinaries>   
  <Preinstall>¦</Preinstall>  
<UninstallConfirm>  
<Executable>full-path-to-exefile</Executable>  
<Arguments>command-line-arguments</Arguments>  
</UninstallConfirm>  
</Package>  
```  
  
 **full-path-to-exefile** spécifie le fichier exécutable déjà installé sur le système. **Arguments** est optionnel et spécifie les arguments de la ligne de commande pour le fichier exécutable. Le fichier exécutable est invoqué avant l'affichage de la boîte de dialogue intégrée de confirmation de la désinstallation.  
  
 Le fichier exécutable peut exécuter les tâche suivantes pendant cette phase :  
  
- Afficher certaines boîtes de dialogue pour l’interaction entre utilisateurs.  
  
- Exécuter certaines tâches en arrière-plan.  
  
  Le code de sortie de ce fichier exécutable détermine comment le processus de désinstallation progresse :  
  
- 0 : le processus de désinstallation continue sans remplir la boîte de dialogue de configuration intégrée, comme l'utilisateur l'a déjà confirmé. (cette approche peut être utilisée pour remplacer la boîte de confirmation intégrée) ;  
  
- 1 : le processus de désinstallation est annulé, et, enfin, un message annulé est affiché pour l'utilisateur. Rien n'est modifié ;  
  
- Autre : le processus de désinstallation continue avec la boîte de dialogue de confirmation intégrée, tout comme l'étape de désinstallation personnalisée n'est pas présente.  
  
  Tout échec de l'invocation du fichier exécutable provoquera le même comportement si le fichier exécutable renvoie un code autre que 0 ou 1.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)