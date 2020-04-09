---
title: Préconfiguration d’un routeur
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 9153ac90-bb0c-4b8d-93b2-e2121ed13636
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c39bf3ac260a23b7fc9cc9feec7f34786b1e8aae
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819942"
---
# <a name="preconfiguring-a-router"></a>Préconfiguration d’un routeur

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

En général, une nouvelle installation du système d’exploitation nécessite un routeur et un pare-feu compatibles avec Internet pour connecter le réseau interne de l’utilisateur à Internet. Si vous prévoyez un routeur en supplément d’un serveur préconfiguré, vous pouvez effectuer des étapes supplémentaires pour préconfigurer le routeur, afin de fournir une meilleure expérience utilisateur.  
  
 Le protocole DHCP doit être activé sur le routeur. Assurez-vous qu’une adresse IP statique est attribuée au serveur. Vous pouvez soit demander la réservation DHCP d’une adresse IP, soit allouer une adresse IP hors de la plage d’adresses DHCP.  
  
 Veillez également à préconfigurer les paramètres de réacheminement de port sur le routeur afin de renvoyer des ports spécifiques depuis l’interface externe du routeur vers l’adresse du serveur sur le réseau interne. Le tableau suivant présente la configuration recommandée.  
  
|Paramètre de configuration|Détails|  
|---------------------------|-------------|  
|DHCP|Activé|  
|Réacheminement de port|Vous devez réacheminer les ports suivants vers l’adresse du serveur :<br /><br /> -80 (pour la configuration hébergée, utilisez uniquement 443)<br />-443|  
|Prise en charge UPnP|Vous devez activer la prise en charge d’UPnP pour fournir la configuration de routeur la plus simple au client et la meilleure expérience client au cours de l’installation.<br /><br /> **Avertissement :** L’architecture UPnP peut poser un risque de sécurité si elle est laissée activée.|  
  
 Outre les paramètres de préconfiguration de base du routeur, voici les différentes tâches qu’il est possible d’effectuer pour gérer le routeur dans un environnement intégré :  
  
-   Extension des fonctionnalités du Tableau de bord en mettant à la disposition du serveur un complément permettant aux utilisateurs de gérer le routeur via une interface utilisateur personnalisée.  
  
-   Amélioration des alertes d’intégrité de façon à ce que les alertes provenant du routeur soient visibles à partir de l’Afficheur des alertes.  
  
-   Si le routeur prend en charge plusieurs sous-réseaux, l’adresse IP du serveur doit être remise en tant que serveur DNS unique via le protocole DHCP.  
  
-   Si le routeur dispose d’une fonctionnalité de contrôle d’accès intégrée pour les services de domaine Active DirectoryÂ&reg;, vous pouvez automatiser l’intégration de Active Directory lors de la configuration initiale du serveur. N’oubliez pas d’exposer cette fonction via le complément de gestion du routeur dans le Tableau de bord.  
  
> [!NOTE]
>  Pour plus d'informations sur la configuration des connexions réseau sans fil, voir [Configuration de la prise en charge d'un réseau sans fil](Configure-Support-for-a-Wireless-Network.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Prise en main avec Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)