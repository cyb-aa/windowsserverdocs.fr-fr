---
title: SLB passerelle réglage des performances dans logiciel défini de réseaux
description: Obtenir des instructions sur le réseau SDN de réglage des performances de passerelle de SLB
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fede7d404ddbb4f465eff435cc340db1907ce9d2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829930"
---
# <a name="slb-gateway-performance-tuning-in-software-defined-networks"></a>SLB passerelle réglage des performances dans logiciel défini de réseaux

Équilibrage de charge est fournie par une combinaison d’un gestionnaire d’équilibrage de charge dans les machines virtuelles du contrôleur de réseau, le commutateur virtuel Hyper-V et un ensemble de machines virtuelles Multixplexor d’équilibrage de charge (Mux).

Aucun réglage des performances supplémentaire n’est nécessaire pour configurer le contrôleur de réseau ou l’hôte Hyper-V pour au-delà de ce que d’équilibrage de charge est décrite dans le [Sdn](index.md) section, sauf si vous l’utiliserez SR-IOV pour le Multiplexeurs comme décrit ci-dessous.

## <a name="slb-mux-vm-configuration"></a>Configuration de machine virtuelle Mux SLB

Machines virtuelles SLB Mux sont déployés dans une configuration actif-actif.  Cela signifie que chaque VM Mux qui est déployé et ajouté au contrôleur de réseau peut traiter les demandes entrantes.  Par conséquent, le débit total cumulé de toutes les connexions est uniquement limité par le nombre de machines virtuelles Mux que vous avez déployées.  

Une connexion individuelle à une adresse IP virtuelle (VIP) est toujours envoyée vers le même Mux, en supposant que le nombre de multiplexeurs reste constant, et par conséquent son débit est limité au débit d’une VM Mux unique.  Multiplexeurs traitent uniquement le trafic entrant destiné à une adresse IP virtuelle.  Paquets de réponse accédez directement à partir de la machine virtuelle qui envoie la réponse au commutateur physique qui le transfère au client.

Dans certains cas lorsque la source de la demande provient d’un hôte SDN est ajouté au même contrôleur de réseau qui gère l’adresse IP virtuelle, une optimisation supplémentaire du chemin d’accès entrant de la demande est également effectuée qui permet la plupart des paquets à circuler directement à partir de la client auprès du serveur, en contournant le VM Mux entièrement.  Aucune configuration supplémentaire n’est nécessaire pour cette optimisation se déroulent.

Chaque machine virtuelle SLB Mux doit être dimensionnée en respectant les instructions fournies dans la section Configuration requise rôle de machine virtuelle d’infrastructure SDN de le [planifier une Infrastructure de réseau défini par logiciel](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md) rubrique.

## <a name="single-root-io-virtualization-sr-iov"></a>Virtualisation d’e/s de racine unique (SR-IOV)

Lorsque vous utilisez 40 Gbit Ethernet, la possibilité pour le commutateur virtuel pour les paquets de processus pour les VM Mux devient le facteur de limitation pour le débit Mux VM.  Pour cette raison, il est recommandé que SR-IOV soit activé sur la carte réseau de machine virtuelle de la SLB VM pour vous assurer que le commutateur virtuel n’est pas le goulot d’étranglement.

Pour activer SR-IOV, vous devez l’activer sur le commutateur virtuel lorsque le commutateur virtuel est créé.  Dans cet exemple, nous allons créer un commutateur virtuel avec SR-IOV et de switch embedded teaming (SET) :
``` syntax
    new-vmswitch -Name SDNSwitch -EnableEmbeddedTeaming $true -NetAdapterName @("NIC1", "NIC2") -EnableIOV $true
```
Ensuite, il doit être activé sur les cartes réseau virtuel de la machine virtuelle SLB Mux qui traitent le trafic de données.  Dans cet exemple, SR-IOV est en cours activée sur toutes les cartes :
``` syntax
    get-vmnetworkadapter -VMName SLBMUX1 | set-vmnetworkadapter -IovWeight 50
```
