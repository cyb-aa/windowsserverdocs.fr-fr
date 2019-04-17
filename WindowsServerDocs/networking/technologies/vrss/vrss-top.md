---
title: Partage du trafic entrant virtuel \(VRSS\)
description: Découvrez les virtuel \(vrss\ () dans Windows Server et comment configurer une carte réseau virtuelle pour charger un équilibre le trafic réseau entrant sur plusieurs cœurs de processeur logique dans une machine virtuelle. Vous pouvez également configurer des cœurs physiques multiples pour un hôte de carte réseau virtuelle (vNIC).
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9be477b3-f81d-4e84-a6b0-ac4c1ea97715
ms.date: 09/05/2018
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0c1cb11cb8ce69463a31cfa5061290f79d8dda91
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133685"
---
# Virtuel recevoir côté \(vRSS\) de mise à l’échelle

>S’applique à: Windows Server (canal semi-annuel), WindowsServer2016

Dans cette rubrique, vous découvrez virtuel \(vrss\ () et comment configurer une carte réseau virtuelle pour charger un équilibre le trafic réseau entrant sur plusieurs cœurs de processeur logique dans une machine virtuelle. Vous pouvez également utiliser vRSS pour configurer plusieurs cœurs physiques pour un hôte virtuel carte d’Interface réseau \(vNIC\).

Cette configuration permet le chargement à partir d’une carte réseau virtuelle réparti sur plusieurs processeurs virtuels dans une machine virtuelle \(VM\), ce qui permet de l’ordinateur virtuel traiter plus rapidement que possible avec un seul processeur logique de davantage de trafic réseau.

>[!TIP]
>Vous pouvez utiliser vRSS dans des machines virtuelles sur les hôtes Hyper\-V avec plusieurs processeurs, un seul cœur plusieurs, ou à plusieurs multiples cœurs installés et configurés pour une utilisation de la machine virtuelle.

vRSS est compatible avec tous les autres technologies de réseau Hyper\-V. vRSS dépend de \(VMQ\) file d’attente de la Machine virtuelle dans l’hôte Hyper\-V et RSS dans la machine virtuelle ou sur la carte réseau virtuelle hôte.

Par défaut, Windows Server permet vRSS, mais vous pouvez la désactiver dans une machine virtuelle à l’aide de Windows PowerShell. Pour plus d’informations, voir [vRSS de gérer](vrss-manage.md) et [Les commandes Windows PowerShell pour RSS et vRSS](vrss-wps.md).



## Compatibilité du système d’exploitation

Vous pouvez utiliser RSS sur n’importe quel plusieurs processeurs multicœur ordinateur - ou ou vRSS sur n’importe quel ordinateur virtuel plusieurs processeurs ou multicœur - qui exécute Windows Server 2016.

Multiprocesseur ou multicœur ordinateurs virtuels qui exécutent les systèmes d’exploitation Microsoft suivants prennent également en charge les vRSS.

- WindowsServer2016
- Windows 10 Professionnel ou entreprise
- WindowsServer2012R2
- Windows 8.1 Professionnel ou entreprise
- Windows Server 2012 avec les composants d’intégration de Windows Server 2012 R2 installés.
- Windows 8 avec les composants d’intégration de Windows Server 2012 R2 installés.

Pour plus d’informations sur la prise en charge de vRSS pour les ordinateurs virtuels en cours d’exécution FreeBSD ou Linux comme système d’exploitation invité sur Hyper-V, voir [les machines virtuelles prises en charge de Linux et FreeBSD pour Hyper-V sur Windows](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows).
  
## Configuration matérielle requise

Voici la configuration matérielle requise pour vRSS.
 
- Cartes réseau physiques doivent prendre en charge la file d’attente de la Machine virtuelle \(VMQ\). Si VMQ est désactivé ou non pris en charge, vRSS est désactivé pour l’hôte Hyper\-V et les ordinateurs virtuels configurés sur l’ordinateur hôte.
- Cartes réseau doivent avoir au moins une vitesse de liaison de 10 Gbits/s.
- Les hôtes Hyper\-V doivent être configurés avec plusieurs processeurs ou au moins un plusieurs processeurs à utiliser vRSS.
- Ordinateurs \(VMs\) doivent être configurés pour utiliser les deux ou plusieurs processeurs logiques.


## Exemples de scénarios d’utilisation

Les suivant deux scénarios représenter d’utilisation courante des vRSS pour l’équilibrage de charge du processeur et d’équilibrage de charge.

### Équilibrage de charge du processeur
  
Anthony, un administrateur de réseau, consiste à configurer un nouvel hôte Hyper-V avec la carte réseau deux qui prend en charge la virtualisation de sortie de l’entrée racine unique \(SR\-IOV\). Il déploie Windows Server 2016 pour héberger un serveur de fichiers de machine virtuelle.

Après avoir installé les matériels et logiciels, Anthony configure un ordinateur virtuel pour utiliser huit processeurs virtuels et 4 096 Mo de mémoire. Malheureusement, Anthony n’a pas la possibilité d’activer SR\-IOV, car ses machines virtuelles s’appuient sur l’application des stratégies via le commutateur virtuel qu'il a créé avec le Gestionnaire de commutateur virtuel Hyper\-V. Pour cette raison, Anthony décide d’utiliser des vRSS au lieu de SR\-IOV.

Initialement, Anthony attribue quatre processeurs virtuels à l’aide de Windows PowerShell pour être disponible pour une utilisation avec vRSS. L’utilisation du serveur de fichiers une fois par semaine semble être très populaires, donc Anthony vérifie les performances de la machine virtuelle.  Il détecte l’intégralité de quatre processeurs virtuels.

Pour cette raison, Anthony décide d’ajouter des processeurs à la machine virtuelle pour une utilisation par vRSS.  Il affecte deux processeurs virtuels plus à la machine virtuelle, qui sont automatiquement disponibles pour vRSS pour aider à gérer la charge réseau de grande taille. Ses efforts de résultat de meilleures performances pour le serveur de fichiers de machine virtuelle, avec les six processeurs gère efficacement la charge de trafic réseau.


### Équilibrage de charge logicielle

Sandra, un administrateur de réseau, consiste à configurer un seul ordinateur virtuel de hautes performances sur un son système d’agir comme un équilibreur de charge logicielle. Elle a affecté tous les processeurs logiques disponibles à cet ordinateur virtuel unique.

Après l’installation de Windows Server, elle utilise vRSS pour obtenir du trafic réseau parallèle de traitement sur plusieurs processeurs logiques dans la machine virtuelle. Étant donné que Windows Server Active vRSS, Sandra ne doit apporter des modifications de configuration.


## Rubriques associées

- [Planifier l’utilisation de vRSS](vrss-plan.md)
- [Activer vRSS sur une carte réseau virtuelle](vrss-enable.md)
- [Gérer les vRSS](vrss-manage.md)
- [vRSS Forum aux Questions](vrss-faq.md)
- [Commandes Windows PowerShell pour RSS et vRSS](vrss-wps.md)

---
