---
title: Vue d’ensemble de mise en réseau de conteneur
description: Cette rubrique est une vue d’ensemble de la pile de mise en réseau pour les conteneurs Windows et inclut des liens vers des informations supplémentaires sur la création, la configuration et la gestion des réseaux de conteneur.
manager: ravirao
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 318659e5-e4a5-4e46-99d6-211dfc46f6b8
ms.author: pashort
author: jmesser81
ms.openlocfilehash: fd2f022948208d4aacce2994ff053e77384b28fc
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="container-networking-overview"></a>Vue d’ensemble de mise en réseau de conteneur

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique est une vue d’ensemble de la pile de mise en réseau pour les conteneurs Windows et inclut des liens vers des informations supplémentaires sur la création, la configuration et la gestion des réseaux de conteneur.

Conteneurs Windows Server constituent une méthode de virtualisation de système d’exploitation léger utilisée pour séparer les services ou applications à partir d’autres services qui s’exécutent sur le même hôte de conteneur. Pour ce faire, chaque conteneur possède sa propre vue du système d’exploitation, processus, système de fichiers, Registre et des adresses IP.

Fonction de conteneurs Windows de la même façon pour les ordinateurs virtuels dans le plan de mise en réseau. Chaque conteneur possède une carte réseau virtuelle qui est connectée à un commutateur virtuel sur lequel le trafic entrant et sortant est transféré. Pour appliquer l’isolation entre les conteneurs sur le même ordinateur hôte, un compartiment réseau est créé pour chaque conteneur Hyper-V dans lequel la carte réseau pour le conteneur est installée et de Windows Server. Conteneurs Windows Server utilisent une carte réseau virtuelle hôte à associer au commutateur virtuel. Conteneurs Hyper-V permet d’associer au commutateur virtuel une carte réseau VM synthétique (ne pas exposée à l’ordinateur virtuel utilitaire). 

Points de terminaison de conteneur peuvent être attachés à un réseau de l’hôte local (par exemple, NAT), le réseau physique ou un réseau virtuel superposition créé par le biais de la pile Microsoft logiciel défini de mise en réseau (SDN). 

Pour plus d’informations sur la création et la gestion des réseaux de conteneur pour les déploiements de superposition/SDN, reportez-vous à la [mise en réseau de conteneur Windows](https://msdn.microsoft.com/en-us/virtualization/windowscontainers/management/container_networking) guide sur MSDN.

Pour plus d’informations sur la création et la gestion des réseaux de conteneur pour les réseaux virtuels superposition avec SDN, reportez-vous à [connecter les points de terminaison de conteneur à un réseau virtuel locataire ](../../manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md). 