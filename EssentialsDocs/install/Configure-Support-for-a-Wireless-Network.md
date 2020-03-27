---
title: Configuration de la prise en charge pour un réseau sans fil
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d7020d4-fd46-4858-a406-de5c0f21ea06
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4ce5f167339d7910b10f90bea5bbeae5606b5cf2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312216"
---
# <a name="configure-support-for-a-wireless-network"></a>Configuration de la prise en charge pour un réseau sans fil

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous pouvez configurer le système d’exploitation de façon à prendre en charge un réseau sans fil. Un certain nombre de conditions doivent être remplies :  
  
-   Le serveur doit être équipé d’une carte réseau câblé.  
  
-   Le pilote de la carte réseau câblé correct doit être préinstallé si la carte n’est pas prise en charge par le système d’exploitation.  
  
-   Il doit être possible d’activer ou de désactiver la carte réseau sans fil (par le biais d’un bouton sur le serveur ou d’une interface utilisateur personnalisée dans le Tableau de bord). Le manuel de référence du produit doit expliquer comment procéder pour activer ou désactiver la carte réseau sans fil.  
  
-   Il doit être possible de sélectionner un réseau sans fil et de s’y connecter. Pour ce faire, il convient d’ajouter une interface utilisateur personnalisée au Tableau de bord. Le manuel de référence du produit doit fournir les instructions nécessaires pour sélectionner un réseau sans fil et établir la connexion.  
  
-   Si la prise en charge d’un réseau ad hoc sans fil est nécessaire, vous devez prévoir une interface utilisateur améliorée dans le Tableau de bord. Il peut s’agir, en l’occurrence, d’un bouton ou d’un lien permettant de lancer l’Assistant Configurer un réseau sans fil ad hoc dans le Panneau de configuration de Windows Server 2008 R2.  
  
## <a name="additional-considerations"></a>Considérations supplémentaires  
 Vous devez également tenir compte des points suivants lors de la configuration de la prise en charge d’un réseau sans fil :  
  
-   Le serveur doit être connecté au réseau à l’aide d’un câble pour pouvoir effectuer l’installation.  
  
-   L’ordinateur du réseau sur lequel a lieu une récupération complète doit être relié au réseau par un câble.  
  
-   Le serveur doit être connecté au réseau par un câble pour procéder à la récupération complète du serveur.  
  
-   Si un réseau ad hoc est créé sur le serveur, la carte réseau sans fil est dédiée au réseau ad hoc. C’est la raison pour laquelle l’utilisateur est tenu de raccorder le câble du réseau au serveur pour obtenir une connexion Internet.  
  
> [!NOTE]
>  Pour plus d'informations sur la configuration des connexions réseau, voir [Préconfiguration d'un routeur](Preconfiguring-a-Router.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)