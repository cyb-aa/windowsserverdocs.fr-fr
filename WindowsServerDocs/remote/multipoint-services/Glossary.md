---
title: Glossaire
description: Définit des mots, des termes et des concepts dans MultiPoint services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 807bce1d-b993-49c6-9783-b01a3c55846c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 759dc5d0b8210dc4d8da3ef18caff2ee3ca2cceb
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871693"
---
# <a name="glossary"></a>Glossaire
**associer une station**  
Pour spécifier l’analyse utilisée avec les périphériques de station et de périphérique, tels qu’un clavier et une souris. Pour les stations connectées à une vidéo directe, cette opération s’effectue en appuyant sur une touche spécifiée sur le clavier de la station quand vous y êtes invité. Pour les stations connectées à un client USB zéro, cela se produit généralement automatiquement.  
  
**concentrateur alimenté par bus**  
Hub qui dessine toute son alimentation à partir de l’interface USB de l’ordinateur. Les hubs alimentés par bus n’ont pas besoin de connexions d’alimentation distinctes. Toutefois, de nombreux appareils ne fonctionnent pas avec ce type de concentrateur, car ils nécessitent plus de puissance que ce type de concentrateur.  
  
**mode console**  
L’un des deux modes que MultiPoint services peut démarrer. Lorsque le système est en mode console, aucune station n’est disponible pour utilisation. Au lieu de cela, toutes les analyses sont traitées comme un bureau étendu unique pour la session de console du système informatique. Le mode de console est généralement utilisé pour installer, mettre à jour ou configurer des logiciels, ce qui ne peut pas être fait lorsque l’ordinateur est en mode station. Voir aussi : *mode station*.  
  
**station directe-vidéo connectée**  
Station MultiPoint qui se compose d’une analyse directement connectée à une sortie vidéo sur le serveur, et qui comprend au minimum un clavier et une souris connectés au serveur via un concentrateur USB.  
  
**compte d’utilisateur de domaine**  
Compte d’utilisateur hébergé sur un ordinateur de domaine. Les comptes d’utilisateur de domaine sont accessibles à partir de tout ordinateur connecté au domaine et ne sont pas liés à un ordinateur particulier.  
  
**Hub en aval**  
Concentrateur connecté à un concentrateur de station pour ajouter d’autres ports disponibles pour les appareils de station. Un concentrateur en aval ne doit pas être associé à un clavier.  
  
**concentrateur alimenté de manière externe**  
Également connu sous le nom de concentrateur auto-alimenté, ce Hub prend sa puissance d’une unité d’alimentation externe ; par conséquent, elle peut fournir une alimentation complète (jusqu’à 500 mA) à chaque port. De nombreux hubs peuvent fonctionner comme des hubs alimentés par bus ou externes.  
  
**Périphérique de contrôle de consommateur HID**  
Un périphérique d’interface utilisateur (HID) est un périphérique informatique qui interagit directement avec les êtres humains. Elle peut prendre en entrée ou fournir une sortie aux êtres humains. Il s’agit par exemple du clavier, de la souris, du trackball, du pavé tactile, du dispositif de pointage, de la table graphique, de la manette, du scanneur d’empreintes digitales, du boîtier à webcam, du casque et de Un périphérique de contrôle de consommateur HID est une classe particulière de périphériques HID qui comprend des contrôles de volume audio et des clés de contrôle de navigateur et de multimédia.  
  
**Hub intermédiaire**  
Concentrateur situé entre un *concentrateur racine* sur le serveur et un concentrateur de station. Les concentrateurs intermédiaires sont généralement utilisés pour augmenter le nombre de ports disponibles pour les concentrateurs de stations ou pour étendre la distance entre les stations et l’ordinateur.  
  
**compte d’utilisateur local**  
Un compte d’utilisateur sur un ordinateur spécifique. Un compte d’utilisateur local est disponible uniquement sur l’ordinateur sur lequel le compte est défini.  
  
**concentrateur multifonction**  
Voir *client USB Zero*.  
  
**Système MultiPoint services**  
Ensemble de matériels et de logiciels comprenant un ordinateur sur lequel Windows Server 2016 est installé avec le rôle MultiPoint services activé et au moins une station MultiPoint. Pour plus d’informations sur les options de disposition du système, consultez [planification de site multipoint services](MultiPoint-services-Site-Planning.md) .  
  
