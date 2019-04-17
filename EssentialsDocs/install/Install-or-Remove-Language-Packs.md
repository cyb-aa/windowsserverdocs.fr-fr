---
title: Installer ou supprimer des modules linguistiques
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98f13f63-4480-40ba-a7ef-d1d9b7582e5f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: a41b491bbe4b4a8ee7f9743dc85e5bdaffb08496
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="install-or-remove-language-packs"></a>Installer ou supprimer des modules linguistiques

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

> [!NOTE]
>  Vous devez d’abord créer une image Windows multilingue comme décrit dans le [linguistiques et déploiement](https://technet.microsoft.com/library/hh824829) avant d’ajouter le module linguistique WindowsServerEssentials.  
  
 Les modules linguistiques sont uniquement disponibles pour la création d’images multilingues. Les informations de cette section sont spécifiques à l’installation ou la suppression des modules linguistiques sur WindowsServerEssentials.  
  
> [!NOTE]
>  Si vous envisagez d’exécuter la Configuration initiale (IC) à partir d’un ordinateur client qui ne prend pas en charge les langues d’Asie de l’est, tels que ja-jp, et si l’anglais n’est pas inclus dans l’image multilingue sur le serveur, la page Web IC affichera des carrés. Pour la page Web IC pour anglais par défaut, l’image multilingue que vous créez doit inclure en anglais.  
  
## <a name="adding-language-packs-to-an-image"></a>Ajout de modules linguistiques à une image  
 Les modules linguistiques sont disponibles sur le DVD des personnalisations OEM. Il est recommandé de copier les modules linguistiques pour votre ordinateur de référence avant d’ajouter les modules linguistiques à l’image.  
  
 Vous devez utiliser la commande suivante pour installer des modules linguistiques:  
  
 **Dism.exe//online /Add-Package /PackagePath: C:\\ < chemin d’accès complet au fichier cab Directory le fichier > \lp.cab**  
  
 Par exemple, la commande suivante montre comment ajouter un module linguistique allemand:  
  
 **Dism.exe//online /Add-Package /PackagePath: C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
> [!IMPORTANT]
>  Vous devez également appliquer les modules linguistiques pour WindowsServerEssentials pour assurer la localisation complète du système d’exploitation.  
  
## <a name="removing-language-packs-from-an-image"></a>Suppression de modules linguistiques à partir d’une image  
 Vous pouvez utiliser la commande suivante pour supprimer un module linguistique que vous ne souhaitez plus à inclure dans une image:  
  
 **Dism.exe//online /Remove-Package /PackagePath: C:\\ < chemin d’accès complet au fichier cab Directory le fichier > \lp.cab**  
  
 Par exemple, la commande suivante montre comment supprimer un module linguistique allemand:  
  
 **Dism.exe//online /Remove-Package /PackagePath: C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
## <a name="see-also"></a>Voir aussi  

 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)

 [Création et personnalisation de l’Image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](../install/Testing-the-Customer-Experience.md)

