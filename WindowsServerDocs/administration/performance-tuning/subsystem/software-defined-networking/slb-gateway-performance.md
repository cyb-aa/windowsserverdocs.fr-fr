---
title: Réglage des performances de la passerelle SLB dans les réseaux à définition logicielle
description: Instructions de réglage des performances de la passerelle SLB sur les réseaux SDN
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; anpaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: d1a497f24eba26b3b9b866772ae5171ea0fcbb24
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851582"
---
# <a name="slb-gateway-performance-tuning-in-software-defined-networks"></a>Réglage des performances de la passerelle SLB dans les réseaux à définition logicielle

L’équilibrage de charge logiciel est fourni par une combinaison d’un gestionnaire d’équilibrage de charge dans les machines virtuelles du contrôleur de réseau, le commutateur virtuel Hyper-V et un ensemble de machines virtuelles Load Balancer Multixplexor (MUX).

Aucun réglage des performances supplémentaire n’est nécessaire pour configurer le contrôleur de réseau ou l’hôte Hyper-V pour l’équilibrage de charge au-delà de ce qui est décrit dans la section [mise en réseau à définition logicielle](index.md) , sauf si vous utiliserez SR-IOV pour le multiplexeurs comme décrit ci-dessous.

## <a name="slb-mux-vm-configuration"></a>Configuration de machine virtuelle SLB MUX

Les ordinateurs virtuels SLB MUX sont déployés dans une configuration actif/actif.  Cela signifie que chaque machine virtuelle MUX déployée et ajoutée au contrôleur de réseau peut traiter les demandes entrantes.  Ainsi, le débit total cumulé de toutes les connexions est limité uniquement par le nombre de machines virtuelles MUX que vous avez déployées.  

Une connexion individuelle à une adresse IP virtuelle (VIP) est toujours envoyée au même multiplexeur, en supposant que le nombre de multiplexeurs reste constant et que, par conséquent, son débit est limité au débit d’une seule machine virtuelle Mux.  Multiplexeurs traite uniquement le trafic entrant qui est destiné à une adresse IP virtuelle.  Les paquets de réponse sont transmis directement à partir de la machine virtuelle qui envoie la réponse au commutateur physique qui le transmet au client.

Dans certains cas, lorsque la source de la requête provient d’un hôte SDN qui est ajouté au même contrôleur de réseau que celui qui gère l’adresse IP virtuelle, une optimisation supplémentaire du chemin d’accès entrant pour la demande est également effectuée, ce qui permet à la plupart des paquets de circuler directement du client vers le serveur, en ignorant entièrement la machine virtuelle Mux.  Aucune configuration supplémentaire n’est requise pour que cette optimisation ait lieu.

Chaque machine virtuelle SLB MUX doit être dimensionnée conformément aux instructions fournies dans la section Configuration requise du rôle de machine virtuelle SDN infrastructure de la rubrique [planifier une infrastructure réseau à définition logicielle](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md) .

## <a name="single-root-io-virtualization-sr-iov"></a>Virtualisation d’e/s de racine unique (SR-IOV)

Lorsque vous utilisez 40 Gbit Ethernet, la capacité du commutateur virtuel à traiter les paquets pour la machine virtuelle MUX devient le facteur de limitation pour le débit des machines virtuelles Mux.  Pour cette raison, il est recommandé d’activer SR-IOV sur la carte réseau de machine virtuelle de la machine virtuelle SLB pour vous assurer que le commutateur virtuel n’est pas le goulot d’étranglement.

Pour activer SR-IOV, vous devez l’activer sur le commutateur virtuel lors de la création du commutateur virtuel.  Dans cet exemple, nous créons un commutateur virtuel avec switch Embedded Teaming (SET) et SR-IOV :
``` syntax
    new-vmswitch -Name SDNSwitch -EnableEmbeddedTeaming $true -NetAdapterName @("NIC1", "NIC2") -EnableIOV $true
```
Ensuite, il doit être activé sur la ou les cartes réseau virtuelles de la machine virtuelle SLB MUX qui traite le trafic de données.  Dans cet exemple, SR-IOV est activé sur tous les adaptateurs :
``` syntax
    get-vmnetworkadapter -VMName SLBMUX1 | set-vmnetworkadapter -IovWeight 50
```
