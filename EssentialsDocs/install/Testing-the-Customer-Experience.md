---
title: "Test de l’expérience client"
description: "Décrit comment utiliser WindowsServerEssentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="testing-the-customer-experience"></a>Test de l’expérience client

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Pour vérifier l’expérience du client et vérifier vos personnalisations de partenaires, exécutez par le biais de la Configuration initiale d’un ordinateur cible. Il est recommandé d’effectuer la Configuration initiale au moins une fois manuellement pour étudier l’expérience utilisateur. Si vous choisi le tableau de bord, vous devez effectuer la Configuration initiale pour vérifier la personnalisation. Si vous choisi le site accès Web à distance, vous devez accéder à http://<servername\&gt; pour vérifier la personnalisation (< servername\ > est le nom du serveur). Vous pouvez utiliser la section Configuration initiale du fichier cfg.ini pour automatiser le test de l’expérience utilisateur. Pour plus d’informations sur la création de cette section dans le fichier cfg.ini [créer le fichier Cfg.ini](Create-the-Cfg.ini-File.md).  
  
> [!IMPORTANT]
>  Vous devez exécuter la commande Sysprep.exe pour préparer l’image de déploiement avant de tester l’expérience de la Configuration initiale. Pour plus d’informations sur l’exécution de Sysprep.exe, consultez [préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md).  
  
> [!IMPORTANT]
>  Une connexion réseau est nécessaire pour tester la Configuration initiale. DHCP n’est pas configuré ou installé sur le serveur, ce qui permet de tester le réseau sans interférence.  
  
 Pour vérifier le partenaire de prise en charge d’informations, dans le tableau de bord, cliquez sur la flèche vers le bas en regard du bouton d’aide.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)