**partition**  
Section d’espace sur un disque physique qui fonctionne comme s’il s’agissait d’un disque distinct.  
  
**station principale**  
Station qui est la première à démarrer au démarrage de MultiPoint services. La station principale peut être utilisée par un administrateur pour accéder aux menus et aux paramètres de démarrage. Lorsqu’il n’est pas utilisé par l’administrateur, il peut être utilisé comme station normale (il ne doit pas être réservé exclusivement à l’administration). Le moniteur de la station principale doit toujours être connecté directement à une sortie vidéo sur l’ordinateur qui exécute MultiPoint services. Voir aussi : station.  
  
**Station de connexion RDP-sur-LAN**  
Station qui est un client léger, un ordinateur de bureau traditionnel ou un ordinateur portable qui se connecte à MultiPoint services à l’aide d’protocole RDP (Remote Desktop Protocol) (RDP) via le réseau local (LAN).  
  
**concentrateur racine**  
Concentrateur USB intégré au contrôleur hôte sur la carte mère d’un ordinateur.  
  
**fractionner l’écran**  
Station dans laquelle une seule analyse peut être utilisée pour afficher deux bureaux utilisateur indépendants. Deux ensembles de hubs, claviers et souris sont associés à une seule analyse. Un jeu est associé au côté gauche de l’analyse et l’autre ensemble est associé au côté droit de l’analyse.  
  
**station standard**  
Contrairement à la *station principale*, qui peut être utilisée par un administrateur pour accéder aux menus de démarrage, les stations standard n’affichent pas les menus de démarrage et ne peuvent être utilisées qu’une fois que multipoint services a terminé le processus de démarrage. Voir aussi : station.  
  
*station*  
Point de terminaison de l’utilisateur pour la connexion à l’ordinateur qui exécute MultiPoint services. Trois types de station sont pris en charge : les stations connectées via une connexion vidéo, USB-zéro-client connectée et RDP-sur-LAN. Pour plus d’informations sur les stations, consultez [multipoint stations](MultiPoint-services-Stations.md).  
  
**concentrateur de station**  
Concentrateur USB qui a été associé à une analyse pour créer une station MultiPoint. Il connecte les périphériques USB aux services MultiPoint. Voir aussi : *Client USB Zero* et *concentrateur USB*.  
  
**mode station**  
L’un des deux modes que MultiPoint services peut démarrer. En règle générale, le système MultiPoint services est en mode station. En mode station, les stations MultiPoint services se comportent comme si chaque station était un ordinateur distinct qui exécute le système d’exploitation Windows, et plusieurs utilisateurs peuvent utiliser le système en même temps. Voir aussi : *mode console*.  
  
**Concentrateur USB**  
Concentrateur d’extension USB multiport générique conforme aux spécifications USB (Universal Serial Bus) 2,0 ou ultérieures. De tels hubs possèdent généralement plusieurs ports USB, ce qui permet de connecter plusieurs périphériques USB à un port USB unique sur l’ordinateur. Les concentrateurs USB sont généralement des périphériques distincts qui peuvent être *alimentés* de manière externe ou *alimentés par bus*. Certains autres appareils, tels que certains claviers et moniteurs vidéo, peuvent intégrer un concentrateur USB à leur conception. Voir aussi : *Client USB Zero*.  
  
**Client USB sur zéro Ethernet**  
Un client USB zéro qui se connecte à l’ordinateur par le biais d’une connexion LAN plutôt que d’un port USB. Ce client apparaît sur le serveur en tant que périphérique USB, même si les données sont envoyées via la connexion Ethernet.  
  
**Client USB Zero**  
Hub de développement qui se connecte à l’ordinateur via un port USB et permet la connexion d’un grand nombre de périphériques non USB au concentrateur. Les clients USB nuls sont générés par des fabricants de matériel spécifiques et nécessitent l’installation d’un pilote spécifique à l’appareil. Les clients USB Zero prennent en charge la connexion d’un moniteur vidéo (via VGA, DVI, etc.) et les périphériques (via USB, parfois PS/2 et audio analogique). Le client zéro USB peut être *alimenté en externe* ou *alimenté par bus*. Voir aussi *concentrateurs USB*.  
  
**Station connectée sans client USB**  
Une station MultiPoint services qui se compose (au minimum) d’un moniteur, d’un clavier et d’une souris, qui sont connectés au serveur via un client USB à zéro.  
  
