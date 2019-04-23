---
title: Glossaire
description: Définit les mots, termes et concepts dans MultiPoint Services
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
ms.openlocfilehash: 4449d2d6fb87f74496b7d482a7a7263f703c7822
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831110"
---
# <a name="glossary"></a>Glossaire
**associer une station**  
Pour spécifier quel moniteur est utilisé avec les station et les périphériques, par exemple un clavier et une souris. Pour les stations connectées vidéo directe, cela en appuyant sur une clé spécifiée sur le clavier de la station lorsque vous êtes invité à le faire. Pour USB zéro client connecté stations, cela se produit généralement automatiquement.  
  
**concentrateur alimenté par bus**  
Hub qui dessine toute sa puissance à partir de l’interface USB de l’ordinateur. Alimenté par bus de hubs n’avez pas besoin de connexions d’alimentation distinctes. Toutefois, de nombreux périphériques ne fonctionnent pas avec ce type de concentrateur, car elles nécessitent plus de puissance que fournit ce type de concentrateur.  
  
**mode console**  
Un des deux modes de MultiPoint services peut démarrer. Lorsque le système est en mode console, aucune stations ne sont disponibles pour utilisation. Au lieu de cela, tous les moniteurs sont traités comme un seul bureau étendu pour la session de console de l’ordinateur. Mode console sert généralement à installer, mettre à jour ou configurer le logiciel, qui ne peut pas être effectué lorsque l’ordinateur est en mode station. Voir aussi : *mode station*.  
  
**station connectée direct-vidéo**  
Une station MultiPoint se compose d’un moniteur est directement connecté à une sortie vidéo sur le serveur et au minimum, il inclut un clavier et une souris qui sont connectés au serveur via un concentrateur USB.  
  
**Compte d’utilisateur de domaine**  
Un compte d’utilisateur qui est hébergé sur un ordinateur de domaine. Comptes d’utilisateur de domaine est accessible à partir de n’importe quel ordinateur connecté au domaine, et ils ne sont pas liés à n’importe quel ordinateur particulier.  
  
**downstream hub**  
Un concentrateur qui est connecté à un concentrateur de station pour ajouter des ports supplémentaires disponibles pour les appareils de la station. Un concentrateur en aval ne doit pas avoir de clavier.  
  
