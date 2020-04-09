---
title: Présentation de la mise en réseau de conteneurs
description: Cette rubrique est une vue d’ensemble de la pile de mise en réseau pour les conteneurs Windows et contient des liens vers des conseils supplémentaires sur la création, la configuration et la gestion des réseaux de conteneurs.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 318659e5-e4a5-4e46-99d6-211dfc46f6b8
ms.author: anpaul
author: AnirbanPaul
ms.date: 09/04/2018
ms.openlocfilehash: 8e2f10c7577f6f271c3766cff9a4fb1cc9d41140
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854312"
---
# <a name="container-networking-overview"></a>Présentation de la mise en réseau de conteneurs

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, nous vous offrons une vue d’ensemble de la pile de mise en réseau pour les conteneurs Windows. nous proposons également des liens vers des conseils supplémentaires sur la création, la configuration et la gestion des réseaux de conteneurs.

Les conteneurs Windows Server sont une méthode de virtualisation de système d’exploitation légère qui sépare les applications ou les services d’autres services qui s’exécutent sur le même hôte de conteneur. Les conteneurs Windows fonctionnent de la même façon que les machines virtuelles. Lorsqu’il est activé, chaque conteneur dispose d’une vue distincte du système d’exploitation, des processus, du système de fichiers, du Registre et des adresses IP, que vous pouvez connecter à des réseaux virtuels. 

Un conteneur Windows partage un noyau avec l’hôte de conteneur et tous les conteneurs en cours d’exécution sur l’ordinateur hôte. En raison de l’espace de noyau partagé, ces conteneurs requièrent la même version et configuration de noyau. Les conteneurs permettent d’isoler les applications grâce à la technologie d’isolation des processus et des espaces de noms.

>[!IMPORTANT]
>Les conteneurs Windows ne fournissent pas de limite de sécurité hostile et ne doivent pas être utilisés pour isoler le code non fiable. 

Avec les conteneurs Windows, vous pouvez déployer un ordinateur hôte Hyper-V dans lequel vous créez un ou plusieurs ordinateurs virtuels sur les hôtes de machines virtuelles. À l’intérieur des hôtes de machines virtuelles, les conteneurs sont créés et l’accès réseau s’effectue via un commutateur virtuel exécuté à l’intérieur de la machine virtuelle. Vous pouvez utiliser des images réutilisables stockées dans un référentiel pour déployer le système d’exploitation et les services dans des conteneurs. Chaque conteneur possède une carte réseau virtuelle qui se connecte à un commutateur virtuel, en transférant le trafic entrant et sortant. Vous pouvez attacher des points de terminaison de conteneur à un réseau hôte local (par exemple, NAT), au réseau physique ou au réseau virtuel de superposition créé via la pile SDN.

Pour appliquer l’isolation entre les conteneurs sur le même hôte, vous créez un compartiment réseau pour chaque conteneur Windows Server et Hyper-V. Les conteneurs Windows Server utilisent une carte réseau virtuelle hôte pour s’attacher au commutateur virtuel. Les conteneurs Hyper-V utilisent une carte réseau de machine virtuelle synthétique (non exposée à la machine virtuelle d’utilitaire) pour s’attacher au commutateur virtuel. 

## <a name="related-topics"></a>Rubriques connexes 

- [Mise en réseau de conteneurs Windows](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/architecture): Découvrez comment créer et gérer des réseaux de conteneurs pour les déploiements non superposés/Sdn.

- [Connecter des points de terminaison de conteneur à un réseau virtuel locataire](../../manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md): Découvrez comment créer et gérer des réseaux de conteneurs pour des réseaux virtuels de superposition avec Sdn. 