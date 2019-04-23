---
title: Présentation de la mise en réseau de conteneurs
description: Cette rubrique est une vue d’ensemble de la pile de mise en réseau pour les conteneurs Windows et inclut des liens vers des conseils supplémentaires sur la création, la configuration et la gestion des réseaux de conteneur.
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
ms.date: 09/04/2018
ms.openlocfilehash: 72b1ac739d9012ac7b90e97abe22e5f321ddba63
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886140"
---
# <a name="container-networking-overview"></a>Présentation de la mise en réseau de conteneurs

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, nous vous donnons une vue d’ensemble de la pile de mise en réseau pour les conteneurs Windows et nous incluons des liens vers des conseils supplémentaires sur la création, la configuration et la gestion des réseaux de conteneur.

Conteneurs Windows Server constituent une méthode de la virtualisation de système d’exploitation léger séparation des applications ou services provenant d’autres services qui s’exécutent sur le même hôte de conteneur. Fonction de conteneurs Windows de la même façon pour les machines virtuelles. Lorsque l’option est activée, chaque conteneur possède une vue distincte du système d’exploitation, processus, au système de fichiers, au Registre et aux adresses IP, vous pouvez vous connecter à des réseaux virtuels. 

Un conteneur Windows partage un noyau avec l’hôte de conteneur et tous les conteneurs en cours d’exécution sur l’ordinateur hôte. En raison de l’espace de noyau partagé, ces conteneurs requièrent la même version et configuration de noyau. Les conteneurs fournissent l’isolation des applications via une technologie d’isolation des processus et l’espace de noms.

>[!IMPORTANT]
>Les conteneurs Windows ne fournissent pas d’une limite de sécurité hostile et ne doivent pas servir à isoler le code non fiable. 

Avec des conteneurs Windows, vous pouvez déployer un hôte Hyper-V, où vous créez un ou plusieurs ordinateurs virtuels sur les hôtes de machine virtuelle. Dans les hôtes des machines virtuelles, créer des conteneurs, et l’accès réseau se fait via un commutateur virtuel en cours d’exécution à l’intérieur de la machine virtuelle. Vous pouvez utiliser des images réutilisables stockées dans un référentiel à déployer le système d’exploitation et des services dans des conteneurs. Chaque conteneur possède une carte réseau virtuelle qui se connecte à un commutateur virtuel, transférer le trafic entrant et sortant. Vous pouvez attacher des points de terminaison de conteneur à un réseau hôte local (par exemple, NAT), que le réseau physique ou surcouche de réseau virtuel créé via la pile SDN.

Pour mettre en œuvre l’isolation entre les conteneurs sur le même hôte, vous créez un compartiment réseau pour chaque conteneur Windows Server et Hyper-V. Les conteneurs Windows Server utilisent une carte réseau virtuelle hôte pour s’attacher au commutateur virtuel. Les conteneurs Hyper-V utilisent une carte réseau de machine virtuelle synthétique (non exposée à la machine virtuelle d’utilitaire) pour s’attacher au commutateur virtuel. 

## <a name="related-topics"></a>Rubriques connexes 

- [Mise en réseau de conteneur Windows](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/architecture): Découvrez comment créer et gérer des réseaux de conteneur pour les déploiements de superposition/SDN.

- [Connecter les points de terminaison de conteneur à un réseau virtuel locataire](../../manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md): Découvrez comment créer et gérer des réseaux de conteneur pour les réseaux virtuels de superposition avec SDN. 