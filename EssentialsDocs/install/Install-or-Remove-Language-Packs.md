---
title: Installation ou suppression des modules linguistiques
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 98f13f63-4480-40ba-a7ef-d1d9b7582e5f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c1fd1d21277d32672398d1dd201e2dda24682c2d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820022"
---
# <a name="install-or-remove-language-packs"></a>Installation ou suppression des modules linguistiques

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

> [!NOTE]
>  Vous devez d’abord créer une image Windows multilingue, comme décrit dans [modules linguistiques et déploiement](https://technet.microsoft.com/library/hh824829) avant d’ajouter le module linguistique Windows Server Essentials.  
  
 Les modules linguistiques sont destinés uniquement à la création d'images multilingues. Les informations de cette section sont spécifiques à l’installation ou à la suppression des modules linguistiques sur Windows Server Essentials.  
  
> [!NOTE]
>  Si vous envisagez d'exécuter la configuration initiale (IC) à partir d'un ordinateur client ne prenant pas en charge les langues d'Asie orientale, tels que ja-jp et si l'anglais n'est pas inclus dans l'image multilingue sur le serveur, la page web IC affichera des carrés. Pour la page web IC de l'anglais par défaut, l'image multilingue que vous créez doit inclure la langue anglaise.  
  
## <a name="adding-language-packs-to-an-image"></a>Ajout de modules linguistiques dans une image  
 Les modules linguistiques sont disponibles sur le DVD des personnalisations OEM. Il est recommandé de copier les modules linguistiques sur l'ordinateur du technicien avant de les ajouter dans une image.  
  
 Vous devez utiliser la commande suivante pour installer les modules linguistiques :  
  
 **DISM. exe/Online/Add-Package/PackagePath : C :\\< chemin d’accès complet au répertoire des fichiers CAB\>\Lp.cab**  
  
 La commande suivante a pour effet, par exemple, d'ajouter un module linguistique allemand :  
  
 **DISM. exe/Online/Add-Package/PackagePath : C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
> [!IMPORTANT]
>  Vous devez également appliquer les modules linguistiques pour Windows Server Essentials afin de localiser complètement le système d’exploitation.  
  
## <a name="removing-language-packs-from-an-image"></a>Suppression de modules linguistiques à partir d'une image  
 Servez-vous de la commande suivante pour retirer un module linguistique dont vous n'avez plus besoin dans l'image :  
  
 **DISM. exe/Online/Remove-Package/PackagePath : C :\\< chemin d’accès complet au répertoire des fichiers CAB\>\Lp.cab**  
  
 La commande suivante a pour effet, par exemple, de supprimer un module linguistique allemand :  
  
 **DISM. exe/Online/Remove-Package/PackagePath : C:\Users\Administrator\Desktop\WindowsHomeServer-Product-r\de-de\lp.cab**  
  
## <a name="see-also"></a>Voir aussi  

 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)

 [Création et personnalisation de l’Image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](../install/Testing-the-Customer-Experience.md)

