---
title: Test de l’expérience utilisateur
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b1a2040-4cfd-48bf-8d04-3ffde9c26b9b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 40590d50441013ebc1dad5e3e52b3aac07419f91
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311517"
---
# <a name="testing-the-customer-experience"></a>Test de l’expérience utilisateur

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Pour vérifier l’expérience utilisateur et contrôler vos personnalisations de partenaires, lancez la procédure de configuration initiale sur l’ordinateur cible. Il est recommandé d’effectuer la configuration initiale au moins une fois en mode manuel pour étudier l’expérience utilisateur. Si vous avez choisi d'associer votre logo au Tableau de bord, vous devez exécuter les tâches de configuration initiales pour vérifier l'effet obtenu. Si vous avez associé le site Accès web distant, vous devez accéder à http://< servername\> pour vérifier la personnalisation (< servername\> est le nom du serveur). Vous pouvez tirer parti de la section relative à la configuration initiale dans le fichier cfg.ini pour automatiser le test de l’expérience utilisateur. Pour plus d’informations sur la création de cette section dans le fichier cfg.ini, voir [Création du fichier Cfg.ini](Create-the-Cfg.ini-File.md).  
  
> [!IMPORTANT]
>  Vous devez exécuter la commande Sysprep.exe pour préparer l’image utilisée dans le cadre du déploiement avant de tester l’expérience utilisateur lors de la configuration initiale. Pour plus d’informations sur l’exécution de la commande Sysprep.exe, voir [Préparation de l’image en vue du déploiement](Preparing-the-Image-for-Deployment.md).  
  
> [!IMPORTANT]
>  Vous avez besoin d’une connexion réseau pour tester la configuration initiale. Le protocole permettant de tester le réseau sans interférence (DHCP) n’est ni configuré, ni installé sur le serveur.  
  
 Pour vérifier les informations de prise en charge de partenaires, cliquez sur la flèche vers le bas en regard du bouton d'aide dans le Tableau de bord.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image en vue du déploiement](Preparing-the-Image-for-Deployment.md)