---
title: "Configurer la prise en charge pour un réseau sans fil"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d7020d4-fd46-4858-a406-de5c0f21ea06
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c5c98727b81bf37fdb3f90c612270462a51908c8
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="configure-support-for-a-wireless-network"></a>Configurer la prise en charge pour un réseau sans fil

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Vous pouvez configurer le système d’exploitation pour prendre en charge un réseau sans fil. Les conditions suivantes doivent être remplies pour prendre en charge sans fil sur le serveur:  
  
-   Le serveur doit disposer d’une carte réseau câblé.  
  
-   Le pilote approprié pour la carte réseau sans fil doit être préinstallé si la carte réseau n’est pas pris en charge par le système d’exploitation.  
  
-   La possibilité d’activer et désactiver la carte réseau sans fil doit être rendue disponible. Méthodes pour cela peuvent inclure un bouton physique sur le serveur ou une interface utilisateur personnalisée dans le tableau de bord. Le manuel du produit doit fournir les étapes d’activation et désactivation de la carte réseau sans fil.  
  
-   La possibilité de sélectionner un réseau sans fil et s’y connecter doit être rendue disponible. Cela doit être fait en ajoutant une interface utilisateur personnalisée au tableau de bord. Le manuel du produit doit fournir les étapes de la sélection et la connexion à un réseau sans fil.  
  
-   Si la possibilité de prendre en charge un réseau sans fil ad hoc est nécessaire, vous devez fournir une interface utilisateur améliorée dans le tableau de bord. L’interface utilisateur peut être un bouton ou un lien qui lance le paramétrage d’un Assistant de réseau sans fil Ad hoc dans le panneau de commandes dans Windows Server2008R2.  
  
## <a name="additional-considerations"></a>Considérations supplémentaires  
 Les informations suivantes sont également à envisager lors de la configuration prise en charge pour un réseau sans fil:  
  
-   Le serveur doit être connecté au réseau par un câble pour exécuter le programme d’installation.  
  
-   Un ordinateur réseau sur lequel est exécutée une restauration complète doit être connecté au réseau par un câble.  
  
-   Le serveur doit être connecté au réseau par un câble pour effectuer une restauration complète du serveur.  
  
-   Si un réseau ad hoc est créé sur le serveur, la carte réseau sans fil est dédiée pour le réseau ad hoc afin de l’utilisateur doit toujours Branchez le câble réseau au serveur pour obtenir une connexion Internet.  
  
> [!NOTE]
>  Pour plus d’informations sur la configuration des connexions réseau, voir [préconfiguration d’un routeur](Preconfiguring-a-Router.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)