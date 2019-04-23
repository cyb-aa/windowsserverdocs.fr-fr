---
title: Test de l’expérience utilisateur
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b1a2040-4cfd-48bf-8d04-3ffde9c26b9b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 223b0e1be3a53e9a7d198dc005fc8725e421db58
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838690"
---
# <a name="testing-the-customer-experience"></a>Test de l’expérience utilisateur

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Pour vérifier l’expérience utilisateur et contrôler vos personnalisations de partenaires, lancez la procédure de configuration initiale sur l’ordinateur cible. Il est recommandé d’effectuer la configuration initiale au moins une fois en mode manuel pour étudier l’expérience utilisateur. Si vous avez choisi d'associer votre logo au Tableau de bord, vous devez exécuter les tâches de configuration initiales pour vérifier l'effet obtenu. Si vous choisi le site accès Web à distance, vous devez accéder à http://<servername\> pour vérifier la personnalisation (< nom_serveur\> est le nom du serveur). Vous pouvez tirer parti de la section relative à la configuration initiale dans le fichier cfg.ini pour automatiser le test de l’expérience utilisateur. Pour plus d’informations sur la création de cette section dans le fichier cfg.ini, consultez [Create the Cfg.ini File](Create-the-Cfg.ini-File.md).  
  
> [!IMPORTANT]
>  Vous devez exécuter la commande Sysprep.exe pour préparer l’image utilisée dans le cadre du déploiement avant de tester l’expérience utilisateur lors de la configuration initiale. Pour plus d’informations sur l’exécution de la commande Sysprep.exe, voir [Préparation de l’image en vue du déploiement](Preparing-the-Image-for-Deployment.md).  
  
> [!IMPORTANT]
>  Vous avez besoin d’une connexion réseau pour tester la configuration initiale. Le protocole permettant de tester le réseau sans interférence (DHCP) n’est ni configuré, ni installé sur le serveur.  
  
 Pour vérifier les informations de prise en charge de partenaires, cliquez sur la flèche vers le bas en regard du bouton d'aide dans le Tableau de bord.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)