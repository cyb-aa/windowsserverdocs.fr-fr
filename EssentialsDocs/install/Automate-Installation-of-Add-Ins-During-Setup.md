---
title: "Automatiser l’Installation d’Add-Ins pendant l’installation"
description: "Décrit comment utiliser WindowsServerEssentials"
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
ms.openlocfilehash: d4c547c2fec8e2b11e5c1d9bde46e55e91c9d6fa
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="automate-installation-of-add-ins-during-setup"></a>Automatiser l’Installation d’Add-Ins pendant l’installation

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

##  <a name="BKMK_AddIns"></a>Automatiser l’installation des compléments lors de l’installation  
 Pour installer des compléments lors de l’installation, utilisez la méthode PostIC.cmd décrite dans le [créer le fichier PostIC.cmd pour Running Post Initial Configuration Tasks](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md) section de ce document.  
  
 Ajoutez l’entrée suivante à votre PostIC.cmd:  
  
```  
C:\Program Files\Windows Server\bin\Installaddin.exe <full path to wssx file> -q  
```  
  
 Le complément prend désormais en charge les étapes préalables à l’installation et de désinstallation personnalisée.  
  
 L’étape de préinstallation est exécutée avant d’installer toutes les **.msi** fichiers spécifiés dans addin.xml. Lorsque vous exécutez en mode interactif, la boîte de dialogue de progression sera indiqué, mais sans modifier la progression. Le bouton d’annulation est désactivé pendant la phase de préinstallation. Pour implémenter une étape de préinstallation, ajoutez le contenu suivant dans addin.xml (directement sous Package):  
  
> [!NOTE]
>  Le schéma xml doit exactement celui montré ci-dessous:  
  
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
  
 Wherein **fichier exécutable** est le fichier exécutable dans le package de compléments pour effectuer l’étape de préinstallation et doit être spécifié. **NormalArgs** spécifie des arguments devant être transmis au fichier exécutable en mode en ligne de commande lorsque interactive est utilisé. Dans ce mode, le fichier exécutable peut afficher certaines boîtes de dialogue pour l’interaction utilisateur. **SilentArgs** spécifie des arguments devant être transmis au fichier exécutable en mode en ligne de commande lorsqu’elle est en mode silencieux est utilisée (--q est spécifié en cas d’invocation de installaddin.exe). Le fichier exécutable ne doit pas afficher toutes les fenêtres dans ce mode. Si **IgnoreExitCode** est spécifié avec la valeur true, l’étape de préinstallation est toujours considérée comme réussie, dans le cas contraire, le code de sortie 0 indique la réussite, 1 indique l’annulation et autres valeurs indiquent un échec. Balises **NormalArgs**, **SilentArgs**, et **IgnoreExitCode** sont toutes facultatives.  
  
 Une étape de désinstallation personnalisée peut être utilisée pour les éléments suivants:  
  
-   Remplacer la boîte de dialogue de confirmation intégrée.  
  
-   Remplir des boîtes de dialogue personnalisées avant la désinstallation.  
  
-   Effectuer certaines tâches avant la désinstallation.  
  
 Pour implémenter une étape de désinstallation, ajoutez le contenu suivant dans addin.xml (directement sous Package):  
  
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
  
 Wherein **plein-path-to-exefile** Spécifie le fichier exécutable déjà installé sur le système. **Arguments** est facultatif et spécifie les arguments de ligne de commande pour le fichier exécutable. Le fichier exécutable est invoqué avant de confirmer la désinstallation intégré boîte de dialogue s’affiche.  
  
 Le fichier exécutable peut exécuter les tâche suivantes pendant cette phase:  
  
-   Certaines boîtes de dialogue pour l’interaction utilisateur s’affiche.  
  
-   Effectuer certaines tâches en arrière-plan.  
  
 Le code de sortie de ce fichier exe détermine comment le processus de désinstallation progresse:  
  
-   0: le processus de désinstallation continue sans remplir la boîte de dialogue de confirmation intégrée, tout comme l’utilisateur a déjà confirmé. (cette approche peut être utilisée pour remplacer la boîte de dialogue de confirmation intégrée);  
  
-   1: le processus de désinstallation est annulé, et enfin un message annulé est affiché pour l’utilisateur. Rien n’est modifié;  
  
-   Autres: le processus de désinstallation continue avec la boîte de dialogue de confirmation intégrée, comme l’étape de désinstallation personnalisée n’est pas présent.  
  
 Tout échec invocation du fichier exécutable entraîne le même comportement que le fichier exécutable renvoie un code autre que 0 ou 1.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)