**concentrateur alimenté de façon externe**  
Également appelé un concentrateur alimenté d’automatique, ce hub prend sa puissance à partir d’une alimentation externe ; Par conséquent, il peut fournir toute la puissance (jusqu'à 500 mA) pour chaque port. De nombreux concentrateurs peuvent fonctionner en tant que hubs alimenté par bus ou alimentés de façon externe.  
  
**Périphérique de contrôle consommateur HID**  
Un périphérique HID (Human Interface) est un appareil de l’ordinateur qui interagit directement avec l’homme. Il peut prendre d’entrée à partir d’ou transmettre une sortie à l’homme. Clavier, souris, trackball, pavé tactile, dispositif de pointage, table graphics, manette, lecteur d’empreintes digitales, gamepad, webcam, casque et conduite simulateurs de périphériques sont des exemples. Un périphérique de contrôle consommateur est une classe particulière de périphériques d’interface utilisateur qui comprend les contrôles de volume audio et les touches de contrôle de navigateur et des applications multimédias.  
  
**concentrateur intermédiaire**  
Un concentrateur est entre un *concentrateur racine* sur le serveur et un concentrateur de station. Hubs intermédiaires sont généralement utilisées pour augmenter le nombre de ports disponibles pour les concentrateurs de stations ou pour étendre la distance des stations à partir de l’ordinateur.  
  
**compte d’utilisateur local**  
Un compte d’utilisateur sur un ordinateur spécifique. Un compte d’utilisateur local est disponible uniquement sur l’ordinateur où le compte est défini.  
  
**concentrateur multifonction**  
Consultez *USB zéro client*.  
  
**Système multiPoint Services**  
Une collection de matériels et logiciels qui se compose d’un ordinateur équipé de Windows Server 2016 est installé avec le rôle de MultiPoint Services activé et au moins une station MultiPoint. Pour plus d’informations sur les options de disposition de système, consultez [planification de Site MultiPoint Services](MultiPoint-services-Site-Planning.md)  
  
**partition**  
Une section d’espace sur un disque physique qui fonctionne comme si elle était un disque distinct.  
  
**station principale**  
La station est le premier à démarrer lorsque MultiPoint Services est démarré. La station principale peut être utilisée par un administrateur pour accéder aux paramètres et les menus de démarrage. Quand il n’est pas utilisé par l’administrateur, il peut être utilisé comme station normale (il ne doit pas être réservé exclusivement pour l’administration). Moniteur de la station principale doit toujours être connecté directement à une sortie vidéo sur l’ordinateur qui exécute MultiPoint Services. Voir aussi : station.  
  
**Station RDP-over-connectés au réseau local**  
Une station est un client léger, un bureau traditionnel ou un ordinateur portable qui se connecte à MultiPoint services à l’aide du protocole RDP (Remote Desktop) via le réseau local (LAN).  
  
**concentrateur racine**  
Un concentrateur USB qui est intégré au contrôleur sur la carte mère d’un ordinateur hôte.  
  
**diviser l’écran**  
Une station où un moniteur unique peut être utilisé pour afficher les deux bureaux d’utilisateurs indépendants. Deux ensembles de concentrateurs, des claviers et des souris sont associés à un même écran. Un ensemble est associé avec le côté gauche de l’analyse, et l’autre ensemble est associé à la partie droite de l’analyse.  
  
**station standard**  
Contrairement à la *station principale*, qui peut être utilisé par un administrateur pour accéder aux menus de démarrage, stations standards n’affichera pas les menus de démarrage, et ils peuvent être utilisés uniquement lorsque MultiPoint Services a terminé le processus de démarrage . Voir aussi : station.  
  
*station*  
Point de terminaison utilisateur pour la connexion à l’ordinateur qui exécute MultiPoint Services. Trois types de station sont prises en charge : stations vidéo-connectés directement, zéro-client-connectés par USB et RDP-over-connectés au réseau local. Pour plus d’informations sur les stations, consultez [Stations MultiPoint](MultiPoint-services-Stations.md).  
  
**concentrateur de station**  
Un concentrateur USB qui a été associé à une analyse pour créer une station MultiPoint. Il connecte les périphériques USB à MultiPoint Services. Voir aussi : *USB zéro client* et *concentrateur USB*.  
  
**mode station**  
Un des deux modes de MultiPoint services peut démarrer. En règle générale, le système MultiPoint Services est en mode station. En mode station, les stations MultiPoint Services se comportent comme si chaque station est un ordinateur distinct qui exécute le système d’exploitation Windows, et plusieurs utilisateurs peuvent utiliser le système en même temps. Voir aussi : *mode console*.  
  
**Concentrateur USB**  
Un concentrateur USB multiport générique d’extension qui est conforme aux spécifications du bus USB (USB) 2.0 ou version ultérieure. Ces concentrateurs disposent généralement plusieurs ports USB, ce qui permet de connecter plusieurs périphériques USB d’être connecté à un seul port USB sur l’ordinateur. Concentrateurs USB sont généralement des périphériques qui peuvent être *alimentés de façon externe* ou *alimenté par bus*. D’autres périphériques, tels que certains claviers et les moniteurs vidéo, peuvent inclure un concentrateur USB dans leur conception. Voir aussi : *USB zéro client*.  
  
**USB sur un client Ethernet zéro**  
USB zéro client qui se connecte à l’ordinateur via une connexion de réseau local plutôt qu’un port USB. Ce client s’affiche sur le serveur comme un périphérique USB même via les données est envoyé via la connexion Ethernet.  
  
**USB zéro client**  
Concentrateur d’extension qui se connecte à l’ordinateur via un port USB et permet de connecter divers appareils non-USB au concentrateur. Les clients USB zéro sont produites par des fabricants de matériel spécifique, et ils nécessitent l’installation d’un pilote spécifique à l’appareil. Les clients USB zéro prend en charge la connexion d’un moniteur vidéo (via VGA, DVI et ainsi de suite) et les périphériques (via USB, parfois PS/2 et audio analogique). USB zéro client peut être *alimentés de façon externe* ou *alimenté par bus*. Voir aussi *concentrateurs USB*.  
  
**Station de connectée USB zéro client**  
Une station MultiPoint Services comprenant (au minimum) un moniteur, clavier et une souris, qui sont connecté au serveur via un client USB zéro.  
  
