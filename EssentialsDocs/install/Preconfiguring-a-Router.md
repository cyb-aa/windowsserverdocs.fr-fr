---
title: "Préconfiguration d’un routeur"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9153ac90-bb0c-4b8d-93b2-e2121ed13636
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7dc66c8a439552c2087d0348b0115adba04027ee
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="preconfiguring-a-router"></a>Préconfiguration d’un routeur

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

En règle générale, une nouvelle installation du système d’exploitation nécessite un routeur compatible avec Internet et un pare-feu pour connecter le réseau interne du client à Internet. Si vous fournissez un routeur en supplément d’un serveur préconfiguré, vous pouvez suivre des étapes supplémentaires préconfigurer le routeur pour fournir une meilleure expérience utilisateur.  
  
 Le routeur doit avoir DHCP activé. Le serveur doit être attribué à une adresse IP statique. Vous pouvez le faire par la réservation DHCP d’une adresse IP ou en affectant une adresse IP qui se trouve en dehors de la plage d’adresses DHCP.  
  
 Veillez également à préconfigurer le paramètres sur le routeur pour transférer les ports spécifiques à partir de l’interface externe du routeur à l’adresse du serveur sur le réseau interne de réacheminement de port. Le tableau suivant répertorie la configuration recommandée.  
  
|Paramètre de configuration|Détails|  
|---------------------------|-------------|  
|DHCP|Sur|  
|Réacheminement de port|Vous devez réacheminer les ports suivants à l’adresse du serveur:<br /><br /> -80 (pour une configuration hébergée, utilisez uniquement 443)<br />-   443|  
|Prise en charge UPnP|Vous devez activer la prise en charge de la UPnP fournir la configuration du routeur plus simple pour le client et la meilleure expérience client lors de l’installation.<br /><br /> **Avertissement:** l’architecture UPnP peut poser un risque de sécurité si elle est activé.|  
  
 Outre les paramètres de préconfiguration de base du routeur, vous pouvez effectuer les tâches suivantes pour fournir une expérience utilisateur plus intégrée pour gérer le routeur:  
  
-   Étendre le tableau de bord en fournissant un complément sur le serveur qui permet aux utilisateurs de gérer le routeur via une interface utilisateur personnalisée.  
  
-   Étendre les alertes d’intégrité pour les alertes à partir du routeur peuvent être observés dans l’Afficheur des alertes.  
  
-   Si le routeur prend en charge plusieurs sous-réseaux, l’adresse IP du serveur doit être remise en comme un serveur DNS via DHCP.  
  
-   Si le routeur possède une fonctionnalité de contrôle d’accès intégré pour les Services de domaine Active DirectoryÂ®, vous pouvez automatiser l’intégration ActiveDirectory lors de la Configuration initiale du serveur. Vous devez également exposer cette fonctionnalité via le complément Gestion de routeur dans le tableau de bord.  
  
> [!NOTE]
>  Pour plus d’informations sur la configuration des connexions sans fil, consultez [configurer la prise en charge pour un réseau sans fil](Configure-Support-for-a-Wireless-Network.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Prise en main du kit ADK WindowsServerEssentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)